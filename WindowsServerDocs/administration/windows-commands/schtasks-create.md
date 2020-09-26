---
title: criar Schtasks
description: Artigo de referência para o comando schtasks create, que
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 4a7c3eb621f4c3d640fcb1f057b9d3cd00834e31
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390682"
---
# <a name="schtasks-create"></a>criar Schtasks

Agenda uma tarefa.

## <a name="syntax"></a>Sintaxe

```
schtasks /create /sc <scheduletype> /tn <taskname> /tr <taskrun> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru {[<domain>\]<user> | system}] [/rp <password>] [/mo <modifier>] [/d <day>[,<day>...] | *] [/m <month>[,<month>...]] [/i <idletime>] [/st <starttime>] [/ri <interval>] [{/et <endtime> | /du <duration>} [/k]] [/sd <startdate>] [/ed <enddate>] [/it] [/z] [/f]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /SC `<scheduletype>` | Especifica o tipo de agendamento. Os valores válidos incluem:<ul><li>**Minute** – especifica o número de minutos antes que a tarefa seja executada.</li><li>Por **hora** – especifica o número de horas antes que a tarefa seja executada.</li><li>**Daily** -especifica o número de dias antes que a tarefa seja executada.</li><li>**Semanalmente** Especifica o número de semanas antes que a tarefa seja executada.</li><li>**Mensal** – especifica o número de meses antes que a tarefa seja executada.</li><li>**Uma vez** -especifica que essa tarefa é executada uma vez em uma data e hora especificadas.</li><li>**OnStart** -especifica que a tarefa é executada toda vez que o sistema é iniciado. Você pode especificar uma data de início ou executar a tarefa na próxima vez que o sistema for iniciado.</li><li>**ONloginn** – especifica que a tarefa é executada sempre que um usuário (qualquer usuário) faz logon. Você pode especificar uma data ou executar a tarefa na próxima vez que o usuário fizer logon.</li><li>**OnIdle** -especifica que a tarefa é executada sempre que o sistema está ocioso por um período de tempo especificado. Você pode especificar uma data ou executar a tarefa na próxima vez em que o sistema estiver ocioso.</li></ul> |
| /TN `<taskname>` | Especifica um nome para a tarefa. Cada tarefa no sistema deve ter um nome exclusivo e deve estar de acordo com as regras para nomes de arquivo, não excedendo 238 caracteres. Use aspas para colocar os nomes que incluam espaços. |
| /TR `<Taskrun>` | Especifica o programa ou o comando que a tarefa executa. Digite o caminho totalmente qualificado e o nome de arquivo de um arquivo executável, arquivo de script ou arquivo em lotes. O nome do caminho não deve exceder 262 caracteres. Se você não adicionar o caminho, **Schtasks** assumirá que o arquivo está no `<systemroot>\System32` diretório. |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. O padrão é as permissões do usuário atual do computador local. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. As permissões da conta especificada são usadas para agendar a tarefa e executar a tarefa. Para executar a tarefa com as permissões de um usuário diferente, use o parâmetro **/ru** . A conta de usuário deve ser um membro do grupo Administradores no computador remoto. Além disso, o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio que seja confiável para o domínio do computador remoto. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /ru `{[<domain>\]<user> | system}` | Executa a tarefa com permissões da conta de usuário especificada. Por padrão, a tarefa é executada com as permissões do usuário atual do computador local ou com a permissão do usuário especificado pelo parâmetro **/u** , se houver uma incluída. O parâmetro **/ru** é válido ao agendar tarefas em computadores locais ou remotos. As opções válidas incluem:<ul><li>**Domínio** -especifica uma conta de usuário alternativa.</li><li>**Sistema** -especifica a conta do sistema local, uma conta altamente privilegiada usada pelo sistema operacional e serviços do sistema.</li></ul> |
| /RP `<password>` | Especifica uma senha para a conta de usuário existente ou a conta de usuário especificada pelo parâmetro **/ru** . Se você não usar esse parâmetro ao especificar uma conta de usuário, SchTasks.exe solicitará a senha na próxima vez que entrar. Não use o parâmetro **/RP** para tarefas que são executadas com credenciais de conta do sistema (**/ru System**). A conta do sistema não tem uma senha e SchTasks.exe não solicita uma. |
| /mo `<modifiers>` | Especifica com que frequência a tarefa é executada dentro de seu tipo de agendamento. As opções válidas incluem:<ul><li>**Minute** – especifica que a tarefa é executada a cada <n> minutos. Você pode usar qualquer valor entre 1-1439 minutos. Por padrão, isso é de 1 minuto.</li><li>**Hora** -especifica que a tarefa é executada a cada hora <n> . Você pode usar qualquer valor entre 1-23 horas. Por padrão, isso é de 1 hora.</li><li>**Daily** -especifica que a tarefa é executada a cada <n> dias. Você pode usar qualquer valor entre 1-365 dias. Por padrão, este é 1 dia.</li><li>**Semanal** -especifica que a tarefa é executada a cada <n> semanas. Você pode usar qualquer valor entre 1-52 semanas. Por padrão, esta é uma semana.</li><li>**Mensal** -especifica que a tarefa é executada a cada <n> meses. Você pode usar qualquer um dos valores a seguir:<ul><li>Um número entre 1-12 meses</li><li>**LastDay** -para executar a tarefa no último dia do mês</li><li>**Primeiro, segundo, terceiro ou quarto, juntamente com o `/d <day>` parâmetro** -especifica a semana e o dia específicos para executar a tarefa. Por exemplo, na terceira quarta-feira do mês.</li></ul></li><li>**Uma vez** -especifica que a tarefa é executada uma vez.</li><li>**OnStart** -especifica que a tarefa é executada na inicialização.</li><li>**ONloginn** – especifica que a tarefa é executada quando o usuário especificado pelo parâmetro **/u** faz logon.</li><li>**OnIdle** -especifica que a tarefa é executada Depois que o sistema está ocioso durante o número de minutos especificado pelo parâmetro **/i**</li></ul> |
| /d dia [, dia...] | Especifica com que frequência a tarefa é executada dentro de seu tipo de agendamento. As opções válidas incluem:<ul><li>**Semanal** -especifica que a tarefa é executada semanalmente fornecendo um valor entre 1-52 semanas. Opcionalmente, você também pode adicionar um dia da semana específico adicionando um valor de MON-dom ou um intervalo de [MON-dom...]).</li><li>**Mensal** -especifica que a tarefa é executada semanalmente por mês, fornecendo um valor de primeiro, segundo, terceiro, quarto, último. Opcionalmente, você também pode adicionar um dia da semana específico adicionando um valor de MON-dom ou fornecendo um número entre 1-12 meses. Se você usar essa opção, também poderá adicionar um dia específico do mês, fornecendo um número entre 1-31.<p>**Observação:** O valor de data de 1-31 é válido somente sem o parâmetro **/mo** ou se o parâmetro **/mo** é mensal (1-12). O padrão é dia 1 (o primeiro dia do mês).</li></ul> |
| /m mês [, mês...] | Especifica um mês ou meses do ano durante o qual a tarefa agendada deve ser executada. As opções válidas incluem JAN-DEC e `*` (todos os meses). O parâmetro **/m** é válido somente com um agendamento mensal. É necessário quando o modificador LASTDAY é usado. Caso contrário, é opcional e o valor padrão é `*` (todos os meses). |
| /i <Idletime> | Especifica quantos minutos o computador está ocioso antes que a tarefa seja iniciada. Um valor válido é um número inteiro de 1 a 999. Esse parâmetro é válido somente com um agendamento ONIDLE e, em seguida, é necessário. |
| /St `<Starttime>` | Especifica a hora de início para a tarefa, usando o formato de 24 horas, HH: mm. O valor padrão é a hora atual no computador local. O parâmetro **/St** é válido com minutos, por hora, diariamente, semanalmente, mensalmente e em agendas. Ele é necessário para uma única agenda. |
| /ri `<interval>` | Especifica o intervalo de repetição para a tarefa agendada, em minutos. Isso não é aplicável para os tipos de agendamento: MINUTE, hora, OnStart, ONLOGON e ONIDLE. O intervalo válido é 1-599940 (599940 minutos = 9999 horas). Se os parâmetros **/et** ou **/du** forem especificados, o padrão será **10 minutos**. |
| /et `<Endtime>` | Especifica a hora do dia em que uma agenda de tarefa de minuto ou hora termina em <HH: MM> formato de 24 horas. Após a hora de término especificada, o Schtasks não inicia a tarefa novamente até que a hora de início ocorra. Por padrão, os agendamentos de tarefas não têm hora de término. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora. |
| /du `<duration>` | Especifica um período máximo de tempo para uma agenda de minuto ou hora em <HHHH: MM> formato de 24 horas. Depois que o tempo especificado expira, o Schtasks não inicia a tarefa novamente até que a hora de início ocorra. Por padrão, os agendamentos de tarefas não têm duração máxima. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora. |
| /k | Interrompe o programa que a tarefa executa no momento especificado por **/et** ou **/du**. Sem **/k**, Schtasks não inicia o programa novamente depois que ele atingir o tempo especificado por **/et** ou **/du** , e não interromperá o programa se ele ainda estiver em execução. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora. |
| /SD <Startdate> | Especifica a data na qual a agenda de tarefas é iniciada. O valor padrão é a data atual no computador local. O formato de **StartDate** varia com a localidade selecionada para o computador local em **Opções regionais e de idioma**. Apenas um formato é válido para cada localidade. Os formatos de data válidos incluem (certifique-se de escolher o formato mais semelhante ao formato selecionado para **data abreviada** em **Opções regionais e de idioma** no computador local):<ul><li>`<MM>//` -Especifica para usar formatos de primeiro mês, como Inglês (Estados Unidos) e espanhol (Panamá).</li><li>`<DD>//` -Especifica para usar os formatos de primeiro dia, como búlgaro e holandês (Países Baixos).</li><li>`<YYYY>//` -Especifica o uso de formatos de primeiro ano, como sueco e francês (Canadá).</li></ul> |
| /Ed `<Enddate>` | Especifica a data em que o agendamento termina. Esse parâmetro é opcional. Ele não é válido em um agendamento uma vez, OnStart, ONLOGON ou ONIDLE. Por padrão, os agendamentos não têm data de término. O valor padrão é a data atual no computador local. O formato de **EndDate** varia com a localidade selecionada para o computador local em **Opções regionais e de idioma**. Apenas um formato é válido para cada localidade. Os formatos de data válidos incluem (certifique-se de escolher o formato mais semelhante ao formato selecionado para **data abreviada** em **Opções regionais e de idioma** no computador local):<ul><li>`<MM>//` -Especifica para usar formatos de primeiro mês, como Inglês (Estados Unidos) e espanhol (Panamá).</li><li>`<DD>//` -Especifica para usar os formatos de primeiro dia, como búlgaro e holandês (Países Baixos).</li><li>`<YYYY>//` -Especifica o uso de formatos de primeiro ano, como sueco e francês (Canadá).</li></ul> |
| /It | Especifica a execução da tarefa agendada somente quando o usuário executar como (a conta de usuário sob a qual a tarefa é executada) está conectado ao computador. Esse parâmetro não tem nenhum efeito nas tarefas que são executadas com permissões do sistema ou tarefas que já têm a propriedade somente interativa definida. Você não pode usar um comando Change para remover a propriedade somente interativa de uma tarefa. Por padrão, executar como usuário é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pelo parâmetro **/u** , se uma for usada. No entanto, se o comando incluir o parâmetro **/ru** , o usuário executar como será a conta especificada pelo parâmetro **/ru** . |
| /z | Especifica a exclusão da tarefa após a conclusão de sua agenda. |
| /f | Especifica para criar a tarefa e suprimir avisos se a tarefa especificada já existir. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="to-schedule-a-task-to-run-every-n-minutes"></a>Para agendar uma tarefa para ser executada a cada `<n>` minutos

