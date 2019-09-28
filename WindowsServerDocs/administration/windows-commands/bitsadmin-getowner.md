---
title: bitsadmin getowner
description: O tópico de comandos do Windows para **Bitsadmin GetOwner** -recupera o proprietário do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab8151ba8b1379c101aa037504ae2021ff0df62f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381450"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Exibe o nome de exibição ou o GUID do proprietário do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir exibe o proprietário do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)