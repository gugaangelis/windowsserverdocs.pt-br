---
title: bitsadmin cache and setlimit
description: Artigo de referência para o Bitsadmin cache e o comando setlimit, que define o limite de tamanho do cache.
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a1331a19f66e7d84dc3eb57b04d42596a40628
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894701"
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
