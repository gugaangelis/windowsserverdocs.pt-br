---
title: bitsadmin setdisplayname
description: O tópico de comandos do Windows para **Bitsadmin SetDisplayName** – define o nome de exibição do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a5607eb26f8199ec415a4cec17d03015a26bcd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380627"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



Define o nome de exibição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|DisplayName|Texto usado para o nome de exibição do trabalho especificado.|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define o nome para exibição do trabalho chamado *myDownloadJob* como *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)