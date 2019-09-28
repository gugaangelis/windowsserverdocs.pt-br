---
title: Bitsadmin getvalidable
description: 'O tópico de comandos do Windows para **Bitsadmin getvalidable** -relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho. '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca4269a596010258edd0479f5a7e9844bc9c98df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381261"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidable



Relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Índice de arquivo|Começa com 0|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir obtém o estado de validação de conteúdo do arquivo 2 no trabalho chamado *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)