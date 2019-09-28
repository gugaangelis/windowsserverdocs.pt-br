---
title: bitsadmin replaceremoteprefix
description: Tópico de comandos do Windows para **Bitsadmin replaceremoteprefix** -todos os arquivos no trabalho cuja URL remota começa com *OldPrefix* são alterados para usar *NewPrefix*.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380802"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Todos os arquivos no trabalho cuja URL remota começa com *OldPrefix* são alterados para usar *NewPrefix*.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|OldPrefix|Prefixo de URL existente|
|NewPrefix|Novo prefixo de URL|

## <a name="examples"></a>Exemplos

O exemplo a seguir altera todos os arquivos no trabalho denominado *myDownloadJob* cuja URL remota começa com *http://stageserver* a *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informações adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)