---
title: findstr
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 547a0abf658ef826cca8c87d451144181f8dac7d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377193"
---
# <a name="findstr"></a>findstr

Pesquisa padrões de texto em arquivos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/b.|Corresponde ao padrão de texto se estiver no início de uma linha.|
|/e|Faz a correspondência do padrão de texto se ele estiver no final de uma linha.|
|/l|Processa cadeias de caracteres de pesquisa literalmente.|
|/r|Processa cadeias de caracteres de pesquisa como expressões regulares. Essa é a configuração padrão.|
|/s|Pesquisa o diretório atual e todos os subdiretórios.|
|/i|Ignora o caso dos caracteres ao pesquisar a cadeia de caracteres.|
|/x|Imprime as linhas que correspondem exatamente.|
|/v|Imprime apenas as linhas que não contêm uma correspondência.|
|opção|Imprime o número de linha de cada linha correspondente.|
|/m|Imprime somente o nome do arquivo se um arquivo contiver uma correspondência.|
|/o|Imprime o deslocamento de caractere antes de cada linha correspondente.|
|/p|Ignora arquivos com caracteres não imprimíveis.|
|/off [linha]|Não ignora arquivos que têm o atributo offline definido.|
|/f: arquivo de\<>|Obtém uma lista de arquivos do arquivo especificado.|
|/c: cadeia de caracteres de\<>|Usa o texto especificado como uma cadeia de caracteres de pesquisa literal.|
|/g: arquivo de\<>|Obtém as cadeias de caracteres de pesquisa do arquivo especificado.|
|/d:\<DirList >|Pesquisa a lista de diretórios especificada. Cada diretório deve ser separado com um ponto e vírgula (;), por exemplo `dir1;dir2;dir3`.|
|/a:\<Colorattribute >|Especifica os atributos de cor com dois dígitos hexadecimais. Digite `color /?` para obter informações adicionais.|
|Cadeias de caracteres \<>|Especifica o texto a ser pesquisado em *nome de arquivo*. Necessário.|
|[\<drive >:] [<Path>]<FileName>[...]|Especifica o local e arquivo ou arquivos a serem pesquisados. É necessário pelo menos um nome de arquivo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Todas as opções de linha de comando **findstr** devem preceder *cadeias* de caracteres e *nome de arquivo* na cadeia de comando.
- As expressões regulares usam caracteres literais e metacaracteres para localizar padrões de texto, em vez de cadeias de caracteres exatas. Um caractere literal é um caractere que não tem um significado especial na sintaxe de expressão regular — ele corresponde a uma ocorrência desse caractere. Por exemplo, letras e números são caracteres literais. Um metacaractere é um símbolo com significado especial (um operador ou delimitador) na sintaxe de expressão regular.

  A tabela a seguir lista os metacaracteres que o **findstr** aceita.  

  |Metacaractere|Valor|
  |-------------|-----|
  |.|Curinga: qualquer caractere|
  |*|Repetir: zero ou mais ocorrências da classe ou caractere anterior|
  |^|Posição da linha: início da linha|
  |$|Posição da linha: fim da linha|
  |classes|Classe de caractere: qualquer caractere em um conjunto|
  |[^ classe]|Classe inversa: qualquer caractere que não esteja em um conjunto|
  |[x-y]|Intervalo: quaisquer caracteres dentro do intervalo especificado|
  |\X|Escape: uso literal de um metacaractere x|
  |Cadeia de < \\|Posição da palavra: início da palavra|
  |Cadeia de caracteres\>|Posição da palavra: fim da palavra|

  Os caracteres especiais na sintaxe de expressão regular têm o máximo de energia ao usá-los juntos. Por exemplo, use a seguinte combinação do caractere curinga (.) e REPEAT (*) caractere para corresponder a qualquer cadeia de caracteres:

  ```
  .*
  ``` 

  Use a expressão a seguir como parte de uma expressão maior para corresponder a qualquer cadeia de caracteres que comece com "b" e terminando com "ing": 

  ```
  b.*ing
  ```

## <a name="examples"></a>Exemplos

Use espaços para separar várias cadeias de caracteres de pesquisa, a menos que o argumento seja prefixado com **/c**.

Para procurar "Olá" ou "lá" no arquivo x. y, digite:

```
findstr "hello there" x.y 
```

Para procurar "Olá," no arquivo x. y, digite:

```
findstr /c:"hello there" x.y 
```

Para localizar todas as ocorrências da palavra "Windows" (com uma letra inicial maiúscula W) no arquivo proposta. txt, digite:

```
findstr Windows proposal.txt 
```

Para pesquisar todos os arquivos no diretório atual e todos os subdiretórios que continham a palavra Windows, independentemente do caso de letra, digite:

```
findstr /s /i Windows *.* 
```

Para localizar todas as ocorrências de linhas que começam com "FOR" e são precedidas por zero ou mais espaços (como em um loop de programa de computador) e para exibir o número de linha em que cada ocorrência é encontrada, digite:

```
findstr /b /n /r /c:"^ *FOR" *.bas 
```

Para pesquisar várias cadeias de caracteres em um conjunto de arquivos, crie um arquivo de texto que contenha cada critério de pesquisa em uma linha separada. Você também pode listar os arquivos exatos que deseja pesquisar em um arquivo de texto. Por exemplo, para usar os critérios de pesquisa no arquivo StringList. txt, pesquise os arquivos listados em FileList. txt e, em seguida, armazene os resultados no arquivo Results. out, digite:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

Para listar todos os arquivos que contêm a palavra "computador" no diretório atual e todos os subdiretórios, independentemente do caso, digite:

```
findstr /s /i /m "\<computer\>" *.*
```

Para listar todos os arquivos que contêm a palavra "computador" e quaisquer outras palavras que comecem com "comp", (como "elogio" e "competir"), digite:

```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)