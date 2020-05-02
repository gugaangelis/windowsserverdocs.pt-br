---
title: BdeHdCfg newdriveletter
description: Tópico de referência para o comando BdeHdCfg newdriveletter, que atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da09ae1469c6fc8370e6bd0f2f7a8f3efd8dc4f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718664"
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

Para atribuir a unidade padrão à letra `P`da unidade:

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
