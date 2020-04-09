---
title: schtasks
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d4c28072a8e4d01ea3a045314796bcda32c8a59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835239"
---
# <a name="schtasks"></a>schtasks



Agenda comandos e programas para serem executados periodicamente ou em um momento específico. Adiciona e remove tarefas da agenda, inicia e interrompe as tarefas sob demanda e exibe e altera as tarefas agendadas.

Para exibir a sintaxe do comando, clique em um dos seguintes comandos:
-   [criar Schtasks](#BKMK_create)
-   [troca de Schtasks](#BKMK_change)
-   [execução de Schtasks](#BKMK_run)
-   [término de Schtasks](#BKMK_end)
-   [exclusão de Schtasks](#BKMK_delete)
-   [consulta Schtasks](#BKMK_query)

## <a name="remarks"></a>Comentários

- **Schtasks. exe** executa as mesmas operações que **as tarefas agendadas** no **painel de controle**. Você pode usar essas ferramentas juntas e de forma intercambiável.
- O **Schtasks** substitui o **. exe**, uma ferramenta incluída nas versões anteriores do Windows. Embora **at. exe** ainda esteja incluído na família Windows Server 2003, o **Schtasks** é a ferramenta de agendamento de tarefa de linha de comando recomendada.
- Os parâmetros em um comando **Schtasks** podem aparecer em qualquer ordem. Digitar **Schtasks** sem parâmetros executa uma consulta.
- Permissões para **Schtasks**  
  -   Você deve ter permissão para executar o comando. Qualquer usuário pode agendar uma tarefa no computador local e pode exibir e alterar as tarefas que eles agendaram. Os membros do grupo Administradores podem agendar, exibir e alterar todas as tarefas no computador local.
  -   Para agendar, exibir ou alterar uma tarefa em um computador remoto, você deve ser membro do grupo Administradores no computador remoto ou deve usar o parâmetro **/u** para fornecer as credenciais de um administrador do computador remoto.
  -   Você pode usar o parâmetro **/u** em uma operação **/Create** ou **/Change** somente quando os computadores locais e remotos estiverem no mesmo domínio ou o computador local estiver em um domínio no qual o domínio do computador remoto confia. Caso contrário, o computador remoto não poderá autenticar a conta de usuário especificada e não poderá verificar se a conta é membro do grupo Administradores.
  -   A tarefa deve ter permissão para ser executada. As permissões necessárias variam de acordo com a tarefa. Por padrão, as tarefas são executadas com as permissões do usuário atual do computador local ou com as permissões do usuário especificado pelo parâmetro **/u** , se houver uma incluída. Para executar uma tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema, use o parâmetro **/ru** .
- Para verificar se uma tarefa agendada foi executada ou para descobrir por que uma tarefa agendada não foi executada, consulte o log de transações do serviço Agendador de Tarefas, *systemroot*\SchedLgU.txt. Esses registros de log tentaram execuções iniciadas por todas as ferramentas que usam o serviço, incluindo **tarefas agendadas** e **Schtasks. exe**.
- Em raras ocasiões, os arquivos de tarefa ficam corrompidos. As tarefas corrompidas não são executadas. Quando você tenta executar uma operação em tarefas corrompidas, o **Schtasks. exe** exibe a seguinte mensagem de erro:  
  ```
  ERROR: The data is invalid.
  ```  
  Não é possível recuperar tarefas corrompidas. Para restaurar os recursos de agendamento de tarefas do sistema, use **Schtasks. exe** ou **tarefas agendadas** para excluir as tarefas do sistema e reagendá-las.

## <a name="schtasks-create"></a><a name=BKMK_create></a>criar Schtasks

Agenda uma tarefa.

**Schtasks** usa combinações de parâmetros diferentes para cada tipo de agendamento. Para ver a sintaxe combinada para criar tarefas ou para ver a sintaxe para criar uma tarefa com um tipo específico de agendamento, clique em uma das opções a seguir.
-   [Descrições de parâmetros e sintaxe combinadas](#BKMK_syntax)
-   [Para agendar uma tarefa que é executada a cada N minutos](#BKMK_minutes)
-   [Para agendar uma tarefa que é executada a cada N horas](#BKMK_hours)
-   [Para agendar uma tarefa que é executada a cada N dias](#BKMK_days)
-   [Para agendar uma tarefa que é executada a cada N semanas](#BKMK_weeks)
-   [Para agendar uma tarefa que é executada a cada N meses](#BKMK_months)
-   [Para agendar uma tarefa que é executada em um dia específico da semana](#BKMK_spec_day)
-   [Para agendar uma tarefa que é executada em uma semana específica do mês](#BKMK_spec_week)
-   [Para agendar uma tarefa que é executada em uma data específica a cada mês](#BKMK_spec_date)
-   [Para agendar uma tarefa que é executada no último dia de um mês](#BKMK_last_day)
-   [Para agendar uma tarefa que é executada uma vez](#BKMK_once)
-   [Para agendar uma tarefa que é executada toda vez que o sistema é iniciado](#BKMK_startup)
-   [Para agendar uma tarefa que é executada quando um usuário faz logon](#BKMK_logon)
-   [Para agendar uma tarefa que é executada quando o sistema está ocioso](#BKMK_idle)
-   [Para agendar uma tarefa que é executada agora](#BKMK_now)
-   [Para agendar uma tarefa que é executada com permissões diferentes](#BKMK_diff_perms)
-   [Para agendar uma tarefa que é executada com permissões do sistema](#BKMK_sys_perms)
-   [Para agendar uma tarefa que executa mais de um programa](#BKMK_multi_progs)
-   [Para agendar uma tarefa que é executada em um computador remoto](#BKMK_remote)

### <a name="combined-syntax-and-parameter-descriptions"></a><a name=BKMK_syntax></a>Descrições de parâmetros e sintaxe combinadas

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

##### <a name="parameters"></a>Parâmetros

##### <a name="sc-scheduletype"></a>/SC \<Scheduler >

Especifica o tipo de agendamento. Os valores válidos são minuto, por hora, diariamente, SEMANAlmente, MENSALmente, uma vez, OnStart, onloginn, ONIDLE.

|Tipo de agendamento|Descrição|
|-------------|-----------|
|MINUTO, POR HORA, DIARIAMENTE, SEMANALMENTE, MENSALMENTE|Especifica a unidade de tempo para o agendamento.|
|MESMO|A tarefa é executada uma vez em uma data e hora especificadas.|
|Star|A tarefa é executada toda vez que o sistema é iniciado. Você pode especificar uma data de início ou executar a tarefa na próxima vez que o sistema for iniciado.|
|Onloginn|A tarefa é executada sempre que um usuário (qualquer usuário) faz logon. Você pode especificar uma data ou executar a tarefa na próxima vez que o usuário fizer logon.|
|AGENDA|A tarefa é executada sempre que o sistema está ocioso por um período de tempo especificado. Você pode especificar uma data ou executar a tarefa na próxima vez em que o sistema estiver ocioso.|

##### <a name="tn-taskname"></a>/TN \<Nome_tarefa >

Especifica um nome para a tarefa. Cada tarefa no sistema deve ter um nome exclusivo. O nome deve estar em conformidade com as regras para nomes de arquivo e não deve exceder 238 caracteres. Use aspas para colocar os nomes que incluam espaços.

##### <a name="tr-taskrun"></a>/TR \<TaskRun >

Especifica o programa ou o comando que a tarefa executa. Digite o caminho totalmente qualificado e o nome de arquivo de um arquivo executável, arquivo de script ou arquivo em lotes. O nome do caminho não deve exceder 262 caracteres. Se você omitir o caminho, **Schtasks** assumirá que o arquivo está no diretório *systemroot*\System32

##### <a name="s-computer"></a>/s \<computador >

Agenda uma tarefa no computador remoto especificado. Digite o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.

##### <a name="u-domainuser"></a>/u [\<\]de domínio > <User>

Executa este comando com as permissões da conta de usuário especificada. O padrão é as permissões do usuário atual do computador local. Os parâmetros **/u** e **/p** são válidos somente para o agendamento de uma tarefa em um computador remoto ( **/s**).

As permissões da conta especificada são usadas para agendar a tarefa e executar a tarefa. Para executar a tarefa com as permissões de um usuário diferente, use o parâmetro **/ru** .

A conta de usuário deve ser um membro do grupo Administradores no computador remoto. Além disso, o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio que seja confiável para o domínio do computador remoto.

##### <a name="p-password"></a>/p \<senha >

Fornece a senha para a conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** , mas omitir o parâmetro **/p** ou o argumento password, **Schtasks** solicitará uma senha e obscurecerá o texto que você digitar.

Os parâmetros **/u** e **/p** são válidos somente para o agendamento de uma tarefa em um computador remoto ( **/s**).

##### <a name="ru-domainuser--system"></a>/ru {[\<> de domínio\]<User> | Sistema

Executa a tarefa com permissões da conta de usuário especificada. Por padrão, a tarefa é executada com as permissões do usuário atual do computador local ou com a permissão do usuário especificado pelo parâmetro **/u** , se houver uma incluída. O parâmetro **/ru** é válido ao agendar tarefas em computadores locais ou remotos.


|       {1&gt;Valor&lt;1}        |                                                    Descrição                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<\]de > de domínio <User> |                                       Especifica uma conta de usuário alternativa.                                        |
|    Sistema ou     | Especifica a conta do sistema local, uma conta altamente privilegiada usada pelo sistema operacional e serviços do sistema. |

##### <a name="rp-password"></a>/RP \<senha >

Fornece a senha para a conta de usuário especificada no parâmetro **/ru** . Se você omitir esse parâmetro ao especificar uma conta de usuário, o **Schtasks. exe** solicitará a senha e obscurecerá o texto que você digitar.

Não use o parâmetro **/RP** para tarefas executadas com credenciais de conta do sistema ( **/ru System**). A conta do sistema não tem uma senha e **Schtasks. exe** não solicita uma.

##### <a name="mo-modifier"></a>/mo \<modificador >

Especifica com que frequência a tarefa é executada dentro de seu tipo de agendamento. Esse parâmetro é válido, mas opcional, por um agendamento de minuto, por hora, diário, semanal e mensal. O valor padrão é 1.

|Tipo de agendamento|Valores de modificador|Descrição|
|-------------|---------------|-----------|
|MINUTE|1 - 1439|A tarefa é executada a cada \<N > minutos.|
|POR hora|1 - 23|A tarefa é executada a cada \<N > horas.|
|DAILY|1 - 365|A tarefa é executada a cada \<N > dias.|
|QUINZENAL|1 - 52|A tarefa é executada a cada \<N > semanas.|
|MESMO|Nenhum modificador.|A tarefa é executada uma vez.|
|Star|Nenhum modificador.|A tarefa é executada na inicialização.|
|Onloginn|Nenhum modificador.|A tarefa é executada quando o usuário especificado pelo parâmetro **/u** faz logon.|
|AGENDA|Nenhum modificador.|A tarefa é executada Depois que o sistema está ocioso durante o número de minutos especificado pelo parâmetro **/i** , que é necessário para uso com OnIdle.|
|MENSAL|1 - 12|A tarefa é executada a cada \<N > meses.|
|MENSAL|LASTDAY|A tarefa é executada no último dia do mês.|
|MENSAL|PRIMEIRO, SEGUNDO, TERCEIRO, QUARTO, ÚLTIMO|Use com o parâmetro **/d**\<dia > para executar uma tarefa em uma semana e dia específicos. Por exemplo, na terceira quarta-feira do mês.|

##### <a name="d-dayday--"></a>/d dia [, dia...] | *

Especifica um dia (ou dias) da semana ou um dia (ou dias) de um mês. Válido somente com um agendamento semanal ou mensal.


| Tipo de agendamento |              Modificador              |     Valores de dia (/d)      |                                                                                                 Descrição                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    QUINZENAL     |               1 - 52               | MON-SOL [, MON-SOL...] |                                                                                                     \*                                                                                                      |
|    MENSAL    | PRIMEIRO, SEGUNDO, TERCEIRO, QUARTO, ÚLTIMO |        SEG-SOL         |                                                                                   Necessário para uma agenda de semana específica.                                                                                    |
|    MENSAL    |          Nenhum ou {1-12}          |          1 - 31          | Opcional e válido somente sem o parâmetro modificador ( **/mo**) (uma agenda de data específica) ou quando a **/mo** é 1-12 (uma agenda a cada \<N > meses). O padrão é dia 1 (o primeiro dia do mês). |

##### <a name="m-monthmonth"></a>/m mês [, mês...]

Especifica um mês ou meses do ano durante o qual a tarefa agendada deve ser executada. Os valores válidos são JAN-DEC e * (todos os meses). O parâmetro **/m** é válido somente com um agendamento mensal. É necessário quando o modificador LASTDAY é usado. Caso contrário, é opcional e o valor padrão é * (a cada mês).

##### <a name="i-idletime"></a>/i \<tempo ocioso >

Especifica quantos minutos o computador está ocioso antes que a tarefa seja iniciada. Um valor válido é um número inteiro de 1 a 999. Esse parâmetro é válido somente com um agendamento ONIDLE e, em seguida, é necessário.

##### <a name="st-starttime"></a>/St \<StartTime >

Especifica a hora do dia em que a tarefa é iniciada (cada vez que ela é iniciada) no formato \<HH: MM > 24 horas. O valor padrão é a hora atual no computador local. O parâmetro **/St** é válido com minutos, por hora, diariamente, semanalmente, mensalmente e em agendas. Ele é necessário para uma única agenda.

##### <a name="ri-interval"></a>/ri \<intervalo >

Especifica o intervalo de repetição em minutos. Isso não é aplicável para os tipos de agendamento: MINUTE, hora, OnStart, ONLOGON e ONIDLE. O intervalo válido é de 1 a 599940 minutos (599940 minutos = 9999 horas). Se/ET ou/DU for especificado, o intervalo de repetição padrão será de 10 minutos.

##### <a name="et-endtime"></a>/et \<EndTime >

Especifica a hora do dia em que uma agenda de tarefa de minuto ou hora termina em \<HH: MM > formato de 24 horas. Após a hora de término especificada, o **Schtasks** não inicia a tarefa novamente até que a hora de início ocorra. Por padrão, os agendamentos de tarefas não têm hora de término. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora.

Para obter um exemplo, consulte:
-   Para agendar uma tarefa que é executada a cada 100 minutos fora do horário comercial na seção **para agendar uma tarefa que executa a cada** \<N > **minutos** .

##### <a name="du-duration"></a>/du \<duração >

Especifica um período máximo de tempo para uma agenda de minuto ou hora em \<HHHH: MM > formato de 24 horas. Depois que o tempo especificado expira, o **Schtasks** não inicia a tarefa novamente até que a hora de início ocorra. Por padrão, os agendamentos de tarefas não têm duração máxima. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora.

Para obter um exemplo, consulte:
-   Para agendar uma tarefa que é executada a cada três horas por 10 horas na seção **para agendar uma tarefa que é executada a cada** \<N > de **horas** .

##### <a name="k"></a>/k

Interrompe o programa que a tarefa executa no momento especificado por **/et** ou **/du**. Sem **/k**, **Schtasks** não inicia o programa novamente depois de atingir o tempo especificado por **/et** ou **/du**, mas não interrompe o programa se ele ainda estiver em execução. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora.

Para obter um exemplo, consulte:
-   Para agendar uma tarefa que é executada a cada 100 minutos fora do horário comercial na seção **para agendar uma tarefa que executa a cada** \<N > **minutos** .

##### <a name="sd-startdate"></a>/SD \<StartDate >

Especifica a data na qual a agenda de tarefas é iniciada. O valor padrão é a data atual no computador local. O parâmetro **/SD** é válido e opcional para todos os tipos de agendamento.

O formato de *StartDate* varia com a localidade selecionada para o computador local em **Opções regionais e de idioma** no **painel de controle**. Apenas um formato é válido para cada localidade.

Os formatos de data válidos são listados na tabela a seguir. Use o formato mais semelhante ao formato selecionado para **data abreviada** em **Opções regionais e de idioma** no **painel de controle** no computador local.


|       {1&gt;Valor&lt;1}       |                                        Descrição                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | Use para formatos de primeiro mês, como **Inglês (Estados Unidos)** e **espanhol (Panamá)** . |
| \<DD >/<MM>/<YYYY> |       Use para formatos de primeiro dia, como **búlgaro** e **holandês (Países Baixos)** .        |
| \<aaaa >/<MM>/<DD> |          Use para formatos de primeiro ano, como **Sueco** e **francês (Canadá)** .          |

/Ed \<EndDate >

Especifica a data em que o agendamento termina. Esse parâmetro é opcional. Ele não é válido de uma vez, OnStart, ONLOGON ou agenda ONIDLE. Por padrão, os agendamentos não têm data de término.

O formato de *EndDate* varia com a localidade selecionada para o computador local em **Opções regionais e de idioma** no **painel de controle**. Apenas um formato é válido para cada localidade.

Os formatos de data válidos são listados na tabela a seguir. Use o formato mais semelhante ao formato selecionado para **data abreviada** em **Opções regionais e de idioma** no **painel de controle** no computador local.


|       {1&gt;Valor&lt;1}       |                                        Descrição                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | Use para formatos de primeiro mês, como **Inglês (Estados Unidos)** e **espanhol (Panamá)** . |
| \<DD >/<MM>/<YYYY> |       Use para formatos de primeiro dia, como **búlgaro** e **holandês (Países Baixos)** .        |
| \<aaaa >/<MM>/<DD> |          Use para formatos de primeiro ano, como **Sueco** e **francês (Canadá)** .          |

##### <a name="it"></a>/It

Especifica a execução da tarefa somente quando o usuário executar como (a conta de usuário sob a qual a tarefa é executada) está conectado ao computador. Esse parâmetro não tem nenhum efeito nas tarefas executadas com permissões do sistema.

Por padrão, o usuário executar como é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pelo parâmetro **/u** , se uma for usada. No entanto, se o comando incluir o parâmetro **/ru** , o usuário executar como será a conta especificada pelo parâmetro **/ru** .

Para obter exemplos, consulte:
-   Para agendar uma tarefa que é executada a cada 70 dias, se eu estiver conectado na seção **para agendar uma tarefa que é executada a cada** *N* **dias** .
-   Executar uma tarefa somente quando um usuário específico estiver conectado na seção **para agendar uma tarefa que é executada com permissões diferentes** .

##### <a name="z"></a>/z

Especifica a exclusão da tarefa após a conclusão de sua agenda.

##### <a name="f"></a>/f

Especifica para criar a tarefa e suprimir avisos se a tarefa especificada já existir.

##### <a name=""></a>/?

Exibe a ajuda no prompt de comando.

### <a name="to-schedule-a-task-that-runs-every-n-minutes"></a><a name=BKMK_minutes></a>Para agendar uma tarefa que é executada a cada N minutos

#### <a name="minute-schedule-syntax"></a>Sintaxe de agendamento de minutos

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um cronograma de minutos, o parâmetro **/SC minute** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de minutos entre cada execução da tarefa. O valor padrão de **/mo** é 1 (a cada minuto). Os parâmetros **/et** (End Time) e **/du** (Duration) são opcionais e podem ser usados com ou sem o parâmetro **/k** (end Task).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Para agendar uma tarefa que é executada a cada 20 minutos

O comando a seguir agenda um script de segurança, SEC. vbs, para ser executado a cada 20 minutos. O comando usa o parâmetro **/SC** para especificar um agendamento de minuto e o parâmetro **/mo** para especificar um intervalo de 20 minutos.

Como o comando não inclui uma data ou hora de início, a tarefa inicia 20 minutos após a conclusão do comando e é executada a cada 20 minutos após a execução do sistema. Observe que o arquivo de origem do script de segurança está localizado em um computador remoto, mas que a tarefa está agendada e executada no computador local.
```
schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Para agendar uma tarefa que é executada a cada 100 minutos fora do horário comercial

O comando a seguir agenda um script de segurança, SEC. vbs, para ser executado no computador local a cada 100 minutos entre 5:00 P.M. e 7:59 A.M. todos os dias. O comando usa o parâmetro **/SC** para especificar um agendamento de minuto e o parâmetro **/mo** para especificar um intervalo de 100 minutos. Ele usa os parâmetros **/St** e **/et** para especificar a hora de início e a hora de término da agenda de cada dia. Ele também usa o parâmetro **/k** para interromper o script se ele ainda estiver em execução às 7:59 A.M. Sem **/k**, **Schtasks** não iniciaria o script após 7:59 a.m., mas se a instância fosse iniciada às 6:20 a.m. ainda estava em execução, ele não o interromperia.
```
schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="to-schedule-a-task-that-runs-every-n-hours"></a><a name=BKMK_hours></a>Para agendar uma tarefa que é executada a cada N horas

#### <a name="hourly-schedule-syntax"></a>Sintaxe de agendamento por hora

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em uma agenda por hora, o parâmetro **/SC Hourly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de horas entre cada execução da tarefa. O valor padrão de **/mo** é 1 (a cada hora). O parâmetro **/k** (end Task) é opcional e pode ser usado com **/et** (end no horário especificado) ou **/du** (terminar após o intervalo especificado).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Para agendar uma tarefa que é executada a cada cinco horas

O comando a seguir agenda o programa MyApp para ser executado a cada cinco horas, começando no primeiro dia de março de 2002. Ele usa o parâmetro **/mo** para especificar o intervalo e o parâmetro **/SD** para especificar a data de início. Como o comando não especifica uma hora de início, a hora atual é usada como a hora de início.

Como o computador local está definido para usar a opção em **Inglês (Zimbábue)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de início é MM/DD/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Para agendar uma tarefa que é executada a cada hora em cinco minutos após a hora

O comando a seguir agenda o programa MyApp para ser executado a cada hora, começando em cinco minutos após a meia-noite. Como o parâmetro **/mo** é omitido, o comando usa o valor padrão para o agendamento por hora, que é a cada (1) hora. Se esse comando for executado após 12:05 A.M., o programa não será executado até o dia seguinte.
```
schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Para agendar uma tarefa que é executada a cada três horas por 10 horas

O comando a seguir agenda o programa MyApp para ser executado a cada 3 horas por 10 horas.

O comando usa o parâmetro **/SC** para especificar um agendamento por hora e o parâmetro **/mo** para especificar o intervalo de 3 horas. Ele usa o parâmetro **/St** para iniciar a agenda à meia-noite e o parâmetro **/du** para encerrar as recorrências após 10 horas. Como o programa é executado por apenas alguns minutos, o parâmetro **/k** , que interrompe o programa, se ainda estiver em execução quando a duração expirar, não é necessário.
```
schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
Neste exemplo, a tarefa é executada às 12:00, às 3:00, às 6:00 A.M. e às 9:00. Como a duração é de 10 horas, a tarefa não é executada novamente às 12:00 Em vez disso, ele começa novamente às 12:00 da manhã no dia seguinte.

### <a name="to-schedule-a-task-that-runs-every-n-days"></a><a name=BKMK_days></a>Para agendar uma tarefa que é executada a cada N dias

#### <a name="daily-schedule-syntax"></a>Sintaxe de agenda diária

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um agendamento diário, o parâmetro **/SC Daily** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de dias entre cada execução da tarefa. O valor padrão de **/mo** é 1 (todos os dias).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Para agendar uma tarefa que é executada todos os dias

O exemplo a seguir agenda o programa MyApp para ser executado uma vez por dia, todos os dias, às 8:00 da manhã até 31 de dezembro de 2002. Como ele omite o parâmetro **/mo** , o intervalo padrão de 1 é usado para executar o comando todos os dias.

Neste exemplo, como o sistema de computador local está definido como a opção **Inglês (Reino Unido)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de término é DD/MM/AAAA (31/12/2002)
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Para agendar uma tarefa que é executada a cada 12 dias

O exemplo a seguir agenda o programa MyApp para ser executado a cada doze dias às 1:00 (13:00) a partir de 31 de dezembro de 2002. O comando usa o parâmetro **/mo** para especificar um intervalo de dois (2) dias e os parâmetros **/SD** e **/St** para especificar a data e a hora.

Neste exemplo, como o sistema está definido como a opção em **Inglês (Zimbábue)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de término é MM/DD/AAAA (12/31/2002)
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Para agendar uma tarefa que é executada a cada 70 dias se eu estiver conectado

O comando a seguir agenda um script de segurança, SEC. vbs, para ser executado a cada 70 dias. O comando usa o parâmetro **/mo** para especificar um intervalo de 70 dias. Ele também usa o parâmetro **/it** para especificar que a tarefa seja executada somente quando o usuário sob a conta em que a tarefa é executada é registrado no computador. Como a tarefa será executada com as permissões da minha conta de usuário, a tarefa será executada somente quando eu estiver conectado.
```
schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Para identificar tarefas com a propriedade somente interativa ( **/it**), use uma consulta detalhada **(/Query/v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o campo de **modo de logon** tem um valor **somente interativo**.

### <a name="to-schedule-a-task-that-runs-every-n-weeks"></a><a name=BKMK_weeks></a>Para agendar uma tarefa que é executada a cada N semanas

#### <a name="weekly-schedule-syntax"></a>Sintaxe de agendamento semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em uma agenda semanal, o parâmetro **/SC Weekly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão de **/mo** é 1 (a cada semana).

Os agendamentos semanais também têm um parâmetro opcional **/d** para agendar a execução da tarefa em dias da semana especificados ou em todos os dias ( *). O padrão é MON (segunda-feira). A opção todos os dias (* ) é equivalente ao agendamento de uma tarefa diária.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Para agendar uma tarefa que é executada a cada seis semanas

O comando a seguir agenda o programa MyApp para ser executado em um computador remoto a cada seis semanas. O comando usa o parâmetro **/mo** para especificar o intervalo. Como o comando omite o parâmetro **/d** , a tarefa é executada nas segundas-feiras.

Esse comando também usa o parâmetro **/s** para especificar o computador remoto e o parâmetro **/u** para executar o comando com as permissões da conta de administrador do usuário. Como o parâmetro **/p** é omitido, o **Schtasks. exe** solicita ao usuário a senha da conta de administrador.

Além disso, como o comando é executado remotamente, todos os caminhos no comando, incluindo o caminho para MyApp. exe, se referem a caminhos no computador remoto.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Para agendar uma tarefa que é executada a cada semana na sexta-feira

O comando a seguir agenda uma tarefa para ser executada a cada sexta-feira. Ele usa o parâmetro **/mo** para especificar o intervalo de duas semanas e o parâmetro **/d** para especificar o dia da semana. Para agendar uma tarefa que é executada toda sexta-feira, omita o parâmetro **/mo** ou defina-o como 1.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="to-schedule-a-task-that-runs-every-n-months"></a><a name=BKMK_months></a>Para agendar uma tarefa que é executada a cada N meses

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Nesse tipo de agendamento, o parâmetro ' **/SC Monthly** ' é necessário. O parâmetro **/mo** (modificador), que especifica o número de meses entre cada execução da tarefa, é opcional e o padrão é 1 (todos os meses). Esse tipo de agendamento também tem um parâmetro opcional **/d** para agendar a tarefa a ser executada em uma data especificada do mês. O padrão é 1 (o primeiro dia do mês).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Para agendar uma tarefa que é executada no primeiro dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no primeiro dia de cada mês. Como um valor de 1 é o padrão para o parâmetro **/mo** (modificador) e o parâmetro **/d** (Day), esses parâmetros são omitidos do comando.
```
schtasks /create /tn My App /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Para agendar uma tarefa que é executada a cada três meses

O comando a seguir agenda o programa MyApp para ser executado a cada três meses. Ele usa o parâmetro **/mo** para especificar o intervalo.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Para agendar uma tarefa que é executada à meia-noite no 21 dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado todos os outros meses no dia 21 do mês à meia-noite. O comando especifica que essa tarefa deve ser executada por um ano, de 2 de julho de 2002 a 30 de junho de 2003.

O comando usa o parâmetro **/mo** para especificar o intervalo mensal (a cada dois meses), o parâmetro **/d** para especificar a data e **/St** para especificar a hora. Ele também usa os parâmetros **/SD** e **/Ed** para especificar a data de início e a data de término, respectivamente. Como o computador local é definido como a opção **Inglês (África do Sul)** nas **Opções regionais e de idioma** no **painel de controle**, as datas são especificadas no formato local, aaaa/mm/dd.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-day-of-the-week"></a><a name=BKMK_spec_day></a>Para agendar uma tarefa que é executada em um dia específico da semana

#### <a name="weekly-schedule-syntax"></a>Sintaxe de agendamento semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

A agenda do dia da semana é uma variação da agenda semanal. Em uma agenda semanal, o parâmetro **/SC Weekly** é necessário. O parâmetro **/mo** (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão de **/mo** é 1 (a cada semana). O parâmetro **/d** , que é opcional, agenda a tarefa para execução em dias da semana especificados ou em todos os dias (\*). O padrão é MON (segunda-feira). A opção de todos os dias ( **/d \*** ) é equivalente a agendar uma tarefa diária.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Para agendar uma tarefa que é executada todas as quartas-feiras

O comando a seguir agenda o programa MyApp para ser executado todas as semanas na quarta-feira. O comando usa o parâmetro **/d** para especificar o dia da semana. Como o comando omite o parâmetro **/mo** , a tarefa é executada todas as semanas.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Para agendar uma tarefa que é executada a cada oito semanas na segunda-feira e na sexta-feira

O comando a seguir agenda uma tarefa para ser executada na segunda-feira e na sexta-feira de cada oitava semana. Ele usa o parâmetro **/d** para especificar os dias e o parâmetro **/mo** para especificar o intervalo de oito semanas.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-week-of-the-month"></a><a name=BKMK_spec_week></a>Para agendar uma tarefa que é executada em uma semana específica do mês

#### <a name="specific-week-syntax"></a>Sintaxe da semana específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Nesse tipo de agendamento, o parâmetro **/SC mensal** , o parâmetro **/mo** (modificador) e o parâmetro **/d** (Day) são necessários. O parâmetro **/mo** (modificador) especifica a semana em que a tarefa é executada. O parâmetro **/d** especifica o dia da semana. (Você pode especificar apenas um dia da semana para esse tipo de agendamento.) Esse agendamento também tem um parâmetro de **/m** (mês) opcional que permite agendar a tarefa para determinados meses ou a cada mês (\*). O padrão para o parâmetro **/m** é todos os meses (\*).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Para agendar uma tarefa para o segundo domingo de cada mês

O comando a seguir agenda o programa MyApp para ser executado no segundo domingo de cada mês. Ele usa o parâmetro **/mo** para especificar a segunda semana do mês e o parâmetro **/d** para especificar o dia.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Para agendar uma tarefa para a primeira segunda-feira em março e setembro

O comando a seguir agenda o programa MyApp para ser executado na primeira segunda-feira em março e setembro. Ele usa o parâmetro **/mo** para especificar a primeira semana do mês e o parâmetro **/d** para especificar o dia. Ele usa o parâmetro **/m** para especificar o mês, separando os argumentos de mês com uma vírgula.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-date-each-month"></a><a name=BKMK_spec_date></a>Para agendar uma tarefa que é executada em uma data específica a cada mês

#### <a name="specific-date-syntax"></a>Sintaxe de data específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento de data específico, o parâmetro **/SC Monthly** e o parâmetro **/d** (Day) são necessários. O parâmetro **/d** especifica uma data do mês (1-31), não um dia da semana. Você pode especificar apenas um dia na agenda. O parâmetro **/mo** (modificador) não é válido com esse tipo de agendamento.

O parâmetro **/m** (month) é opcional para esse tipo de agendamento, e o padrão é todos os meses (<em>). **Schtasks</em>*  não permite agendar uma tarefa para uma data que não ocorra em um mês especificado pelo parâmetro **/m** . No entanto, se omitir o parâmetro **/m** e agendar uma tarefa para uma data que não apareça em todos os meses, como o dia 31, a tarefa não será executada nos meses mais curtos. Para agendar uma tarefa para o último dia do mês, use o tipo de agendamento do último dia.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Para agendar uma tarefa para o primeiro dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no primeiro dia de cada mês. Como o modificador padrão é nenhum (nenhum modificador), o dia padrão é o dia 1 e o mês padrão é todos os meses, o comando não precisa de nenhum parâmetro adicional.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Para agendar uma tarefa nos 15º dias de maio e junho

O comando a seguir agenda o programa MyApp para ser executado em 15 de maio e 15 de junho às 3:00 P.M. (15:00). Ele usa o parâmetro **/m** para especificar a data e o parâmetro **/m** para especificar os meses. Ele também usa o parâmetro **/St** para especificar a hora de início.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="to-schedule-a-task-that-runs-on-the-last-day-of-a-month"></a><a name=BKMK_last_day></a>Para agendar uma tarefa que é executada no último dia de um mês

#### <a name="last-day-syntax"></a>Sintaxe do último dia

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento do último dia, o parâmetro **/SC Monthly** , o parâmetro **/mo lastDay** (modificador) e o parâmetro **/m** (month) são necessários. O parâmetro **/d** (Day) não é válido.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Para agendar uma tarefa para o último dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no último dia de cada mês. Ele usa o parâmetro **/mo** para especificar o último dia e o parâmetro **/m** com o caractere curinga (*) para indicar que o programa é executado todos os meses.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Para agendar uma tarefa às 6:00 nos últimos dias de fevereiro e março

O comando a seguir agenda o programa MyApp para ser executado no último dia de fevereiro e no último dia de março às 6:00 P.M. Ele usa o parâmetro **/mo** para especificar o último dia, o parâmetro **/m** para especificar os meses e o parâmetro **/St** para especificar a hora de início.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="to-schedule-a-task-that-runs-once"></a><a name=BKMK_once></a>Para agendar uma tarefa que é executada uma vez

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento de execução única, o parâmetro **/sc once** é necessário. O parâmetro **/St** , que especifica a hora em que a tarefa é executada, é necessário. O parâmetro **/SD** , que especifica a data em que a tarefa é executada, é opcional. Os parâmetros **/mo** (modificador) e **/Ed** (data de término) não são válidos para esse tipo de agendamento.

**Schtasks** não permite que você agende uma tarefa para ser executada uma vez se a data e hora especificadas estiverem no passado, com base na hora do computador local. Para agendar uma tarefa que é executada uma vez em um computador remoto em um fuso horário diferente, você deve agendá-la antes que essa data e hora ocorra no computador local.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Para agendar uma tarefa que é executada uma vez

O comando a seguir agenda o programa MyApp para ser executado à meia-noite em 1º de janeiro de 2003. Ele usa o parâmetro **/SC** para especificar o tipo de agendamento e **/SD** e **St** para especificar a data e a hora.

Como o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de início é mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="to-schedule-a-task-that-runs-every-time-the-system-starts"></a><a name=BKMK_startup></a>Para agendar uma tarefa que é executada toda vez que o sistema é iniciado

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento de início, o parâmetro **/SC OnStart** é necessário. O parâmetro **/SD** (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Para agendar uma tarefa que é executada quando o sistema é iniciado

O comando a seguir agenda o programa MyApp para ser executado toda vez que o sistema for iniciado, começando em 15 de março de 2001:

Como o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de início é mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on"></a><a name=BKMK_logon></a>Para agendar uma tarefa que é executada quando um usuário faz logon

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

O tipo de agendamento no logon agenda uma tarefa que é executada sempre que qualquer usuário faz logon no computador. No tipo de agendamento de logon, o parâmetro **/SC ONLOGON** é necessário. O parâmetro **/SD** (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Para agendar uma tarefa que é executada quando um usuário faz logon em um computador remoto

O comando a seguir agenda um arquivo em lotes a ser executado toda vez que um usuário (qualquer usuário) fizer logon no computador remoto. Ele usa o parâmetro **/s** para especificar o computador remoto. Como o comando é remoto, todos os caminhos no comando, incluindo o caminho para o arquivo em lotes, se referem a um caminho no computador remoto.
```
schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="to-schedule-a-task-that-runs-when-the-system-is-idle"></a><a name=BKMK_idle></a>Para agendar uma tarefa que é executada quando o sistema está ocioso

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

O tipo de agendamento on Idle agenda uma tarefa que é executada sempre que não há atividade de usuário durante o tempo especificado pelo parâmetro **/i** . No tipo de agendamento inativo, o parâmetro **/SC OnIdle** e o parâmetro **/i** são necessários. O **/SD** (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Para agendar uma tarefa que é executada sempre que o computador está ocioso

O comando a seguir agenda o programa MyApp para ser executado sempre que o computador estiver ocioso. Ele usa o parâmetro **/i** necessário para especificar que o computador deve permanecer ocioso por dez minutos antes que a tarefa seja iniciada.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="to-schedule-a-task-that-runs-now"></a><a name=BKMK_now></a>Para agendar uma tarefa que é executada agora

**Schtasks** não tem uma opção executar agora, mas você pode simular essa opção criando uma tarefa que é executada uma vez e inicia em alguns minutos.

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Para agendar uma tarefa que executa alguns minutos a partir de agora.

O comando a seguir agenda uma tarefa para ser executada uma vez, em 13 de novembro de 2002 às 2:18 P.M. Hora local.

Como o computador local usa a opção **Inglês (Estados Unidos)** em **Opções regionais e de idioma** no **painel de controle**, o formato da data de início é mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="to-schedule-a-task-that-runs-with-different-permissions"></a><a name=BKMK_diff_perms></a>Para agendar uma tarefa que é executada com permissões diferentes

Você pode agendar tarefas de todos os tipos para execução com permissões de uma conta alternativa no computador local e em um remoto. Além dos parâmetros necessários para o tipo de agendamento específico, o parâmetro **/ru** é necessário e o parâmetro **/RP** é opcional.

#### <a name="examples"></a>Exemplos

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Para executar uma tarefa com permissões de administrador no computador local

O comando a seguir agenda o programa MyApp para ser executado no computador local. Ele usa a **/ru** para especificar que a tarefa deve ser executada com as permissões da conta de administrador do usuário (Admin06). Neste exemplo, a tarefa está agendada para ser executada todas as terças-feiras, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões alternativas.
```
schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
Em resposta, o **Schtasks. exe** solicita a senha executar como para a conta Admin06 e, em seguida, exibe uma mensagem de êxito.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Para executar uma tarefa com permissões alternativas em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador de marketing a cada quatro dias.

O comando usa o parâmetro **/SC** para especificar um agendamento diário e um parâmetro **/mo** para especificar um intervalo de quatro dias.

O comando usa o parâmetro **/s** para fornecer o nome do computador remoto e o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto (Admin01 no computador de marketing). Ele também usa o parâmetro **/ru** para especificar que a tarefa deve ser executada com as permissões da conta de não administrador do usuário (User01 no domínio Reskits). Sem o parâmetro **/ru** , a tarefa seria executada com as permissões da conta especificada por **/u**.
```
schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
O **Schtasks** primeiro solicita a senha do usuário chamado pelo parâmetro **/u** (para executar o comando) e, em seguida, solicita a senha do usuário chamado pelo parâmetro **/ru** (para executar a tarefa). Depois de autenticar as senhas, **Schtasks** exibe uma mensagem indicando que a tarefa está agendada.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Para executar uma tarefa somente quando um usuário específico tiver feito logon

O comando a seguir agenda o programa AdminCheck. exe para ser executado no computador público toda sexta-feira às 4:00 A.M., mas somente se o administrador do computador estiver conectado.

O comando usa o parâmetro **/SC** para especificar uma agenda semanal, o parâmetro **/d** para especificar o dia e o parâmetro **/St** para especificar a hora de início.

O comando usa o parâmetro **/s** para fornecer o nome do computador remoto e o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto. Ele também usa o parâmetro **/ru** para configurar a tarefa a ser executada com as permissões do administrador do computador público (Public\Admin01) e o parâmetro **/it** para indicar que a tarefa é executada somente quando a conta Public\Admin01 está conectada.
```
schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Observação**
-   Para identificar tarefas com a propriedade somente interativa ( **/it**), use uma consulta detalhada **(/Query/v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o campo de **modo de logon** tem um valor **somente interativo**.

### <a name="to-schedule-a-task-that-runs-with-system-permissions"></a><a name=BKMK_sys_perms></a>Para agendar uma tarefa que é executada com permissões do sistema

Tarefas de todos os tipos podem ser executadas com permissões da conta do sistema tanto no computador local quanto em um remoto. Além dos parâmetros necessários para o tipo de agendamento específico, o parâmetro do **sistema/ru** (ou * */ru * *) é necessário e o parâmetro **/RP** não é válido.

**Importante**
-   A conta do sistema não tem direitos de logon interativos. Os usuários não podem ver ou interagir com programas ou tarefas executadas com permissões do sistema.
-   O parâmetro **/ru** determina as permissões sob as quais a tarefa é executada, não as permissões usadas para agendar a tarefa. Somente os administradores podem agendar tarefas, independentemente do valor do parâmetro **/ru** .

**Observação**

Para identificar tarefas que são executadas com permissões do sistema, use uma consulta detalhada ( **/Query** **/v**). Em uma exibição de consulta detalhada de uma tarefa de execução do sistema, o campo **Executar como usuário** tem um valor de **NT AUTHORITY\SYSTEM** e o campo **modo de logon** tem apenas um valor de **segundo plano**.

#### <a name="examples"></a>Exemplos

#### <a name="to-run-a-task-with-system-permissions"></a>Para executar uma tarefa com permissões do sistema

O comando a seguir agenda o programa MyApp para ser executado no computador local com permissões da conta do sistema. Neste exemplo, a tarefa está agendada para ser executada no décimo-quinto dia de cada mês, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões do sistema.

O comando usa o parâmetro de **sistema/ru** para especificar o contexto de segurança do sistema. Como as tarefas do sistema não usam uma senha, o parâmetro **/RP** é omitido.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
Em resposta, o **Schtasks. exe** exibe uma mensagem informativa e uma mensagem de êxito. Ele não solicita uma senha.
```
INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
SUCCESS: The Scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Para executar uma tarefa com permissões do sistema em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador Finance01 todas as manhãs às 4:00 A.M. com permissões do sistema.

O comando usa o parâmetro **/TN** para nomear a tarefa e o parâmetro **/TR** para especificar a cópia remota do programa MyApp. Ele usa o parâmetro **/SC** para especificar um agendamento diário, mas omite o parâmetro **/mo** porque 1 (todos os dias) é o padrão. Ele usa o parâmetro **/St** para especificar a hora de início, que também é a hora em que a tarefa será executada todos os dias.

O comando usa o parâmetro **/s** para fornecer o nome do computador remoto e o parâmetro **/u** para especificar uma conta com permissão para agendar uma tarefa no computador remoto. Ele também usa o parâmetro **/ru** para especificar que a tarefa deve ser executada na conta do sistema. Sem o parâmetro **/ru** , a tarefa seria executada com as permissões da conta especificada por **/u**.
```
schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks** solicita a senha do usuário nomeada pelo parâmetro **/u** e, depois de autenticar a senha, exibe uma mensagem indicando que a tarefa foi criada e que será executada com permissões da conta do sistema.
```
Type the password for Admin01:**********

INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
SYSTEM).
SUCCESS: The scheduled task My App has successfully been created.
```

### <a name="to-schedule-a-task-that-runs-more-than-one-program"></a><a name=BKMK_multi_progs></a>Para agendar uma tarefa que executa mais de um programa

Cada tarefa executa apenas um programa. No entanto, você pode criar um arquivo em lotes que executa vários programas e, em seguida, agendar uma tarefa para executar o arquivo em lotes. O procedimento a seguir demonstra esse método:
1. Crie um arquivo em lotes que inicie os programas que você deseja executar.

   Neste exemplo, você cria um arquivo em lotes que inicia Visualizador de Eventos (eventvwr. exe) e o monitor do sistema (Perfmon. exe).  
   - Abra um editor de texto, como o Bloco de Notas.
   - Digite o nome e o caminho totalmente qualificado para o arquivo executável para cada programa. Nesse caso, o arquivo inclui as instruções a seguir.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Salve o arquivo como myapps. bat.
2. Use **Schtasks. exe** para criar uma tarefa que executa myapps. bat.

   O comando a seguir cria a tarefa monitor, que é executada sempre que alguém faz logon. Ele usa o parâmetro **/TN** para nomear a tarefa e o parâmetro **/TR** para executar myapps. bat. Ele usa o parâmetro **/SC** para indicar o tipo de agendamento onloginn e o parâmetro **/ru** para executar a tarefa com as permissões da conta de administrador do usuário.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   Como resultado desse comando, sempre que um usuário fizer logon no computador, a tarefa iniciará o Visualizador de Eventos e o monitor do sistema.

### <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a><a name=BKMK_remote></a>Para agendar uma tarefa que é executada em um computador remoto

Para agendar uma tarefa para ser executada em um computador remoto, você deve adicionar a tarefa à agenda do computador remoto. Tarefas de todos os tipos podem ser agendadas em um computador remoto, mas as condições a seguir devem ser atendidas.
-   Você deve ter permissão para agendar a tarefa. Dessa forma, você deve estar conectado ao computador local com uma conta que seja membro do grupo Administradores no computador remoto, ou deve usar o parâmetro **/u** para fornecer as credenciais de um administrador do computador remoto.
-   Você pode usar o parâmetro **/u** somente quando os computadores locais e remotos estiverem no mesmo domínio ou o computador local estiver em um domínio no qual o domínio do computador remoto confia. Caso contrário, o computador remoto não poderá autenticar a conta de usuário especificada e não poderá verificar se a conta é membro do grupo Administradores.
-   A tarefa deve ter permissão suficiente para ser executada no computador remoto. As permissões necessárias variam de acordo com a tarefa. Por padrão, a tarefa é executada com a permissão do usuário atual do computador local ou, se o parâmetro **/u** for usado, a tarefa será executada com a permissão da conta especificada pelo parâmetro **/u** . No entanto, você pode usar o parâmetro **/ru** para executar a tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema.

#### <a name="examples"></a>Exemplos

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Um administrador agenda uma tarefa em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV01 a cada dez dias, começando imediatamente. O comando usa o parâmetro **/s** para fornecer o nome do computador remoto. Como o usuário atual local é um administrador do computador remoto, o parâmetro **/u** , que fornece permissões alternativas para o agendamento da tarefa, não é necessário.

Observe que, ao agendar tarefas em um computador remoto, todos os parâmetros se referem ao computador remoto. Portanto, o arquivo executável especificado pelo parâmetro **/TR** refere-se à cópia do MyApp. exe no computador remoto.
```
schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
```
Em resposta, **Schtasks** exibe uma mensagem de êxito indicando que a tarefa está agendada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Um usuário agenda um comando em um computador remoto (caso 1)

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV06 a cada três horas. Como as permissões de administrador são necessárias para agendar uma tarefa, o comando usa os parâmetros **/u** e **/p** para fornecer as credenciais da conta de administrador do usuário (Admin01 no domínio Reskits). Por padrão, essas permissões também são usadas para executar a tarefa. No entanto, como a tarefa não precisa de permissões de administrador para ser executada, o comando inclui os parâmetros **/u** e **/RP** para substituir o padrão e executar a tarefa com permissão de conta de não administrador do usuário no computador remoto.
```
schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
Em resposta, **Schtasks** exibe uma mensagem de êxito indicando que a tarefa está agendada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Um usuário agenda um comando em um computador remoto (caso 2)

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV02 no último dia de cada mês. Como o usuário atual local (user03) não é um administrador do computador remoto, o comando usa o parâmetro **/u** para fornecer as credenciais da conta de administrador do usuário (Admin01 no domínio Reskits). As permissões de conta de administrador serão usadas para agendar a tarefa e executar a tarefa.
```
schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Como o comando não incluiu o parâmetro **/p** (password), **Schtasks** solicita a senha. Em seguida, ele exibe uma mensagem de êxito e, nesse caso, um aviso.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task My App has successfully been created.

WARNING: The Scheduled task My App has been created, but may not run because
the account information could not be set.
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

-   Para executar um comando **/Create** com as permissões de um usuário diferente, use o parâmetro **/u** . O parâmetro **/u** é válido somente para tarefas de agendamento em computadores remotos.
-   Para exibir mais exemplos de **schtasks/create** , digite **Schtasks/?** em um prompt de comando.
-   Para agendar uma tarefa que é executada com permissões de um usuário diferente, use o parâmetro **/ru** . O parâmetro **/ru** é válido para tarefas em computadores locais e remotos.
-   Para usar o parâmetro **/u** , o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio no qual o domínio do computador remoto confia. Caso contrário, a tarefa não será criada ou o trabalho de tarefa estará vazio e a tarefa não será executada.
-   **Schtasks** sempre solicita uma senha, a menos que você forneça uma, mesmo quando você agenda uma tarefa no computador local usando a conta de usuário atual. Esse comportamento é normal para **Schtasks**.
-   **Schtasks** não verifica locais de arquivos de programas ou senhas de contas de usuário. Se você não inserir o local de arquivo correto ou a senha correta para a conta de usuário, a tarefa será criada, mas não executará. Além disso, se a senha de uma conta for alterada ou expirar e você não alterar a senha salva na tarefa, a tarefa não será executada.
-   A conta do sistema não tem direitos de logon interativos. Os usuários não veem e não podem interagir com programas executados com permissões do sistema.
-   Cada tarefa executa apenas um programa. No entanto, você pode criar um arquivo em lotes que inicia várias tarefas e, em seguida, agendar uma tarefa que executa o arquivo em lotes.
-   Você pode testar uma tarefa assim que criá-la. Use a operação **executar** para testar a tarefa e, em seguida, verifique se há erros no arquivo SchedLgU. txt (*systemroot*\SchedLgU.txt).

## <a name="schtasks-change"></a><a name=BKMK_change></a>troca de Schtasks

Altera uma ou mais das propriedades a seguir de uma tarefa.
-   O programa que a tarefa executa ( **/TR**).
-   A conta de usuário sob a qual a tarefa é executada ( **/ru**).
-   A senha da conta de usuário ( **/RP**).
-   Adiciona a propriedade somente interativa à tarefa ( **/it**).

### <a name="syntax"></a>Sintaxe

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

#### <a name="parameters"></a>Parâmetros

|          Termo           |                                                                                                                                                                                                                                                                                                                                     Definição                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /TN \<Nome_tarefa >     |                                                                                                                                                                                                                                                                                                               Identifica a tarefa a ser alterada. Insira o nome da tarefa.                                                                                                                                                                                                                                                                                                               |
|     /s \<computador >      |                                                                                                                                                                                                                                                                               Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                                                                                                                                                                                               |
|  /u [\<\]de domínio > <User>  |                                                                                                                                                                 Executa este comando com as permissões da conta de usuário especificada. O padrão é as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente para alterar uma tarefa em um computador remoto ( **/s**).                                                                                                                                                                  |
|     /p \<senha >      |                                                                                                                                                                                              Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** , mas omitir o parâmetro **/p** ou o argumento password, **Schtasks** solicitará uma senha.</br>Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.                                                                                                                                                                                               |
| /ru {[\<\]de domínio > <User> |                                                                                                                                                                                                                                                                                                                                       Sistema                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<senha >     |                                                                                                                                                                                                                                                 Especifica uma nova senha para a conta de usuário existente ou a conta de usuário especificada pelo parâmetro **/ru** . Esse parâmetro é ignorado com o usado com a conta sistema local.                                                                                                                                                                                                                                                  |
|     /TR \<TaskRun >      |                                                                                                                                                                                  Altera o programa que a tarefa executa. Insira o caminho totalmente qualificado e o nome de arquivo de um arquivo executável, arquivo de script ou arquivo em lotes. Se você omitir o caminho, **Schtasks** assumirá que o arquivo está no diretório \<SystemRoot > \System32 O programa especificado substitui o programa original executado pela tarefa.                                                                                                                                                                                  |
|    /St \<StartTime >     |                                                                                                                                                                                                                                                              Especifica a hora de início para a tarefa, usando o formato de 24 horas, HH: mm. Por exemplo, um valor de 14:30 é equivalente ao tempo de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                               |
|     /ri \<intervalo >     |                                                                                                                                                                                                                                                                           Especifica o intervalo de repetição para a tarefa agendada, em minutos. O intervalo válido é 1-599940 (599940 minutos = 9999 horas).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               Especifica a hora de término da tarefa, usando o formato de 24 horas, HH: mm. Por exemplo, um valor de 14:30 é equivalente ao tempo de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                                |
|     /du \<duração >     |                                                                                                                                                                                                                                                                                                     Especifica para fechar a tarefa no \<EndTime > ou <Duration>, se especificado.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Interrompe o programa que a tarefa executa no momento especificado por **/et** ou **/du**. Sem **/k**, **Schtasks** não inicia o programa novamente depois de atingir o tempo especificado por **/et** ou **/du**, mas não interrompe o programa se ele ainda estiver em execução. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora.                                                                                                                                                                   |
|    /SD \<StartDate >     |                                                                                                                                                                                                                                                                                              Especifica a primeira data em que a tarefa deve ser executada. O formato de data é MM/DD/AAAA.                                                                                                                                                                                                                                                                                               |
|     /Ed \<EndDate >      |                                                                                                                                                                                                                                                                                                 Especifica a última data em que a tarefa deve ser executada. O formato é MM/DD/AAAA.                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       Especifica a habilitação da tarefa agendada.                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      Especifica a desabilitação da tarefa agendada.                                                                                                                                                                                                                                                                                                                       |
|           /It           | Especifica a execução da tarefa agendada somente quando o usuário executar como (a conta de usuário sob a qual a tarefa é executada) está conectado ao computador.</br>Esse parâmetro não tem nenhum efeito nas tarefas que são executadas com permissões do sistema ou tarefas que já têm a propriedade somente interativa definida. Você não pode usar um comando Change para remover a propriedade somente interativa de uma tarefa.</br>Por padrão, o usuário executar como é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pelo parâmetro **/u** , se uma for usada. No entanto, se o comando incluir o parâmetro **/ru** , o usuário executar como será a conta especificada pelo parâmetro **/ru** . |
|           /z            |                                                                                                                                                                                                                                                                                                          Especifica a exclusão da tarefa após a conclusão de sua agenda.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Comentários

-   Os parâmetros **/TN** e **/s** identificam a tarefa. Os parâmetros **/TR**, **/ru**e **/RP** especificam as propriedades da tarefa que você pode alterar.
-   Os parâmetros **/ru**e **/RP** especificam as permissões sob as quais a tarefa é executada. Os parâmetros **/u** e **/p** especificam as permissões usadas para alterar a tarefa.
-   Para alterar tarefas em um computador remoto, o usuário deve estar conectado ao computador local com uma conta que seja membro do grupo de administradores no computador remoto.
-   Para executar um comando **/Change** com as permissões de um usuário diferente ( **/u**, **/p**), o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio no qual o domínio do computador remoto confia.
-   A conta do sistema não tem direitos de logon interativos. Os usuários não veem e não podem interagir com programas executados com permissões do sistema.
-   Para identificar tarefas com a propriedade **/it** , use uma consulta detalhada ( **/Query/v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o campo de **modo de logon** tem um valor **somente interativo**.

### <a name="examples"></a>Exemplos

### <a name="to-change-the-program-that-a-task-runs"></a>Para alterar o programa que uma tarefa executa

O comando a seguir altera o programa que a tarefa de verificação de vírus executa de VirusCheck. exe para VirusCheck2. exe. Esse comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/TR** para especificar o novo programa para a tarefa. (Não é possível alterar o nome da tarefa.)
```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```
Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task Virus Check have been changed.
```
Como resultado desse comando, a tarefa de verificação de vírus agora executa o VirusCheck2. exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Para alterar a senha de uma tarefa remota

O comando a seguir altera a senha da conta de usuário para a tarefa RemindMe no computador remoto, SVR01. O comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/s** para especificar o computador remoto. Ele usa o parâmetro **/RP** para especificar a nova senha, p@ssWord3.

Esse procedimento é necessário sempre que a senha de uma conta de usuário expirar ou for alterada. Se a senha salva em uma tarefa não for mais válida, a tarefa não será executada.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task RemindMe have been changed.
```
Como resultado desse comando, a tarefa RemindMe agora é executada em sua conta de usuário original, mas com uma nova senha.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Para alterar o programa e a conta de usuário de uma tarefa

O comando a seguir altera o programa que uma tarefa executa e altera a conta de usuário na qual a tarefa é executada. Basicamente, ele usa uma agenda antiga para uma nova tarefa. Esse comando altera a tarefa ChkNews, que inicia o notepad. exe todas as manhãs às 9:00 A.M., para iniciar o Internet Explorer em vez disso.

O comando usa o parâmetro **/TN** para identificar a tarefa. Ele usa o parâmetro **/TR** para alterar o programa que a tarefa executa e o parâmetro **/ru** para alterar a conta de usuário na qual a tarefa é executada.

O parâmetro **/ru**e **/RP** , que fornece a senha para a conta de usuário, é omitido. Você deve fornecer uma senha para a conta, mas pode usar o parâmetro **/ru**e **/RP** e digitar a senha em texto não criptografado ou aguardar o **Schtasks. exe** solicitar uma senha e, em seguida, digitar a senha no texto obscuro.
```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```
Em resposta, o **Schtasks. exe** solicita a senha para a conta de usuário. Ele obscurece o texto que você digita, portanto, a senha não é visível.
```
Please enter the password for DomainX\Admin01: 
```
Observe que o parâmetro **/TN** identifica a tarefa e que os parâmetros **/TR** e **/ru** alteram as propriedades da tarefa. Você não pode usar outro parâmetro para identificar a tarefa e não pode alterar o nome da tarefa.

Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task ChkNews have been changed.
```
Como resultado desse comando, a tarefa ChkNews agora executa o Internet Explorer com as permissões de uma conta de administrador.

### <a name="to-change-a-program-to-the-system-account"></a>Para alterar um programa para a conta do sistema

O comando a seguir altera a tarefa SecurityScript para que ela seja executada com permissões da conta System. Ele usa o parâmetro * */ru * * para indicar a conta do sistema.
```
schtasks /change /tn SecurityScript /ru 
```
Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de êxito:
```
INFO: The run as user name for the scheduled task SecurityScript will be changed to NT AUTHORITY\SYSTEM.
SUCCESS: The parameters of the scheduled task SecurityScript have been changed.
```
Como as tarefas executadas com permissões de conta do sistema não exigem uma senha, o **Schtasks. exe** não solicita um.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Para executar um programa somente quando eu estiver conectado

O comando a seguir adiciona a propriedade somente interativa a MyApp, uma tarefa existente. Essa propriedade garante que a tarefa seja executada somente quando o usuário executar como, ou seja, a conta de usuário sob a qual a tarefa é executada, está conectado ao computador.

O comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/it** para adicionar a propriedade interativa somente à tarefa. Como a tarefa já é executada com as permissões da minha conta de usuário, não preciso alterar o parâmetro **/ru** da tarefa.
```
schtasks /change /tn MyApp /it
```
Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de êxito.
```
SUCCESS: The parameters of the scheduled task MyApp have been changed.
```

## <a name="schtasks-run"></a><a name=BKMK_run></a>execução de Schtasks

Inicia uma tarefa agendada imediatamente. A operação de **execução** ignora o agendamento, mas usa o local do arquivo de programa, a conta de usuário e a senha salvos na tarefa para executar a tarefa imediatamente.

### <a name="syntax"></a>Sintaxe

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<Nome_tarefa >    |                                                                                                                                                       Obrigatório. Identifica a tarefa.                                                                                                                                                        |
|    /s \<computador >     |                                                                                                           Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<\]de domínio > <User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
|    /p \<senha >     |                          Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** , mas omitir o parâmetro **/p** ou o argumento password, **Schtasks** solicitará uma senha.</br>Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.                           |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

-   Use esta operação para testar suas tarefas. Se uma tarefa não for executada, verifique o log de transações do serviço Agendador de Tarefas, \<SystemRoot > \SchedLgU.txt, para erros.
-   A execução de uma tarefa não afeta a agenda da tarefa e não altera o tempo de execução seguinte agendado para a tarefa.
-   Para executar uma tarefa remotamente, a tarefa deve ser agendada no computador remoto. Quando você o executa, a tarefa é executada somente no computador remoto. Para verificar se uma tarefa está em execução em um computador remoto, use o Gerenciador de tarefas ou o log de transações Agendador de Tarefas, \<SystemRoot > \SchedLgU.txt.

### <a name="examples"></a>Exemplos

### <a name="to-run-a-task-on-the-local-computer"></a>Para executar uma tarefa no computador local

O comando a seguir inicia a tarefa script de segurança.
```
schtasks /run /tn Security Script
```
Em resposta, o **Schtasks. exe** inicia o script associado à tarefa e exibe a seguinte mensagem:
```
SUCCESS: Attempted to run the scheduled task Security Script.
```
Como a mensagem implica, o **Schtasks** tenta iniciar o programa, mas não é possível que o programa seja realmente iniciado.

### <a name="to-run-a-task-on-a-remote-computer"></a>Para executar uma tarefa em um computador remoto

O comando a seguir inicia a tarefa de atualização em um computador remoto, SVR01:
```
schtasks /run /tn Update /s Svr01
```
Nesse caso, o **Schtasks. exe** exibe a seguinte mensagem de erro:
```
ERROR: Unable to run the scheduled task Update.
```
Para encontrar a causa do erro, procure no log de transações de tarefas agendadas, C:\Windows\SchedLgU.txt em SVR01. Nesse caso, a seguinte entrada aparece no log:
```
Update.job (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Aparentemente, o nome de usuário ou a senha na tarefa não é válido no sistema. O comando **SCHTASKS/Change** a seguir atualiza o nome de usuário e a senha da tarefa de atualização em SVR01:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Após a conclusão do comando de **alteração** , o comando **executar** é repetido. Desta vez, o programa Update. exe é iniciado e **Schtasks. exe** exibe a seguinte mensagem:
```
SUCCESS: Attempted to run the scheduled task Update.
```
Como a mensagem implica, o **Schtasks** tenta iniciar o programa, mas não é possível que o programa seja realmente iniciado.

## <a name="schtasks-end"></a><a name=BKMK_end></a>término de Schtasks

Interrompe um programa iniciado por uma tarefa.

### <a name="syntax"></a>Sintaxe

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                               Definição                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<Nome_tarefa >    |                                                                                                                                         Obrigatório. Identifica a tarefa que iniciou o programa.                                                                                                                                         |
|    /s \<computador >     |                                                                                                                        Especifica o nome ou o endereço IP de um computador remoto. O padrão é o computador local.                                                                                                                        |
| /u [\<\]de domínio > <User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
|    /p \<senha >     |                        Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** , mas omitir o parâmetro **/p** ou o argumento password, **Schtasks** solicitará uma senha.</br>Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.                         |
|          /?           |                                                                                                                                                             Exibe a ajuda.                                                                                                                                                              |

### <a name="remarks"></a>Comentários

**Schtasks. exe** encerra apenas as instâncias de um programa iniciadas por uma tarefa agendada. Para interromper outros processos, use o TaskKill. Para obter mais informações, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Exemplos

### <a name="to-end-a-task-on-a-local-computer"></a>Para finalizar uma tarefa em um computador local

O comando a seguir interrompe a instância do Notepad. exe que foi iniciada pela tarefa meu bloco de notas:
```
schtasks /end /tn My Notepad
```
Em resposta, o **Schtasks. exe** interrompe a instância do Notepad. exe que a tarefa iniciou e exibe a seguinte mensagem de êxito:
```
SUCCESS: The scheduled task My Notepad has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Para finalizar uma tarefa em um computador remoto

O comando a seguir interrompe a instância do Internet Explorer que foi iniciada pela tarefa de Internet no computador remoto, SVR01:
```
schtasks /end /tn InternetOn /s Svr01
```
Em resposta, o **Schtasks. exe** interrompe a instância do Internet Explorer que a tarefa iniciou e exibe a seguinte mensagem de êxito:
```
SUCCESS: The scheduled task InternetOn has been terminated successfully.
```

## <a name="schtasks-delete"></a><a name=BKMK_delete></a>exclusão de Schtasks

Exclui uma tarefa agendada.

### <a name="syntax"></a>Sintaxe

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /TN {\<Nome_tarefa >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Suprime a mensagem de confirmação. A tarefa é excluída sem aviso.                                                                                                                                  |
|    /s \<computador >     |                                                                                                           Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<\]de domínio > <User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
|    /p \<senha >     |                          Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** , mas omitir o parâmetro **/p** ou o argumento password, **Schtasks** solicitará uma senha.</br>Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.                           |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

- A operação de **exclusão** exclui a tarefa da agenda. Ele não exclui o programa que a tarefa executa ou interrompe um programa em execução.
- O comando **delete \\** * exclui todas as tarefas agendadas para o computador, não apenas as tarefas agendadas pelo usuário atual.

### <a name="examples"></a>Exemplos

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Para excluir uma tarefa do agendamento de um computador remoto

O comando a seguir exclui a tarefa iniciar email do agendamento de um computador remoto. Ele usa o parâmetro **/s** para identificar o computador remoto.
```
schtasks /delete /tn Start Mail /s Svr16
```
Em resposta, o **Schtasks. exe** exibe a seguinte mensagem de confirmação. Para excluir a tarefa, pressione Y<strong>.</strong> Para cancelar o comando, digite **n**:
```
WARNING: Are you sure you want to remove the task Start Mail (Y/N )? 
SUCCESS: The scheduled task Start Mail was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Para excluir todas as tarefas agendadas para o computador local

O comando a seguir exclui todas as tarefas do agendamento do computador local, incluindo tarefas agendadas por outros usuários. Ele usa o parâmetro **/tn \\** * para representar todas as tarefas no computador e o parâmetro **/f** para suprimir a mensagem de confirmação.
```
schtasks /delete /tn * /f
```
Em resposta, o **Schtasks. exe** exibe as seguintes mensagens de êxito indicando que a única tarefa agendada, SecureScript, é excluída.

`SUCCESS: The scheduled task SecureScript was successfully deleted.`

## <a name="schtasks-query"></a><a name=BKMK_query></a>consulta Schtasks

Exibe as tarefas agendadas para execução no computador.

### <a name="syntax"></a>Sintaxe

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Query        |                                                                                                                        O nome da operação é opcional. Digitar **Schtasks** sem parâmetros executa uma consulta.                                                                                                                         |
|      /FO {tabela       |                                                                                                                                                                    LISTA                                                                                                                                                                     |
|          /NH          |                                                                                                            Omite cabeçalhos de coluna da exibição da tabela. Esse parâmetro é válido com os formatos de saída de **tabela** e **CSV** .                                                                                                             |
|          /v           |                                                                                                         Adiciona propriedades avançadas das tarefas à exibição.</br>As consultas que usam **/v** devem ser formatadas como **list** ou **CSV**.                                                                                                          |
|    /s \<computador >     |                                                                                                           Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<\]de domínio > <User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
|    /p \<senha >     |                                        Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar **/u**, mas omitir **/p** ou o argumento password, **Schtasks** solicitará uma senha.</br>Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**.                                         |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

**Schtasks. exe** encerra apenas as instâncias de um programa iniciadas por uma tarefa agendada. Para interromper outros processos, use o TaskKill. Para obter mais informações, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Exemplos

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Para exibir as tarefas agendadas no computador local

Os comandos a seguir exibem todas as tarefas agendadas para o computador local. Esses comandos produzem o mesmo resultado e podem ser usados de forma intercambiável.
```
schtasks
schtasks /query
```
Em resposta, o **Schtasks. exe** exibe as tarefas no formato de tabela simples padrão, conforme mostrado na tabela a seguir:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Para exibir propriedades avançadas de tarefas agendadas

O comando a seguir solicita uma exibição detalhada das tarefas no computador local. Ele usa o parâmetro **/v** para solicitar uma exibição detalhada (detalhada) e o parâmetro **/fo List** para formatar a exibição como uma lista para facilitar a leitura. Você pode usar esse comando para verificar se uma tarefa criada tem o padrão de recorrência pretendido.

**SCHTASKS/Query/FO lista/v**

Em resposta, o **Schtasks. exe** exibe uma lista detalhada de propriedades para todas as tarefas. A exibição a seguir mostra a lista de tarefas para uma tarefa agendada para execução às 4:00. na última sexta-feira de cada mês:
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Para registrar tarefas agendadas para um computador remoto

O comando a seguir solicita uma lista de tarefas agendadas para um computador remoto e adiciona as tarefas a um arquivo de log separado por vírgulas no computador local. Você pode usar esse formato de comando para coletar e controlar tarefas que estão agendadas para vários computadores.

O comando usa o parâmetro **/s** para identificar o computador remoto, Reskit16, o parâmetro **/fo** para especificar o formato e o parâmetro **/NH** para suprimir os cabeçalhos de coluna. O símbolo de acréscimo de **>>** redireciona a saída para o log de tarefa, p0102. csv, no computador local, SVR01. Como o comando é executado no computador remoto, o caminho do computador local deve ser totalmente qualificado.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
Em resposta, o **Schtasks. exe** adiciona as tarefas agendadas para o computador Reskit16 ao arquivo p0102. csv no computador local, SVR01.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
