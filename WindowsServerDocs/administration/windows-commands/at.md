---
title: at
description: Tópico de comandos do Windows para **em** - agenda comandos e programas que serão executados em um computador em uma data e hora especificadas.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc9d9f3d008db1bb85bfb6afa0308834c929b5f0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435283"
---
# <a name="at"></a>at

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agenda comandos e programas para ser executado em um computador em uma data e hora especificadas. Você pode usar **em** somente quando o serviço de agendamento está em execução. Usado sem parâmetros, **em** lista os comandos agendados.
## <a name="syntax"></a>Sintaxe
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \\\\\<computerName\> |                                                                                                                                                        Especifica um computador remoto. Se você omitir esse parâmetro, **em** agenda os comandos e programas no computador local.                                                                                                                                                        |
|        \<id\>        |                                                                                                                                                                                   Especifica o número de identificação atribuído a um comando programado.                                                                                                                                                                                   |
|       /delete        |                                                                                                                                                                Cancela um comando programado. Se você omitir *ID*, todos os comandos agendados no computador são cancelados.                                                                                                                                                                |
|         /Yes         |                                                                                                                                                                               Respostas Sim para todas as consultas do sistema quando você exclui os eventos agendados.                                                                                                                                                                               |
|       \<time\>       |                                                                                                                                          Especifica a hora de quando você deseja executar o comando. tempo é expresso como horas: minutos na notação de 24 horas (ou seja, 00:00 [meia-noite] por meio do 23:59).                                                                                                                                          |
|     /interactive     |                                                                                                                                                                  Permite *comando* para interagir com a área de trabalho do usuário que está conectado no momento *comando* é executado.                                                                                                                                                                  |
|       /every:        |                                                                                                                                                    Execuções *comando* em cada dia especificado ou dias da semana ou mês (por exemplo, toda quinta-feira, ou o terceiro dia de cada mês).                                                                                                                                                    |
|       \<date\>       |                                                  Especifica a data quando desejar executar o comando. Você pode especificar um ou mais dias da semana (ou seja, digite **M**,**T**,**W**,**Th**,**F**,**S**,**Su**) ou um ou mais dias do mês (ou seja, tipo de 1 a 31). Separe várias entradas de data com vírgulas. Se você omitir *data*, **em** usa o dia do mês atual.                                                  |
|        /next:        |                                                                                                                                                                              Execuções *comando* na próxima ocorrência do dia (por exemplo, a próxima quinta-feira).                                                                                                                                                                              |
|     \<command\>      | Especifica o comando do Windows, o programa (ou seja,. .exe ou arquivo) ou arquivo em lotes (isto é, arquivo. bat ou. cmd) que você deseja executar. Quando o comando requer um caminho como um argumento, use o caminho absoluto (ou seja, o início de todo o caminho com a letra da unidade). Se o comando estiver em um computador remoto, especifique a notação de convenção de nomenclatura Universal (UNC) para o servidor e compartilhe o nome, em vez de uma letra de unidade remota. |
|          /?          |                                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentários
- **SCHTASKS** é outra ferramenta de agendamento de linha de comando que você pode usar para criar e gerenciar tarefas agendadas. Para obter mais informações sobre **schtasks**, consulte os tópicos relacionados.
- Usando **em**  
  Para usar **em**, você deve ser um membro do grupo Administradores local.
- Carregando Cmd.exe  
  **em** não carregará automaticamente Cmd.exe, o interpretador de comando, antes de executar comandos. Se você não estiver executando um arquivo executável (.exe), você deverá carregar explicitamente Cmd.exe no início do comando da seguinte maneira: **cmd /c dir > c:\test.out**
- Exibindo comandos agendados  
  Quando você usa **em** sem opções de linha de comando, tarefas agendadas são exibidas em uma tabela formatada semelhantes à seguinte:
  ```
  Status  ID   Day        time        Command Line
  OK      1    Each F     4:30 PM     net send group leads status due
  OK      2    Each M     12:00 AM    chkstor > check.file
  OK      3    Each F     11:59 PM    backup2.bat
  ```
- Incluindo o número de identificação (*ID*)  
  Quando você inclui o número de identificação (*identificação*) com **em** em um prompt de comando, informações de uma única entrada são exibidas em um formato semelhante ao seguinte:  
  ```
  Task ID:      1
  Status:       OK
  Schedule:     Each  F
  time of Day:  4:30 PM
  Command:      net send group leads status due
  ```
  Depois de você agendar um comando com **na**, especialmente um comando que tem opções de linha de comando, verifique se a sintaxe de comando está correta digitando **em** sem opções de linha de comando. Se as informações da coluna da linha de comando estiverem incorretas, exclua o comando e digite-a novamente. Se ele ainda estiver incorreto, digite novamente o comando com menos opções de linha de comando.
