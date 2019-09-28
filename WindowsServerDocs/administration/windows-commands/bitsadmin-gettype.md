---
title: bitsadmin gettype
description: Tópico de comandos do Windows para **Bitsadmin GetType** – recupera o tipo de trabalho do trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381342"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Recupera o tipo de trabalho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

O tipo pode ser DOWNLOAD, UPLOAD, UPLOAD-responder ou desconhecido.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o tipo de trabalho para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)