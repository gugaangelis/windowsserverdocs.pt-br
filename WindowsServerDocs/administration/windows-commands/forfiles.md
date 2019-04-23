---
title: forfiles
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 127f715620321354792d46f024ee12a06925d866
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881307"
---
# <a name="forfiles"></a>forfiles



Seleciona e executa um comando em um arquivo ou conjunto de arquivos. Esse comando é útil para processamento em lotes.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/p \<Path>|Especifica o caminho do qual iniciar a pesquisa. Por padrão, a pesquisa começa no diretório de trabalho atual.|
|/m \<SearchMask>|Pesquisa arquivos de acordo com a máscara de pesquisa especificado. A máscara de pesquisa padrão é **\*.\***.|
|/s|Instrui o **forfiles** comando para pesquisar em subdiretórios, recursivamente.|
|/c "\<Command>"|Executa o comando especificado em cada arquivo. Cadeias de caracteres de comando devem ser colocadas entre aspas. O comando padrão é **"cmd /c echo @file"**.|
|/d&nbsp;[{+\|-}]&#8288;[{\<Date>\|&#8288;\<Days>}]|Seleciona os arquivos com uma data da última modificação no período de tempo especificado.</br>– Seleciona arquivos com uma data da última modificação posterior ou igual a (**+**) ou anterior ou igual a (**-**) da data especificada, onde *data* está no formato MM/DD/AAAA.</br>– Seleciona arquivos com uma data da última modificação posterior ou igual a (**+**) a data atual mais o número de dias especificado, ou anterior ou igual a (**-**) a data atual menos o número de dias especificado.</br>-Os valores válidos para *dias* incluir qualquer número no intervalo 0 – 32, 768. Se nenhuma entrada for especificada, **+** é usado por padrão.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   **Forfiles** é mais comumente usado em arquivos em lotes.
-   **/S forfiles** é semelhante ao **pesquisado.**
-   Você pode usar as seguintes variáveis na cadeia de caracteres de comando conforme especificado pelo **/c** opção de linha de comando.  

|Variável|Descrição|
|--------|-----------|
|@FILE|Nome do arquivo.|
|@FNAME|Nome do arquivo sem extensão.|
|@EXT|Extensão de nome de arquivo.|
|@PATH|Caminho completo do arquivo.|
|@RELPATH|Caminho relativo do arquivo.|
|@ISDIR|Avalia como TRUE se um tipo de arquivo é um diretório. Caso contrário, essa variável é avaliada como FALSE.|
|@FSIZE|Tamanho do arquivo, em bytes.|
|@FDATE|Último carimbo de data de modificação no arquivo.|
|@FTIME|Último carimbo de hora da modificação no arquivo.|

-   Com o **forfiles**, você pode executar um comando ou passar argumentos para vários arquivos. Por exemplo, você pode executar o **tipo** comando em todos os arquivos em uma árvore com a extensão de nome de arquivo. txt. Ou você pode executar todos os arquivos em lote (*. bat) na unidade C, com o arquivo de nome "MinhaEntrada" como o primeiro argumento.
-   Com o **forfiles**, você pode fazer qualquer um dos seguintes:  
    -   Selecione os arquivos por uma data absoluta ou uma data relativa, usando o **/d** parâmetro.
    -   Criar uma árvore de arquivamento de arquivos usando variáveis, como @FSIZE e @FDATE.
    -   Diferenciar arquivos de diretórios usando o @ISDIR variável.
    -   Incluir caracteres especiais na linha de comando usando o código hexadecimal do caractere, na 0x*HH* formato (por exemplo, 0x09 por uma guia).
-   **Forfiles** funciona com a implementação de **opera recursivamente nos subdiretórios** sinalizador ferramentas projetadas para processar apenas um único arquivo.

## <a name="BKMK_examples"></a>Exemplos

Para listar todos os arquivos em lotes na unidade C, digite:
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
Para listar todos os diretórios na unidade C, digite:
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
Para listar todos os arquivos no diretório atual que são pelo menos um ano antigos, digite:
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
Para exibir o texto "*arquivo* está desatualizado" para cada um dos arquivos no diretório atual que têm mais de 1 de janeiro de 2007, digite:
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
Para listar as extensões de nome de arquivo de todos os arquivos no diretório atual em formato de coluna e, em seguida, adicionar uma guia antes da extensão, digite:
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
