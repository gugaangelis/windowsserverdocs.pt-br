---
title: Bitsadmin getproxylist – recupera a lista de proxy para o trabalho especificado.
description: Artigo de referência para o comando Bitsadmin getproxylist, que recupera a lista de proxy para o trabalho especificado.
ms.topic: reference
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9038f9faaedce30bfdf6025a40e3f6369805ac2a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024410"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera a lista delimitada por vírgulas de servidores proxy a serem usados para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a lista de proxy para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
