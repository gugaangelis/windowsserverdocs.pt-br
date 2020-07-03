---
title: bdehdcfg newdriveletter
description: Artigo de referência para o comando BdeHdCfg newdriveletter, que atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f210056f74e930ad39361c9fc0cbf05d6e1894f4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923494"
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
