---
title: Bitsadmin getvalidationstate
description: 'Tópico de comandos do Windows para **getvalidationstate bitsadmin** -relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho. '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879157"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidationstate



Relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Índice de arquivo|Começa em 0|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir obtém o estado de validação de conteúdo do arquivo 2 dentro do trabalho nomeado *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)