---
title: bitsadmin cache and setlimit
description: Artigo de referência para o Bitsadmin cache e o comando setlimit, que define o limite de tamanho do cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de218990d9176336e779b551bfacc0897df5d114
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923212"
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
