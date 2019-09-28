---
title: title
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385788"
---
# <a name="title"></a>title



Cria um título para a janela de prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
title [<String>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<String >|Especifica o título da janela do prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para criar o título da janela para programas do lote, inclua o comando **title** no início de um programa do lote.
-   Depois que um título de janela é definido, você pode redefini-lo somente usando o comando **title** .

## <a name="BKMK_examples"></a>Disso

No script de exemplo a seguir, o título da janela de prompt de comando é alterado para "Atualizando arquivos" enquanto o arquivo em lotes executa o comando de **cópia** . Depois que o comando é executado, o texto `Files Updated` é exibido e o título da janela do prompt de comando é alterado de volta para "prompt de comando".
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)