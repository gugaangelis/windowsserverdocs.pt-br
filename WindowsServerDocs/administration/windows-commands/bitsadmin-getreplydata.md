---
title: bitsadmin getreplydata
description: Tópico de comandos do Windows para **Bitsadmin getreplydata** – recupera os dados de resposta do servidor em formato hexadecimal.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd3ee77e5d442467f49bb209c560f089f2271b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381282"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera os dados de resposta do servidor em formato hexadecimal.

**BITS 1,2 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera os dados de resposta para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)