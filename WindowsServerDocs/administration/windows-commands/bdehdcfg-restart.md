---
title: reinicialização BdeHdCfg
description: Tópico de comandos do Windows para BdeHdCfg Restart – informa BdeHdCfg de que o computador deve ser reiniciado depois que a preparação da unidade for concluída.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c4e48b051f567c98ea679feaa22f995982a899
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382213"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg: reiniciar



Informa a ferramenta de linha de comando BdeHdCfg de que o computador deve ser reiniciado depois que a preparação da unidade for concluída. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parâmetros

Este comando não usa parâmetros adicionais.

## <a name="remarks"></a>Comentários

Se outros usuários estiverem conectados ao computador e o comando **silencioso** não for especificado, um prompt será exibido para confirmar que o computador deve ser reiniciado.

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **Restart** .
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)