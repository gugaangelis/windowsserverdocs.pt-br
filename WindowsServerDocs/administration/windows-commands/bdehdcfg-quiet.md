---
title: bdehdcfg quiet
description: Tópico de comandos do Windows para silencioso bdehdcfg - informa ao bdehdcfg para não exibir todos os erros e ações.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865747"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: quiet



Informa a ferramenta de linha de comando Bdehdcfg que todas as ações e erros não devem ser exibidos na interface de linha de comando. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parâmetros

Esse comando não usa nenhum parâmetro adicional.

## <a name="remarks"></a>Comentários

Se algum prompt Sim/Não (S/N) tiver sido exibido durante a preparação da unidade, uma resposta "Sim" será presumida. Para exibir os erros ocorridos durante a preparação da unidade, examine o log de eventos do sistema no provedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **silencioso** comando.
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)