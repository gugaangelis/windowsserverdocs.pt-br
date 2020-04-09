---
title: '{1&gt;list&lt;1}'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d60c42b868a1e9a26e3168e4b489573f9f87e179
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841099"
---
# <a name="list"></a>{1&gt;list&lt;1}



Lista gravadores, cópias de sombra ou provedores de cópia de sombra atualmente registrados que estão no sistema. Se usado sem parâmetros, **list** exibirá a ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|usadas|Lista gravadores. Consulte [listar gravadores](list-writers.md) para sintaxe e parâmetros.|
|sombrea|Lista cópias de sombra persistentes e existentes não persistentes. Consulte [listar sombras](list-shadows.md) para sintaxe e parâmetros.|
|provedores|Lista os provedores de cópia de sombra atualmente registrados. Consulte [listar provedores](list-providers.md) para sintaxe e parâmetros.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para listar todas as cópias de sombra, digite:
```
list shadows all
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)