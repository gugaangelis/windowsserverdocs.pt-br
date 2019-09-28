---
title: bitsadmin getcustomheaders
description: O tópico de comandos do Windows para **Bitsadmin getcustomheaders** -recupera os cabeçalhos HTTP personalizados do trabalho.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 039669fca42803ff22eb4e3d13dfdef5f0a06f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381663"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders



Recupera os cabeçalhos HTTP personalizados do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir obtém os cabeçalhos personalizados para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)