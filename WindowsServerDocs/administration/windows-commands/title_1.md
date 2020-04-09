---
title: título
description: Tópico de comandos do Windows para título, que cria um título para a janela de prompt de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae18e97daaef226443c5f18c6c401a4e2b82b59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832799"
---
# <a name="title"></a>título

Cria um título para a janela de prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
title [<String>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Cadeia de caracteres \<>|Especifica o título da janela do prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para criar o título da janela para programas do lote, inclua o comando **title** no início de um programa do lote.
-   Depois que um título de janela é definido, você pode redefini-lo somente usando o comando **title** .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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