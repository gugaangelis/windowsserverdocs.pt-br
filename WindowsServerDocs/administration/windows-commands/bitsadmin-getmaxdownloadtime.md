---
title: bitsadmin getmaxdownloadtime
description: Artigo de referência para o comando Bitsadmin getmaxdownloadtime, que recupera o tempo limite de download em segundos.
ms.prod: windows-servemr
ms.topic: reference
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 610553a839bb36bd764da1283ba398af49824731
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030334"
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
