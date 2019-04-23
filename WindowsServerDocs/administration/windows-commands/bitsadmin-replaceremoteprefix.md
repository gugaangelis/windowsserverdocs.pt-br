---
title: bitsadmin replaceremoteprefix
description: Tópico de comandos do Windows para **bitsadmin replaceremoteprefix** -todos os arquivos no trabalho cujo URL remota começa com *OldPrefix* são alteradas para usar *NewPrefix*.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3eba4f62842fa7f862cd4eaea6830e6a08397a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868127"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix



Todos os arquivos no trabalho cujo URL remota começa com *OldPrefix* são alteradas para usar *NewPrefix*.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|OldPrefix|Prefixo de URL existente|
|NewPrefix|Novo prefixo de URL|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir altera todos os arquivos em um trabalho denominado *myDownloadJob* cuja URL remota começa com *http://stageserver* para *http://prodserver*.
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informações adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)