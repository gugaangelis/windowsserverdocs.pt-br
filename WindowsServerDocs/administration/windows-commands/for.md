---
title: for
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 13a44bc3497b44d60bd4d351e389d493a50f1269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869457"
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
|{%%\|%}\<Variable>|Obrigatório. Representa um parâmetro de substituição. Use um único sinal de porcentagem (**%**) para executar o **para** no prompt de comando. Use dois sinais de porcentagem (**%%**) para executar o **para** comando dentro de um arquivo em lotes. Variáveis diferenciam maiusculas de minúsculas, e eles devem ser representados com um valor alfabético, como **%A**, **%B**, ou **%C**.|
|(\<Set>)|Obrigatório. Especifica um ou mais arquivos, diretórios, ou cadeias de caracteres de texto ou um intervalo de valores no qual executar o comando. Os parênteses são necessários.|
|\<Comando >|Obrigatório. Especifica o comando que você deseja realizar fora em cada arquivo, diretório ou cadeia de caracteres de texto ou no intervalo de valores incluídos na *definir*.|
|\<CommandLineOptions>|Especifica as opções de linha de comando que você deseja usar com o comando especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando **para**

    Você pode usar o **para** comando dentro de um arquivo em lotes ou diretamente do prompt de comando.
-   Usando parâmetros em lotes

    Os atributos a seguir se aplicam para o **para** comando:  
    -   O **para** comando substitui **% * * * variável* ou **% % * * * variável* com cada cadeia de caracteres no conjunto especificado até que o comando especificado processa todos os arquivos de texto.
    -   Nomes de variáveis são diferencia maiusculas de minúsculas, global e não mais de 52 pode estar ativa por vez.
    -   Para evitar confusão com os parâmetros do lote **%0** por meio **%9**, você pode usar qualquer caractere para o *variável* exceto dos números de 0 a 9. Para simples arquivos em lote, um único caractere, como **% %** funcionará.
    -   Você pode usar vários valores para *variável* em arquivos em lote complexos para distinguir diferentes variáveis substituíveis.
-   Especificando um grupo de arquivos

    O *definir* parâmetro pode representar um único grupo de arquivos ou vários grupos de arquivos. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar um arquivo de conjunto. São os seguintes conjuntos de arquivo válido:  
    ```
    (*.doc) 
    (*.doc *.txt *.me)
    (jan*.doc jan*.rpt feb*.doc feb*.rpt)
    (ar??1991.* ap??1991.*)
    ```  
    Quando você usa o **para** de comando, o primeiro valor na *definir* substitui **% * * * variável* ou **% % * * * variável*e, em seguida, especificado comando processa esse valor. Isso continua até que todos os arquivos (ou grupos de arquivos) que correspondem do *definir* valor são processados.
-   Usando o **na** e **fazer** palavras-chave

    **Na** e **fazer** não são parâmetros, mas você deve usá-los com **para**. Se você omitir qualquer uma dessas palavras-chave, uma mensagem de erro é exibida.