Em um cronograma de minutos, o parâmetro **/SC minute** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de minutos entre cada execução da tarefa. O valor padrão de **/mo** é *1* (a cada minuto). Os parâmetros **/et** (End Time) e **/du** (Duration) são opcionais e podem ser usados com ou sem o parâmetro **/k** (end Task).

### <a name="examples"></a>Exemplos

- Para agendar um script de segurança, *s. vbs*, para ser executado a cada 20 minutos, digite:

    ```
    schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
    ```

    Como esse exemplo não inclui uma data ou hora de início, a tarefa inicia 20 minutos após a conclusão do comando e é executada a cada 20 minutos após a execução do sistema. Observe que o arquivo de origem do script de segurança está localizado em um computador remoto, mas que a tarefa está agendada e executada no computador local.

- Para agendar um script de segurança, *SEC. vbs*, para ser executado no computador local a cada 100 minutos entre 5:00 P.M. e 7:59 A.M. todos os dias, digite:

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
    ```

    Este exemplo usa o parâmetro **/SC** para especificar um agendamento de minuto e o parâmetro **/mo** para especificar um intervalo de 100 minutos. Ele usa os parâmetros **/St** e **/et** para especificar a hora de início e a hora de término da agenda de cada dia. Ele também usa o parâmetro **/k** para interromper o script se ele ainda estiver em execução às 7:59 A.M. Sem **/k**, Schtasks não iniciaria o script após 7:59 a.m., mas se a instância fosse iniciada às 6:20 a.m. ainda estava em execução, ele não o interromperia.

## <a name="to-schedule-a-task-to-run-every-n-hours"></a>Para agendar uma tarefa para ser executada a cada `<n>` hora

Em uma agenda por hora, o parâmetro **/SC Hourly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de horas entre cada execução da tarefa. O valor padrão de **/mo** é *1* (a cada hora). O parâmetro **/k** (end Task) é opcional e pode ser usado com **/et** (end no horário especificado) ou **/du** (terminar após o intervalo especificado).

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado a cada cinco horas, começando no primeiro dia de março de 2002, digite:

    ```
    schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
    ```

    Neste exemplo, o computador local usa a opção **Inglês (Zimbábue)** em **Opções regionais e de idioma**, portanto, o formato da data de início é MM/DD/AAAA (03/01/2002).

- Para agendar o programa MyApp para ser executado por hora, começando em cinco minutos após a meia-noite, digite:

    ```
    schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
    ```

- Para agendar o programa MyApp para ser executado a cada 3 horas, no total de 10 horas, digite:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
    ```

    Neste exemplo, a tarefa é executada às 12:00, às 3:00, às 6:00 A.M. e às 9:00. Como a duração é de 10 horas, a tarefa não é executada novamente às 12:00 Em vez disso, ele começa novamente às 12:00 da manhã no dia seguinte. Além disso, como o programa é executado por apenas alguns minutos, o parâmetro **/k** , que interrompe o programa se ele ainda estiver em execução quando a duração expirar, não é necessário.

