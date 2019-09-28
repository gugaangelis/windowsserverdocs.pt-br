---
title: doskey
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d45a2ddfeba7ec136add07eac11c3a8522ef872b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377692"
---
# <a name="doskey"></a>doskey



Chama o Doskey. exe (que recupera comandos de linha de comando inseridos anteriormente), edita linhas de comando e cria macros.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>Parâmetros

|       Parâmetro        |                                                                                                                          Descrição                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /REINSTALL       |                                                                                            Instala uma nova cópia do Doskey. exe e limpa o buffer de histórico de comandos.                                                                                            |
|   /ListSize = \<Size >    |                                                                                                Especifica o número máximo de comandos no buffer de histórico.                                                                                                 |
|        /macros         |                                        Exibe uma lista de todas as macros do **doskey** . Você pode usar o símbolo de redirecionamento ( **>** ) com **/macros** para redirecionar a lista para um arquivo. Você pode abreviar **/macros** para **/m**.                                         |
|      /macros: todos       |                                                                                                        Exibe as macros do **doskey** para todos os executáveis.                                                                                                         |
|   /macros: \<ExeName >   |                                                                                             Exibe as macros do **doskey** para o executável especificado por *EXEName*.                                                                                              |
|        /History        |                                    Exibe todos os comandos que estão armazenados na memória. Você pode usar o símbolo de redirecionamento ( **>** ) com **/history** para redirecionar a lista para um arquivo. Você pode abreviar **/history** como **/h**.                                    |
| /Insert | Especifica que o novo texto digitado é inserido no texto antigo. |
| /overstrike | Especifica que o novo texto substitui o texto antigo. |
|  /EXEName = \<ExeName >   |                                                                                        Especifica o programa (ou seja, executável) no qual a macro do **doskey** é executada.                                                                                         |
| /MACROFILE = \<FileName > |                                                                                              Especifica um arquivo que contém as macros que você deseja instalar.                                                                                               |
| \<MacroName > = [\<Text >]  | Cria uma macro que executa os comandos especificados pelo *texto*. *Macroname* especifica o nome que você deseja atribuir à macro. *Texto* especifica os comandos que você deseja registrar. Se o *texto* for deixado em branco, *macroname* será desmarcada de todos os comandos atribuídos. |
|           /?           |                                                                                                              Exibe a ajuda no prompt de comando.                                                                                                              |

## <a name="remarks"></a>Comentários

- Usando o Doskey. exe

  O Doskey. exe está sempre disponível para todos os programas interativos, baseados em caracteres (como os depuradores de programas ou programas de transferência de arquivos) e mantém um buffer de histórico de comandos e macros para cada programa que ele inicia. Você não pode usar as opções de linha de comando do **doskey** em um programa. Você deve executar as opções de linha de comando do **doskey** antes de iniciar um programa. As atribuições de chave do programa substituem atribuições de chave do **doskey**
- Rechamando um comando

  Para lembrar um comando, você pode usar qualquer uma das seguintes chaves depois de iniciar o Doskey. exe. Se você usar o Doskey. exe em um programa, as atribuições de chave do programa têm precedência.  

  |    Chave     |                              Descrição                              |
  |------------|-----------------------------------------------------------------------|
  |  SETA PARA CIMA  |  Recupera o comando que você usou antes do que é exibido.  |
  | SETA PARA BAIXO |  Recupera o comando que você usou após aquele que é exibido.   |
  |  PÁGINA ACIMA   |    Recupera o primeiro comando que você usou na sessão atual.    |
  | PAGE DOWN  | Recupera o comando mais recente que você usou na sessão atual. |


