---
title: bitsadmin getmaxdownloadtime
description: Tópico de referência para o comando Bitsadmin getmaxdownloadtime, que recupera o tempo limite de download em segundos.
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c63acee7629267ed10df11fb8cf4eeb0c911e118
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717862"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o tempo limite de download em segundos.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para obter o tempo de download máximo para o trabalho chamado *myDownloadJob* em segundos:

```
bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
