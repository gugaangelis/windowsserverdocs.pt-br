---
title: bitsadmin setdescription
description: Tópico de comandos do Windows para **setdescription bitsadmin** -define a descrição do trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830747"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



Define a descrição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Descrição|Texto usado para descrever o trabalho.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a descrição do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)