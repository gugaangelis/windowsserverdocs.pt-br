---
title: lista
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91b42925fc822b10157bb488167d06fe82cfe1e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374703"
---
# <a name="list"></a>lista



lista gravadores, cópias de sombra ou provedores de cópia de sombra atualmente registrados que estão no sistema. Se usado sem parâmetros, **list** exibirá a ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|usadas|Lista gravadores. Consulte [listar gravadores](list-writers.md) para sintaxe e parâmetros.|
|Sombrea|Lista cópias de sombra persistentes e existentes não persistentes. Consulte [listar sombras](list-shadows.md) para sintaxe e parâmetros.|
|Fornecedor|Lista os provedores de cópia de sombra atualmente registrados. Consulte [listar provedores](list-providers.md) para sintaxe e parâmetros.|

## <a name="BKMK_examples"></a>Disso

Para listar todas as cópias de sombra, digite:
```
list shadows all
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)