---
title: bitsadmin getfilestotal
description: Tópico de comandos do Windows para **getfilestotal bitsadmin** -recupera o número de arquivos em que o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5f6d32b3410b182c510cf40b9def5370efafdc4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435125"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera o número de arquivos em que o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o número de arquivos incluídos no trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Chave de sintaxe de linha de comando](command-line-syntax-key.md) Consulte também