---
title: list shadows
description: Artigo de referência para o comando listar sombras, que lista as cópias de sombra persistentes e existentes não persistentes que estão no sistema.
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d06c529740d7137fb1bdc4cb8d7661eb3f69ffb7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887573"
---
# <a name="list-shadows"></a>list shadows

Lista cópias de sombra persistentes e existentes não persistentes que estão no sistema.

## <a name="syntax"></a>Sintaxe

```
list shadows {all | set <setID> | id <shadowID>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ---------- |
| all | Lista todas as cópias de sombra. |
| Definição`<setID>` | Lista as cópias de sombra que pertencem à ID do conjunto de cópias de sombra especificado. |
| sessão`<shadowID>` | Lista qualquer cópia de sombra com a ID de cópia de sombra especificada. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)