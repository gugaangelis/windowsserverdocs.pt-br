---
title: bitsadmin setmaxdownloadtime
description: Artigo de referência para o comando Bitsadmin setmaxdownloadtime, que define o tempo limite de download em segundos.
ms.topic: reference
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9937a4f2945e45d351cfa3506a9aefbd4723f37c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630813"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

Define o tempo limite de download em segundos.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| tempo limite | O comprimento do tempo limite de download, em segundos. |

## <a name="examples"></a>Exemplos

Para definir o tempo limite para o trabalho chamado *myDownloadJob* como 10 segundos.

```
bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
