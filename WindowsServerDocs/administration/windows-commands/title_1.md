---
title: title
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1d1ea70849c3beb4503edfdaa5116384c14a2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848497"
---
# <a name="title"></a>title



Cria um título para a janela de Prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
title [<String>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<cadeia de caracteres >|Especifica o título da janela do Prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para criar o título da janela para programas em lotes, inclua o **título** comando no início de um arquivo em lotes.
-   Depois que um título de janela é definido, você poderá redefini-lo somente usando o **título** comando.

## <a name="BKMK_examples"></a>Exemplos

No script de exemplo a seguir, o título da janela do Prompt de comando é alterado para "Arquivos de atualização" enquanto o arquivo em lotes executa o **cópia** comando. Depois que o comando é executado, o texto `Files Updated` é exibido, e o título da janela do Prompt de comando é alterado para "Prompt de comando".
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)