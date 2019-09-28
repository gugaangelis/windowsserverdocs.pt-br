---
title: BdeHdCfg silencioso
description: O tópico de comandos do Windows para BdeHdCfg Quiet-informa BdeHdCfg para não exibir todas as ações e erros.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d59a14e34200e3fa8e18e36e166ef62ceca1afe7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382223"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: Quiet



Informa a ferramenta de linha de comando BdeHdCfg de que todas as ações e erros não devem ser exibidos na interface de linha de comando. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parâmetros

Este comando não usa parâmetros adicionais.

## <a name="remarks"></a>Comentários

Se algum prompt Sim/Não (S/N) tiver sido exibido durante a preparação da unidade, uma resposta "Sim" será presumida. Para exibir os erros ocorridos durante a preparação da unidade, examine o log de eventos do sistema no provedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **Quiet** .
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)