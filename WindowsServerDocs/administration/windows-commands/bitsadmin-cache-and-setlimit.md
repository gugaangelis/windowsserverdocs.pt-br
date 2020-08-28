---
title: bitsadmin cache and setlimit
description: Artigo de referência para o Bitsadmin cache e o comando setlimit, que define o limite de tamanho do cache.
ms.topic: reference
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edcd83ace72e301471b03ac0c1fc85439a1c4d94
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028784"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache and setlimit

Define o limite de tamanho do cache.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| {1&gt;percent&lt;1} | O limite de cache definido como uma porcentagem do espaço total no disco rígido. |

## <a name="examples"></a>Exemplos

Para definir o limite de tamanho do cache como 50%:

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
