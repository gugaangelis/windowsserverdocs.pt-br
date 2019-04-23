---
title: bdehdcfg restart
description: Tópico de comandos do Windows para a reinicialização de bdehdcfg - informa ao bdehdcfg que o computador deve ser reiniciado após a conclusão da preparação de unidade.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879457"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: restart



Informa a ferramenta de linha de comando Bdehdcfg que o computador deve ser reiniciado após a conclusão da preparação de unidade. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parâmetros

Esse comando não usa nenhum parâmetro adicional.

## <a name="remarks"></a>Comentários

Se outros usuários estiverem conectados ao computador e o **silencioso** comando não for especificado, será exibido um prompt para confirmar que o computador deve ser reiniciado.

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **reiniciar** comando.
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)