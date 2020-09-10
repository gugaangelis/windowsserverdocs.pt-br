---
title: delete shadows
description: Artigo de referência do comando excluir sombras, que exclui cópias de sombra.
ms.topic: reference
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f8fe95ac21c36f4605a544c97036c0a02bddaf42
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628853"
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
