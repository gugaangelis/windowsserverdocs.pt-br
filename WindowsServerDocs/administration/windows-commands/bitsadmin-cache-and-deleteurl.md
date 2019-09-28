---
title: cache Bitsadmin e DeleteUrl
description: Tópico de comandos do Windows para **cache Bitsadmin e DeleteUrl** -exclui todas as entradas de cache para a URL fornecida.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382061"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin e DeleteUrl



Exclui todas as entradas de cache para a URL fornecida.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|url|O localizador de recursos uniforme que identifica um arquivo remoto.|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir exclui todas as entradas de cache para https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)