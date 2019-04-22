---
title: doskey
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61629a6122ec83cae35a8797966ef3d712ee5808
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825857"
---
# <a name="doskey"></a>doskey



Chama Doskey.exe (quais recalls inserido anteriormente comandos de linha de comando), edita linhas de comando e cria macros.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/reinstall|Instala uma nova cópia do Doskey.exe e limpa o buffer de histórico de comando.|
|/listsize=\<Size>|Especifica o número máximo de comandos no buffer de histórico.|
|/macros|Exibe uma lista de todos os **doskey** macros. Você pode usar o símbolo de redirecionamento (**>**) com **/macros** para redirecionar a lista para um arquivo. Você pode abreviar **/macros** à **/m**.|
|/macros:all|Exibe **doskey** macros para todos os executáveis.|
|/macros:\<ExeName>|Exibe **doskey** macros para o executável especificado pelo *executável*.|
|/History|Exibe todos os comandos que são armazenados na memória. Você pode usar o símbolo de redirecionamento (**>**) com **/history** para redirecionar a lista para um arquivo. Você pode abreviar **/history** como **/h**.|
|[/insert | /overstrike]|Especifica se deve inserir ou substituir o texto conforme você digita. Se você usar **/inserção**, novo texto digitado em uma linha é inserido no meio do texto existente. Se você usar **//overstrike**, o novo texto substitui o texto existente. A configuração padrão é **//overstrike**.|
|/exename=\<ExeName>|Especifica o programa (ou seja, executáveis) em que o **doskey** macro é executada.|
|/macrofile=\<FileName>|Especifica um arquivo que contém as macros que você deseja instalar.|
|\<MacroName>=[<Text>]|Cria uma macro que executa os comandos especificados por *texto*. *MacroName* Especifica o nome que você deseja atribuir à macro. *Texto* Especifica os comandos que você deseja registrar. Se *texto* é deixado em branco, *MacroName* será apagado de todos os comandos atribuídos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando Doskey.exe

    Doskey.exe está sempre disponível para programas interativos todos baseados em caracteres (como depuradores de programas ou programas de transferência de arquivo), e ele mantém um buffer de histórico de comandos e macros para cada programa que ele é iniciado. Não é possível usar **doskey** opções de linha de comando de um programa. Você deve executar **doskey** opções de linha de comando antes de iniciar um programa. As atribuições de teclas do programa **doskey** atribuições da chave.
-   Repetir um comando

    Para repetir um comando, você pode usar qualquer uma das seguintes chaves de depois de iniciar Doskey.exe. Se você usar Doskey.exe dentro de um programa, as atribuições de teclas desse programa têm precedência.  
    |Chave|Descrição|
    |---|-----------|
    |SETA PARA CIMA|Repete o comando que você usou antes do que é exibido.|
    |SETA PARA BAIXO|Repete o comando que você usou após o que é exibido.|
    |PAGE UP|Repete o primeiro comando que você usou na sessão atual.|
    |PAGE DOWN|Repete o comando mais recente que você usou na sessão atual.|
