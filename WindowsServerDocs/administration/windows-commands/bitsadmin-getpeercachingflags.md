---
title: bitsadmin getpeercachingflags
description: O tópico de comandos do Windows para **Bitsadmin getpeercachingflags** -recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o bits pode baixar conteúdo para o trabalho de pares.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381440"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso
O exemplo a seguir recupera os sinalizadores para o trabalho chamado *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)


