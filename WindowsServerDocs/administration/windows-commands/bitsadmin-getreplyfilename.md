---
title: bitsadmin getreplyfilename
description: Tópico de comandos do Windows para **getreplyfilename bitsadmin** -obtém o caminho do arquivo que contém a resposta do servidor.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46130700e9ac7e2d0076b368712e5dcb3f02ba2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862147"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtém o caminho do arquivo que contém a resposta do servidor.

**BITS 1.2 e anteriores**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o nome do arquivo de resposta do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)