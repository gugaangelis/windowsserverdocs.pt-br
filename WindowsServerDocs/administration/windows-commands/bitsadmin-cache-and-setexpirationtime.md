---
title: bitsadmin cache and setexpirationtime
description: Artigo de referência para o Bitsadmin cache e o comando setexpiretime, que define o tempo de expiração do cache.
ms.topic: reference
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e07e862d8577c33daec24bbc93fe5859f13944db
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028804"
---
# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache and setexpirationtime

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o tempo de expiração do cache.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| seg | O número de segundos até que o cache expire. |

## <a name="examples"></a>Exemplos

Para definir o cache para expirar em 60 segundos:

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
