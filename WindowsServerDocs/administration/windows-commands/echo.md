---
title: echo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfe6c936ee5606e286aab076bea08db04b8b6500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811170"
---
# <a name="echo"></a>echo



Exibe mensagens ou ativa ou desativa o recurso de eco de comando. Se usado sem parâmetros, **echo** exibe a configuração atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[em \| off]|Ativa ou desativa o recurso de eco de comando. Comando ecoar é ativado por padrão.|
|\<mensagem >|Especifica o texto a ser exibido na tela.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **echo** *mensagem* comando é particularmente útil quando **eco** está desativado. Para exibir uma mensagem de várias linhas sem exibir qualquer comando, você pode incluir vários **echo** *mensagem* comandos depois do **echo off** no seu arquivo em lotes.
-   Quando **echo** estiver desativada, o prompt de comando não aparece na janela do Prompt de comando. Para exibir o prompt de comando, digite **eco.**
-   Se usado em um arquivo de lote **eco** e **echo off** não afetam a configuração no prompt de comando.
-   Para evitar repetir um determinado comando em um arquivo em lotes, insira um sinal de arroba (@) na frente do comando. Para evitar repetir todos os comandos em um arquivo em lotes, inclua o **echo off** comando no início do arquivo.
-   Para exibir um pipe ( **|** ) ou o caractere de redirecionamento ( **<** ou **>** ) quando você estiver usando **eco**, use um acento circunflexo (^) imediatamente antes do caractere de pipe ou redirecionamento (por exemplo, **^|** , **^>** , ou **^<** ). Para exibir um acento circunflexo, digite acentos dois circunflexos em sucessão ( **^^** ).

## <a name="examples"></a>Exemplos

Para exibir o atual **echo** configurando, digite:

```
echo
```

Para repetir uma linha em branco na tela, digite:

```
echo.
```

> [!NOTE]
> Não inclua um espaço antes do período. Caso contrário, o período será exibido em vez de uma linha em branco.

Para evitar repetir comandos no prompt de comando, digite:

```
echo off 
```

> [!NOTE]
> Quando **echo** estiver desativada, o prompt de comando não aparece na janela do Prompt de comando. Para exibir novamente o prompt de comando, digite **echo em**.

Para impedir que todos os comandos em um arquivo em lote (incluindo o **echo off** comando) sejam exibidas na tela, na primeira linha do tipo de arquivo em lotes:

```
@echo off
```

Você pode usar o **echo** comando como parte de uma **se** instrução. Por exemplo, para pesquisar o diretório atual para qualquer arquivo com a extensão de nome de arquivo. rpt e para ecoar uma mensagem se esse arquivo está localizado, digite:

```
if exist *.rpt echo The report has arrived.
```

O seguinte arquivo em lotes pesquisará o diretório atual para arquivos com a extensão de nome de arquivo. txt e exibe uma mensagem indicando que os resultados da pesquisa:

```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```

Se nenhum arquivo. txt é encontrado quando o arquivo em lotes é executado, a seguinte mensagem será exibida:

```
This directory contains no text files.
```

Se os arquivos. txt são encontrados quando o arquivo em lotes é executado a saída a seguir exibe (neste exemplo, suponha que os arquivos File1.txt, File2.txt e File3.txt existe):

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
