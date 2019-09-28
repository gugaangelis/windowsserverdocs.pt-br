---
title: bitsadmin setcustomheaders
description: Tópico de comandos do Windows para **Bitsadmin setcustomheaders** – adicione um cabeçalho HTTP personalizado a uma solicitação get.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380645"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders



Adicione um cabeçalho HTTP personalizado a uma solicitação GET.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Header1 Header2 . . .|Os cabeçalhos personalizados para o trabalho|

## <a name="remarks"></a>Comentários

-   Essa opção é usada para adicionar um cabeçalho HTTP personalizado a uma solicitação GET enviada a um servidor HTTP.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir adiciona um cabeçalho HTTP personalizado para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)