-   Usar formas adicionais de **para**

    Se as extensões de comando estiverem habilitadas (que é o padrão), as seguintes formas adicionais de **para** têm suporte:  
    -   Somente os diretórios

        Se *definir* contém caracteres curinga (**&#42;** ou **?**), especificado *comando* é executado para cada diretório (em vez de um conjunto arquivos em um diretório especificado) que corresponde ao *definir*.

        A sintaxe é:  
        ```
        for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
        ```  
    -   Recursive

        Percorre a árvore de diretório que está enraizada no *Drive*:*caminho* e executa o **para** instrução em cada diretório da árvore. Se nenhum diretório for especificado, após **/r**, o diretório atual é usado como o diretório raiz. Se *definir* é apenas um único ponto (.), ele apenas enumerará a árvore de diretório.

        A sintaxe é:  
        ```
        for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        ```  
    -   Iteração de um intervalo de valores

        Use uma variável iterativa para definir o valor inicial (*inicie*#) e, em seguida, percorra um intervalo definido de valores até que o valor excede o valor final definido (*final*#). **/l** executará a iteratividade comparando *começar*# com *End*#. Se *inicie*# é menor que *final*# o comando será executado. Quando a variável iterativa excede *final*#, o shell de comando sai do loop. Você também pode usar um negativo *etapa*# para percorrer um intervalo em valores decrescentes. Por exemplo, (1,1,5) gera a sequência 1 2 3 4 5 e (5,-1,1) gera a sequência 5 4 3 2 1.

        A sintaxe é:  
        ```
        for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
        ```  
    -   Iteração e análise do arquivo

        Use análise de arquivos para a saída do comando de processo, cadeias de caracteres e o conteúdo do arquivo.  Use variáveis iterativas para definir o conteúdo ou cadeias de caracteres que você deseja examinar e usar os vários *Palavras-chaveDeAnálise* opções para modificar ainda mais a análise.  Use o *Palavras-chaveDeAnálise* opção para especificar quais símbolos devem ser passados como variáveis iterativas de token. Observe que, quando usada sem a opção de token **/f** examinará apenas o primeiro token.

        Análise do arquivo consiste em ler a saída, a cadeia de caracteres ou o conteúdo do arquivo e, em seguida, dividindo-a em linhas individuais de texto e analisar cada linha em zero ou mais símbolos. O **para** loop é então chamado com o valor da variável iterativo definido para o símbolo. Por padrão, **/f** passa o primeiro valor em branco separados por token de cada linha de cada arquivo. Linhas em branco são ignoradas.

        As sintaxes são:  
        ```
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
        for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
        ```  
        O *definir* argumento especifica um ou mais nomes de arquivo. Cada arquivo é aberto, leia e processado antes de passar para o próximo arquivo na *definir*. Para substituir o comportamento de análise padrão, especifique *Palavras-chaveDeAnálise*. Isso é uma cadeia de caracteres entre aspas que contém um ou mais palavras-chave para especificar diferentes opções de análise.

        Se você usar o **usebackq** opção, use uma das seguintes sintaxes:  
        ```
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
        for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
        ```  
        A tabela a seguir lista as palavras-chave análise que você pode usar para *Palavras-chaveDeAnálise*.  
        |Palavra-chave|Descrição|
        |-------|-----------|
        |eol=\<c>|Especifica um final do caractere de linha (apenas um caractere).|
        |skip=\<N>|Especifica o número de linhas a serem ignoradas no início do arquivo.|
        |delims=\<xxx>|Especifica um conjunto de delimitadores. Isso substitui o conjunto padrão de delimitadores de espaço e tabulação.|
        |tokens=\<X,Y,M–N>|Especifica quais símbolos de cada linha são a serem passados para o **para** loop para cada iteração. Como resultado, os nomes de variáveis adicionais são alocados. *M*–*N* Especifica um intervalo da *M*ésimo até a *N*ésimo símbolo. Se o último caractere a **tokens =** cadeia de caracteres for um asterisco (**&#42;**), uma variável adicional é alocada e ele receberá o texto restante na linha após o último símbolo é analisado.|
        |usebackq|Especifica a: executar uma cadeia de caracteres entre aspas back como um comando, use uma cadeia de caracteres de aspas como uma cadeia de caracteres literal ou, para nomes de arquivo longos que contêm espaços, permitir que os nomes de arquivo no  *\<defina\>*, para cada ser colocado entre marcas de aspas duplas.|
    -   Substituição de variável

        A tabela a seguir lista a sintaxe opcional (para qualquer variável **eu**).  
        |Variável com o modificador|Descrição|
        |----------------------|-----------|
        |%~I|Expande **%I** que remove quaisquer aspas ao redor ("").|
        |%~fI|Expande **%I** para um nome de caminho totalmente qualificado.|
        |%~dI|Expande **%I** para uma letra de unidade.|
        |%~pI|Expande **%I** para somente um caminho.|
        |%~nI|Expande **%I** para um nome de arquivo.|
        |%~xI|Expande **%I** para apenas uma extensão de nome de arquivo.|
        |%~sI|Expande o caminho para conter somente nomes curtos.|
        |%~aI|Expande **%I** para os atributos de arquivo do arquivo.|
        |%~tI|Expande **%I** como a data e hora do arquivo.|
        |%~zI|Expande **%I** para o tamanho do arquivo.|
        |%~$PATH:I|Pesquisa as pastas listadas na variável de ambiente PATH e expande **%I** para o nome totalmente qualificado do diretório primeiro encontrado. Se o nome da variável de ambiente não está definido ou o arquivo não for encontrado pela pesquisa, esse modificador expande a cadeia de caracteres vazia.|

        A tabela a seguir lista as combinações de modificador que você pode usar para obter resultados compostos.  
        |Variável com modificadores combinados|Descrição|
        |--------------------------------|-----------|
        |%~dpI|Expande **%I** para uma letra de unidade e caminho apenas.|
        |%~nxI|Expande **%I** para um nome de arquivo e extensão somente.|
        |%~fsI|Expande **%I** para um nome de caminho completo com somente nomes curtos.|
        |%~dp$PATH:I|Pesquisa os diretórios que são listados na variável de ambiente PATH **%I** e expande para a letra da unidade e o caminho da primeira encontrada.|
        |%~ftzaI|Expande **%I** para uma linha de saída é semelhante **dir**.|

        Nos exemplos acima, você pode substituir **%I** e o caminho com outros valores válidos. Válida **para** nome da variável encerra o **%~** sintaxe.

        Usando nomes de variável em letras maiusculas, como **%I**, você pode tornar seu código mais legível e evitar confusão com modificadores, que não diferenciam maiusculas de minúsculas.
-   Analisar uma cadeia de caracteres

    Você pode usar o **para /f** analisando a lógica em uma cadeia de caracteres de imediata, encapsulando *\<LiteralString\>* em qualquer um: aspas duplas (*sem* " usebackq") ou aspas simples (*com* "usebackq") – por exemplo, ("MyString") ou ('MyString'). *\<LiteralString\>*  é tratado como uma única linha de entrada de um arquivo. Ao analisar *\<LiteralString\>* entre aspas duplas, símbolos de comando (como **\\ \& \| \> \< \^**) são tratados como caracteres comuns.
-   Analisar a saída

    Você pode usar o **para /f** comando para analisar a saída de um comando colocando um back-entre aspas *\<comando\>* entre os parênteses. Ele é tratado como uma linha de comando, que é passada para um filho Cmd.exe. A saída é capturada na memória e analisada como se fosse um arquivo.

## <a name="BKMK_examples"></a>Exemplos

Para usar **para** em um arquivo em lotes, use a seguinte sintaxe:
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Para exibir o conteúdo de todos os arquivos no diretório atual que têm a extensão. doc ou. txt usando a variável substituível **%f**, tipo:
```
for %f in (*.doc *.txt) do type %f 
```
No exemplo anterior, cada arquivo que tem a extensão. doc ou. txt no diretório atual é substituído para o **%f** variável até que o conteúdo de todos os arquivos é exibido. Para usar esse comando em um arquivo em lotes, substitua cada ocorrência de **%f** com **% %**. Caso contrário, a variável é ignorada e uma mensagem de erro é exibida.

Para analisar um arquivo, ignorando linhas comentadas, digite:
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Esse comando analisa cada linha de myfile. txt. Ele ignora as linhas que começam com um ponto e vírgula e passa o token de segundo e terceiro de cada linha para o **para** corpo (tokens são delimitados por vírgulas ou espaços). O corpo dos **para** referências de instrução **%i** para obter um token, o segundo **%j** para obter o token de terceiro, e **%k** obter todo o restante tokens. Se os nomes de arquivo que você fornecer contiverem espaços, use aspas ao redor do texto (por exemplo, "nome do arquivo"). Para usar aspas, você deve usar **usebackq**. Caso contrário, as aspas são interpretadas como definir uma cadeia de caracteres literal para analisar.

**%i** for declarado explicitamente na **para** instrução. **%j** e **%k** implicitamente são declaradas usando **tokens =**. Você pode usar **tokens =** para especificar até 26 símbolos, desde que não faz com que uma tentativa para declarar uma variável maior que a letra "z" ou "Z".

O exemplo a seguir enumera os nomes de variáveis de ambiente no ambiente atual. Para analisar a saída de um comando colocando *definir* entre parênteses, digite:
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
