---
title: cache Bitsadmin e deleteURL
description: Artigo de referência para o cache Bitsadmin e o comando deleteURL, que exclui todas as entradas de cache para a URL fornecida.
ms.topic: reference
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6e1c3af4cfbb6c1ef21a86ead6d3975bf1f44cf6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632626"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin e deleteURL

Exclui todas as entradas de cache para a URL fornecida.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| URL | O localizador de recursos uniforme que identifica um arquivo remoto. |

## <a name="examples"></a>Exemplos

Para excluir todas as entradas de cache para `https://www.contoso.com/en/us/default.aspx` :

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
