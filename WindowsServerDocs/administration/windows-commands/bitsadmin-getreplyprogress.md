---
title: bitsadmin getreplyprogress
description: Tópico de comandos do Windows para **getreplyprogress bitsadmin** -recupera o tamanho e o progresso da resposta do servidor.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aafecfb5873392ef86e6f7cceb139091b15e3b99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852927"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Recupera o tamanho e o progresso da resposta do servidor.

**BITS 1.2 e anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o progresso de resposta do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)