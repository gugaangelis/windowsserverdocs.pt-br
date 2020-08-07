---
title: delete shadows
description: Artigo de referência do comando excluir sombras, que exclui cópias de sombra.
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d1679b2d05265aa1fb5a089fab9cf99f840cd9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891374"
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
| volume`<volume>` | Exclui todas as cópias de sombra do volume especificado. |
| antigas`<volume>` | Exclui a cópia de sombra mais antiga do volume especificado. |
| Definição`<setID>` | Exclui as cópias de sombra no conjunto de cópias de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| sessão`<shadowID>` | Exclui uma cópia de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| exposto {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Excluir comando](delete.md)
