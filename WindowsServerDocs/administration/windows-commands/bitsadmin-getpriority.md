---
title: Bitsadmin getpriority
description: Tópico de comandos do Windows para **getpriority bitsadmin** -recupera a prioridade do trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 6be2461ed87b75144367b1bd74376381e4674b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841437"
---
# <a name="bitsadmin-getpriority"></a>Bitsadmin getpriority

Recupera a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

É a prioridade **em primeiro plano**, **alta**, **NORMAL**, **baixa**, ou **desconhecido**.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a prioridade do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
