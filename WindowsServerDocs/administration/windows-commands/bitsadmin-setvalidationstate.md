---
title: Bitsadmin setvalidationstate
description: Tópico de comandos do Windows para **setvalidationstate bitsadmin** -define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 647389aaac06d1eb109052548c1b24f7579bde2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851237"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Define o estado de validação de conteúdo do arquivo determinado dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Índice de arquivo|Começa em 0|
|True|False|Definido como TRUE se o conteúdo do arquivo é válido, caso contrário, defina como FALSE|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho de *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)