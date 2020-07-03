---
title: list shadows
description: Artigo de referência para o comando listar sombras, que lista as cópias de sombra persistentes e existentes não persistentes que estão no sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcf1946f5b2424eb7aa13af51bd6ff13c43349c1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931792"
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