## <a name="to-schedule-a-task-to-run-every-n-days"></a>Para agendar uma tarefa para ser executada a cada `<n>` dias

Em um agendamento diário, o parâmetro **/SC Daily** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de dias entre cada execução da tarefa. O valor padrão de **/mo** é *1* (todos os dias).

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado uma vez por dia, todos os dias, às 8:00 da manhã até 31 de dezembro de 2021, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2021
    ```

     Neste exemplo, o sistema de computador local é definido como a opção **Inglês (Reino Unido)** nas **Opções regionais e de idioma**, portanto, o formato da data de término é DD/MM/AAAA (31/12/2021). Além disso, como esse exemplo não inclui o parâmetro **/mo** , o intervalo padrão de *1* é usado para executar o comando todos os dias.

- Para agendar o programa MyApp para ser executado a cada doze dias às 1:00 (13:00) a partir de 31 de dezembro de 2021, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
    ```

    Neste exemplo, o sistema está definido como a opção em **Inglês (Zimbábue)** nas **Opções regionais e de idioma**, portanto, o formato da data de término é MM/DD/AAAA (12/31/2021).

- Para agendar um script de segurança, *SEC. vbs*, para ser executado a cada 70 dias, digite:

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
    ```

    Neste exemplo, o parâmetro **/it** é usado para especificar que a tarefa seja executada somente quando o usuário sob a conta em que a tarefa é executada é registrado no computador. Como a tarefa é executada com as permissões de uma conta de usuário específica, essa tarefa é executada somente quando o usuário está conectado.

    > [!NOTE]
    > Para identificar tarefas com a propriedade somente interativa (**/it**), use uma consulta detalhada (**/Query/v**). Em uma exibição de consulta detalhada de uma tarefa com/it, o campo de **modo de logon** tem um valor somente interativo.

## <a name="to-schedule-a-task-to-run-every-n-weeks"></a>Para agendar uma tarefa para ser executada a cada `<n>` semanas

Em uma agenda semanal, o parâmetro **/SC Weekly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão de **/mo** é *1* (a cada semana).

Os agendamentos semanais também têm um parâmetro opcional **/d** para agendar a execução da tarefa em dias da semana especificados ou em todos os dias (). O padrão é *Mon (segunda-feira)*. A opção todos os dias () é equivalente ao agendamento de uma tarefa diária.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado em um computador remoto a cada seis semanas, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
    ```

    Como esse exemplo deixa o parâmetro **/d** , a tarefa é executada nas segundas-feiras. Este exemplo também usa o parâmetro **/s** para especificar o computador remoto e o parâmetro **/u** para executar o comando com as permissões da conta de administrador do usuário. Além disso, como o parâmetro **/p** é deixado fora, SchTasks.exe solicita ao usuário a senha da conta de administrador e, como o comando é executado remotamente, todos os caminhos no comando, incluindo o caminho para MyApp.exe, se referem a caminhos no computador remoto.

