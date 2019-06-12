---
title: findstr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8d080420d250deee9bef701272e936d33733a9d6
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811202"
---
# <a name="findstr"></a>findstr

Procura por padrões de texto em arquivos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/b|Corresponde ao padrão de texto se ele estiver no início de uma linha.|
|/e|Corresponde ao padrão de texto se ele estiver no final de uma linha.|
|/l|Processos de pesquisar cadeias de caracteres literalmente.|
|/r|Processos de pesquisar cadeias de caracteres como expressões regulares. Essa é a configuração padrão.|
|/s|Pesquisa o diretório atual e todos os subdiretórios.|
|/i|Ignora o caso dos caracteres ao pesquisar a cadeia de caracteres.|
|/x|Imprime as linhas que correspondem exatamente.|
|/v|Imprime somente as linhas que não contêm uma correspondência.|
|/n|Imprime o número de linha de cada linha que corresponde ao.|
|/m|Imprime o nome de arquivo se um arquivo contiver uma correspondência.|
|/o|Deslocamento de caractere imprime antes de cada linha correspondente.|
|/p|Ignora os arquivos com caracteres não imprimíveis.|
|/off[line]|Não ignorar arquivos que têm o atributo offline definido.|
|/f:\<File>|Obtém uma lista de arquivos do arquivo especificado.|
|/c:\<String>|Usa o texto especificado como uma cadeia de caracteres de pesquisa literal.|
|/g:\<File>|Obtém pesquisa cadeias de caracteres do arquivo especificado.|
|/d:\<DirList>|Pesquisa a lista de diretórios especificada. Cada diretório deverão ser separado por ponto e vírgula (;), por exemplo `dir1;dir2;dir3`.|
|/a:\<ColorAttribute>|Especifica atributos de cor com dois dígitos hexadecimais. Tipo `color /?` para obter informações adicionais.|
|\<Cadeias de caracteres >|Especifica o texto a ser pesquisado na *FileName*. Obrigatório.|
|[\<Drive>:][<Path>]<FileName>[ ...]|Especifica o local e o arquivo ou arquivos a pesquisar. É necessário o nome de pelo menos um arquivo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Todos os **findstr** opções de linha de comando devem preceder *cadeias de caracteres* e *FileName* na cadeia de caracteres de comando.
- Expressões regulares usam metacaracteres e caracteres literais para localizar padrões de texto, em vez de cadeias de caracteres exatas de caracteres. Um caractere literal é um caractere que não tem um significado especial na sintaxe de expressão regular — ela corresponde a uma ocorrência do caractere. Por exemplo, letras e números são caracteres literais. Um metacaractere é um símbolo com significado especial (um operador ou um delimitador) na sintaxe de expressão regular.

  A tabela a seguir lista os metacaracteres que **findstr** aceita.  

  |Metacaractere|Valor|
  |-------------|-----|
  |.|Curinga: qualquer caractere|
  |*|Repetir: zero ou mais ocorrências do caractere anterior ou classe|
  |^|Posição da linha: a partir da linha|
  |$|Posição da linha: final da linha|
  |[class]|Classe de caractere: qualquer caractere em um conjunto|
  |[^class]|Classe inverso: qualquer caractere não em um conjunto|
  |[x-y]|Intervalo: qualquer caractere dentro do intervalo especificado|
  |\x|Escape: uso de literal de um metacaractere x|
  |\\< cadeia de caracteres|Posição do Word: a partir da palavra|
  |cadeia de caracteres\>|Posição do Word: fim da palavra|

  Os caracteres especiais na sintaxe de expressão regular tem mais poder ao usá-los juntos. Por exemplo, use a seguinte combinação do caractere curinga (.) e repetir o caractere (*) para corresponder a qualquer cadeia de caracteres:

  ```
  .*
  ``` 

  Use a expressão a seguir como parte de uma expressão maior para corresponder a qualquer cadeia de caracteres começando com "b" e terminando com "ing": 

  ```
  b.*ing
  ```

## <a name="examples"></a>Exemplos

Use espaços para separar várias cadeias de caracteres de pesquisa, a menos que o argumento é prefixado com **/c**.

Para procurar por "Olá" ou "there" no arquivo x. y, digite:

```
findstr "hello there" x.y 
```

Para procurar por "Olá" no arquivo x. y, digite:

```
findstr /c:"hello there" x.y 
```

Para localizar todas as ocorrências da palavra "Windows" (com inicial maiuscula W) no arquivo txt, digite:

```
findstr Windows proposal.txt 
```

Para pesquisar todos os arquivos no diretório atual e todas as subpastas que contêm a palavra Windows, sem diferenciar maiusculas de minúsculas, digite:

```
findstr /s /i Windows *.* 
```

Para localizar todas as ocorrências de linhas que começam com "FOR" e são precedidas por zero ou mais espaços (como em um loop de programa de computador) e para exibir o número da linha em que cada ocorrência for encontrada, digite:

```
findstr /b /n /r /c:"^ *FOR" *.bas 
```

Para pesquisar várias cadeias de caracteres em um conjunto de arquivos, crie um arquivo de texto que contém cada critério de pesquisa em uma linha separada. Você também pode listar os arquivos exatos que você deseja pesquisar em um arquivo de texto. Por exemplo, para usar os critérios de pesquisa no arquivo Stringlist.txt, pesquisar os arquivos listados na Filelist. txt e, em seguida, armazenar os resultados no arquivo Results, tipo:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

Para listar todos os arquivos que contêm a palavra "computador" no diretório atual e todos os subdiretórios, independentemente do caso, digite:

```
findstr /s /i /m "\<computer\>" *.*
```

Para listar todos os arquivos que contêm a palavra "computer" e todas as palavras que começam com "composição", (como "complemento" e "competem"), digite:

```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)