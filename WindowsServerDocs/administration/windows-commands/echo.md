---
title: echo
description: Tópico de referência para o comando echo, que exibe mensagens ou ativa ou desativa o recurso de eco de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c87a6cb5e3fb52af9a13a7be35218e35b24d7ddc
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436921"
---
# <a name="echo"></a>echo

Exibe mensagens ou ativa ou desativa o recurso de eco de comando. Se usado sem parâmetros, **Echo** exibe a configuração de eco atual.

## <a name="syntax"></a>Sintaxe

```
echo [<message>]
echo [on | off]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| no | desconto | Ativa ou desativa o recurso de eco de comando. O eco de comando está ativado por padrão. |
| `<message>` | Especifica o texto a ser exibido na tela. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O `echo <message>` comando é particularmente útil quando o **eco** é desativado. Para exibir uma mensagem com várias linhas, sem exibir nenhum comando, você pode incluir vários `echo <message>` comandos após o comando **echo off** em seu programa em lotes.

- Depois que o **eco** é desativado, o prompt de comando não aparece na janela do prompt de comando. Para exibir o prompt de comando, digite **Echo on.**

- Se usado em um arquivo em lotes, **Echo on** e **echo off** não afetam a configuração no prompt de comando.

- Para evitar o eco de um comando específico em um arquivo em lotes, insira uma `@` entrada na frente do comando. Para evitar o eco de todos os comandos em um arquivo em lotes, inclua o comando **echo off** no início do arquivo.

- Para exibir um pipe ( `|` ) ou um caractere de redirecionamento ( `<` ou `>` ) quando você estiver usando **Echo**, use um cursor ( `^` ) imediatamente antes do pipe ou caractere de redirecionamento. Por exemplo, `^|` , `^>` ou `^<` ). Para exibir um cursor, digite dois Cursors em sucessão ( `^^` ).

### <a name="examples"></a>Exemplos

Para exibir a configuração de **eco** atual, digite:

```
echo
```

Para ecoar uma linha em branco na tela, digite:

```
echo.
```

> [!NOTE]
> Não inclua um espaço antes do período. Caso contrário, o período aparecerá em vez de uma linha em branco.

Para evitar comandos de eco no prompt de comando, digite:

```
echo off
```

> [!NOTE]
> Quando o **eco** é desativado, o prompt de comando não aparece na janela do prompt de comando. Para exibir o prompt de comando novamente, digite **Echo on**.

Para impedir que todos os comandos em um arquivo em lotes (incluindo o comando **echo off** ) sejam exibidos na tela, na primeira linha do tipo de arquivo em lotes:

```
@echo off
```

Você pode usar o comando **Echo** como parte de uma instrução **If** . Por exemplo, para pesquisar o diretório atual em busca de qualquer arquivo com a extensão de nome de arquivo. RPT e ecoar uma mensagem se tal arquivo for encontrado, digite:

```
if exist *.rpt echo The report has arrived.
```

O arquivo em lotes a seguir pesquisa o diretório atual em busca de arquivos com a extensão de nome de arquivo. txt e exibe uma mensagem indicando os resultados da pesquisa:

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

Se não forem encontrados arquivos. txt quando o arquivo em lotes for executado, a seguinte mensagem será exibida:

```
This directory contains no text files.
```

Se arquivos. txt forem encontrados quando o arquivo em lotes for executado, a saída a seguir será exibida (para este exemplo, suponha que os arquivos arquivo1. txt, arquivo2. txt e arquivo3. txt existam):

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
