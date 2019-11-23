---
title: for
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db0bf54e35e4226cb020b040d5fc36ddd88dc02b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377118"
---
# <a name="for"></a>for



Executa um comando especificado para cada arquivo em um conjunto de arquivos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|{%%\|%}\<variável >|Necessário. Representa um parâmetro substituível. Use um único sinal de porcentagem ( **%** ) para executar o comando **for** no prompt de comando. Use sinais de porcentagem duplas ( **%%** ) para executar o comando **for** em um arquivo em lotes. As variáveis diferenciam maiúsculas de minúsculas e devem ser representadas com um valor alfabético, como **% A**, **% B**ou **% C**.|
|(conjunto de\<>)|Necessário. Especifica um ou mais arquivos, diretórios ou cadeias de caracteres de texto ou um intervalo de valores nos quais o comando deve ser executado. Os parênteses são necessários.|
|> de comando \<|Necessário. Especifica o comando que você deseja executar em cada arquivo, diretório ou cadeia de caracteres de texto ou no intervalo de valores incluídos no *conjunto*.|
|\<CommandLineOptions >|Especifica as opções de linha de comando que você deseja usar com o comando especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Usando **for**

  Você pode usar o comando **for** em um arquivo em lotes ou diretamente do prompt de comando.
- Usando parâmetros de lote

  Os seguintes atributos se aplicam ao comando **for** :  
  - O comando **for** substitui **%** <em>variável</em> ou **%%** <em>variável</em> com cada cadeia de texto no conjunto especificado até que o comando especificado processe todos os arquivos.
  - Os nomes de variáveis diferenciam maiúsculas de minúsculas, globais e não mais do que 52 podem estar ativos por vez.
  - Para evitar confusão com os parâmetros de lote **%0** a **%9**, você pode usar qualquer caractere para a *variável* , exceto os numerais de 0 a 9. Para arquivos em lotes simples, um único caractere como **%% f** funcionará.
  - Você pode usar vários valores para a *variável* em arquivos de lote complexos para distinguir variáveis substituíveis diferentes.
