---
title: bitsadmin suspend
description: Tópico de comandos do Windows para **bitsadmin suspender** -suspende o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e1bbd1b068d68fb60655043735c6c1aeb07707
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825917"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Para reiniciar o trabalho, use o [bitsadmin retomar](bitsadmin-resume.md) alternar.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir suspende o trabalho denominado *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
