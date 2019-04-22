---
title: lista
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aacc93e1c7a16a7327ddbd17515f19cf41a5b458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825537"
---
# <a name="list"></a>lista



Lista os gravadores, cópias de sombra ou provedores de cópia de sombra registrados atualmente estão no sistema. Se usado sem parâmetros, **lista** exibe a Ajuda no prompt de comando.

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
|gravadores|Lista de gravadores. Ver [List writers](list-writers.md) para sintaxe e parâmetros.|
|Sombras|Lista as cópias de sombra não persistente existente e persistente. Ver [lista sombras](list-shadows.md) para sintaxe e parâmetros.|
|provedores|Listas registrado no momento, os provedores de cópia de sombra. Ver [Listar provedores](list-providers.md) para sintaxe e parâmetros.|

## <a name="BKMK_examples"></a>Exemplos

Para listar todas as cópias de sombra, digite:
```
list shadows all
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)