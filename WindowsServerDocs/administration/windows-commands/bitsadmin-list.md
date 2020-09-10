---
title: bitsadmin list
description: Artigo de referência do comando lista de Bitsadmin, que lista os trabalhos de transferência de Propriedade do usuário atual.
ms.topic: reference
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 81fecf15f16cfa28933b63f9de693ba4e07679e8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631505"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Lista os trabalhos de transferência de Propriedade do usuário atual.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| /allusers | Opcional. Lista os trabalhos para todos os usuários. Você deve ter privilégios de administrador para usar esse parâmetro. |
| /verbose | Opcional. Fornece informações detalhadas sobre cada trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações sobre trabalhos pertencentes ao usuário atual.

```
bitsadmin /list
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
