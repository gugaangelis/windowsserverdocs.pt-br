---
title: bitsadmin list
description: Artigo de referência do comando lista de Bitsadmin, que lista os trabalhos de transferência de Propriedade do usuário atual.
ms.topic: reference
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1d36a9236c834cea76e653b9a2e639c3b8f964e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024370"
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
