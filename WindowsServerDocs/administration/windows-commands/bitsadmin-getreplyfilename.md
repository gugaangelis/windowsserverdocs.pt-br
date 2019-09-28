---
title: bitsadmin getreplyfilename
description: Tópico de comandos do Windows para **Bitsadmin getreplyfilename** -Obtém o caminho do arquivo que contém a resposta do servidor.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 96b77e9bd19cdc094e6b025e143b05aff7bc60d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381269"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtém o caminho do arquivo que contém a resposta do servidor.

**BITS 1,2 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o nome de arquivo de resposta para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)