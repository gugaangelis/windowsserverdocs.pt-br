---
title: cache Bitsadmin e deleteURL
description: Tópico de referência para o cache Bitsadmin e o comando deleteURL, que exclui todas as entradas de cache para a URL fornecida.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 075c48e5c8c205cbbf3fe476260ec7909edcc3e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718444"
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

Para excluir todas as entradas de `https://www.contoso.com/en/us/default.aspx`cache para:

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
