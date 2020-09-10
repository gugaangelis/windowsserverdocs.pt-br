---
title: at
description: Artigo de referência para o comando at, que agenda comandos e programas a serem executados em um computador em uma data e hora especificadas.
ms.topic: reference
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8cd6762c6a88e24b6092dcce519582a0f627c77f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633421"
---
# <a name="at"></a>at

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agenda comandos e programas para serem executados em um computador em uma data e hora especificadas. Você pode usar somente quando o **serviço de agendamento** estiver em execução. Usado sem parâmetros, **em** lista comandos agendados. Você deve ser um membro do grupo local de administradores para executar esse comando.

## <a name="syntax"></a>Sintaxe

```
at [\computername] [[id] [/delete] | /delete [/yes]]
at [\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `\<computername\>` | Especifica um computador remoto. Se você omitir esse parâmetro, **em** agendará os comandos e programas no computador local. |
| `<id>` | Especifica o número de identificação atribuído a um comando agendado. |
| /delete | Cancela um comando agendado. Se você omitir a *ID*, todos os comandos agendados no computador serão cancelados. |
| /Yes | Responde sim a todas as consultas do sistema quando você exclui eventos agendados. |
| `<time>` | Especifica a hora em que você deseja executar o comando. o tempo é expresso como horas: minutos na notação de 24 horas (ou seja, 00:00 (meia-noite) até 23:59). |
| Interativo | Permite que o *comando* interaja com a área de trabalho do usuário que está conectado no momento da execução do *comando* . |
| cada | Executa o *comando* em todos os dias especificados ou dias da semana ou mês (por exemplo, todas as quintas-feiras ou o terceiro dia de cada mês). |
| `<date>` | Especifica a data em que você deseja executar o comando. Você pode especificar um ou mais dias da semana (ou seja, digite **M**,**T**,**W**,**th**,**F**,**S**,**Su**) ou um ou mais dias do mês (ou seja, digite 1 a 31). Separe várias entradas de data com vírgulas. Se você omitir *Data*, **em** usará o dia atual do mês. |
| última | Executa o *comando*  na próxima ocorrência do dia (por exemplo, próxima quinta-feira). |
| `<command>` | Especifica o comando do Windows, programa (ou seja, arquivo. exe ou. com) ou programa em lotes (ou seja, arquivo. bat ou. cmd) que você deseja executar. Quando o comando exigir um caminho como um argumento, use o caminho absoluto (ou seja, o caminho inteiro que começa com a letra da unidade). Se o comando estiver em um computador remoto, especifique a notação UNC (Convenção de nomenclatura universal) para o nome do servidor e do compartilhamento, em vez de uma letra de unidade remota. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="remarks"></a>Comentários

- Este comando não carrega automaticamente cmd.exe antes de executar comandos. Se você não estiver executando um arquivo executável (. exe), deverá carregar explicitamente cmd.exe no início do comando da seguinte maneira:

    ```
    cmd /c dir > c:\test.out
    ```

- Se você estiver usando este comando sem opções de linha de comando, as tarefas agendadas aparecerão em uma tabela formatada de forma semelhante à seguinte:

    ```
    Status  ID   Day        time        Command Line
    OK      1    Each F     4:30 PM     net send group leads status due
    OK      2    Each M     12:00 AM    chkstor > check.file
    OK      3    Each F     11:59 PM    backup2.bat
    ```

- Se incluir um número de identificação (*ID*) com esse comando, somente as informações de uma única entrada aparecerão em um formato semelhante ao seguinte:

    ```
    Task ID: 1
    Status: OK
    Schedule: Each  F
    Time of Day: 4:30 PM
    Command: net send group leads status due
    ```

- Depois de agendar um comando, especialmente um comando que tenha opções de linha de comando, verifique se a sintaxe do comando está correta digitando sem nenhuma opção de **linha de comando** . Se as informações na coluna **linha de comando** estiverem erradas, exclua o comando e digite-o novamente. Se ainda estiver incorreto, digite novamente o comando usando menos opções de linha de comando.

- Comandos agendados com **em** execução como processos em segundo plano. A saída não é exibida na tela do computador. Para redirecionar a saída para um arquivo, use o símbolo de redirecionamento `>` . Se você redirecionar a saída para um arquivo, precisará usar o símbolo de escape `^` antes do símbolo de redirecionamento, independentemente de estar usando **em** na linha de comando ou em um arquivo em lotes. Por exemplo, para redirecionar a saída para *output.txt*, digite:

    ```
    at 14:45 c:\test.bat ^>c:\output.txt
    ```

    O diretório atual para o comando em execução é a pasta systemroot.

- Se você alterar a hora do sistema depois de agendar um comando para execução, sincronize o **no** Agendador com a hora do sistema revisada digitando **em** sem opções de linha de comando.

- Os comandos agendados são armazenados no registro. Como resultado, você não perderá as tarefas agendadas se reiniciar o serviço de agendamento.

- Não use uma unidade redirecionada para trabalhos agendados que acessam a rede. O serviço de agendamento pode não ser capaz de acessar a unidade redirecionada, ou a unidade redirecionada pode não estar presente se um usuário diferente estiver conectado no momento em que a tarefa agendada for executada. Em vez disso, use caminhos UNC para trabalhos agendados. Por exemplo:

    ```
    at 1:00pm my_backup \\server\share
    ```

    Não use a seguinte sintaxe, em que **x:** é uma conexão feita pelo usuário:

    ```
    at 1:00pm my_backup x:
    ```

    Se você agendar um comando **at** que usa uma letra de unidade para se conectar a um diretório compartilhado, inclua um comando **at** para desconectar a unidade quando terminar de usar a unidade. Se a unidade não estiver desconectada, a letra da unidade atribuída não estará disponível no prompt de comando.

- Por padrão, as tarefas agendadas usando esse comando serão interrompidas após 72 horas. Você pode modificar o registro para alterar esse valor padrão.

    **Para modificar o registro**

    > [!Caution]
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

    1. Inicie o editor do registro (regedit.exe).

    2. Localize e clique na seguinte chave no registro: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule`

    3. No menu **Editar** , clique em **adicionar valor**e adicione os seguintes valores de registro:

        - **Nome do valor.** atTaskMaxHours

        - **Tipo de dados.** reg_DWOrd

        - **Radix.** Decimal

        - **Dados do valor:** 0. Um valor de **0** no campo de **dados de valor** indica que não há limite e não é interrompido. Os valores de 1 a 99 indicam o número de horas.

