---
title: getanteriority Bitsadmin
description: Os comandos do Windows tópico para **Bitsadmin getanteriority** – recupera a prioridade do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 0b8914f27c690aa9bb9cbf30430b3edf55f2eb92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381434"
---
# <a name="bitsadmin-getpriority"></a>getanteriority Bitsadmin

Recupera a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

A prioridade é em **primeiro plano**, **alta**, **normal**, **baixa**ou **desconhecida**.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a prioridade para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
