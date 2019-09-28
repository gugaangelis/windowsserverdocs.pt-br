---
title: bitsadmin setmaxdownloadtime
description: O tópico de comandos do Windows para **Bitsadmin setmaxdownloadtime** -define o tempo limite de download em segundos.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380563"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime



Define o tempo limite de download em segundos.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Limite de tempo|O tempo limite em segundos|

## <a name="remarks"></a>Comentários

-   N/D

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define o tempo limite para o trabalho chamado *myDownloadJob* como 10 segundos.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)