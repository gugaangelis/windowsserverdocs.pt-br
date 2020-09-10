---
title: list shadows
description: Artigo de referência para o comando listar sombras, que lista as cópias de sombra persistentes e existentes não persistentes que estão no sistema.
ms.topic: reference
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4781587fd7bb82525746184c3fee2f0f9258c510
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635271"
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
| Definição `<setID>` | Lista as cópias de sombra que pertencem à ID do conjunto de cópias de sombra especificado. |
| sessão `<shadowID>` | Lista qualquer cópia de sombra com a ID de cópia de sombra especificada. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)