- Para agendar uma tarefa para ser executada a cada sexta-feira, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
    ```

    Este exemplo usa o parâmetro **/mo** para especificar o intervalo de duas semanas e o parâmetro **/d** para especificar o dia da semana. Para agendar uma tarefa que é executada a cada sexta-feira, deixe o parâmetro **/mo** ou defina-o como *1*.

## <a name="to-schedule-a-task-to-run-every-n-months"></a>Para agendar uma tarefa para ser executada a cada `<n>` meses

Nesse tipo de agendamento, o parâmetro ' **/SC Monthly** ' é necessário. O parâmetro **/mo** (modificador), que especifica o número de meses entre cada execução da tarefa, é opcional e o padrão é *1* (todos os meses). Esse tipo de agendamento também tem um parâmetro opcional **/d** para agendar a tarefa a ser executada em uma data especificada do mês. O padrão é *1* (o primeiro dia do mês).

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado no primeiro dia de cada mês, digite:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc monthly
    ```

    O valor padrão para o parâmetro **/mo** (modificador) e o parâmetro **/d** (Day) é *1*, portanto, você não precisa usar nenhum desses parâmetros para este exemplo.

- Para agendar o programa MyApp para ser executado a cada três meses, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
    ```

    Este exemplo usa o parâmetro **/mo** para especificar um intervalo de 3 meses.

- Para agendar o programa MyApp para ser executado a cada dois meses no dia 21 do mês à meia-noite de um ano, de 2 de julho de 2002 a 30 de junho de 2003, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30
    ```

    Este exemplo usa o parâmetro **/mo** para especificar o intervalo mensal (a cada dois meses), o parâmetro **/d** para especificar a data, o parâmetro **/St** para especificar a hora e os parâmetros **/SD** e **/Ed** para especificar a data de início e a data de término, respectivamente. Também neste exemplo, o computador local é definido como a opção **Inglês (África do Sul)** nas **Opções regionais e de idioma**, portanto, as datas são especificadas no formato local, aaaa/mm/dd.

## <a name="to-schedule-a-task-to-run-on-a-specific-day-of-the-week"></a>Para agendar uma tarefa para ser executada em um dia específico da semana

