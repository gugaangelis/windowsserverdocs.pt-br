---
title: bitsadmin getmodificationtime
description: Tópico de comandos do Windows para **getmodificationtime bitsadmin** -recupera a última hora em que o trabalho foi modificado ou dados foi transferidos com êxito.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4257f0ae4868b2f18221ab99268384f778c4bbbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837017"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



Recupera a última vez em que o trabalho foi modificado ou dados foi transferidos com êxito.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a hora da última modificação do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)