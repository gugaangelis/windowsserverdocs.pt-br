---
title: excluir sombras
description: Tópico de referência para o comando excluir sombras, que exclui cópias de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b757314c96024741795c6770a98d10ac23b5bd0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993108"
---
# <a name="delete-shadows"></a>excluir sombras

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