-   Edição de linha de comando

    Com Doskey.exe, você pode editar a linha de comando atual. Se você usar Doskey.exe dentro de um programa, as atribuições de teclas desse programa têm precedência e alguns Doskey.exe teclas de edição podem não funcionar.

    A seguinte tabela lista **doskey** editar chaves e suas funções.  
    |Tecla ou combinação de teclas|Descrição|
    |----------------------|-----------|
    |SETA PARA A ESQUERDA|Move o caractere de um ponto de inserção.|
    |SETA PARA A DIREITA|Move o ponto de inserção um caractere avanço.|
    |CTRL + SETA PARA ESQUERDA|Move o ponto de inserção novamente uma palavra.|
    |CTRL + SETA PARA DIREITA|Move o ponto de inserção uma palavra avanço.|
    |PÁGINA INICIAL|Move o ponto de inserção para o início da linha.|
    |FINAL|Move o ponto de inserção para o final da linha.|
    |ESC|Limpa a exibição do comando.|
    |F1|Copia um caractere de uma coluna do modelo para a mesma coluna na janela do Prompt de comando. (O modelo é um buffer de memória que contém o último comando digitado.)|
    |F2|Procura o modelo para a próxima chave que você digita depois que você pressione F2. Doskey.exe insere o texto do modelo — até, mas sem incluir, o caractere que você especificar.|
    |F3|Copia o restante do modelo para a linha de comando. Doskey.exe começa a copiar caracteres da posição no modelo que corresponde à posição indicada pelo ponto de inserção na linha de comando.|
    |F4|Exclui que todos os caracteres de inserção atual apontam posição até, mas sem incluir, a próxima ocorrência do caractere digitado depois que você pressione F4.|
    |F5|Copia o modelo na linha de comando atual.|
    |F6|Insere um caractere de fim-de-arquivo (CTRL + Z) na posição atual do ponto de inserção.|
    |F7|Exibe todos os comandos para este programa que são armazenados na memória (em uma caixa de diálogo). Use a tecla de seta para cima e a tecla de seta para baixo para selecionar o comando desejado e pressione ENTER para executar o comando. Você pode também Observe o número sequencial na frente do comando e usar esse número em conjunto com a tecla F9.|
    |ALT + F7|Exclui todos os comandos armazenados na memória para o buffer de histórico atual.|
    |F8|Exibe todos os comandos no buffer de histórico que iniciam com os caracteres do comando atual.|
    |F9|Solicita um número de comando do buffer de histórico e, em seguida, exibe o comando associado ao número especificado por você. Pressione ENTER para executar o comando. Para exibir todos os números e os comandos associados, pressione F7.|
    |ALT + F10|Exclui todas as definições de macro.|
-   Usando o **doskey** dentro de um programa

    Determinados programas interativos, baseados em caracteres, como depuradores do programa ou programas de transferência de arquivo (FTP) usam Doskey.exe automaticamente. Para usar Doskey.exe, um programa deve ser um processo de console e usar em buffer de entrada. As atribuições de teclas do programa **doskey** atribuições da chave. Por exemplo, se o programa usa a tecla F7 para uma função, não é possível obter um **doskey** histórico em uma janela pop-up de comandos.

    Com Doskey.exe, você pode manter um histórico de comandos para cada programa que você iniciar ou repita. Você pode editar comandos anteriores no prompt do programa e iniciar **doskey** macros criadas para o programa. Se você sair e, em seguida, reiniciar um programa a partir da mesma janela de Prompt de comando, o histórico de comandos da sessão anterior do programa está disponível.

    Você deve executar Doskey.exe antes de iniciar um programa. Não é possível usar **doskey** solicitam que as opções de linha de comando do comando de um programa, mesmo se o programa tem um comando do shell.

    Se você quiser personalizar como Doskey.exe funciona com um programa e criar **doskey** macros para que o programa, você pode criar um arquivo em lotes que modifica Doskey.exe e inicia o programa.
-   Especificando um modo de inserção padrão

    Se você pressionar a tecla INSERT, você pode digitar o texto sobre o **doskey** linha de comando no meio do texto existente sem substituir o texto. No entanto, depois de pressionar ENTER, Doskey.exe retornará o teclado para o modo de substituição. Você deve pressionar INSERT novamente para retornar ao modo de inserção.

    Use **/inserção** para mudar o teclado para o modo de inserção cada vez que você pressione ENTER. O teclado permanecerá no modo de inserção até que você use **//overstrike**. Você pode retornar ao modo substituir temporariamente pressionando a tecla INSERT, mas depois de pressionar ENTER, Doskey.exe retornará o teclado para o modo de inserção.

    A inserção ponto muda de forma quando você usa a tecla INSERT para alternar de um modo para outro.
