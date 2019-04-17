---
title: Fluxos de integridade ReFS
description: 
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 11/14/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: d9e14e74591b341048316e9c2e69a312062c3304
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="refs-integrity-streams"></a>Fluxos de integridade ReFS
>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10

Os fluxos de integridade são um recurso opcional do ReFS que valida e mantém a integridade dos dados usando somas de verificação. Embora o ReFS sempre use somas de verificação para metadados, por padrão, o ReFS não gera ou valida somas de verificação para dados de arquivos. Fluxos de integridade é um recurso opcional que permite que os usuários utilizam somas de verificação para dados de arquivo. Quando fluxos de integridade estiverem habilitados, ReFS pode determinar claramente se os dados são válidos ou corrompidos. Além disso, o ReFS e os Espaços de Armazenamento podem corrigir dados e metadados corrompidos automaticamente em conjunto.

## <a name="how-it-works"></a>Como funciona 

Os fluxos de integridade podem ser habilitados para arquivos individuais, diretórios ou todo o volume, as configurações de fluxo de integridade podem ser ativadas e desativadas a qualquer momento. Além disso, as configurações de fluxo de integridade para arquivos e diretórios são herdadas dos diretórios pai. 

Depois que os fluxos de integridade são habilitados, o ReFS criam e mantêm uma soma de verificação para os arquivos especificados nos metadados do arquivo. Essa soma de verificação permite que o ReFS valide a integridade dos dados antes de acessá-los. Antes de retornar dados que tenham fluxos de integridade habilitados, o ReFS calculam a soma de verificação primeiro:

<img src=media/compute-checksum.gif alt="Compute checksum for file data"/>

Em seguida, essa soma de verificação é comparada com a soma de verificação contida nos metadados do arquivo. Se as somas de verificação coincidirem, os dados serão marcados como válidos e retornados ao usuário. Se as somas de verificação não coincidirem, então os dados estão corrompidos. A resiliência do volume determina como o ReFS responde a danos:

- Se o ReFS for montado em um espaço simples não resiliente ou em uma unidade básica, o ReFS retornará um erro para o usuário sem retornar os dados corrompidos. 
- Se o ReFS for montado em um espaço de espelhamento ou paridade resiliente, o ReFS tentará corrigir dano. 
    - Se a tentativa for bem-sucedida, o ReFS aplicará a uma gravação de corretiva para restaurar a integridade dos dados e retornará os dados válidos para o aplicativo. O aplicativo continuará não ciente dos danos.
    - Se a tentativa for malsucedida, o ReFS retornará um erro. 

O ReFS registrará todos os danos no Log de Eventos do Sistema, e no log constará se os danos foram corrigidos. 

<img src=media/corrective-write.gif alt="Corrective write restores data integrity."/>

## <a name="performance"></a>Desempenho 

Embora os fluxos de integridade forneçam maior integridade de dados para o sistema, eles também geram um custo de desempenho. Há alguns motivos diferentes para isso:
- Se os fluxos de integridade forem habilitados, todas as operações de gravação se tornarão operações de alocação mediante gravação. Embora isso evite quaisquer afunilamentos de leitura/modificação/gravação pois o ReFS não precisa ler ou modificar qualquer dado existente, os dados de arquivo frequentemente tornam-se fragmentados, o que atrasa as leituras. 
- Dependendo da carga de trabalho e do armazenamento subjacente do sistema, o custo da computação e validação da soma de verificação pode causar o aumento da latência de E/S. 

Como os fluxos de integridade têm um custo de desempenho, recomendamos deixar os fluxos de integridade desabilitados em sistemas de desempenho sensível. 

## <a name="integrity-scrubber"></a>Depurador de integridade

Conforme descrito acima, ReFS validará automaticamente a integridade dos dados antes de acessar quaisquer dados. ReFS também usa um depurador em segundo plano, que permite que ReFS valide dados acessados com pouca frequência. Esse programa de depuração verifica periodicamente o volume, identifica danos latentes e aciona o reparo de quaisquer dados corrompidos de forma proativa.

  >[!NOTE]
  >O depurador de integridade de dados pode validar apenas dados de arquivo para arquivos onde os fluxos de integridade estão habilitado.

Por padrão, o depurador é executado a cada quatro semanas, embora esse intervalo possa ser configurado no Agendador de tarefas em Microsoft\Windows\Verificação de Integridade de Dados. 

## <a name="examples"></a>Exemplos
Para monitorar e alterar as configurações de integridade de dados de arquivos, o ReFS usa os cmdlets **Get-FileIntegrity** e **Set-FileIntegrity**.

### <a name="get-fileintegrity"></a>Get-FileIntegrity
Para ver se os fluxos de integridade estão habilitados para dados de arquivos, use o cmdlet **Get-FileIntegrity**. 

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

Você também pode usar o cmdlet **Get-Item** para obter as configurações de fluxo integridade para todos os arquivos do diretório especificado. 

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
Para habilitar/desabilitar fluxos de integridade para dados de arquivos, use o cmdlet **Set-FileIntegrity**. 

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

Você também pode usar o cmdlet **Get-Item** para definir as configurações de fluxo integridade para todos os arquivos da pasta especificada. 

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True 
```

O cmdlet **Set-FileIntegrity** também pode ser usado diretamente em volumes e diretórios. 

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="see-also"></a>Consulte também

-   [Visão geral do ReFS](refs-overview.md)
-   [Clonagem de blocos ReFS](block-cloning.md)
-   [Visão geral de Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)
