---
title: bitsadmin getbytestransferred
description: Tópico de comandos do Windows para **getbytestransferred bitsadmin** -recupera o número de bytes transferidos para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce2c051af169385c43fdff4efdeff46d8422926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814607"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred



Recupera o número de bytes transferidos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetBytesTransferred <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o número de bytes transferidos do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetBytesTransferred myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)