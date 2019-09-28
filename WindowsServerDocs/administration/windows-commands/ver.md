---
title: ver
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e48b3b1061edf793c88693b3353753c6a4cedcfc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362716"
---
# <a name="ver"></a>ver



Exibe o número de versão do sistema operacional.

Esse comando tem suporte no prompt de comando do Windows (cmd. exe), mas não no PowerShell.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ver
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Disso

Para obter o número de versão do sistema operacional do Shell de comando (cmd. exe), digite:

```
ver
```

O comando ver não funciona no PowerShell. Para obter a versão do sistema operacional do PowerShell, digite:

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
