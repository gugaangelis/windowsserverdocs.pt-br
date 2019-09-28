---
title: bdehdcfg
description: O tópico de comandos do Windows para **BdeHdCfg** -prepara um disco rígido com as partições necessárias para criptografia de unidade de disco BitLocker.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acfe2582905402f7be149148520784be6ad47453
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382187"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara um disco rígido com as partições necessárias para Criptografia de Unidade de Disco BitLocker. A maioria das instalações do Windows 7 não precisarão usar essa ferramenta, pois a configuração do BitLocker inclui a capacidade de preparar e reparticionar unidades conforme necessário.

> [!WARNING]
> Existe um conflito conhecido com a configuração **Negar acesso de gravação a unidades fixas não protegidas por BitLocker** da Política de Grupo localizada em **Configuração do Computador\Modelos Administrativos\Componentes do Windows\Criptografia de Unidade de Disco BitLocker\Unidades de Dados Fixas**.</br>> Se o BdeHdCfg for executado em um computador quando essa configuração de política estiver habilitada, você poderá encontrar os seguintes problemas:</br>>-Se você tentou reduzir a unidade e criar a unidade do sistema, o tamanho da unidade será reduzido com êxito e uma partição bruta será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: "A nova unidade ativa não pode ser formatada. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>>-Se você tentou usar espaço não alocado para criar a unidade do sistema, uma partição bruta será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: "A nova unidade ativa não pode ser formatada. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>>-Se você tentou mesclar uma unidade existente na unidade do sistema, a ferramenta falhará ao copiar o arquivo de inicialização necessário para a unidade de destino para criar a unidade do sistema. A seguinte mensagem de erro é exibida: "Falha da configuração do BitLocker ao copiar arquivos de inicialização. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>> Se essa configuração de política estiver sendo imposta, um disco rígido não poderá ser reparticionado porque a unidade está protegida. Se estiver atualizando os computadores de sua organização a partir de uma versão anterior do Windows e eles estiverem configurados com uma única partição, você deverá criar a partição de sistema BitLocker necessária antes de aplicar a configuração da política aos computadores.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[BdeHdCfg: DriveInfo](bdehdcfg-driveinfo.md)|Exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características de partição das partições na unidade especificada. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas.|
|[BdeHdCfg: destino](bdehdcfg-target.md)|Define qual parte de uma unidade usar como a unidade do sistema e torna a parte ativa.|
|[BdeHdCfg: newdriveletter](bdehdcfg-newdriveletter.md)|Atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema.|
|[BdeHdCfg: tamanho](bdehdcfg-size.md)|Determina o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada.|
|[BdeHdCfg: Quiet](bdehdcfg-quiet.md)|Impede a exibição de todas as ações e erros na interface de linha de comando e direciona o BdeHdCfg para usar a resposta "Sim" para quaisquer prompts Sim/Não que possam ocorrer durante a preparação de unidade subsequente.|
|[BdeHdCfg: reiniciar](bdehdcfg-restart.md)|Instrui o computador a reiniciar após a conclusão da preparação da unidade.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir descreve BdeHdCfg sendo usado com a unidade padrão para criar uma partição de sistema de 500 MB. Como nenhuma letra da unidade foi especificada, a nova partição do sistema não terá uma letra da unidade.
```
bdehdcfg -target default -size 500
```
O exemplo a seguir ilustra o BdeHdCfg que está sendo usado com a unidade padrão para criar uma partição do sistema (P:) do tamanho padrão de 300 MB de espaço não alocado na unidade. A ferramenta não solicitará a entrada do usuário nem nenhum erro será exibido. Depois que a unidade do sistema tiver sido criada, o computador será reiniciado automaticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)