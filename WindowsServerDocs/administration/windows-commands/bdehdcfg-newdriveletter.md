---
title: bdehdcfg newdriveletter
description: Artigo de referência para o comando BdeHdCfg newdriveletter, que atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema.
ms.topic: reference
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5ac14224b55782e8e36cfe156a900e1614bd118d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632884"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter

Atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema. Como prática recomendada, recomendamos não atribuir uma letra de unidade à unidade do sistema.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------| ----------- |
| `<drive_letter>` | Define a letra da unidade que será atribuída à unidade de destino especificada. |

## <a name="examples"></a>Exemplos

Para atribuir a unidade padrão à letra da unidade `P` :

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
