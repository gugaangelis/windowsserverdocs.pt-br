---
title: bitsadmin getbytestotal
description: Artigo de referência para o comando Bitsadmin getbytestotal, que recupera o tamanho do trabalho especificado.
ms.topic: reference
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8496234c0907061997ddd790e03f78d84c5380
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024420"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera o tamanho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o tamanho do trabalho chamado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
