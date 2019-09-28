---
title: bitsadmin getfilestotal
description: Tópico de comandos do Windows para **Bitsadmin getfilestotal** – recupera o número de arquivos no trabalho especificado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 27cf04e8745aeab5cd1f2ce379c8506be642fea2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381611"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera o número de arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o número de arquivos incluídos no trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Chave de sintaxe de linha de comando](command-line-syntax-key.md) Consulte também