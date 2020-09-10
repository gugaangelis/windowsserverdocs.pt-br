---
title: bitsadmin cache and setlimit
description: Artigo de referência para o Bitsadmin cache e o comando setlimit, que define o limite de tamanho do cache.
ms.topic: reference
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7547a2a51104285b10af6b02c1962c89f75d51fa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632487"
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
