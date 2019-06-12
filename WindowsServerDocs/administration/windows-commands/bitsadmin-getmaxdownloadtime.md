---
title: Bitsadmin getmaxdownloadtime
description: Tópico de comandos do Windows para **getmaxdownloadtime bitsadmin** -recupera o tempo limite de download em segundos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d067d6a0821d9af4784c02c6a332e8eddd2352c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434939"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>Bitsadmin getmaxdownloadtime

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o tempo limite de download em segundos.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

-   N\/A

## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir obtém o tempo máximo de download do trabalho nomeado *myDownloadJob* em segundos.

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)


