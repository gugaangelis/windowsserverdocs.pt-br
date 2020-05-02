---
title: forfiles
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 21cbc24028af5c4194d36258aecdd5432fb4069f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725571"
---
# <a name="forfiles"></a>forfiles



Seleciona e executa um comando em um arquivo ou conjunto de arquivos. Esse comando é útil para processamento em lotes.



## <a name="syntax"></a>Sintaxe

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c <Command>] [/d [{+|-}][{<Date>|<Days>}]]
```


### <a name="parameters"></a>Parâmetros

|                     Parâmetro                      |                                                                                                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<caminho>                     |                                                                                                                                                                                                                                                 Especifica o caminho do qual iniciar a pesquisa. Por padrão, a pesquisa começa no diretório de trabalho atual.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask>                  |                                                                                                                                                                                                                                                           Pesquisa arquivos de acordo com a máscara de pesquisa especificada. A máscara de pesquisa padrão ** \*é\\ .** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Instrui o comando **Forfiles** a Pesquisar de forma recursiva os subdiretórios.                                                                                                                                                                                                                                                                    |
|                  /c \<> de comando                   |                                                                                                                                                                                                                                  Executa o comando especificado em cada arquivo. As cadeias de caracteres de comando devem ser colocadas entre aspas. O comando padrão é **cmd/c Echo @file **.                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}] &#8288; [{\<Date>\|&#8288;\<dias>}] | Seleciona os arquivos com uma data da última modificação dentro do período de tempo especificado.</br>-Seleciona arquivos com uma data da última modificação posterior ou igual a (**+**) ou anterior ou igual a (**-**) a data especificada, em que a *Data* está no formato mm/dd/aaaa.</br>-Seleciona os arquivos com uma data da última modificação posterior ou igual a**+**() a data atual mais o número de dias especificado ou anterior ou igual a (**-**) a data atual menos o número de dias especificado.</br>-Os valores válidos para *dias* incluem qualquer número no intervalo de 0 a 32768. Se nenhum sinal for especificado, **+** será usado por padrão. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Comentários

-   **Forfiles** é mais comumente usado em arquivos em lotes.
-   **Forfiles/s** é semelhante a **dir/s.**
-   Você pode usar as seguintes variáveis na cadeia de caracteres de comando, conforme especificado pela opção de linha de comando **/c** .  

|Variável|Descrição|
|--------|-----------|
|@FILE|Nome do arquivo.|
|@FNAME|Nome do arquivo sem extensão.|
|@EXT|Extensão de nome de arquivo.|
|@PATH|Caminho completo do arquivo.|
|@RELPATH|Caminho relativo do arquivo.|
|@ISDIR|Avaliará como TRUE se um tipo de arquivo for um diretório. Caso contrário, essa variável será avaliada como FALSE.|
|@FSIZE|Tamanho do arquivo, em bytes.|
|@FDATE|Carimbo de data da última modificação no arquivo.|
|@FTIME|Carimbo de data/hora da última modificação no arquivo.|

-   Com **Forfiles**, você pode executar um comando ou passar argumentos para vários arquivos. Por exemplo, você pode executar o comando **Type** em todos os arquivos em uma árvore com a extensão de nome de arquivo. txt. Ou você pode executar cada arquivo em lotes (*. bat) na unidade C, com o nome de arquivo MyInput. txt como o primeiro argumento.
-   Com **Forfiles**, você pode fazer o seguinte:  
    -   Selecione arquivos por uma data absoluta ou uma data relativa usando o parâmetro **/d** .
    -   Crie uma árvore de arquivos de arquivo usando variáveis como @FSIZE e. @FDATE
    -   Diferenciar arquivos de diretórios usando a @ISDIR variável.
    -   Inclua caracteres especiais na linha de comando usando o código hexadecimal para o caractere, no formato 0x*hh* (por exemplo, 0x09 para uma tabulação).
-   **Forfiles** funciona implementando o sinalizador **recurse de subdiretórios** em ferramentas que são projetadas para processar apenas um único arquivo.

## <a name="examples"></a>Exemplos

Para listar todos os arquivos em lotes na unidade C, digite:
```
forfiles /p c:\ /s /m *.bat /c cmd /c echo @file is a batch file
```
Para listar todos os diretórios na unidade C, digite:
```
forfiles /p c:\ /s /m *.* /c cmd /c if @isdir==TRUE echo @file is a directory
```
Para listar todos os arquivos no diretório atual que têm pelo menos um ano de idade, digite:
```
forfiles /s /m *.* /d -365 /c cmd /c echo @file is at least one year old.
```
Para exibir o *arquivo* de texto está desatualizado para cada um dos arquivos no diretório atual que são anteriores a 1º de janeiro de 2007, digite:
```
forfiles /s /m *.* /d -01/01/2007 /c cmd /c echo @file is outdated. 
```
Para listar as extensões de nome de arquivo de todos os arquivos no diretório atual no formato de coluna e adicionar uma guia antes da extensão, digite:
```
forfiles /s /m *.* /c cmd /c echo The extension of @file is 0x09@ext 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
