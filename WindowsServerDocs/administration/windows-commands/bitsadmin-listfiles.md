---
title: bitsadmin listfiles
description: Tópico de comandos do Windows para **listfiles bitsadmin** -lista os arquivos de trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f0f86a7e176c601c51dbdf403baf51f70e53dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852967"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles



Lista os arquivos de trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /ListFiles <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a lista de arquivos do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)