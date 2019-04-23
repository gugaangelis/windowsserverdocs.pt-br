---
title: bdehdcfg newdriveletter
description: Tópico de comandos do Windows para newdriveletter bdehdcfg - atribui uma nova letra de unidade para a parte de uma unidade usada como a unidade do sistema.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887137"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



Atribui uma nova letra de unidade para a parte de uma unidade usada como a unidade do sistema. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DriveLetter>|Define a letra da unidade que será atribuída à unidade de destino especificado.|

## <a name="remarks"></a>Comentários

Como prática recomendada, é recomendável que você não atribua uma letra de unidade para a unidade do sistema.

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir mostra a unidade padrão que está sendo atribuída a letra P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)