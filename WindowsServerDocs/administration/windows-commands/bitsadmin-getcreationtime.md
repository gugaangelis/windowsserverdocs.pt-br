---
title: bitsadmin getcreationtime
description: O tópico de comandos do Windows para **Bitsadmin GetCreationTime** – recupera a hora de criação para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ea92133c90e20e37e5d281116e91bf1f109e83f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381693"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime



Recupera a hora de criação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetCreationTime <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a hora de criação para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetCreationTime myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)