- Você pode usar a pasta tarefas agendadas para exibir ou modificar as configurações de uma tarefa que foi criada usando esse comando. Quando você agenda uma tarefa usando esse comando, a tarefa é listada na pasta tarefas agendadas, com um nome como o seguinte:**at3478**. No entanto, se você modificar uma tarefa por meio da pasta tarefas agendadas, ela será atualizada para uma tarefa agendada normal. A tarefa não é mais visível para o comando **at** e a configuração de conta at não se aplica mais a ela. Você deve inserir explicitamente uma conta de usuário e uma senha para a tarefa.

## <a name="examples"></a>Exemplos

Para exibir uma lista de comandos agendados no servidor de marketing, digite:

```
at \\marketing
```

Para saber mais sobre um comando com o número de identificação 3 no servidor Corp, digite:

```
at \\corp 3
```

Para agendar um comando net share para ser executado no servidor Corp às 8:00 A.M. e redirecione a listagem para o servidor de manutenção, no diretório compartilhado relatórios e no arquivo Corp.txt, digite:

```
at \\corp 08:00 cmd /c net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt
```

Para fazer backup do disco rígido do servidor de marketing em uma unidade de fita à meia-noite a cada cinco dias, crie um programa em lotes chamado Archive. cmd, que contém os comandos de backup e, em seguida, agende o programa em lotes a ser executado, digite:

```
at \\marketing 00:00 /every:5,10,15,20,25,30 archive
```

Para cancelar todos os comandos agendados no servidor atual, desmarque as informações **de agendamento a** seguir:

```
at /delete
```

Para executar um comando que não é um arquivo executável (. exe), preceda o comando com **cmd/c** para carregar cmd.exe da seguinte maneira:

```
cmd /c dir > c:\test.out
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Schtasks](schtasks.md). Outra ferramenta de agendamento de linha de comando.
