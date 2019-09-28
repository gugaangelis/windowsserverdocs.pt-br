---
title: bitsadmin getdescription
description: O tópico de comandos do Windows para **Bitsadmin GetDescription** – recupera a descrição do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02ab91ad9b6d1d6d1ef67465bb5c982fbddc1bb4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381652"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



Recupera a descrição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a descrição para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)