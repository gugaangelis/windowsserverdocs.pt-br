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
ms.openlocfilehash: a832e8f3d21681f67a4486df33c387e5a8456718
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434877"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Define o estado de validação de conteúdo do arquivo determinado dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parâmetros

| Parâmetro  |          Descrição           |
|------------|--------------------------------|
|    Job     | Nome de exibição ou o GUID do trabalho |
| Índice de arquivo |         Começa em 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho de *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)