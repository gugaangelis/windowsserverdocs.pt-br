---
title: Bitsadmin getcustomheaders
description: Tópico de comandos do Windows para **getcustomheaders bitsadmin** -recupera os cabeçalhos HTTP personalizados do trabalho.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2f5959541f0e3190e26bbb298a9cd7c63ab32cae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812087"
---
# <a name="bitsadmin-getcustomheaders"></a>Bitsadmin getcustomheaders



Recupera os cabeçalhos HTTP personalizados do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir obtém os cabeçalhos personalizados do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)