-   Criando uma macro

    Você pode usar Doskey.exe para criar macros que executar um ou mais comandos. A tabela a seguir lista os caracteres especiais que você pode usar para controlar as operações de comando quando você define uma macro.  
    |Caractere|Descrição|
    |---------|-----------|
    |$G ou $g|Redireciona a saída. Usar qualquer um desses caracteres especiais para enviar a saída para um dispositivo ou um arquivo em vez de na tela. Esse caractere é equivalente ao símbolo de redirecionamento para a saída (**>**).|
    |$ $G G ou $g$ g|Anexa a saída ao final de um arquivo. Use um desses caracteres de duplos para acrescentar a saída para um arquivo existente em vez de substituir os dados no arquivo. Esses caracteres duplos são equivalentes ao símbolo de redirecionamento de acréscimo para saída (**>>**).|
    |$L ou $l|Redirecionamentos de entrada. Use qualquer um desses caracteres especiais para ler dados de entrada de um dispositivo ou um arquivo em vez de no teclado. Esse caractere é equivalente ao símbolo de redirecionamento para a entrada (**<**).|
    |$B ou $b|Envia a saída de macro para um comando. Esses caracteres especiais são equivalentes ao uso do pipe (* *|*) em uma linha de comando.|
    |$T ou $t|Separa os comandos. Usar qualquer um desses caracteres especiais para separar comandos quando você cria macros ou digitar comandos na **doskey** linha de comando. Esses caracteres especiais são equivalentes a usar o e comercial (**&**) em uma linha de comando.|
    |$$|Especifica o caractere de sinal de cifrão (**$**).|
    |US $1 a $9|Representam todas as informações de linha de comando para especificar quando executar a macro. Os caracteres especiais **US $1** por meio **$9** são parâmetros de lote que permitem que você usar dados diferentes na linha de comando sempre que você executar a macro. O **US $1** caractere em uma **doskey** comando é semelhante de **%1** caractere em um arquivo em lotes.|
    |$*|Representa todas as informações de linha de comando que você deseja especificar quando você digita o nome da macro. O caractere especial **$ \*** é um parâmetro substituível semelhante aos parâmetros em lotes **$1** por meio de **$9**, com uma importante diferença: tudo o que você digitar na linha de comando após o nome da macro é substituído para o **$ \*** na macro.|
-   Executando uma **doskey** macro

    Para executar uma macro, digite o nome da macro, no prompt de comando, começando na posição do primeiro. Se a macro foi definida com **$ \*** ou qualquer um dos parâmetros do lote **$1** por meio de **$9**, use um espaço para separar os parâmetros. Não é possível executar uma **doskey** macro a partir de um arquivo em lotes.
-   Criando uma macro com o mesmo nome que um comando da família Windows Server 2003

    Se você usar sempre um determinado comando com opções específicas de linha de comando, você pode criar uma macro que tem o mesmo nome que o comando. Para especificar se deseja executar a macro ou o comando, siga estas diretrizes:  
    -   Para executar a macro, digite o nome da macro, no prompt de comando. Não adicione um espaço antes do nome da macro.
    -   Para executar o comando, insira um ou mais espaços no prompt de comando e, em seguida, digite o nome do comando.
-   Exclusão de uma macro

    Para excluir uma macro, digite:  
    ```
    doskey <MacroName> =
    ```

## <a name="BKMK_examples"></a>Exemplos

O **/macros** e **/history** opções de linha de comando são úteis para criar programas em lotes que salvam macros e comandos. Por exemplo, para armazenar todos os atuais **doskey** macros, digite:
```
doskey /macros > macinit 
```
Para usar as macros armazenadas em Macinit, digite:
```
doskey /macrofile=macinit 
```
Para criar um lote denominado tmp contém recentemente usado comandos, digite:
```
doskey /history> tmp.bat 
```
Para definir uma macro com vários comandos, use **$t** para separar comandos, da seguinte maneira:
```
doskey tx=cd temp$tdir/w $*
```
No exemplo anterior, a macro TX altera o diretório atual para Temp e, em seguida, exibe uma listagem no formato de exibição ampla de diretório. Você pode usar **$ \*** ao final da macro para acrescentar outras opções de linha de comando para **dir** quando você executa o TX.

A macro a seguir usa um parâmetro de lote para um novo nome de diretório:
```
doskey mc=md $1$tcd $1
```
A macro cria um novo diretório e, em seguida, altera para o novo diretório do diretório atual.

Para usar a macro anterior para criar e alterar para um diretório chamado manuais, digite:
```
mc books
```
Para criar uma **doskey** macro para um programa chamado Ftp.exe, inclua **/exename** da seguinte maneira:
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Para usar a macro anterior, inicie o FTP. No prompt de FTP, digite:
```
go
```
Execuções FTP a **abra**, **mget**, e **bye** comandos.

Para criar uma macro que rapidamente e incondicionalmente formata um disco, digite:
```
doskey qf=format $1 /q /u
```
Para rapidamente e incondicionalmente formatar um disco na unidade A, digite:
```
qf a:
```
Para excluir uma macro chamada vlist, digite:
```
doskey vlist =
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)