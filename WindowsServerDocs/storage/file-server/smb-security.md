---
title: Aprimoramentos de segurança SMB
description: Obter uma explicação sobre o recurso de criptografia SMB no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233522"
---
# <a name="smb-security-enhancements"></a>Aprimoramentos de segurança SMB

>Aplica-se a: Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2016

Este tópico explica os aprimoramentos de segurança no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016 SMB.

## <a name="smb-encryption"></a>Criptografia SMB

Criptografia SMB fornece a criptografia de ponta a ponta de dados SMB e protege os dados contra interceptação ocorrências em redes não confiáveis. Você pode implantar criptografia SMB com esforço mínimo, mas ele pode exigir pequeno custo adicional para especializados de hardware ou software. Ele não tem requisitos para Internet Protocol security (IPsec) ou aceleradores de WAN. Criptografia SMB podem ser configurada em uma base por compartilhar ou para o servidor de todo o arquivo, e ele pode ser habilitado para uma variedade de cenários onde dados percorrem redes não confiáveis.

>[!NOTE]
>Criptografia SMB não aborda a segurança em repouso, que é geralmente tratada pela criptografia de unidade de disco BitLocker.

Criptografia SMB devem ser considerada para qualquer cenário no qual os dados confidenciais precisam ser protegidos contra ataques man-in-the-middle. Os possíveis cenários incluem:

- Dados confidenciais de um profissional da informação são movidos por meio do protocolo SMB. Criptografia SMB oferece uma garantia de privacidade e a integridade de ponta a ponta entre o servidor de arquivo e o cliente, independentemente das redes desviada, como longa distância (WAN) conexões de rede que são mantidas por provedores de não-Microsoft.
- SMB 3.0 permite que os servidores de arquivo fornecer armazenamento continuamente disponível para aplicativos de servidor, SQL Server ou o Hyper-V. Habilitando a criptografia SMB oferece uma oportunidade para proteger essas informações contra ataques snooping. Criptografia SMB é mais simples de usar do que as soluções de hardware dedicado que são necessárias para a maioria das redes de área de armazenamento (SANs).

>[!IMPORTANT]
>Observe que não há um custo com qualquer proteção de criptografia de ponta a ponta, quando comparada com não-criptografado operacional notável no desempenho.

## <a name="enable-smb-encryption"></a>Habilitar criptografia SMB

Você pode habilitar criptografia SMB para o servidor de arquivo inteiro ou somente para os compartilhamentos de arquivo específicos. Use um dos procedimentos a seguir para habilitar a criptografia SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar criptografia SMB com o Windows PowerShell

1. Para habilitar a criptografia SMB para um compartilhamento de arquivo individuais, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar a criptografia SMB para o servidor de todo o arquivo, digite o seguinte script no servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para criar um novo compartilhamento de arquivos SMB com criptografia SMB habilitada, digite o seguinte script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar criptografia SMB com o Gerenciador de servidores

1. No Gerenciador de servidores, abra o **arquivo e serviços de armazenamento**.
2. Selecione **compartilhamentos** para abrir a página de gerenciamento de compartilhamentos.
3. Com o botão direito no compartilhamento no qual você deseja habilitar a criptografia SMB e selecione **Propriedades**.
4. Na página **configurações** do compartilhamento, selecione **acesso a criptografar dados**. Acesso remoto a arquivos para esse compartilhamento é criptografado.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerações sobre a implantação de criptografia SMB

Por padrão, quando criptografia SMB está habilitada para um compartilhamento de arquivo ou o servidor, somente os clientes SMB 3.0 têm permissão para acessar os compartilhamentos de arquivo especificado. Isso impõe a intenção de proteger os dados para todos os clientes que acessam os compartilhamentos do administrador. No entanto, em alguns casos, um administrador talvez queira permitir o acesso não criptografado para clientes que não têm suporte SMB 3.0 (por exemplo, durante um período de transição quando as versões de sistema operacional do cliente misto estão sendo usadas). Para permitir o acesso não criptografado para clientes que não têm suporte SMB 3.0, digite o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