- Exibindo resultados  
  Comandos programados com **em** executados como processos em segundo plano. Saída não é exibida na tela do computador. Para redirecionar a saída para um arquivo, use o símbolo de redirecionamento (>). Se você redirecionar a saída para um arquivo, você precisará usar o símbolo de escape (^) antes do símbolo de redirecionamento, se você estiver usando **em** na linha de comando ou em um arquivo em lotes. Por exemplo, para redirecionar a saída para Output. Text, digite:  
  `at 14:45 c:\test.bat ^>c:\output.txt`  
  O diretório atual para o comando de execução é a pasta raiz.
- Alterar a hora do sistema  
  Se você alterar a hora do sistema em um computador depois de você agendar um comando para executar com **na**, sincronizar a **na** Agendador com a hora do sistema revisado, digitando **em** sem Opções de linha de comando.
- Comandos de armazenamento  
  Comandos agendados são armazenados no registro. Como resultado, você não perca as tarefas agendadas se você reiniciar o serviço de agendamento.
- Conectando-se à rede unidades  
  Não use uma unidade redirecionada para trabalhos agendados que acessam a rede. O serviço de agendamento não pode ser capaz de acessar a unidade redirecionada ou a unidade redirecionada pode não estar presente se outro usuário estiver conectado no momento em que a tarefa agendada é executada. Em vez disso, use caminhos UNC para trabalhos agendados. Por exemplo:  
  `at 1:00pm my_backup \\\server\share`  
  Não use a seguinte sintaxe, onde **x:** é uma conexão feita pelo usuário:  
  `at 1:00pm my_backup x:`  
  Se você agendar uma **na** comando que usa uma letra de unidade para se conectar a uma pasta compartilhada, inclua um **em** comando para desconectar a unidade quando tiver terminado de usar a unidade. Se a unidade não está desconectada, a letra da unidade atribuída não está disponível no prompt de comando.
- Parando após 72 horas de tarefas  
  Por padrão, as tarefas agendadas usando o **em** comando stop após 72 horas. Você pode modificar o registro para alterar esse valor padrão.
  1.  Inicie o editor do registro (regedit.exe).
  2.  Localize e clique na seguinte chave do registro: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule**
  3.  No menu Editar, clique em Adicionar valor e, em seguida, adicione o seguinte valor de registro: Nome do valor: atTaskMaxHours tipo de dados: reg_DWOrd base: Dados do valor decimal: 0. Um valor de 0 no campo de dados de valor não indica nenhum limite, não para. Valores de 1 a 99 indica o número de horas.
  **Cuidado**
- A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.
- Agendador de tarefas e o **em** comando  
  Você pode usar a pasta tarefas agendadas para exibir ou modificar as configurações de uma tarefa que foi criado usando o **em** comando. Quando você agendar uma tarefa usando o **na** de comando, a tarefa é listada na pasta tarefas agendadas, com um nome como o seguinte:**at3478**. No entanto, se você modificar uma tarefa através da pasta de tarefas agendadas, ela é atualizada para uma tarefa agendada normal. A tarefa não está mais visível para o **em** comando e na conta da configuração não se aplica a ele. Você deve digitar explicitamente uma conta de usuário e senha para a tarefa.
  ## <a name="examples"></a>Exemplos
  Para exibir uma lista de comandos agendada no servidor de Marketing, digite:

`at \\marketing`

Para saber mais sobre um comando com o número de identificação 3 no servidor Corp, digite:

`at \\corp 3`

Para agendar um comando net share para ser executado no servidor Corp às 8 horas. e redirecionar a listagem para o servidor de manutenção, na pasta compartilhada de relatórios e o arquivo Corp., tipo:

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

Para fazer backup de disco rígido do servidor Marketing em uma unidade de fita à meia-noite a cada cinco dias, crie um arquivo em lotes chamado arquivo. cmd, que contém os comandos de backup, e, em seguida, agendar o programa de lote para executar, digite:

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Para cancelar todos os comandos agendados no servidor atual, limpe o **em** informações de agendamento da seguinte maneira:

`at /delete`

Para executar um comando que não é um arquivo executável (ou seja, .exe), preceda o comando com **cmd /c** carregar Cmd.exe da seguinte maneira:

`cmd /c dir > c:\test.out`
