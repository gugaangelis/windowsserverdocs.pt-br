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
ms.openlocfilehash: 848c57736c3530e296cffb970237149b4634de67
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266520"
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

## <a name="examples"></a>Exemplos

O exemplo a seguir altera todos os arquivos em um trabalho denominado *myDownloadJob* cuja URL remota começa com *http://stageserver* para *http://prodserver* .
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informações adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)