---
title: findstr
description: Artigo de referência para o comando Findstr, que procura padrões de texto em arquivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0cf30f19ef23c1b3275b6b7632b03f0dd8e433a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931432"
---
# <a name="findstr"></a>findstr

Pesquisa padrões de texto em arquivos.

## <a name="syntax"></a>Sintaxe

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<file>] [/c:<string>] [/g:<file>] [/d:<dirlist>] [/a:<colorattribute>] [/off[line]] <strings> [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /b | Corresponde ao padrão de texto se estiver no início de uma linha. |
| /e | Faz a correspondência do padrão de texto se ele estiver no final de uma linha. |
| /l | Processa cadeias de caracteres de pesquisa literalmente. |
| /r | Processa cadeias de caracteres de pesquisa como expressões regulares. Essa é a configuração padrão. |
| /s | Pesquisa o diretório atual e todos os subdiretórios. |
| /i | Ignora o caso dos caracteres ao pesquisar a cadeia de caracteres. |
| /x | Imprime as linhas que correspondem exatamente. |
| /v | Imprime apenas as linhas que não contêm uma correspondência. |
| /n | Imprime o número de linha de cada linha correspondente. |
| /m | Imprime somente o nome do arquivo se um arquivo contiver uma correspondência. |
| /o | Imprime o deslocamento de caractere antes de cada linha correspondente. |
| /p | Ignora arquivos com caracteres não imprimíveis. |
| /off [linha] | Não ignora arquivos que têm o atributo offline definido. |
| f`<file>` | Obtém uma lista de arquivos do arquivo especificado. |
| /c`<string>` | Usa o texto especificado como uma cadeia de caracteres de pesquisa literal. |
| /g`<file>` | Obtém as cadeias de caracteres de pesquisa do arquivo especificado. |
| /d`<dirlist>` | Pesquisa a lista de diretórios especificada. Cada diretório deve ser separado com um ponto e vírgula (;), por exemplo `dir1;dir2;dir3` . |
| SRDF`<colorattribute>` | Especifica os atributos de cor com dois dígitos hexadecimais. Digite `color /?` para obter informações adicionais. |
| `<strings>` | Especifica o texto a ser pesquisado em *nome de arquivo*. Obrigatórios. |
| `[\<drive>:][<path>]<filename>[ ...]` | Especifica o local e arquivo ou arquivos a serem pesquisados. É necessário pelo menos um nome de arquivo. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Todas as opções de linha de comando **findstr** devem preceder *cadeias* de caracteres e *nome de arquivo* na cadeia de comando.

- As expressões regulares usam caracteres literais e metacaracteres para localizar padrões de texto, em vez de cadeias de caracteres exatas.

  - Um caractere literal é um caractere que não tem um significado especial na sintaxe de expressão regular; em vez disso, ele corresponde a uma ocorrência desse caractere. Por exemplo, letras e números são caracteres literais.

  - Um meta-caractere é um símbolo com significado especial (um operador ou delimitador) na sintaxe de expressão regular.

    Os meta-caracteres aceitos são:

    | Meta-caractere | Valor |
    | -------------- | ----- |
    | `.` | **Curinga** -qualquer caractere |
    | `*` | **REPEAT** -zero ou mais ocorrências da classe ou caractere anterior. |
    | `^` | **Posição da linha inicial** – início da linha. |
    | `$` | **Posição da linha final** -fim da linha. |
    | `[class]` | **Classe de caractere** -qualquer caractere em um conjunto. |
    | `[^class]` | **Classe inversa** -qualquer caractere que não esteja em um conjunto. |
    | `[x-y]` | **Intervalo** -todos os caracteres dentro do intervalo especificado. |
    | `\x` | **Escape** -uso literal de um caractere de meta. |
    | `<string` | **Iniciando posição do Word** -início da palavra. |
    | `string>` | **Posição da palavra final** -fim da palavra. |

    Os caracteres especiais na sintaxe de expressão regular têm o máximo de energia ao usá-los juntos. Por exemplo, use a combinação do caractere curinga ( `.` ) e o caractere REPEAT ( `*` ) para corresponder a qualquer cadeia de caracteres:`.*`

    Use a expressão a seguir como parte de uma expressão maior para corresponder a qualquer cadeia de caracteres que comece com *b* e terminando com *ing*:`b.*ing`

- Para pesquisar várias cadeias de caracteres em um conjunto de arquivos, você deve criar um arquivo de texto que contenha cada critério de pesquisa em uma linha separada.

- Use espaços para separar várias cadeias de caracteres de pesquisa, a menos que o argumento seja prefixado com **/c**.

### <a name="examples"></a>Exemplos

Para procurar *Olá* *ou no* arquivo *x. y*, digite:

```
findstr hello there x.y
```

Para procurar *Olá* no arquivo *x. y*, digite:

```
findstr /c:hello there x.y
```

Para localizar todas as ocorrências da palavra *Windows* (com uma letra inicial maiúscula W) no arquivo *proposal.txt*, digite:

```
findstr Windows proposal.txt
```

Para pesquisar todos os arquivos no diretório atual e todos os subdiretórios que continham a palavra *Windows*, independentemente do caso de letra, digite:

```
findstr /s /i Windows *.*
```

Para localizar todas as ocorrências de linhas que começam com *for* e são precedidas por zero ou mais espaços (como em um loop de programa de computador) e para exibir o número de linha em que cada ocorrência é encontrada, digite:

```
findstr /b /n /r /c:^ *FOR *.bas
```

Para listar os arquivos exatos que você deseja pesquisar em um arquivo de texto, use os critérios de pesquisa no arquivo *stringlist.txt*, para pesquisar os arquivos listados em *filelist.txt*e, em seguida, para armazenar os resultados no arquivo *Results. out*, digite:

```
findstr /g:stringlist.txt /f:filelist.txt > results.out
```

Para listar cada arquivo que contém a palavra *Computer* no diretório atual e todos os subdiretórios, independentemente do caso, digite:

```
findstr /s /i /m <computer> *.*
```

Para listar todos os arquivos que contêm o computador do Word e as outras palavras que começam com comp, (como elogio e competir), digite:

```
findstr /s /i /m <comp.* *.*
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)