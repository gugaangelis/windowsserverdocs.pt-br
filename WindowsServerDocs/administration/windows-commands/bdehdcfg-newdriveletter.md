---
title: BdeHdCfg newdriveletter
description: Tópico de comandos do Windows para BdeHdCfg newdriveletter – atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382235"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter



Atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DriveLetter >|Define a letra da unidade que será atribuída à unidade de destino especificada.|

## <a name="remarks"></a>Comentários

Como prática recomendada, é recomendável que você não atribua uma letra de unidade à unidade do sistema.

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir mostra a unidade padrão que está sendo atribuída à letra de unidade P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)