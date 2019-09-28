---
title: bitsadmin info
description: O tópico de comandos do Windows para **exibe informações de resumo sobre o trabalho especificado.** -informações do Bitsadmin
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381084"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Exibe informações de resumo sobre o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Use o parâmetro/Verbose para fornecer informações detalhadas sobre o trabalho.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera informações sobre o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)