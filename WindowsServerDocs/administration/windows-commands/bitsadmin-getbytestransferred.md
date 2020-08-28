---
title: bitsadmin getbytestransferred
description: Artigo de referência para o comando Bitsadmin getbytestransferred, que recupera o número de bytes transferidos para o trabalho especificado.
ms.topic: reference
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67e1432f2fbef32ac47320d87993f5d62053bb71
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028734"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

Recupera o número de bytes transferidos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o número de bytes transferidos para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
