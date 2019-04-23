---
title: Bitsadmin setcustomheaders
description: Tópico de comandos do Windows para **setcustomheaders bitsadmin** -adicionar um cabeçalho HTTP personalizado a uma solicitação GET.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853847"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders



Adicione um cabeçalho HTTP personalizado a uma solicitação GET.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Oriel1 Oriel2. . .|Os cabeçalhos personalizados para o trabalho|

## <a name="remarks"></a>Comentários

-   Essa opção é usada para adicionar um cabeçalho HTTP personalizado a uma solicitação GET enviada a um servidor HTTP.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir adiciona um cabeçalho HTTP personalizado do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)