O recurso de negociação de dialeto seguro descrito na próxima seção impede que um ataque man-in-the-middle desatualizar uma conexão do SMB 3.0 para SMB 2.0 (que usaria o access não criptografado). No entanto, ele não impede um downgrade para SMB 1.0, que também for resultar em acesso não criptografado. Para garantir que os clientes SMB 3.0 sempre usam criptografia SMB acessar compartilhamentos criptografados, você deve desativar o servidor SMB 1.0. (Para obter instruções, consulte a seção [Desabilitando SMB 1.0](#disabling-smb-1.0).) Se a configuração **– RejectUnencryptedAccess** for deixada em sua configuração padrão de **$true**, clientes que somente suporte à criptografia SMB 3.0 têm permissão para acessar os compartilhamentos de arquivos (SMB 1.0 clientes também serão rejeitados).

>[!NOTE]
>* Criptografia SMB usa o Advanced criptografia AES (padrão)-algoritmo CCM para criptografar e descriptografar os dados. AES-CCM também fornece validação da integridade dos dados (assinatura) para compartilhamentos de arquivos criptografados, independentemente das configurações de assinatura de SMB. Se você deseja habilitar a assinatura sem criptografia SMB, você pode continuar a fazer isso. Para obter mais informações, consulte [Noções básicas sobre o da assinatura de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Você pode encontrar problemas quando você tenta acessar o compartilhamento de arquivos ou o servidor se sua organização usa aparelhos de aceleração de rede (WAN) de longa distância.
>* Com uma configuração padrão (onde não há nenhum acesso descriptografado permitido nos compartilhamentos de arquivos criptografados), se os clientes que não têm suporte SMB 3.0 tentar acessar um compartilhamento de arquivos criptografados, o ID do evento 1003 é registrada no log de eventos do Microsoft-Windows-SmbServer/operacional , e o cliente receberá uma mensagem de erro de **acesso negado** .
>* Criptografia SMB e o Encrypting File System (EFS) no sistema de arquivos NTFS estão relacionados e criptografia SMB não exige ou dependem usando EFS.
>* Criptografia SMB e a criptografia de unidade de disco BitLocker não estão relacionados e criptografia SMB não exige ou dependem usando a criptografia de unidade de disco BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociação de dialeto seguro

SMB 3.0 é capaz de detecção de ataques de man-in-the-middle que tentam downgrade o protocolo SMB 2.0 ou SMB 3.0 ou os recursos que o cliente e servidor negociar. Quando esse ataque é detectada pelo cliente ou servidor, a conexão é desconectada e ID de evento 1005 é registrada no log de eventos do Microsoft-Windows-SmbServer/operacional. Seguro dialeto negociação não pode detectar ou impedir rebaixamentos de SMB 2.0 ou 3.0 para SMB 1.0. Dessa forma e para aproveitar os recursos completos de criptografia SMB, é altamente recomendável que você desabilite o servidor SMB 1.0. Para obter mais informações, consulte [desativando SMB 1.0](#disabling-smb-1.0).

O recurso de negociação de dialeto segura é descrito na próxima seção impede que um ataque man-in-the-middle desatualizar uma conexão de SMB 3 como 2 SMB (que usaria o access não criptografado); No entanto, ele não impede rebaixamentos para SMB 1, que também for resultar em acesso não criptografado. Para obter mais informações sobre possíveis problemas com anteriormente não Windows implementações de SMB, consulte o [Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Novo algoritmo de assinatura

SMB 3.0 usa um algoritmo de criptografia mais recente para assinar: avançadas criptografia AES (padrão) - cifra - based código message authentication code (CMAC). SMB 2.0 usado o algoritmo de criptografia HMAC-SHA256 mais antigo. AES-CMAC e AES-CCM podem acelerar significativamente a criptografia de dados em CPUs mais modernas que tem a instrução AES de suporte. Para obter mais informações, consulte [Noções básicas sobre o da assinatura de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Desabilitando o SMB 1.0

O serviço de navegador do computador herdado e recursos de protocolo de administração remota no SMB 1.0 agora são separados, e eles podem ser eliminados. Esses recursos ainda estão habilitados por padrão, mas se você não tiver mais antigos clientes SMB, como computadores que executam o Windows Server 2003 ou Windows XP, você pode remover os recursos de SMB 1.0 para aumentar a segurança e possivelmente reduzir a aplicação de patch.

>[!NOTE]
>SMB 2.0 foi introduzido no Windows Server 2008 e Windows Vista. Clientes mais antigos, como computadores que executam o Windows Server 2003 ou Windows XP, não suportam SMB 2.0; e, portanto, eles não poderão acessar compartilhamentos de arquivos ou compartilhamentos de impressão se o servidor SMB 1.0 estiver desabilitado. Além disso, alguns clientes SMB não Microsoft podem não ser capazes de acessar compartilhamentos de arquivos SMB 2.0 ou imprimir compartilhamentos (por exemplo, impressoras com funcionalidade "varredura para compartilhar").

Antes de começar a desabilitação SMB 1.0, você precisará descobrir se seus clientes SMB atualmente estão conectados ao servidor que executa o SMB 1.0. Para fazer isso, insira o seguinte cmdlet do Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Você deve executar esse script várias vezes ao longo de uma semana (várias vezes por dia) para criar uma trilha de auditoria. Você também pode executar este como uma tarefa agendada.

Para desabilitar o SMB 1.0, insira o seguinte script no Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se uma conexão de cliente SMB foi negada porque o servidor que executa o SMB 1.0 tiver sido desabilitado, ID de evento 1001 será registrada no log de eventos do Microsoft-Windows-SmbServer/operacional.

## <a name="more-information"></a>Mais informações

Aqui estão alguns recursos adicionais sobre SMB e as tecnologias relacionadas no Windows Server 2012.

- [Protocolo SMB](file-server-smb-overview.md)
- [Armazenamento no Windows Server](../storage.md)
- [Servidor de arquivos de dimensionamento para dados de aplicativo](../../failover-clustering/sofs-overview.md)