---
title: listar sombras
description: Tópico de referência para o comando listar sombras, que lista as cópias de sombra persistentes e existentes não persistentes que estão no sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0261a25c7a70a0c8690d578cadc9e73ff9a62e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817166"
---
# <a name="list-shadows"></a>listar sombras

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