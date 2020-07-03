---
title: bitsadmin list
description: Artigo de referência do comando lista de Bitsadmin, que lista os trabalhos de transferência de Propriedade do usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d9a2c86536ff0910b4e0a8bea15ec43d9371087
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926564"
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
