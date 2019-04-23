---
title: bitsadmin info
description: Tópico de comandos do Windows para **exibe informações resumidas sobre o trabalho especificado.** -info bitsadmin
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851517"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Exibe informações resumidas sobre o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Use o /verbose parâmetro para fornecer informações detalhadas sobre o trabalho.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera informações sobre o trabalho denominado *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)