- Editando a linha de comando

  Com o Doskey. exe, você pode editar a linha de comando atual. Se você usar o Doskey. exe em um programa, as atribuições de chave do programa têm precedência e algumas chaves de edição do Doskey. exe podem não funcionar.

  A tabela a seguir lista as chaves de edição do **doskey** e suas funções.  

  | Combinação de tecla ou chave |                                                                                                                                                       Descrição                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       SETA PARA A ESQUERDA       |                                                                                                                                      Move o ponto de inserção de volta um caractere.                                                                                                                                      |
  |      SETA PARA A DIREITA       |                                                                                                                                    Move o ponto de inserção para frente um caractere.                                                                                                                                     |
  |    CTRL + SETA PARA A ESQUERDA     |                                                                                                                                        Move o ponto de inserção de volta uma palavra.                                                                                                                                         |
  |    CTRL + SETA PARA A DIREITA    |                                                                                                                                       Move o ponto de inserção para frente uma palavra.                                                                                                                                       |
  |          PÁGINA INICIAL          |                                                                                                                                 Move o ponto de inserção para o início da linha.                                                                                                                                 |
  |          COMPLETO           |                                                                                                                                    Move o ponto de inserção para o fim da linha.                                                                                                                                    |
  |          ESC           |                                                                                                                                          Limpa o comando da exibição.                                                                                                                                           |
  |           F1           |                                                                      Copia um caractere de uma coluna no modelo para a mesma coluna na janela de prompt de comando. (O modelo é um buffer de memória que contém o último comando digitado.)                                                                       |
  |           F2           |                                                                 Pesquisa adiante no modelo para a próxima chave que você digitar depois de pressionar F2. O Doskey. exe insere o texto do modelo — até, mas não incluindo, o caractere que você especificar.                                                                  |
  |           F3           |                                                 Copia o restante do modelo para a linha de comando. O Doskey. exe começa a copiar os caracteres da posição no modelo que corresponde à posição indicada pelo ponto de inserção na linha de comando.                                                 |
  |           F4           |                                                                            Exclui todos os caracteres da posição do ponto de inserção atual até, mas não incluindo, a próxima ocorrência do caractere que você digitar depois de pressionar F4.                                                                            |
  |           F5           |                                                                                                                                   Copia o modelo para a linha de comando atual.                                                                                                                                    |
  |           F6           |                                                                                                                    Coloca um caractere de fim de arquivo (CTRL + Z) na posição atual do ponto de inserção.                                                                                                                    |
  |           F7           | Exibe (em uma caixa de diálogo) todos os comandos para esse programa que são armazenados na memória. Use a tecla de seta para cima e a tecla de seta para baixo para selecionar o comando desejado e pressione ENTER para executar o comando. Você também pode observar o número sequencial na frente do comando e usar esse número em conjunto com a tecla F9. |
  |         ALT + F7         |                                                                                                                          Exclui todos os comandos armazenados na memória para o buffer de histórico atual.                                                                                                                          |
  |           F8           |                                                                                                           Exibe todos os comandos no buffer de histórico que começam com os caracteres no comando atual.                                                                                                            |
  |           F9           |                                             Solicita um número de comando de buffer de histórico e, em seguida, exibe o comando associado ao número que você especificar. Pressione ENTER para executar o comando. Para exibir todos os números e seus comandos associados, pressione F7.                                             |
  |        ALT + F10         |                                                                                                                                             Exclui todas as definições de macro.                                                                                                                                              |


- Usando o **doskey** dentro de um programa

  Determinados programas interativos baseados em caracteres, como depuradores de programa ou FTP (programas de transferência de arquivos), usam automaticamente o Doskey. exe. Para usar o Doskey. exe, um programa deve ser um processo de console e usar uma entrada armazenada em buffer. As atribuições de chave do programa substituem atribuições de chave do **doskey** Por exemplo, se o programa usar a tecla F7 para uma função, você não poderá obter um histórico de comandos do **doskey** em uma janela pop-up.

  Com o Doskey. exe, você pode manter um histórico de comandos para cada programa que você inicia ou repete. Você pode editar os comandos anteriores no prompt do programa e iniciar as macros do **doskey** criadas para o programa. Se você sair e reiniciar um programa da mesma janela de prompt de comando, o histórico de comandos da sessão de programa anterior estará disponível.

  Você deve executar o Doskey. exe antes de iniciar um programa. Você não pode usar as opções de linha de comando do **doskey** no prompt de comando de um programa, mesmo que o programa tenha um comando do Shell.

  Se desejar personalizar como o Doskey. exe funciona com um programa e criar macros do **doskey** para esse programa, você poderá criar um programa em lotes que modifique o Doskey. exe e inicie o programa.
- Especificando um modo de inserção padrão

  Se você pressionar a tecla INSERT, poderá digitar o texto na linha de comando do **doskey** no meio do texto existente sem substituir o texto. No entanto, depois de pressionar ENTER, o Doskey. exe retorna o teclado para o modo de substituição. Você deve pressionar INSERT novamente para retornar ao modo de inserção.

  Use **/Insert** para alternar o teclado para o modo de inserção cada vez que você pressionar Enter. O teclado permanecerá efetivamente no modo de inserção até que você use o **/overstrike**. Você pode retornar temporariamente para o modo de substituição pressionando a tecla INSERT, mas depois de pressionar ENTER, o Doskey. exe retornará o teclado para o modo de inserção.

  O ponto de inserção muda de forma quando você usa a tecla INSERT para mudar de um modo para outro.
