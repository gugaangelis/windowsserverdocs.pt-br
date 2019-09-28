---
title: bitsadmin getcompletiontime
description: O tópico de comandos do Windows para **Bitsadmin getcompletetime** – recupera a hora em que o trabalho concluiu a transferência de dados.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 190467d5f3a7b7244ed0d7ab3b75d4cbbf56c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381736"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Recupera a hora em que o trabalho concluiu a transferência de dados.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a hora em que o trabalho nomeado *myDownloadJob* concluiu a transferência de dados.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)