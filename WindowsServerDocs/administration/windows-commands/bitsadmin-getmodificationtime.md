---
title: bitsadmin getmodificationtime
description: O tópico de comandos do Windows para **Bitsadmin getmodificatime** -recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48b4d252ce6161b288726190f41f08c64efdbcf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381530"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



Recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a hora da última modificação para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)