A agenda do dia da semana é uma variação da agenda semanal. Em uma agenda semanal, o parâmetro **/SC Weekly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão de **/mo** é *1* (a cada semana). O parâmetro **/d** , que é opcional, agenda a tarefa para execução em dias da semana especificados ou em todos os dias (**&#42;**). O padrão é *Mon (segunda-feira)*. A opção todos os dias `(/d *)` é equivalente ao agendamento de uma tarefa diária.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado todas as semanas na quarta-feira, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
    ```

    Este exemplo usa o parâmetro **/d** para especificar o dia da semana. Como o comando deixa o parâmetro **/mo** , a tarefa é executada todas as semanas.

- Para agendar uma tarefa para ser executada na segunda-feira e na sexta-feira de cada oitava semana, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
    ```

    Este exemplo usa o parâmetro **/d** para especificar os dias e o parâmetro **/mo** para especificar o intervalo de oito semanas.

## <a name="to-schedule-a-task-to-run-on-a-specific-week-of-the-month"></a>Para agendar uma tarefa para ser executada em uma semana específica do mês

Nesse tipo de agendamento, o parâmetro **/SC mensal** , o parâmetro **/mo** (modificador) e o parâmetro **/d** (Day) são necessários. O parâmetro **/mo** (modificador) especifica a semana em que a tarefa é executada. O parâmetro **/d** especifica o dia da semana. Você pode especificar apenas um dia da semana para esse tipo de agendamento. Esse agendamento também tem um parâmetro de **/m** (mês) opcional que permite agendar a tarefa para determinados meses ou a cada mês (**&#42;**). O padrão para o parâmetro **/m** é todos os meses (**&#42;**).

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado no segundo domingo de cada mês, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
    ```

    Este exemplo usa o parâmetro **/mo** para especificar a segunda semana do mês e o parâmetro **/d** para especificar o dia.

- Para agendar o programa MyApp para ser executado na primeira segunda-feira em março e setembro, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
    ```

    Este exemplo usa o parâmetro **/mo** para especificar a primeira semana do mês e o parâmetro **/d** para especificar o dia. Ele usa o parâmetro **/m** para especificar o mês, separando os argumentos de mês com uma vírgula.

## <a name="to-schedule-a-task-to-run-on-a-specific-day-each-month"></a>Para agendar uma tarefa para ser executada em um dia específico a cada mês

Nesse tipo de agendamento, o parâmetro **/SC mensal** e o parâmetro **/d** (Day) são necessários. O parâmetro **/d** especifica uma data do mês (1-31), não um dia da semana, e você pode especificar apenas um dia na agenda. O parâmetro **/m** (month) é opcional, com o padrão sendo cada mês *()*, enquanto o parâmetro **/mo** (modificador) não é válido com esse tipo de agendamento.

Schtasks.exe não permitirá que você agende uma tarefa para uma data que não esteja em um mês especificado pelo parâmetro **/m** . Por exemplo, tentar agendar o dia 31 de fevereiro. No entanto, se você não usar o parâmetro **/m** e agendar uma tarefa para uma data que não aparecerá em todos os meses, a tarefa não será executada nos meses mais curtos. Para agendar uma tarefa para o último dia do mês, use o tipo de agendamento do último dia.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado no primeiro dia de cada mês, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
    ```

    Como o modificador padrão é *nenhum* (nenhum modificador), esse comando usa o dia padrão de *1*e o mês padrão de *cada mês*, sem exigir parâmetros adicionais.

- Para agendar o programa MyApp para ser executado em 15 de maio e 15 de junho às 3:00 P.M. (15:00), digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
    ```

    Este exemplo usa o parâmetro **/d** para especificar a data e o parâmetro **/m** para especificar os meses. Ele também usa o parâmetro **/St** para especificar a hora de início.

## <a name="to-schedule-a-task-to-run-on-the-last-day-of-a-month"></a>Para agendar uma tarefa a ser executada no último dia de um mês

No tipo de agendamento do último dia, o parâmetro **/SC Monthly** , o parâmetro **/mo lastDay** (modificador) e o parâmetro **/m** (month) são necessários. O parâmetro **/d** (Day) não é válido.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado no último dia de cada mês, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
    ```

    Este exemplo usa o parâmetro **/mo** para especificar o último dia e o parâmetro **/m** com o caractere curinga (**&#42;**) para indicar que o programa é executado todos os meses.

- Para agendar o programa MyApp para ser executado no último dia de fevereiro e no último dia de março às 6:00 P.M., digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
    ```

    Este exemplo usa o parâmetro **/mo** para especificar o último dia, o parâmetro **/m** para especificar os meses e o parâmetro **/St** para especificar a hora de início.

## <a name="to-schedule-to-run-once"></a>Para agendar para execução única

No tipo de agendamento de execução única, o parâmetro **/sc once** é necessário. O parâmetro **/St** , que especifica a hora em que a tarefa é executada, é necessário. O parâmetro **/SD** , que especifica a data em que a tarefa é executada, é opcional, enquanto os parâmetros **/mo** (modificador) e **/Ed** (data de término) não são válidos.

Schtasks não permitirá que você agende uma tarefa para ser executada uma vez se a data e hora especificadas estiverem no passado, com base na hora do computador local. Para agendar uma tarefa que é executada uma vez em um computador remoto em um fuso horário diferente, você deve agendá-la antes que essa data e hora ocorra no computador local.

### <a name="example"></a>Exemplo

- Para agendar o programa MyApp para ser executado à meia-noite em 1º de janeiro de 2003, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
    ```

    Este exemplo usa o parâmetro **/SC** para especificar o tipo de agendamento e os parâmetros **/SD** e **/St** para especificar a data e a hora. Também neste exemplo, o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma**, o formato para a data de início é mm/dd/aaaa.

## <a name="to-schedule-a-task-to-run-every-time-the-system-starts"></a>Para agendar uma tarefa para ser executada toda vez que o sistema for iniciado

No tipo de agendamento de início, o parâmetro **/SC OnStart** é necessário. O parâmetro **/SD** (data de início) é opcional e o padrão é a data atual.

### <a name="example"></a>Exemplo

- Para agendar o programa MyApp para ser executado toda vez que o sistema for iniciado, começando em 15 de março de 2001, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
    ```

    Neste exemplo, o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma**, o formato para a data de início é mm/dd/aaaa.

## <a name="to-schedule-a-task-to-run-when-a-user-logs-on"></a>Para agendar uma tarefa a ser executada quando um usuário fizer logon

O tipo de agendamento no logon agenda uma tarefa que é executada sempre que qualquer usuário faz logon no computador. No tipo de agendamento de logon, o parâmetro **/SC ONLOGON** é necessário. O parâmetro **/SD** (data de início) é opcional e o padrão é a data atual.

### <a name="example"></a>Exemplo

- Para agendar uma tarefa que é executada quando um usuário faz logon em um computador remoto, digite:

    ```
    schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
    ```

    Este exemplo agenda um arquivo em lotes para ser executado toda vez que um usuário (qualquer usuário) fizer logon no computador remoto. Ele usa o parâmetro **/s** para especificar o computador remoto. Como o comando é remoto, todos os caminhos no comando, incluindo o caminho para o arquivo em lotes, se referem a um caminho no computador remoto.

## <a name="to-schedule-a-task-to-run-when-the-system-is-idle"></a>Para agendar uma tarefa a ser executada quando o sistema estiver ocioso

O tipo de agendamento on Idle agenda uma tarefa que é executada sempre que não há atividade de usuário durante o tempo especificado pelo parâmetro **/i** . No tipo de agendamento inativo, o parâmetro **/SC OnIdle** e o parâmetro **/i** são necessários. O **/SD** (data de início) é opcional e o padrão é a data atual.

### <a name="example"></a>Exemplo

- Para agendar o programa MyApp para ser executado sempre que o computador estiver ocioso, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
    ```

    Este exemplo usa o parâmetro **/i** necessário para especificar que o computador deve permanecer ocioso por dez minutos antes que a tarefa seja iniciada.

## <a name="to-schedule-a-task-to-run-now"></a>Para agendar uma tarefa a ser executada agora

Schtasks não tem uma opção executar agora, mas você pode simular essa opção criando uma tarefa que é executada uma vez e inicia em alguns minutos.

### <a name="example"></a>Exemplo

- Para agendar uma tarefa para ser executada uma vez, em 13 de novembro de 2020 às 2:18 P.M. Hora local, digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
    ```

    Neste exemplo, o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma**, portanto, o formato da data de início é mm/dd/aaaa.

## <a name="to-schedule-a-task-that-runs-with-different-permissions"></a>Para agendar uma tarefa que é executada com permissões diferentes

Você pode agendar tarefas de todos os tipos para execução com permissões de uma conta alternativa no computador local e em um remoto. Além dos parâmetros necessários para o tipo de agendamento específico, o parâmetro **/ru** é necessário e o parâmetro **/RP** é opcional.

### <a name="examples"></a>Exemplos

- Para executar o programa MyApp no computador local, digite:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
    ```

    Este exemplo usa o parâmetro **/ru** para especificar que a tarefa deve ser executada com as permissões da conta de administrador do usuário (*Admin06*). Também neste exemplo, a tarefa está agendada para ser executada todas as terças-feiras, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões alternativas.

    Em resposta, SchTasks.exe solicita a senha executar como para a conta *Admin06* e, em seguida, exibe uma mensagem de êxito:

    ```
    Please enter the run as password for Admin06: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- Para executar o programa MyApp no computador de *marketing* a cada quatro dias, digite:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
    ```

    Este exemplo usa o parâmetro **/SC** para especificar um agendamento diário e o parâmetro **/mo** para especificar um intervalo de quatro dias. Além disso, este exemplo usa o parâmetro **/s** para fornecer o nome do computador remoto e o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto (*Admin01 no computador de marketing*). Por fim, este exemplo usa o parâmetro **/ru** para especificar que a tarefa deve ser executada com as permissões da conta de não administrador do usuário (*User01* no domínio *Reskits* ). Sem o parâmetro **/ru** , a tarefa seria executada com as permissões da conta especificada por **/u**.

    Ao executar este exemplo, o SchTasks primeiro solicita a senha do usuário chamado pelo parâmetro **/u** (para executar o comando) e, em seguida, solicita a senha do usuário chamado pelo parâmetro **/ru** (para executar a tarefa). Depois de autenticar as senhas, Schtasks exibe uma mensagem indicando que a tarefa está agendada:

    ```
    Type the password for Marketing\Admin01:********
    Please enter the run as password for Reskits\User01: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- Para executar a agenda, o programa de *AdminCheck.exe* para ser executado no computador *público* a cada sexta-feira às 4:00 A.M., mas somente se o administrador do computador estiver conectado, digite:

    ```
    schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
    ```

    Este exemplo usa o parâmetro **/SC** para especificar uma agenda semanal, o parâmetro **/d** para especificar o dia e o parâmetro **/St** para especificar a hora de início. Ele também usa o parâmetro **/s** para fornecer o nome do computador remoto, o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto, o parâmetro **/ru** para configurar a tarefa a ser executada com as permissões do administrador do computador público (*Public\Admin01*) e o parâmetro **/it** para indicar que a tarefa é executada somente quando a conta *Public\Admin01* está conectada.

    > [!NOTE]
    > Para identificar tarefas com a propriedade somente interativa (**/it**), use uma consulta detalhada ( `/query /v` ). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o campo de **modo de logon** tem um valor **somente interativo**.

## <a name="to-schedule-a-task-that-runs-with-system-permissions"></a>Para agendar uma tarefa que é executada com permissões do sistema

Tarefas de todos os tipos podem ser executadas com permissões da conta do **sistema** tanto no computador local quanto em um remoto. Além dos parâmetros necessários para o tipo de agendamento específico, o parâmetro do **sistema/ru** (ou **/ru**) é necessário, enquanto o parâmetro **/RP** não é válido.

> [!IMPORTANT]
> A conta do **sistema** não tem direitos de logon interativos. Os usuários não podem ver ou interagir com programas ou tarefas executadas com permissões do sistema. O parâmetro **/ru** determina as permissões sob as quais a tarefa é executada, não as permissões usadas para agendar a tarefa. Somente os administradores podem agendar tarefas, independentemente do valor do parâmetro **/ru** .
>
> Para identificar tarefas que são executadas com permissões do sistema, use uma consulta detalhada ( `/query /v` ). Em uma exibição de consulta detalhada de uma tarefa de execução do sistema, o campo **Executar como usuário** tem um valor de **NT AUTHORITY\SYSTEM** e o campo **modo de logon** tem apenas um valor de **segundo plano**.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp para ser executado no computador local com permissões da conta do **sistema** , digite:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
    ```

    Neste exemplo, a tarefa está agendada para ser executada no décimo-quinto dia de cada mês, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões do sistema. Além disso, este exemplo usa o parâmetro de **sistema/ru** para especificar o contexto de segurança do sistema. Como as tarefas do sistema não usam uma senha, o parâmetro **/RP** é deixado de fora.

    Em resposta, SchTasks.exe exibe uma mensagem informativa e uma mensagem de êxito, sem solicitar uma senha:

    ```
    INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
    SUCCESS: The Scheduled task My App has successfully been created.
    ```

- Para agendar o programa MyApp para ser executado no computador *Finance01* todas as manhãs às 4:00 A.M., usando permissões do sistema, digite:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
    ```

    Este exemplo usa o parâmetro **/TN** para nomear a tarefa e o parâmetro **/TR** para especificar a cópia remota do programa MyApp, o parâmetro **/SC** para especificar um agendamento diário, mas deixa o parâmetro **/mo** porque *1* (todos os dias) é o padrão. Este exemplo também usa o parâmetro **/St** para especificar a hora de início, que também é a hora em que a tarefa será executada todos os dias, o parâmetro **/s** para fornecer o nome do computador remoto, o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto e o parâmetro **/ru** para especificar que a tarefa deve ser executada sob a conta do sistema. Sem o parâmetro **/ru** , a tarefa seria executada usando as permissões da conta especificada pelo parâmetro **/u** .

    Schtasks.exe solicita a senha do usuário nomeada pelo parâmetro **/u** e, depois de autenticar a senha, exibe uma mensagem indicando que a tarefa foi criada e que será executada com as permissões da conta do **sistema** :

    ```
    Type the password for Admin01:**********

    INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
    SYSTEM).
    SUCCESS: The scheduled task My App has successfully been created.
    ```

## <a name="to-schedule-a-task-that-runs-more-than-one-program"></a>Para agendar uma tarefa que executa mais de um programa

Cada tarefa executa apenas um programa. No entanto, você pode criar um arquivo em lotes que executa vários programas e, em seguida, agendar uma tarefa para executar o arquivo em lotes.

1. Usando um editor de texto, como o bloco de notas, crie um arquivo em lotes que inclua o nome e o caminho totalmente qualificado para o arquivo. exe necessário para iniciar os programas Visualizador de Eventos (Eventvwr.exe) e monitor do sistema (Perfmon.exe).

    ```
    C:\Windows\System32\Eventvwr.exe
    C:\Windows\System32\Perfmon.exe
    ```

2. Salve o arquivo como *MyApps.bat*, abra schtasks.exe e, em seguida, crie uma tarefa para executar *MyApps.bat* digitando:

    ```
    schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
    ```

    Esse comando cria a tarefa de monitor, que é executada sempre que alguém faz logon. Ele usa o parâmetro **/TN** para nomear a tarefa, o parâmetro **/TR** a ser executado MyApps.bat, o parâmetro **/SC** para indicar o tipo de agendamento onloginn e o parâmetro **/ru** para executar a tarefa com as permissões da conta de administrador do usuário.

    Como resultado desse comando, sempre que um usuário fizer logon no computador, a tarefa iniciará o Visualizador de Eventos e o monitor do sistema.

## <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a>Para agendar uma tarefa que é executada em um computador remoto

Para agendar uma tarefa para ser executada em um computador remoto, você deve adicionar a tarefa à agenda do computador remoto. Tarefas de todos os tipos podem ser agendadas em um computador remoto, mas as seguintes condições devem ser atendidas:

- Você deve ter permissão para agendar a tarefa. Dessa forma, você deve estar conectado ao computador local com uma conta que seja membro do grupo Administradores no computador remoto, ou deve usar o parâmetro **/u** para fornecer as credenciais de um administrador do computador remoto.

- Você pode usar o parâmetro **/u** somente quando os computadores locais e remotos estiverem no mesmo domínio ou o computador local estiver em um domínio no qual o domínio do computador remoto confia. Caso contrário, o computador remoto não poderá autenticar a conta de usuário especificada e não poderá verificar se a conta é membro do grupo Administradores.

- A tarefa deve ter permissão suficiente para ser executada no computador remoto. As permissões necessárias variam de acordo com a tarefa. Por padrão, a tarefa é executada com a permissão do usuário atual do computador local ou, se o parâmetro **/u** for usado, a tarefa será executada com a permissão da conta especificada pelo parâmetro **/u** . No entanto, você pode usar o parâmetro **/ru** para executar a tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema.

### <a name="examples"></a>Exemplos

- Para agendar o programa MyApp (como administrador) para ser executado no computador remoto *Srv01* a cada dez dias, começando imediatamente, digite:

    ```
    schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
    ```

    Este exemplo usa o parâmetro **/s** para fornecer o nome do computador remoto. Como o usuário atual local é um administrador do computador remoto, o parâmetro **/u** , que fornece permissões alternativas para o agendamento da tarefa, não é necessário.

    > [!NOTE]
    > Ao agendar tarefas em um computador remoto, todos os parâmetros se referem ao computador remoto. Portanto, o arquivo especificado pelo parâmetro **/TR** refere-se à cópia de MyApp.exe no computador remoto.

- Para agendar o programa MyApp (como um usuário) para ser executado no computador remoto *SRV06* a cada três horas, digite:

    ```
    schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
    ```

    Como as permissões de administrador são necessárias para agendar uma tarefa, o comando usa os parâmetros **/u** e **/p** para fornecer as credenciais da conta de administrador do usuário (*Admin01* no domínio *Reskits* ). Por padrão, essas permissões também são usadas para executar a tarefa. No entanto, como a tarefa não precisa de permissões de administrador para ser executada, o comando inclui os parâmetros **/u** e **/RP** para substituir o padrão e executar a tarefa com permissão de conta de não administrador do usuário no computador remoto.

- Para agendar o programa MyApp (como um usuário) para ser executado no computador remoto *SRV02* no último dia de cada mês.

    ```
    schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
    ```

    Como o usuário atual local (*user03*) não é um administrador do computador remoto, o comando usa o parâmetro **/u** para fornecer as credenciais da conta de administrador do usuário (*Admin01* no domínio *Reskits* ). As permissões de conta de administrador serão usadas para agendar a tarefa e executar a tarefa.

    Como o comando não incluiu o parâmetro **/p** (password), Schtasks solicita a senha. Em seguida, ele exibe uma mensagem de êxito e, nesse caso, um aviso:

    ```
    Type the password for reskits\admin01:********

    SUCCESS: The scheduled task My App has successfully been created.
    WARNING: The scheduled task My App has been created, but may not run because the account information could not be set.
    ```

    Esse aviso indica que o domínio remoto não pôde autenticar a conta especificada pelo parâmetro **/u** . Nesse caso, o domínio remoto não pôde autenticar a conta de usuário porque o computador local não é membro de um domínio no qual o domínio do computador remoto confia. Quando isso ocorre, o trabalho de tarefa é exibido na lista de tarefas agendadas, mas a tarefa está realmente vazia e não será executada.

    A exibição a seguir de uma consulta detalhada expõe o problema com a tarefa. Na exibição, observe que o valor do **próximo tempo de execução** **nunca** é e que o valor de **Executar como usuário** **não pôde ser recuperado do banco de dados do Agendador de tarefas**.

    Esse computador era membro do mesmo domínio ou de um domínio confiável, a tarefa teria sido agendada com êxito e teria sido executada conforme especificado.

    ```
    HostName: SRV44
    TaskName: My App
    Next Run Time: Never
    Status:
    Logon mode: Interactive/Background
    Last Run Time: Never
    Last Result: 0
    Creator: user03
    Schedule: At 3:52 PM on day 31 of every month, start
    starting 12/14/2001
    Task To Run: c:\program files\corpapps\myapp.exe
    Start In: myapp.exe
    Comment: N/A
    Scheduled Task State: Disabled
    Scheduled Type: Monthly
    Start Time: 3:52:00 PM
    Start Date: 12/14/2001
    End Date: N/A
    Days: 31
    Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
    V,DEC
    Run As User: Could not be retrieved from the task sched
    uler database
    Delete Task If Not Rescheduled: Enabled
    Stop Task If Runs X Hours and X Mins: 72:0
    Repeat: Every: Disabled
    Repeat: Until: Time: Disabled
    Repeat: Until: Duration: Disabled
    Repeat: Stop If Still Running: Disabled
    Idle Time: Disabled
    Power Management: Disabled
    ```

#### <a name="remarks"></a>Comentários

- Para executar o comando **/Create** com as permissões de um usuário diferente, use o parâmetro **/u** . O parâmetro **/u** é válido somente para tarefas de agendamento em computadores remotos.

- Para exibir mais `schtasks /create` exemplos, digite `schtasks /create /?` em um prompt de comando.

- Para agendar uma tarefa que é executada com permissões de um usuário diferente, use o parâmetro **/ru** . O parâmetro **/ru** é válido para tarefas em computadores locais e remotos.

- Para usar o parâmetro **/u** , o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio no qual o domínio do computador remoto confia. Caso contrário, a tarefa não será criada ou o trabalho da tarefa estará vazio e a tarefa não será executada.

- Schtasks sempre solicita uma senha, a menos que você forneça uma, mesmo quando você agenda uma tarefa no computador local usando a conta de usuário atual. Esse comportamento é normal para Schtasks.

- Schtasks não verifica locais de arquivos de programas ou senhas de contas de usuário. Se você não inserir o local de arquivo correto ou a senha correta para a conta de usuário, a tarefa será criada, mas ela não será executada. Além disso, se a senha de uma conta for alterada ou expirar, e você não alterar a senha salva na tarefa, a tarefa não será executada.

- A conta do **sistema** não tem direitos de logon interativos. Os usuários não veem e não podem interagir com programas executados com permissões do sistema.

- Cada tarefa executa apenas um programa. No entanto, você pode criar um arquivo em lotes que inicia várias tarefas e, em seguida, agendar uma tarefa que executa o arquivo em lotes.

- Você pode testar uma tarefa assim que criá-la. Use a operação executar para testar a tarefa e, em seguida, verifique o arquivo de SchedLgU.txt (SystemRoot\SchedLgU.txt) em busca de erros.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de alteração de Schtasks](schtasks-change.md)

- [comando de exclusão de Schtasks](schtasks-delete.md)

- [comando de término de Schtasks](schtasks-end.md)

- [comando de consulta Schtasks](schtasks-query.md)

- [comando Schtasks execute](schtasks-run.md)