---
title: forfiles
description: Artigo de referência para o comando Forfiles, que seleciona e executa um comando em um arquivo ou conjunto de arquivos.
ms.topic: reference
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/20/2020
ms.openlocfilehash: b5b2511e49c379be20c7be5abf08581a17f0a463
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634795"
---
# <a name="forfiles"></a>forfiles

Seleciona e executa um comando em um arquivo ou conjunto de arquivos. Esse comando é usado com mais frequência em arquivos em lotes.

## <a name="syntax"></a>Sintaxe

```
forfiles [/P pathname] [/M searchmask] [/S] [/C command] [/D [+ | -] [{<date> | <days>}]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /P `<pathname>` | Especifica o caminho do qual iniciar a pesquisa. Por padrão, a pesquisa começa no diretório de trabalho atual. |
| Opção `<searchmask>` | Pesquisa arquivos de acordo com a máscara de pesquisa especificada. O searchmask padrão é `*` . |
| /S | Instrui o comando **Forfiles** para pesquisar em subdiretórios de forma recursiva. |
| /C `<command>` | Executa o comando especificado em cada arquivo. As cadeias de caracteres de comando devem ser encapsuladas entre aspas duplas. O comando padrão é `"cmd /c echo @file"` . |
| /D `[{+\|-}][{<date> | <days>}]` | Seleciona os arquivos com uma data da última modificação dentro do período de tempo especificado:<ul><li>Seleciona arquivos com uma data da última modificação posterior ou igual a ( **+** ) ou anterior ou igual a ( **-** ) a data especificada, em que a *Data* está no formato mm/dd/aaaa.</li><li>Seleciona arquivos com uma data da última modificação posterior ou igual a ( **+** ) a data atual mais o número de dias especificado ou anterior ou igual a ( **-** ) a data atual menos o número de dias especificado.</li><li>Os valores válidos para *dias* incluem qualquer número no intervalo de 0 a 32768. Se nenhum sinal for especificado, **+** será usado por padrão.</li></ul> |
| /? | Exibe o texto de ajuda na janela cmd. |

#### <a name="remarks"></a>Comentários

- O `forfiles /S` comando é semelhante a `dir /S` .

- Você pode usar as seguintes variáveis na cadeia de caracteres de comando, conforme especificado pela opção de linha de comando **/c** :

    | Variável | Descrição |
    | -------- | ----------- |
    | @FILE | Nome do arquivo. |
    | @FNAME | Nome do arquivo sem extensão. |
    | @EXT | Extensão de nome de arquivo. |
    | @PATH | Caminho completo do arquivo. |
    | @RELPATH | Caminho relativo do arquivo. |
    | @ISDIR | Avaliará como TRUE se um tipo de arquivo for um diretório. Caso contrário, essa variável será avaliada como FALSE. |
    | @FSIZE | Tamanho do arquivo, em bytes. |
    | @FDATE | Carimbo de data da última modificação no arquivo. |
    | @FTIME | Carimbo de data/hora da última modificação no arquivo. |

- O comando **Forfiles** permite executar um comando ou passar argumentos para vários arquivos. Por exemplo, você pode executar o comando **Type** em todos os arquivos em uma árvore com a extensão de nome de arquivo. txt. Ou você pode executar cada arquivo em lotes (*. bat) na unidade C, com o nome do arquivo Myinput.txt como o primeiro argumento.

- Este comando pode:

    - Selecione arquivos por uma data absoluta ou uma data relativa usando o parâmetro **/d** .

    - Crie uma árvore de arquivos de arquivo usando variáveis como @FSIZE e @FDATE .

    - Diferenciar arquivos de diretórios usando a @ISDIR variável.

    - Inclua caracteres especiais na linha de comando usando o código hexadecimal para o caractere, no formato 0x*hh* (por exemplo, 0x09 para uma tabulação).

- Esse comando funciona implementando o `recurse subdirectories` sinalizador em ferramentas que foram projetadas para processar apenas um único arquivo.

### <a name="examples"></a>Exemplos

Para listar todos os arquivos em lotes na unidade C, digite:

```
forfiles /P c:\ /S /M *.bat /C "cmd /c echo @file is a batch file"
```

Para listar todos os diretórios na unidade C, digite:

```
forfiles /P c:\ /S /M *.* /C "cmd /c if @isdir==TRUE echo @file is a directory"
```

Para listar todos os arquivos no diretório atual que têm pelo menos um ano de idade, digite:

```
forfiles /S /M *.* /D -365 /C "cmd /c echo @file is at least one year old."
```

Para exibir o *arquivo* de texto está desatualizado para cada um dos arquivos no diretório atual que são anteriores a 1º de janeiro de 2007, digite:

```
forfiles /S /M *.* /D -01/01/2007 /C "cmd /c echo @file is outdated."
```

Para listar as extensões de nome de arquivo de todos os arquivos no diretório atual no formato de coluna e adicionar uma guia antes da extensão, digite:

```
forfiles /S /M *.* /C "cmd /c echo The extension of @file is 0x09@ext"
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
