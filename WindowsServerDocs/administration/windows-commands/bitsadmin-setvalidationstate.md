---
title: Bitsadmin setvalidable
description: O tópico de comandos do Windows para **Bitsadmin setvalidable** – define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380404"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidable



Define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parâmetros

| Parâmetro  |          Descrição           |
|------------|--------------------------------|
|    Job     | O nome de exibição ou o GUID do trabalho |
| Índice de arquivo |         Começa com 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho chamado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)