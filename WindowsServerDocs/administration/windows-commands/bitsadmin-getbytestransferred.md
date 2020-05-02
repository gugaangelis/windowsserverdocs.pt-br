---
title: bitsadmin getbytestransferred
description: Tópico de referência para o comando Bitsadmin getbytestransferred, que recupera o número de bytes transferidos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c333926ed46dd2e66e0e2507f838f721a73c192
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718144"
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