- Criando uma macro

  Você pode usar o Doskey. exe para criar macros que executam um ou mais comandos. A tabela a seguir lista os caracteres especiais que você pode usar para controlar as operações de comando ao definir uma macro.  

  |   espaço   |                                                                                                                                                                               Descrição                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G ou $g    |                                                                                   Redireciona a saída. Use qualquer um desses caracteres especiais para enviar a saída para um dispositivo ou um arquivo em vez de para a tela. Esse caractere é equivalente ao símbolo de redirecionamento para saída ( **>** ).                                                                                    |
  | $G $ G ou $g $ g  |                                                         Anexa a saída ao final de um arquivo. Use qualquer um desses caracteres duplos para acrescentar a saída a um arquivo existente em vez de substituir os dados no arquivo. Esses caracteres duplos são equivalentes ao símbolo de redirecionamento de acréscimo para saída ( **>>** ).                                                         |
  |   $L ou $l    |                                                                                  Redireciona a entrada. Use qualquer um desses caracteres especiais para ler a entrada de um dispositivo ou de um arquivo, em vez do teclado. Esse caractere é equivalente ao símbolo de redirecionamento para entrada ( **<** ).                                                                                  |
  |   $B ou $b    |                                                                                                                                    Envia a saída da macro para um comando. Esses caracteres especiais são equivalentes ao uso do pipe (\* @ no__t-1                                                                                                                                     |
  |   $T ou $t    |                                                            Separa os comandos. Use qualquer um desses caracteres especiais para separar comandos ao criar macros ou digitar comandos na linha de comando do **doskey** . Esses caracteres especiais são equivalentes a usar o e comercial ( **&** ) em uma linha de comando.                                                            |
  |      $$       |                                                                                                                                                              Especifica o caractere de cifrão ( **$** ).                                                                                                                                                               |
  | $1 a $9 |             Represente as informações de linha de comando que você deseja especificar ao executar a macro. Os caracteres especiais **$1** a **$9** são parâmetros de lote que permitem que você use dados diferentes na linha de comando sempre que executar a macro. O caractere **$1** em um comando do **doskey** é semelhante ao caractere **% 1** em um programa em lotes.             |
  |      $\*      | Representa todas as informações de linha de comando que você deseja especificar ao digitar o nome da macro. O caractere especial **$ @ no__t-2**\* é um parâmetro substituível que é semelhante aos parâmetros de lote de **$1** a **$9**, com uma diferença importante: tudo que você digitar na linha de comando depois que o nome da macro for substituído pelo **$ @ no__t-8**\* na macro. |


- Executando uma macro do **doskey**

  Para executar uma macro, digite o nome da macro no prompt de comando, começando na primeira posição. Se a macro tiver sido definida com **$ @ no__t-2*** ou qualquer um dos parâmetros de lote **$1** a **$9**, use um espaço para separar os parâmetros. Não é possível executar uma macro do **doskey** a partir de um programa em lotes.
- Criando uma macro com o mesmo nome que um comando da família do Windows Server 2003

  Se você sempre usar um comando específico com opções de linha de comando específicas, poderá criar uma macro com o mesmo nome do comando. Para especificar se você deseja executar a macro ou o comando, siga estas diretrizes:  
  -   Para executar a macro, digite o nome da macro no prompt de comando. Não adicione um espaço antes do nome da macro.
  -   Para executar o comando, insira um ou mais espaços no prompt de comando e, em seguida, digite o nome do comando.
- Excluindo uma macro

  Para excluir uma macro, digite:  
  ```
  doskey <MacroName> =
  ```

## <a name="BKMK_examples"></a>Disso

As opções de linha de comando **/macros** e **/history** são úteis para criar programas em lotes para salvar macros e comandos. Por exemplo, para armazenar todas as macros atuais do **doskey** , digite:
```
doskey /macros > macinit 
```
Para usar as macros armazenadas em MACINIT, digite:
```
doskey /macrofile=macinit 
```
Para criar um programa em lotes denominado tmp. bat que contenha comandos usados recentemente, digite:
```
doskey /history> tmp.bat 
```
Para definir uma macro com vários comandos, use **$t** para separar comandos, da seguinte maneira:
```
doskey tx=cd temp$tdir/w $*
```
No exemplo anterior, a macro TX altera o diretório atual para temp e, em seguida, exibe uma listagem de diretório no formato de exibição largo. Você pode usar **$ @ no__t-2*** no final da macro para acrescentar outras opções de linha de comando ao **dir** ao executar TX.

A macro a seguir usa um parâmetro de lote para um novo nome de diretório:
```
doskey mc=md $1$tcd $1
```
A macro cria um novo diretório e, em seguida, muda para o novo diretório a partir do diretório atual.

Para usar a macro anterior para criar e alterar para um diretório chamado livros, digite:
```
mc books
```
Para criar uma macro do **doskey** para um programa chamado FTP. exe, inclua **/EXEName** da seguinte maneira:
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Para usar a macro anterior, inicie o FTP. No prompt do FTP, digite:
```
go
```
O FTP executa os comandos **Open**, **MGET**e **adeus** .

Para criar uma macro que formata um disco de forma rápida e incondicional, digite:
```
doskey qf=format $1 /q /u
```
Para Formatar de forma rápida e incondicional um disco na unidade A, digite:
```
qf a:
```
Para excluir uma macro chamada Vlist, digite:
```
doskey vlist =
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
