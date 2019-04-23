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
ms.openlocfilehash: 868f7ac58a69c067681bf0597524fbaa5a25984f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842417"
---
#<a name="bitsadmin-getmaxdownloadtime"></a>Bitsadmin getmaxdownloadtime

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
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)


