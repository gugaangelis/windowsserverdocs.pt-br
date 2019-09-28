---
title: bitsadmin geterror
description: O tópico de comandos do Windows para **Bitsadmin GetError** -recupera informações detalhadas de erro para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f9bd607886d00ede4e1da91ed73eff2794db6ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381637"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



Recupera informações detalhadas de erro para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera as informações de erro para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)