---
title: bitsadmin getcompletiontime
description: Tópico de comandos do Windows para **getcompletiontime bitsadmin** -recupera o tempo que o trabalho terminou a transferência de dados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3790a91c4b347b982c0f0a023d5977a8d6cd1f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857377"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Recupera o tempo que o trabalho terminou a transferência de dados.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a hora em que o trabalho chamado *myDownloadJob* terminou a transferência de dados.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)