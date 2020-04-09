---
title: BdeHdCfg newdriveletter
description: O tópico de comandos do Windows para **BdeHdCfg newdriveletter**, que atribui uma nova letra da unidade à parte de uma unidade usada como unidade do sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a8757e7d0684912525817708fbe34953b049582
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851049"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter

Atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------| ----------- |
|`<DriveLetter>`|Define a letra da unidade que será atribuída à unidade de destino especificada.|

## <a name="remarks"></a>Comentários

Como prática recomendada, é recomendável que você não atribua uma letra de unidade à unidade do sistema.

## <a name="examples"></a><a name="BKMK_Examples"></a>Disso

O exemplo a seguir mostra a unidade padrão que está sendo atribuída à letra de unidade P.

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)