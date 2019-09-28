---
title: bitsadmin suspend
description: O tópico de comandos do Windows para **suspender Bitsadmin** -suspende o trabalho especificado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7a3a484df2b50cdc8893512020b835f913793d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380376"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Para reiniciar o trabalho, use a opção [Bitsadmin resume](bitsadmin-resume.md) .

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir suspende o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
