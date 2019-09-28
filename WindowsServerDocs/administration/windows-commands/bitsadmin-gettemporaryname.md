---
title: Bitsadmin gettemporárioname
description: O tópico de comandos do Windows para **Bitsadmin gettemporárioname** -relata o nome de arquivo temporário do arquivo fornecido dentro do trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381304"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporárioname



Informa o nome de arquivo temporário do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Índice de arquivo|Começa com 0|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir relata o nome de arquivo temporário de File 2 para o trabalho chamado *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)