---
title: título
description: Artigo de referência para título, que cria um título para a janela de prompt de comando.
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20d0baf3c006fafd3ef6fb45a8cb69724c19929a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036064"
---
# <a name="title"></a>título

Cria um título para a janela de prompt de comando.



## <a name="syntax"></a>Sintaxe

```
title [<String>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<String>|Especifica o título da janela do prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para criar o título da janela para programas do lote, inclua o comando **title** no início de um programa do lote.
-   Depois que um título de janela é definido, você pode redefini-lo somente usando o comando **title** .

## <a name="examples"></a>Exemplos

No script de exemplo a seguir, o título da janela de prompt de comando é alterado para atualizar os arquivos enquanto o arquivo em lotes executa o comando de **cópia** . Depois que o comando é executado, o texto `Files Updated` é exibido e o título da janela do prompt de comando é alterado de volta para o prompt de comando.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)