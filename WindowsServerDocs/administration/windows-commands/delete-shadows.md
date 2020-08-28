---
title: delete shadows
description: Artigo de referência do comando excluir sombras, que exclui cópias de sombra.
ms.topic: reference
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2613fc978db8c8e5b323df142b204a7270f6bad
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024200"
---
# <a name="delete-shadows"></a>delete shadows

Exclui cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
delete shadows [all | volume <volume> | oldest <volume> | set <setID> | id <shadowID> | exposed {<drive> | <mountpoint>}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---- | ---- |
| all | Exclui todas as cópias de sombra. |
| volume `<volume>` | Exclui todas as cópias de sombra do volume especificado. |
| antigas `<volume>` | Exclui a cópia de sombra mais antiga do volume especificado. |
| Definição `<setID>` | Exclui as cópias de sombra no conjunto de cópias de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| sessão `<shadowID>` | Exclui uma cópia de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| exposto {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Excluir comando](delete.md)