- Especificando um grupo de arquivos

  O parâmetro *set* pode representar um único grupo de arquivos ou vários grupos de arquivos. Você pode usar caracteres curinga ( **&#42;** e **?** ) para especificar um conjunto de arquivos. Estes são os conjuntos de arquivos válidos:  
  ```
  (*.doc) 
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```  
  Quando você usa o comando **for** , o primeiro valor em *conjunto* substitui **%** <em>variável</em> ou **%%** <em>variável</em>e, em seguida, o comando especificado processa esse valor. Isso continuará até que todos os arquivos (ou grupos de arquivos) que correspondam ao valor *definido* sejam processados.
- Usando as palavras-chave **in** e **do**

  **No** e **não** são parâmetros, mas você deve usá-los com o **para**. Se você omitir qualquer uma dessas palavras-chave, uma mensagem de erro será exibida.
- Usando formas adicionais do **para**

  Se as extensões de comando estiverem habilitadas (ou seja, o padrão), as seguintes formas adicionais do **para** têm suporte:  
  - Somente diretórios

    Se *set* contiver caracteres curinga **&#42;** (ou **?** ), o *comando* especificado será executado para cada diretório (em vez de um conjunto de arquivos em um diretório especificado) que corresponda ao *conjunto*.

    A sintaxe é:  
    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
    ```  
  - Recursive

    Percorre a árvore de diretórios com raiz na *unidade*:*Path* e executa a instrução **for** em cada diretório da árvore. Se nenhum diretório for especificado após **/r**, o diretório atual será usado como o diretório raiz. Se *definido* for apenas um único ponto (.), ele apenas enumerará a árvore de diretórios.

    A sintaxe é:  
    ```
    for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```  
  - Iterando um intervalo de valores

    Use uma variável iterativa para definir o valor inicial (*Start*#) e, em seguida, percorra um intervalo definido de valores até que o valor exceda o valor final definido (*end*#). **/l** executará a iteração comparando *Start*# com *end*#. Se *Start*# for menor que *end*#, o comando será executado. Quando a variável iterativa excede o *end*#, o Shell de comando sai do loop. Você também pode usar uma *etapa*negativa # para percorrer um intervalo em valores decrescentes. Por exemplo, (1, 1, 5) gera a sequência 1 2 3 4 5 e (5,-1, 1) gera a sequência 5 4 3 2 1.

    A sintaxe é:  
    ```
    for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
    ```  
  - Iteração e análise de arquivo

    Use a análise de arquivo para processar a saída do comando, cadeias de caracteres e conteúdo do arquivo.  Use variáveis iterativas para definir o conteúdo ou as cadeias de caracteres que você deseja examinar e use as várias opções de *ParsingKeywords* para modificar ainda mais a análise.  Use a opção de token *ParsingKeywords* para especificar quais tokens devem ser passados como variáveis iterativas. Observe que, quando usado sem a opção de token, **/f** examinará apenas o primeiro token.

    A análise de arquivo consiste em ler a saída, a cadeia de caracteres ou o conteúdo do arquivo e, em seguida, dividi-lo em linhas individuais de texto e analisar cada linha em zero ou mais tokens. O loop **for** é chamado com o valor de variável iterativa definido como o token. Por padrão, **/f** passa o primeiro token separado em branco de cada linha de cada arquivo. Linhas em branco são ignoradas.

    As sintaxes são:  
    ```
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
    ```  
    O argumento *set* especifica um ou mais nomes de arquivo. Cada arquivo é aberto, lido e processado antes de passar para o próximo arquivo no *conjunto*. Para substituir o comportamento de análise padrão, especifique *ParsingKeywords*. Esta é uma cadeia de caracteres entre aspas que contém uma ou mais palavras-chave para especificar opções de análise diferentes.

    Se você usar a opção **usebackq** , use uma das seguintes sintaxes:  
    ```
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
    ```  
    A tabela a seguir lista as palavras-chave de análise que você pode usar para *ParsingKeywords*.  

    |      Palavra-chave      |                                                                                                                                                                                                          Descrição                                                                                                                                                                                                          |
    |-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |     EOL =\<c >      |                                                                                                                                                                                   Especifica um caractere de fim de linha (apenas um caractere).                                                                                                                                                                                    |
    |     Skip =\<N >     |                                                                                                                                                                              Especifica o número de linhas a serem ignoradas no início do arquivo.                                                                                                                                                                              |
    |   delims =\<xxx >   |                                                                                                                                                                     Especifica um conjunto de delimitadores. Isso substitui o conjunto de delimitadores padrão de espaço e tabulação.                                                                                                                                                                      |
    | tokens =\<X, Y, M – N > | Especifica quais tokens de cada linha devem ser passados para o loop **for** para cada iteração. Como resultado, são alocados nomes de variáveis adicionais. *M*–*n* especifica um intervalo, do *m*ésimo até os *N*ésimo tokens. Se o último caractere em **tokens =** String for um asterisco ( **&#42;** ), uma variável adicional será alocada e receberá o texto restante na linha após o último token analisado. |
    |     usebackq      |                                                                                             Especifica para: executar uma cadeia de caracteres entre aspas invertidas como um comando, usar uma cadeia de caracteres entre aspas simples como uma cadeia de caracteres literal ou, para nomes de arquivo longos que contenham espaços, permitir nomes de arquivos no *conjunto de\<\>* , a cada um será colocado entre aspas duplas.                                                                                              |


  - Substituição de variável

    A tabela a seguir lista a sintaxe opcional (para qualquer variável **I**).  

    |Variável com modificador|Descrição|
    |----------------------|-----------|
    |% ~ I|Expande **% I** que remove qualquer aspas ao redor ("").|
    |% ~ fI|Expande **% I** para um nome de caminho totalmente qualificado.|
    |% ~ dI|Expande **% I** somente para uma letra da unidade.|
    |% ~ pI|Expande **% I** para apenas um caminho.|
    |% ~ nI|Expande **% I** somente para um nome de arquivo.|
    |% ~ xI|Expande **% I** para uma extensão de nome de arquivo apenas.|
    |% ~ sI|Expande o caminho para conter apenas nomes curtos.|
    |% ~ Ia|Expande **% I** para os atributos de arquivo do arquivo.|
    |% ~ tI|Expande **% I** para a data e a hora do arquivo.|
    |% ~ zI|Expande **% I** para o tamanho do arquivo.|
    |% ~ $PATH: I|Pesquisa os diretórios listados na variável de ambiente PATH e expande **% I** para o nome totalmente qualificado do primeiro diretório encontrado. Se o nome da variável de ambiente não for definido ou o arquivo não for encontrado pela pesquisa, esse modificador se expandirá para a cadeia de caracteres vazia.|

    A tabela a seguir lista as combinações de modificadores que você pode usar para obter resultados compostos.  

    |Variável com modificadores combinados|Descrição|
    |--------------------------------|-----------|
    |% ~ dpI|Expande **% I** para apenas uma letra de unidade e um caminho.|
    |% ~ nxI|Expande **% I** para apenas um nome de arquivo e uma extensão.|
    |% ~ fsI|Expande **% I** para um nome de caminho completo apenas com nomes curtos.|
    |% ~ DP $ caminho: I|Pesquisa os diretórios listados na variável de ambiente PATH para **% I** e expande para a letra da unidade e o caminho do primeiro encontrado.|
    |% ~ ftzaI|Expande **% I** para uma linha de saída que é como **dir**.|

    Nos exemplos acima, você pode substituir **% I** e Path por outros valores válidos. Um válido **para nome de** variável encerra a sintaxe de **%~** .

    Usando nomes de variáveis em letras maiúsculas, como **% I**, você pode tornar seu código mais legível e evitar confusão com os modificadores, que não diferenciam maiúsculas de minúsculas.
- Analisando uma cadeia de caracteres

  Você pode usar a lógica **de análise for/f** em uma cadeia de caracteres imediata encapsulando *\<literastring\>* em: aspas duplas (*sem* "usebackq") ou entre aspas simples (*com* "usebackq")--por exemplo, ("MyString") ou ("MyString"). *\<literalstring\>* é tratado como uma única linha de entrada de um arquivo. Ao analisar *\<literastring\>* em aspas duplas, símbolos de comando (como **\\ \& \|** \> \< \^) são tratados como caracteres comuns.
- Saída de análise

  Você pode usar o comando **for/f** para analisar a saída de um comando colocando um *comando de\<* entre aspas invertidas\>entre os parênteses. Ele é tratado como uma linha de comando, que é passada para um cmd. exe filho. A saída é capturada na memória e analisada como se fosse um arquivo.

## <a name="BKMK_examples"></a>Disso

Para usar o **para** o em um arquivo em lotes, use a seguinte sintaxe:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Para exibir o conteúdo de todos os arquivos no diretório atual que têm a extensão. doc ou. txt usando a variável substituível **% f**, digite:
```
for %f in (*.doc *.txt) do type %f 
```
No exemplo anterior, cada arquivo que tem a extensão. doc ou. txt no diretório atual é substituído pela variável **% f** até que o conteúdo de cada arquivo seja exibido. Para usar esse comando em um arquivo em lotes, substitua todas as ocorrências de **% f** por **%% f**. Caso contrário, a variável será ignorada e uma mensagem de erro será exibida.

Para analisar um arquivo, ignorando linhas comentadas, digite:
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Esse comando analisa cada linha em MyFile. txt. Ele ignora as linhas que começam com um ponto e vírgula e passa o segundo e o terceiro token de cada linha para o corpo **de para** (os tokens são delimitados por vírgulas ou espaços). O corpo da instrução **for** faz referência a **% i** para obter o segundo token, **% j** para obter o terceiro token e **% k** para obter todos os tokens restantes. Se os nomes de arquivo fornecidos contiverem espaços, use aspas em volta do texto (por exemplo, "nome do arquivo"). Para usar aspas, você deve usar **usebackq**. Caso contrário, as aspas são interpretadas como definição de uma cadeia de caracteres literal para análise.

**% i** é declarado explicitamente na instrução **for** . **% j** e **% k** são declarados implicitamente usando **tokens =** . Você pode usar **tokens =** para especificar até 26 tokens, desde que ele não cause uma tentativa de declarar uma variável maior que a letra "z" ou "z".

O exemplo a seguir enumera os nomes de variáveis de ambiente no ambiente atual. Para analisar a saída de um comando colocando *definido* entre parênteses, digite:
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
