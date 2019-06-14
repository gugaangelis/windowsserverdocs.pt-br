---
title: schtasks
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76ae264ded3cd0611c06b56c1074a2d916513da4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441609"
---
# <a name="schtasks"></a>schtasks



Agenda comandos e programas para ser executado periodicamente ou em um momento específico. Adiciona e remove tarefas da agenda, inicia e interrompe tarefas sob demanda e exibe e altera tarefas agendadas.

Para exibir a sintaxe de comando, clique em um dos seguintes comandos:
-   [schtasks create](#BKMK_create)
-   [schtasks change](#BKMK_change)
-   [schtasks run](#BKMK_run)
-   [schtasks end](#BKMK_end)
-   [schtasks delete](#BKMK_delete)
-   [schtasks query](#BKMK_query)

## <a name="remarks"></a>Comentários

- **SchTasks.exe** executa as mesmas operações **tarefas agendadas** na **painel de controle**. Você pode usar essas ferramentas juntos e alternadamente.
- **SCHTASKS** substitui **At.exe**, uma ferramenta incluída em versões anteriores do Windows. Embora **At.exe** ainda está incluída na família Windows Server 2003, **schtasks** é a tarefa de linha de comando recomendada ferramenta de agendamento.
- Os parâmetros em uma **schtasks** comando pode aparecer em qualquer ordem. Digitação **schtasks** sem quaisquer parâmetros executa uma consulta.
- Permissões para **schtasks**  
  -   Você deve ter permissão para executar o comando. Qualquer usuário pode agendar uma tarefa no computador local, e eles podem exibir e alterar as tarefas agendadas por eles. Os membros do grupo Administradores podem agendar, exibir e alterar todas as tarefas no computador local.
  -   Para agendar, exibir ou alterar uma tarefa em um computador remoto, você deve ser membro do grupo Administradores no computador remoto, ou você deve usar o **/u** parâmetro para fornecer as credenciais de administrador do computador remoto.
  -   Você pode usar o **/u** parâmetro em uma **/ criar** ou **/altere** operação somente quando os computadores locais e remotos estão no mesmo domínio ou computador local está em um domínio que as relações de confiança de domínio do computador remoto. Caso contrário, o computador remoto não é possível autenticar a conta de usuário especificada e não é possível verificar que a conta é um membro do grupo Administradores.
  -   A tarefa deve ter permissão para executar. As permissões necessárias variam de acordo com a tarefa. Por padrão, as tarefas executadas com as permissões do usuário atual do computador local ou com as permissões do usuário especificado pela **/u** parâmetro, se uma é incluída. Para executar uma tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema, use o **/ru** parâmetro.
- Para verificar que foi executado em uma tarefa agendada ou para descobrir por que uma tarefa agendada não foi executado, consulte o log de transações de serviço do Agendador de tarefas, *SystemRoot*\SchedLgU.txt. Esse log registra as tentativas execuções iniciadas por todas as ferramentas que usam o serviço, incluindo **tarefas agendadas** e **SchTasks.exe**.
- Em raras ocasiões, arquivos de tarefas estão corrompidos. Não execute tarefas corrompidas. Quando você tenta executar uma operação em tarefas corrompidas, **SchTasks.exe** exibe a seguinte mensagem de erro:  
  ```
  ERROR: The data is invalid.
  ```  
  Não é possível recuperar tarefas corrompidas. Para restaurar as recursos do sistema de agendamento de tarefas, use **SchTasks.exe** ou **tarefas agendadas** para excluir as tarefas do sistema e reagendá-los.

## <a name="BKMK_create"></a>Criar Schtasks

Agenda uma tarefa.

**SCHTASKS** usa combinações de parâmetros diferentes para cada tipo de agenda. Para ver a sintaxe combinada para a criação de tarefas ou para ver a sintaxe para criar uma tarefa com um determinado tipo de agendamento, clique em uma das opções a seguir.
-   [Descrições de sintaxe e parâmetro combinadas](#BKMK_syntax)
-   [Para agendar uma tarefa que executa cada N minutos](#BKMK_minutes)
-   [Para agendar uma tarefa que executa cada N horas](#BKMK_hours)
-   [Para agendar uma tarefa que executa cada N dias](#BKMK_days)
-   [Para agendar uma tarefa que executa cada N semanas](#BKMK_weeks)
-   [Para agendar uma tarefa que executa cada N meses](#BKMK_months)
-   [Para agendar uma tarefa que é executado em um dia específico da semana](#BKMK_spec_day)
-   [Para agendar uma tarefa que é executado em uma semana específica do mês](#BKMK_spec_week)
-   [Para agendar uma tarefa que é executado em uma data específica de cada mês](#BKMK_spec_date)
-   [Para agendar uma tarefa executada no último dia de um mês](#BKMK_last_day)
-   [Para agendar uma tarefa que executa uma vez](#BKMK_once)
-   [Para agendar uma tarefa que é executada sempre que o sistema é iniciado](#BKMK_startup)
-   [Para agendar uma tarefa que é executado quando um usuário fizer logon](#BKMK_logon)
-   [Para agendar uma tarefa que é executado quando o sistema estiver ocioso](#BKMK_idle)
-   [Para agendar uma tarefa que executa o agora](#BKMK_now)
-   [Para agendar uma tarefa que é executado com permissões diferentes](#BKMK_diff_perms)
-   [Para agendar uma tarefa executada com permissões do sistema](#BKMK_sys_perms)
-   [Para agendar uma tarefa que executa mais de um programa](#BKMK_multi_progs)
-   [Para agendar uma tarefa que é executado em um computador remoto](#BKMK_remote)

### <a name="BKMK_syntax"></a>Descrições de sintaxe e parâmetro combinadas

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Parâmetros

##### <a name="sc-scheduletype"></a>/SC \<ScheduleType >

Especifica o tipo de agendamento. Os valores válidos são minuto, por hora, DIARIAMENTE, SEMANALMENTE, MENSALMENTE, uma vez, ONSTART, no login, ONIDLE.

|Tipo de agenda|Descrição|
|-------------|-----------|
|MINUTO, POR HORA, DIÁRIA, SEMANAL, MENSAL|Especifica a unidade de tempo para a agenda.|
|VEZ|A tarefa é executada uma vez em uma data e hora especificadas.|
|ONSTART|A tarefa é executada sempre que o sistema é iniciado. Você pode especificar uma data de início ou executar a tarefa na próxima vez que o sistema é iniciado.|
|NO LOGIN|A tarefa é executada sempre que o logon do usuário (qualquer usuário). Você pode especificar uma data ou executar a tarefa na próxima vez que o usuário fizer logon.|
|ONIDLE|A tarefa é executada sempre que o sistema fica ocioso por um período de tempo especificado. Você pode especificar uma data ou executar a tarefa na próxima vez que o sistema estiver ocioso.|

##### <a name="tn-taskname"></a>/tn \<TaskName>

Especifica um nome para a tarefa. Cada tarefa do sistema deve ter um nome exclusivo. O nome deve estar em conformidade com as regras para nomes de arquivo e não deve exceder 238 caracteres. Use aspas para delimitar nomes que incluam espaços.

##### <a name="tr-taskrun"></a>/tr \<TaskRun>

Especifica o programa ou comando executado pela tarefa. Digite o nome de arquivo e caminho totalmente qualificado de um arquivo executável, arquivo de script ou arquivo em lotes. O nome do caminho não deve exceder 262 caracteres. Se você omitir o caminho **schtasks** supõe que o arquivo está no *SystemRoot*diretório \System32.

##### <a name="s-computer"></a>/s \<Computer>

Agenda uma tarefa no computador remoto especificado. Digite o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.

##### <a name="u-domainuser"></a>/u [\<Domain>\]<User>

Executa este comando com as permissões da conta de usuário especificada. O padrão é que as permissões do usuário atual do computador local. O **/u** e **/p** parâmetros são válidos somente para agendar uma tarefa em um computador remoto ( **/s**).

As permissões da conta especificada são usadas para agendar a tarefa e para executar a tarefa. Para executar a tarefa com as permissões de um usuário diferente, use o **/ru** parâmetro.

A conta de usuário deve ser um membro do grupo Administradores no computador remoto. Além disso, o computador local deve estar no mesmo domínio que o computador remoto, ou deve estar em um domínio que seja confiável pelo domínio do computador remoto.

##### <a name="p-password"></a>/p \<Password>

Fornece a senha para a conta de usuário especificado na **/u** parâmetro. Se você usar o **/u** parâmetro, mas omita a **/p** parâmetro ou o argumento de senha, **schtasks** solicita uma senha e obscurece o texto digitado.

O **/u** e **/p** parâmetros são válidos somente para agendar uma tarefa em um computador remoto ( **/s**).

##### <a name="ru-domainuser--system"></a>/ru {[\<Domain>\]<User> | System}

Executa a tarefa com permissões de conta de usuário especificada. Por padrão, a tarefa é executada com as permissões do usuário atual do computador local ou com a permissão do usuário especificado pela **/u** parâmetro, se uma é incluída. O **/ru** parâmetro é válido quando o agendamento de tarefas em computadores locais ou remotos.


|       Valor        |                                                    Descrição                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain>\]<User> |                                       Especifica uma conta de usuário alternativas.                                        |
|    Sistema ou ""    | Especifica a conta sistema local, uma conta altamente privilegiada usada pelo sistema operacional e dos serviços do sistema. |

##### <a name="rp-password"></a>/RP \<senha >

Fornece a senha da conta de usuário que é especificada na **/ru** parâmetro. Se você omitir esse parâmetro ao especificar uma conta de usuário **SchTasks.exe** solicita a senha e obscurece o texto digitado.

Não use o **/rp** parâmetro para tarefas de execução com credenciais de conta do sistema ( **/ru System**). A conta do sistema não tem uma senha e **SchTasks.exe** não solicitará um.

##### <a name="mo-modifier"></a>/mo \<Modifier>

Especifica a frequência com que a tarefa é executada dentro de seu tipo de agenda. Esse parâmetro é válido, mas opcional, para um minuto, por hora, DIÁRIA, SEMANAL e mensal programar. O valor padrão é 1.

|Tipo de agenda|Valores de modificador|Descrição|
|-------------|---------------|-----------|
|MINUTO|1 - 1439|A tarefa é executada cada \<N > minutos.|
|POR HORA|1 - 23|A tarefa é executada cada \<N > horas.|
|DIÁRIO|1 - 365|A tarefa é executada cada \<N > dias.|
|SEMANAL|1 - 52|A tarefa é executada cada \<N > semanas.|
|VEZ|Nenhum modificador.|A tarefa é executada uma vez.|
|ONSTART|Nenhum modificador.|A tarefa é executada na inicialização.|
|NO LOGIN|Nenhum modificador.|A tarefa é executada quando o usuário especificado pela **/u** parâmetro fizer logon.|
|ONIDLE|Nenhum modificador.|A tarefa é executada depois que o sistema estiver ocioso para o número de minutos especificado pela **/i** parâmetro, que é necessário para uso com ONIDLE.|
|MENSAL|1 - 12|A tarefa é executada cada \<N > meses.|
|MENSAL|LASTDAY|A tarefa é executada no último dia do mês.|
|MENSAL|FIRST, SECOND, THIRD, FOURTH, LAST|Usar com o **/d**\<dia > parâmetro para executar uma tarefa em uma semana e dia específicos. Por exemplo, na terceira quarta-feira do mês.|

##### <a name="d-dayday--"></a>Dia /d [,... dias] | *

Especifica um dia (ou dias) da semana ou um dia (ou dias) de um mês. Válido somente com um agendamento SEMANAL ou mensal.


| Tipo de agenda |              Modificador              |     Os valores de dia (/ d)      |                                                                                                 Descrição                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    SEMANAL     |               1 - 52               | MON - SUN [, MON - SUN...] |                                                                                                     \*                                                                                                      |
|    MENSAL    | FIRST, SECOND, THIRD, FOURTH, LAST |        MON - SUN         |                                                                                   Necessário para o agendamento de uma semana específica.                                                                                    |
|    MENSAL    |          Nenhum ou {1-12}          |          1 - 31          | Opcional e válido apenas com nenhum modificador ( **/mês**) parâmetro (uma agenda de datas específico) ou quando o **/mês** é de 1 a 12 (um "cada \<N > meses" agenda). O padrão é dia 1 (o primeiro dia do mês). |

##### <a name="m-monthmonth"></a>/m mês [,... mês]

Especifica um mês ou meses do ano durante o qual a tarefa agendada deve ser executado. Os valores válidos são JAN - dez e * (todos os meses). O **/m** parâmetro é válido somente com um agendamento mensal. É necessário quando o modificador LASTDAY é usado. Caso contrário, ele é opcional e o valor padrão é * (todos os meses).

##### <a name="i-idletime"></a>/i \<IdleTime>

Especifica quantos minutos o computador estiver ocioso antes que a tarefa for iniciada. Um valor válido é um número inteiro de 1 a 999. Esse parâmetro é válido somente com uma agenda ONIDLE e, em seguida, é necessário.

##### <a name="st-starttime"></a>/st \<StartTime>

Especifica a hora do dia em que a tarefa começa (cada vez que for iniciado) no \<formato hh: mm > 24 - hora. O valor padrão é a hora atual no computador local. O **/st** parâmetro é válido com o minuto, por hora, DIARIAMENTE, SEMANALMENTE, MENSALMENTE e agenda uma vez. Ele é necessário para um agendamento de uma vez.

##### <a name="ri-interval"></a>/RI \<intervalo >

Especifica o intervalo de repetição em minutos. Isso não é aplicável para tipos de agenda: Por hora, minuto, ONSTART, no login e ONIDLE. Intervalo válido é de 1 a 599940 minutos (599940 minutos = 9999 horas). Se /ET ou /DU for especificado, em seguida, o intervalo de repetição padrão é 10 minutos.

##### <a name="et-endtime"></a>/et \<EndTime>

Especifica a hora do dia em que uma agenda por hora ou minuto tarefa termina em \<formato hh: mm > 24 - hora. Após a hora de término especificada **schtasks** não inicia a tarefa novamente até que a hora de início se repete. Por padrão, os agendamentos de tarefa não têm nenhuma hora de término. Esse parâmetro é opcional e é válido somente com um agendamento por hora ou minutos.

Para obter um exemplo, consulte:
-   "Para agendar uma tarefa que executa a cada 100 minutos durante o horário comercial" **para agendar uma tarefa que executa cada** \<N > **minutos** seção.

##### <a name="du-duration"></a>/du \<Duration>

Especifica um comprimento máximo de tempo para um minuto ou o agendamento por hora no \<HHHH: mm > 24 - formato de hora. Depois do tempo especificado, **schtasks** não inicia a tarefa novamente até que a hora de início se repete. Por padrão, os agendamentos de tarefa não possuem duração máxima. Esse parâmetro é opcional e é válido somente com um agendamento por hora ou minutos.

Para obter um exemplo, consulte:
-   "Para agendar uma tarefa executada a cada 3 horas por 10 horas" **para agendar uma tarefa que executa cada** \<N > **horas** seção.

##### <a name="k"></a>/k

Interrompe o programa que a tarefa é executada no momento especificado por **/et** ou **/du**. Sem **/k**, **schtasks** não inicia o programa novamente depois que ele atinge o tempo especificado por **/et** ou **/du**, mas não interrompe o programa se ele ainda está em execução. Esse parâmetro é opcional e é válido somente com um agendamento por hora ou minutos.

Para obter um exemplo, consulte:
-   "Para agendar uma tarefa que executa a cada 100 minutos durante o horário comercial" **para agendar uma tarefa que executa cada** \<N > **minutos** seção.

##### <a name="sd-startdate"></a>/SD \<StartDate >

Especifica a data na qual o Agendador de tarefas é iniciada. O valor padrão é a data atual no computador local. O **/sd** parâmetro é opcional e válido para todos os tipos de agendamento.

O formato para *StartDate* varia de acordo com o local selecionado para o computador local nas **opções regionais e idioma** na **painel de controle**. Apenas um formato é válido para cada localidade.

Os formatos de data válida são listados na tabela a seguir. Use o formato mais semelhante ao formato selecionado para **data abreviada** na **opções regionais e idioma** na **painel de controle** no computador local.


|       Valor       |                                        Descrição                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Usar para o primeiro mês de formatos, como **inglês (Estados Unidos)** e **Espanhol (Panamá)** . |
| \<DD>/<MM>/<YYYY> |       Usar para o primeiro dia de formatos, como **búlgaro** e **holandês (países baixos)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Usar para o primeiro ano formatos, como **sueco** e **francês (Canadá)** .          |

/ED \<EndDate >

Especifica a data na qual termina a agenda. Este parâmetro é opcional. Não é válido em uma agenda ONIDLE, ONSTART, no login ou uma vez. Por padrão, os agendamentos têm sem data de término.

O formato para *EndDate* varia de acordo com o local selecionado para o computador local nas **opções regionais e idioma** na **painel de controle**. Apenas um formato é válido para cada localidade.

Os formatos de data válida são listados na tabela a seguir. Use o formato mais semelhante ao formato selecionado para **data abreviada** na **opções regionais e idioma** na **painel de controle** no computador local.


|       Valor       |                                        Descrição                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Usar para o primeiro mês de formatos, como **inglês (Estados Unidos)** e **Espanhol (Panamá)** . |
| \<DD>/<MM>/<YYYY> |       Usar para o primeiro dia de formatos, como **búlgaro** e **holandês (países baixos)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Usar para o primeiro ano formatos, como **sueco** e **francês (Canadá)** .          |

##### <a name="it"></a>/it

Especifica a tarefa seja executada somente quando o usuário "Executar como" (a conta de usuário sob a qual a tarefa é executada) fizer logon no computador. Esse parâmetro não tem nenhum efeito nas tarefas que são executados com permissões do sistema.

Por padrão, o usuário "Executar como" é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pela **/u** parâmetro, se um for usado. No entanto, se o comando inclui o **/ru** parâmetro, o usuário, em seguida, o "Executar como" é a conta especificada pela **/ru** parâmetro.

Para obter exemplos, consulte:
-   "Para agendar uma tarefa que é executado a cada 70 dias se eu estiver conectado" no **para agendar uma tarefa que executa cada** *N* **dias** seção.
-   "Executar uma tarefa somente quando um usuário específico conectado a" a **para agendar uma tarefa que é executado com permissões diferentes** seção.

##### <a name="z"></a>/z

Especifica para excluir a tarefa após a conclusão da sua agenda.

##### <a name="f"></a>/f

Especifica a criação da tarefa e suprimir avisos se a tarefa especificada já existe.

##### <a name=""></a>/?

Exibe a ajuda no prompt de comando.

### <a name="BKMK_minutes"></a>Para agendar uma tarefa que executa cada N minutos

#### <a name="minute-schedule-syntax"></a>Sintaxe de agendamento em minutos

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um agendamento em minutos, o **/sc minuto** parâmetro é obrigatório. O **/mês** parâmetro (modificador) é opcional e especifica o número de minutos entre cada execução da tarefa. O valor padrão para **/mês** é 1 (cada minuto). O **/et** (hora de término) e **/du** parâmetros (duração) são opcionais e pode ser usados com ou sem o **/k** parâmetro (Finalizar tarefa).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Para agendar uma tarefa que executa a cada 20 minutos

O comando a seguir agenda um script de segurança, vbs, para executar a cada 20 minutos. O comando usa o **/sc** parâmetro para especificar um agendamento em minutos e o **/mês** parâmetro para especificar um intervalo de 20 minutos.

Como o comando não inclui uma data de início ou hora, a tarefa começa 20 minutos após o comando for concluído e é executado a cada 20 minutos depois disso, sempre que o sistema está em execução. Observe que o arquivo de origem do script de segurança está localizado em um computador remoto, mas que a tarefa está agendada e é executado no computador local.
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Para agendar uma tarefa que executa a cada 100 minutos durante o horário comercial

O comando a seguir agenda um script de segurança, vbs, para ser executado no computador local a cada 100 minutos entre 17:00. e 7 3:59. cada dia. O comando usa o **/sc** parâmetro para especificar um agendamento em minutos e o **/mês** parâmetro para especificar um intervalo de 100 minutos. Ele usa o **/st** e **/et** parâmetros para especificar a hora de início e a hora de término da agenda de cada dia. Ele também usa o **/k** parâmetro para interromper o script se ele ainda estiver em execução 7 3:59. Sem **/k**, **schtasks** não inicia o script após 7 3:59., mas se a instância iniciada às 6:00h: 20 foi ainda em execução, ele não será interrompida.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>Para agendar uma tarefa que executa cada N horas

#### <a name="hourly-schedule-syntax"></a>Sintaxe de agendamento por hora

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um agendamento por hora, o **/sc por hora** parâmetro é obrigatório. O **/mês** parâmetro (modificador) é opcional e especifica o número de horas entre cada execução da tarefa. O valor padrão para **/mês** é 1 (a cada hora). O **/k** parâmetro (Finalizar tarefa) é opcional e pode ser usado com o **/et** (término na hora especificada) ou **/du** (end após o intervalo especificado).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Para agendar uma tarefa que executa a cada cinco horas

O comando a seguir agenda o programa MyApp para ser executado a cada cinco horas, começando em 1º de março de 2002. Ele usa o **/mês** parâmetro para especificar o intervalo e o **/sd** parâmetro para especificar a data de início. Como o comando não especifica uma hora de início, a hora atual é usada como a hora de início.

Porque o computador local está configurado para usar o **inglês (Zimbábue)** opção **opções regionais e idioma** na **painel de controle**, o formato da data de início é MM/DD/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Para agendar uma tarefa que executa a cada hora cinco minutos após a hora

O comando a seguir agenda o programa MyApp para ser executado a cada hora começando em cinco minutos após a meia noite. Porque o **/mês** parâmetro for omitido, o comando usa o valor padrão para o agendamento por hora, o que é a cada hora (1). Se esse comando for executado depois de 12:05:00, o programa não executar até o próximo dia.
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Para agendar uma tarefa que é executado a cada três horas por 10 horas

O comando a seguir agenda o programa MyApp para executar a cada três horas por 10 horas.

O comando usa o **/sc** parâmetro para especificar uma agenda por hora e o **/mês** parâmetro para especificar o intervalo de 3 horas. Ele usa o **/st** parâmetro para iniciar o agendamento à meia-noite e o **/du** parâmetro para finalizar as recorrências após 10 horas. Como o programa é executado por apenas alguns minutos, o **/k** parâmetro, que interrompe o programa se ele ainda está em execução quando a duração expira, não é necessário.
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
Neste exemplo, a tarefa é executada às 12:00, 3h00, 6h00 e às 9h Como a duração é de 10 horas, a tarefa não é executada novamente às 12:00. Em vez disso, ele começa novamente às 12:00. no dia seguinte.

### <a name="BKMK_days"></a>Para agendar uma tarefa que executa cada N dias

#### <a name="daily-schedule-syntax"></a>Sintaxe de agenda diária

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um agendamento diário, o **/sc diariamente** parâmetro é obrigatório. O **/mês** parâmetro (modificador) é opcional e especifica o número de dias entre cada execução da tarefa. O valor padrão para **/mês** é 1 (cada dia).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Para agendar uma tarefa que executa todos os dias

O exemplo a seguir agenda o programa MyApp para ser executado uma vez por dia, todos os dias às 8 horas. até 31 de dezembro de 2002. Como ele omite o **/mês** parâmetro, o intervalo padrão de 1 é usado para executar o comando todos os dias.

Neste exemplo, porque o sistema de computador local é definido como o **inglês (Reino Unido)** opção **opções regionais e idioma** na **painel de controle**, o formato para a data de término é MM/DD/AAAA (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Para agendar uma tarefa executada a cada 12 dias

O exemplo a seguir agenda o programa MyApp para executar a cada doze dias às 1:00. (UTC+13:00) a partir de 31 de dezembro de 2002. O comando usa o **/mês** parâmetro para especificar um intervalo de dois (2) dias e o **/sd** e **/st** parâmetros para especificar a data e hora.

Neste exemplo, porque o sistema está definido como o **inglês (Zimbábue)** opção **opções regionais e idioma** na **painel de controle**, o formato da data de término é MM/DD / AAAA (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Para agendar uma tarefa que executa a cada 70 dias se eu estiver conectado

O comando a seguir agenda um script de segurança, vbs, para executar a cada 70 dias. O comando usa o **/mês** parâmetro para especificar um intervalo de 70 dias. Ele também usa o **/it** parâmetro para especificar que a tarefa é executada somente quando o usuário cuja conta a tarefa é executado é registrado no computador. Porque a tarefa será executada com as permissões da minha conta de usuário, a tarefa será executada somente quando eu estiver conectado.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Para identificar as tarefas com o interativo somente ( **/it**) propriedade, use uma consulta detalhada **(/ consulta /v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o **modo de Logon** campo tem um valor de **interativo apenas**.

### <a name="BKMK_weeks"></a>Para agendar uma tarefa que executa cada N semanas

#### <a name="weekly-schedule-syntax"></a>Sintaxe de agendamento semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Em um agendamento semanal, o **/sc semanalmente** parâmetro é obrigatório. O **/mês** parâmetro (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão para **/mês** é 1 (todas as semanas).

Agendamentos semanais também tem um recurso opcional **/d** parâmetro para agendar a tarefa seja executada em dias específicos da semana ou em todos os dias ( *). O padrão é MON (segunda-feira). O cada dia (* ) opção é equivalente ao agendamento de uma tarefa diária.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Para agendar uma tarefa que é executado a cada seis semanas

O comando a seguir agenda o programa MyApp para ser executado em um computador remoto a cada seis semanas. O comando usa o **/mês** parâmetro para especificar o intervalo. Como o comando omite os **/d** parâmetro, a tarefa é executada às segundas-feiras.

Esse comando também usa o **/s** parâmetro para especificar o computador remoto e o **/u** parâmetro para executar o comando com as permissões do usuário conta de administrador. Porque o **/p** parâmetro for omitido, **SchTasks.exe** solicita ao usuário a senha da conta de administrador.

Além disso, como o comando é executado remotamente, todos os caminhos de comando, incluindo o caminho para MyApp.exe, se referir aos caminhos no computador remoto.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Para agendar uma tarefa que é executado a cada duas semanas na sexta-feira

O comando a seguir agenda uma tarefa para executar toda sexta-feira outro. Ele usa o **/mês** parâmetro para especificar o intervalo de duas semanas e o **/d** parâmetro para especificar o dia da semana. Para agendar uma tarefa que executa toda sexta-feira, omita a **/mês** parâmetro ou defini-lo como 1.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>Para agendar uma tarefa que executa cada N meses

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Nesse tipo de agenda, o **/sc mensalmente** parâmetro é obrigatório. O **/mês** parâmetro (modificador), que especifica o número de meses entre cada execução da tarefa, é opcional e o padrão é 1 (todos os meses). Esse tipo de agendamento também tem um recurso opcional **/d** parâmetro para agendar a tarefa seja executada em uma data específica do mês. O padrão é 1 (o primeiro dia do mês).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Para agendar uma tarefa que é executado no primeiro dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no primeiro dia de cada mês. Como um valor de 1 é o padrão para ambos os **/mês** parâmetro (modificador) e o **/d** parâmetro (dia), esses parâmetros são omitidos do comando.
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Para agendar uma tarefa que é executado a cada três meses

O comando a seguir agenda o programa MyApp para ser executado a cada três meses. Ele usa o **/mês** parâmetro para especificar o intervalo.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Para agendar uma tarefa executada à meia-noite no dia 21 de cada mês

O comando a seguir agenda o programa MyApp para executar a cada dois meses no dia 21 de mês à meia-noite. O comando Especifica que essa tarefa deve ser executado por um ano, de 2 de julho de 2002 para 30 de junho de 2003.

O comando usa o **/mês** parâmetro para especificar o intervalo mensal (a cada dois meses), o **/d** parâmetro para especificar a data e a **/st** para especificar o tempo. Ele também usa o **/sd** e **/ed** data de parâmetros para especificar o início e término, respectivamente. Porque o computador local é definido como o **inglês (África do Sul)** opção **opções regionais e idioma** na **painel de controle**, as datas são especificadas no formato local , AAAA/MM/DD.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>Para agendar uma tarefa que é executado em um dia específico da semana

#### <a name="weekly-schedule-syntax"></a>Sintaxe de agendamento semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

O agendamento de "dia da semana" é uma variação do agendamento semanal. Em um agendamento semanal, o **/sc semanalmente** parâmetro é obrigatório. O **/mês** parâmetro (modificador) é opcional e especifica o número de semanas entre cada execução da tarefa. O valor padrão para **/mês** é 1 (todas as semanas). O **/d** parâmetro, que é opcional, agenda a tarefa seja executada em dias específicos da semana ou em todos os dias (<em>). O padrão é MON (segunda-feira). A opção a cada dia (</em>* /d \** *) é equivalente ao agendamento de uma tarefa diária.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Para agendar uma tarefa executada às quartas-feiras

O comando a seguir agenda o programa MyApp para executar todas as semanas na quarta-feira. O comando usa o **/d** parâmetro para especificar o dia da semana. Como o comando omite os **/mês** parâmetro, a tarefa é executada toda semana.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Para agendar uma tarefa que é executado a cada oito semanas de segunda a sexta-feira

O comando a seguir agenda uma tarefa para ser executado de segunda a sexta-feira de cada oito semanas. Ele usa o **/d** parâmetro para especificar os dias e o **/mês** parâmetro para especificar o intervalo de oito semanas.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>Para agendar uma tarefa que é executado em uma semana específica do mês

#### <a name="specific-week-syntax"></a>Sintaxe de uma semana específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

Nesse tipo de agenda, o **/sc mensalmente** parâmetro, o **/mês** parâmetro (modificador) e o **/d** parâmetro (dia) são necessários. O **/mês** parâmetro (modificador) Especifica a semana no qual a tarefa é executada. O **/d** parâmetro especifica o dia da semana. (Você pode especificar apenas um dia da semana para esse tipo de agendamento). Esse agendamento também tem um recurso opcional **/m** parâmetro (mês) que permite que você agende a tarefa para determinados meses ou todos os meses (<em>). O padrão para o * */m</em>* parâmetro é a cada mês (* ).

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Para agendar uma tarefa para o segundo domingo de cada mês

O comando a seguir agenda o programa MyApp para executar no segundo domingo de cada mês. Ele usa o **/mês** parâmetro para especificar a segunda semana do mês e o **/d** parâmetro para especificar o dia.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Para agendar uma tarefa para a primeira segunda-feira de março e setembro

O comando a seguir agenda o programa MyApp para ser executado na primeira segunda-feira de março e setembro. Ele usa o **/mês** parâmetro para especificar a primeira semana do mês e o **/d** parâmetro para especificar o dia. Ele usa **/m** parâmetro para especificar o mês, separando os argumentos de mês com uma vírgula.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>Para agendar uma tarefa que é executado em uma data específica de cada mês

#### <a name="specific-date-syntax"></a>Sintaxe de uma data específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento data específica, o **/sc mensalmente** parâmetro e o **/d** parâmetro (dia) são necessários. O **/d** parâmetro especifica uma data do mês (31-1), não há um dia da semana. Você pode especificar apenas um dia na agenda. O **/mês** parâmetro (modificador) não é válido com esse tipo de agendamento.

O **/m** parâmetro (mês) é opcional para esse tipo de agendamento e o padrão é a cada mês (<em>). **Schtasks</em>*  não lhe permite agendar uma tarefa para uma data que não ocorra em um mês especificado o **/m** parâmetro. No entanto, se omitir as **/m** parâmetro e o agendamento de uma tarefa para uma data que não aparece em cada mês, como o dia 31, em seguida, a tarefa não é executado nos meses menores. Para agendar uma tarefa para o último dia do mês, use o último tipo de agenda do dia.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Para agendar uma tarefa para o primeiro dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no primeiro dia de cada mês. Como o modificador padrão é none (nenhum modificador), o dia padrão é o dia 1 e o mês padrão é a cada mês, o comando não precisa de parâmetros adicionais.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Para agendar uma tarefa para os dias 15 de maio e junho

O comando a seguir agenda o programa MyApp para ser executado em 15 de maio e 15 de junho em 15:00. (15:00). Ele usa o **/m** parâmetro para especificar a data e a **/m** parâmetro para especificar os meses. Ele também usa o **/st** parâmetro para especificar a hora de início.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>Para agendar uma tarefa executada no último dia de um mês

#### <a name="last-day-syntax"></a>Sintaxe do último dia

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No último tipo de agenda do dia, o **/sc mensalmente** parâmetro, o **/mês LASTDAY** parâmetro (modificador) e o **/m** parâmetro (mês) são necessários. O **/d** parâmetro (dia) não é válido.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Para agendar uma tarefa para o último dia de cada mês

O comando a seguir agenda o programa MyApp para ser executado no último dia de cada mês. Ele usa o **/mês** parâmetro para especificar o último dia e o **/m** parâmetro com o caractere curinga (*) para indicar que o programa é executado a cada mês.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Para agendar uma tarefa às 6:00. nos últimos dias de fevereiro e março

O comando a seguir agenda o programa MyApp para executar no último dia de fevereiro e o último dia de março às 6:00 Ele usa o **/mês** parâmetro para especificar o último dia, o **/m** parâmetro para especificar os meses e o **/st** parâmetro para especificar a hora de início.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>Para agendar uma tarefa que executa uma vez

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento de execução única, o **uma vez /sc** parâmetro é obrigatório. O **/st** parâmetro, que especifica a hora em que a tarefa é executada, é necessário. O **/sd** parâmetro, que especifica a data em que a tarefa é executada, é opcional. O **/mês** (modificador) e **/ed** parâmetros (data de término) não são válidos para esse tipo de agendamento.

**SCHTASKS** não permite que você agendar uma tarefa para executar uma vez se a data e hora especificadas estão no passado, com base na hora do computador local. Para agendar uma tarefa que executa uma vez em um computador remoto em um fuso horário diferente, você deve programá-lo antes da data e hora ocorre no computador local.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Para agendar uma tarefa que é executada uma vez

O comando a seguir agenda o programa MyApp para ser executado à meia-noite de 1º de janeiro de 2003. Ele usa o **/sc** parâmetro para especificar o tipo de agendamento e o **/sd** e **st** para especificar a data e hora.

Porque o computador local usa o **inglês (Estados Unidos)** opção **opções regionais e idioma** na **painel de controle**, o formato da data de início é MM/DD/AAAA.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>Para agendar uma tarefa que é executada sempre que o sistema é iniciado

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

No tipo de agendamento de inicialização, o **/sc onstart** parâmetro é obrigatório. O **/sd** parâmetro (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Para agendar uma tarefa que é executado quando o sistema é iniciado

O comando a seguir agenda o programa MyApp para ser executado sempre que o sistema é iniciado, começando em 15 de março de 2001:

Porque o computador local é usa o **inglês (Estados Unidos)** opção **opções regionais e idioma** na **painel de controle**, o formato da data de início é MM/DD/AAAA .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>Para agendar uma tarefa que é executado quando um usuário fizer logon

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

O tipo de agendamento "no logon" agenda uma tarefa que é executado sempre que qualquer usuário fizer logon no computador. No tipo de agendamento "no logon", o **no /sc login** parâmetro é obrigatório. O **/sd** parâmetro (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Para agendar uma tarefa que é executado quando um usuário faz logon um computador remoto

O comando a seguir agenda um arquivo em lotes para ser executado sempre que um usuário (qualquer usuário) faz logon no computador remoto. Ele usa o **/s** parâmetro para especificar o computador remoto. Como o comando é remoto, todos os caminhos de comando, incluindo o caminho para o arquivo em lotes, consultem um caminho no computador remoto.
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>Para agendar uma tarefa que é executado quando o sistema estiver ocioso

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentários

A agenda "quando ocioso" tipo de agenda uma tarefa que é executado sempre que não houver nenhuma atividade de usuário durante o tempo especificado pela **/i** parâmetro. No agendamento "quando ocioso" tipo, o **/sc onidle** parâmetro e o **/i** parâmetro são necessários. O **/sd** (data de início) é opcional e o padrão é a data atual.

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Para agendar uma tarefa que é executado sempre que o computador estiver ocioso

O comando a seguir agenda o programa MyApp para ser executado sempre que o computador estiver ocioso. Ele usa o necessária **/i** parâmetro para especificar que o computador deve permanecer ocioso por dez minutos antes do início da tarefa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>Para agendar uma tarefa que executa o agora

**SCHTASKS** não tem um "Executar agora" opção, mas você pode simular essa opção com a criação de uma tarefa que é executado uma vez e é iniciado em alguns minutos.

#### <a name="syntax"></a>Sintaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Exemplos

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Para agendar uma tarefa que é executado em alguns minutos a partir de agora.

O comando a seguir agenda uma tarefa para executar uma vez, em 13 de novembro de 2002 às 14h: 18 hora local.

Porque o computador local é usa o **inglês (Estados Unidos)** opção **opções regionais e idioma** na **painel de controle**, o formato da data de início é MM/DD/AAAA .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>Para agendar uma tarefa que é executado com permissões diferentes

Você pode agendar tarefas de todos os tipos para executar com permissões de uma conta alternativa em um computador remoto e o local. Além dos parâmetros necessários para o tipo de agendamento em particular, o **/ru** parâmetro é obrigatório e o **/rp** parâmetro é opcional.

#### <a name="examples"></a>Exemplos

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Para executar uma tarefa com permissões de administrador no computador local

O comando a seguir agenda o programa MyApp para ser executado no computador local. Ele usa o **/ru** para especificar que a tarefa deve ser executado com as permissões do usuário conta de administrador (Admin06). Neste exemplo, a tarefa está agendada para ser executado às terças-feiras, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões alternativas.
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
Em resposta, **SchTasks.exe** solicitará a senha de "Executar como" para a conta Admin06 e, em seguida, exibe uma mensagem de êxito.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Para executar uma tarefa com permissões alternativas em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador Marketing a cada quatro dias.

O comando usa o **/sc** parâmetro para especificar uma agenda diária e **/mês** parâmetro para especificar um intervalo de quatro dias.

O comando usa o **/s** parâmetro para fornecer o nome do computador remoto e o **/u** parâmetro para especificar uma conta com permissão para agendar uma tarefa no computador remoto (Admin01 de Marketing computador). Ele também usa o **/ru** parâmetro para especificar que a tarefa deve ser executado com as permissões da conta de administrador do usuário (User01 no domínio Reskits). Sem o **/ru** parâmetro, a tarefa seria executada com as permissões da conta especificada por **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**SCHTASKS** primeiro solicita a senha do usuário indicado pelo **/u** parâmetro (para executar o comando) e, em seguida, solicita a senha do usuário indicado pelo **/ru** parâmetro (para executar a tarefa). Depois de autenticar as senhas **schtasks** exibe uma mensagem indicando que a tarefa é agendada.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Para executar uma tarefa apenas quando um determinado usuário fizer logon

O comando a seguir agenda o programa de AdminCheck.exe para ser executado no computador público toda sexta-feira às 4:00, mas somente se o administrador do computador estiver conectado.

O comando usa o **/sc** parâmetro para especificar uma agenda semanal, a **/d** parâmetro para especificar o dia e o **/st** parâmetro para especificar a hora de início.

O comando usa o **/s** parâmetro para fornecer o nome do computador remoto e o **/u** parâmetro para especificar uma conta com permissão para agendar uma tarefa no computador remoto. Ele também usa o **/ru** parâmetro para configurar a tarefa seja executada com as permissões do administrador do computador público (Public\Admin01) e o **/it** parâmetro para indicar que a tarefa é executada somente Quando a conta Public\Admin01 está conectada.
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Observação**
-   Para identificar as tarefas com o interativo somente ( **/it**) propriedade, use uma consulta detalhada **(/ consulta /v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o **modo de Logon** campo tem um valor de **interativo apenas**.

### <a name="BKMK_sys_perms"></a>Para agendar uma tarefa executada com permissões do sistema

Tarefas de todos os tipos podem executar com permissões da conta do sistema em um computador remoto e o local. Além dos parâmetros necessários para o tipo de agendamento em particular, o **sistema /ru** (ou **/ru ""** ) o parâmetro é obrigatório e o **/rp** parâmetro não é válido.

**Importante**
-   A conta do sistema não tem direitos de logon interativo. Os usuários não podem ver ou interajam com programas ou tarefas executadas com permissões do sistema.
-   O **/ru** parâmetro determina as permissões sob a qual a tarefa é executada, não as permissões usadas para agendar a tarefa. Somente os administradores podem agendar tarefas, independentemente do valor de **/ru** parâmetro.

**Observação**

Para identificar as tarefas que são executados com permissões do sistema, use uma consulta detalhada ( **/consultar** **/v**). Em uma exibição de consulta detalhada de uma tarefa de execução do sistema, o **executar como usuário** campo tem um valor de **NT AUTHORITY\SYSTEM** e o **modo de Logon** campo tem um valor de  **Em segundo plano somente**.

#### <a name="examples"></a>Exemplos

#### <a name="to-run-a-task-with-system-permissions"></a>Para executar uma tarefa com permissões do sistema

O comando a seguir agenda o programa MyApp para ser executado no computador local com permissões da conta do sistema. Neste exemplo, a tarefa está agendada para ser executado no décimo-quinto dia de cada mês, mas você pode usar qualquer tipo de agendamento para uma tarefa executada com permissões do sistema.

O comando usa o **/ru System** parâmetro para especificar o contexto de segurança do sistema. Como as tarefas do sistema não usam uma senha, o **/rp** parâmetro for omitido.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
Em resposta, **SchTasks.exe** exibe uma mensagem informativa e uma mensagem de êxito. Ele não solicita uma senha.
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Para executar uma tarefa com permissões do sistema em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador Finance01 todas as manhãs às 4:00 A.M. com permissões do sistema.

O comando usa o **/tn** parâmetro para nomear a tarefa e o **/TR** parâmetro para especificar a cópia remota do programa MyApp. Ele usa o **/sc** parâmetro para especificar um agendamento diário, mas omite o **/mês** parâmetro porque 1 (cada dia) é o padrão. Ele usa o **/st** parâmetro para especificar a hora de início, que também é o tempo que a tarefa será executada a cada dia.

O comando usa o **/s** parâmetro para fornecer o nome do computador remoto e o **/u** parâmetro para especificar uma conta com permissão para agendar uma tarefa no computador remoto. Ele também usa o **/ru** parâmetro para especificar que a tarefa deve ser executado na conta do sistema. Sem o **/ru** parâmetro, a tarefa seria executada com as permissões da conta especificada por **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**SCHTASKS** solicita a senha do usuário indicado pelo **/u** parâmetro e, depois de autenticar a senha, exibe uma mensagem indicando que a tarefa é criada e que ele será executado com permissões do sistema conta.
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>Para agendar uma tarefa que executa mais de um programa

Cada tarefa executa somente um programa. No entanto, você pode criar um arquivo em lotes que executa vários programas e, em seguida, agendar uma tarefa para executar o arquivo em lotes. O procedimento a seguir demonstra esse método:
1. Crie um arquivo em lotes que inicia os programas que você deseja executar.

   Neste exemplo, você criará um arquivo em lotes que inicia o Visualizador de eventos (Eventvwr.exe) e o Monitor do sistema (Perfmon.exe).  
   - Abra um editor de texto, como o Bloco de Notas.
   - Digite o nome e o caminho totalmente qualificado para o arquivo executável para cada programa. Nesse caso, o arquivo inclui as instruções a seguir.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Salve o arquivo como Meus_apls.
2. Use **Schtasks.exe** para criar uma tarefa que executa Meus_apls.

   O comando a seguir cria a tarefa de monitoramento, que é executado sempre que um usuário faz logon. Ele usa o **/tn** parâmetro para nomear a tarefa e o **/TR** parâmetro para executar Meus_apls. Ele usa o **/sc** parâmetro para indicar o tipo de agenda no login e a **/ru** parâmetro para executar a tarefa com as permissões do usuário conta de administrador.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   Como resultado desse comando, sempre que um usuário faz logon no computador, a tarefa inicia o Visualizador de eventos e Monitor do sistema.

### <a name="BKMK_remote"></a>Para agendar uma tarefa que é executado em um computador remoto

Para agendar uma tarefa para execução em um computador remoto, você deve adicionar a tarefa a agenda de um computador remoto. Tarefas de todos os tipos podem ser agendadas em um computador remoto, mas as seguintes condições devem ser atendidas.
-   Você deve ter permissão para agendar a tarefa. Como tal, você deve estar conectado ao computador local com uma conta que seja membro do grupo Administradores no computador remoto, ou você deve usar o **/u** parâmetro para fornecer as credenciais de administrador do computador remoto .
-   Você pode usar o **/u** parâmetro somente quando os computadores locais e remotos estão no mesmo domínio ou computador local está em um domínio que confia no domínio do computador remoto. Caso contrário, o computador remoto não é possível autenticar a conta de usuário especificada e não é possível verificar que a conta é um membro do grupo Administradores.
-   A tarefa deve ter permissão suficiente para ser executado no computador remoto. As permissões necessárias variam de acordo com a tarefa. Por padrão, a tarefa é executada com a permissão do usuário atual do computador local ou, se o **/u** parâmetro é usado, a tarefa é executada com a permissão da conta especificada pela **/u** parâmetro. No entanto, você pode usar o **/ru** parâmetro para executar a tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema.

#### <a name="examples"></a>Exemplos

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Um administrador agenda uma tarefa em um computador remoto

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV01 a cada dez dias, iniciando imediatamente. O comando usa o **/s** parâmetro para fornecer o nome do computador remoto. Como o usuário local atual é um administrador do computador remoto, o **/u** parâmetro, que fornece permissões alternativas para o agendamento da tarefa, não é necessário.

Observe que, quando o agendamento de tarefas em um computador remoto, todos os parâmetros se referem ao computador remoto. Portanto, o arquivo executável especificado pelo **/TR** parâmetro refere-se à cópia do MyApp.exe no computador remoto.
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
Em resposta, **schtasks** exibe uma mensagem de êxito indicando que a tarefa é agendada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Agenda a um usuário de um comando em um computador remoto (caso 1)

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV06 a cada três horas. Como as permissões de administrador são necessários para agendar uma tarefa, o comando usa o **/u** e **/p** parâmetros para fornecer as credenciais de administrador do usuário da conta (Admin01 no Reskits domínio). Por padrão, essas permissões também são usadas para executar a tarefa. No entanto, como a tarefa não precisa de permissões de administrador para ser executado, o comando inclui o **/u** e **/rp** parâmetros para substituir o padrão e executar a tarefa com a permissão do usuário conta de não-administrador no computador remoto.
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
Em resposta, **schtasks** exibe uma mensagem de êxito indicando que a tarefa é agendada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Agenda a um usuário de um comando em um computador remoto (caso 2)

O comando a seguir agenda o programa MyApp para ser executado no computador remoto SRV02 no último dia de cada mês. Como o usuário local atual (user03) não é um administrador do computador remoto, o comando usa o **/u** parâmetro para fornecer as credenciais de administrador do usuário da conta (Admin01 no domínio Reskits). As permissões de conta de administrador serão usadas para agendar a tarefa e para executar a tarefa.
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Porque o comando não incluía a **/p** parâmetro (senha), **schtasks** solicitará a senha. Em seguida, ele exibirá uma mensagem de êxito e, nesse caso, um aviso.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
Este aviso indica que o domínio remoto não foi possível autenticar a conta especificada o **/u** parâmetro. Nesse caso, o domínio remoto não foi possível autenticar a conta de usuário porque o computador local não é um membro de um domínio que confia no domínio do computador remoto. Quando isso ocorrer, o trabalho aparece na lista de tarefas agendadas, mas a tarefa está vazia, na verdade, e ele não será executado.

A exibição de uma consulta detalhada a seguir expõe o problema com a tarefa. Na tela, observe que o valor de **próximo tempo de execução** é **Never** e que o valor de **executar como usuário** é **não pôde ser recuperado do Agendador de tarefas banco de dados**.

Este computador fosse um membro do mesmo domínio ou um domínio confiável, a tarefa poderiam ter sido agendada com êxito e executada conforme especificado.
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

-   Para executar uma **/ criar** comando com as permissões de um usuário diferente, use o **/u** parâmetro. O **/u** parâmetro é válido somente para o agendamento de tarefas em computadores remotos.
-   Para exibir mais **schtasks /Create** exemplos, digite **schtasks / criar /?** em um prompt de comando.
-   Para agendar uma tarefa que é executado com permissões de um usuário diferente, use o **/ru** parâmetro. O **/ru** parâmetro é válido para tarefas em computadores locais e remotos.
-   Para usar o **/u** parâmetro, o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio que confia no domínio do computador remoto. Caso contrário, a tarefa não for criada, ou o trabalho está vazio e a tarefa não é executado.
-   **SCHTASKS** sempre solicitará uma senha, a menos que você fornece um, mesmo quando você agenda uma tarefa no computador local usando a conta de usuário atual. Esse comportamento é normal para **schtasks**.
-   **SCHTASKS** não verifica os locais de arquivos de programa ou senhas de conta de usuário. Se você não inserir a localização de arquivo ou a senha correta para a conta de usuário, a tarefa é criada, mas ele não será executado. Além disso, se a senha de uma conta for alterada ou expira e você não alterar a senha salva na tarefa, em seguida, a tarefa não será executado.
-   A conta do sistema não tem direitos de logon interativo. Os usuários não aparecerem e não podem interagir com programas executados com permissões do sistema.
-   Cada tarefa executa somente um programa. No entanto, você pode criar um arquivo em lotes que inicia diversas tarefas e, em seguida, agendar uma tarefa que executa o arquivo em lotes.
-   Você pode testar uma tarefa assim que criá-lo. Use o **executados** operação para testar a tarefa e, em seguida, verifique o arquivo de SchedLgU (*SystemRoot*\SchedLgU.txt.) para erros.

## <a name="BKMK_change"></a>Alterar Schtasks

Altera um ou mais das seguintes propriedades de uma tarefa.
-   O programa que a tarefa é executada ( **/TR**).
-   A conta de usuário sob a qual a tarefa é executada ( **/ru**).
-   A senha da conta de usuário ( **/rp**).
-   Adiciona a propriedade interativa apenas à tarefa ( **/it**).

### <a name="syntax"></a>Sintaxe

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parâmetros

|          Termo           |                                                                                                                                                                                                                                                                                                                                     Definição                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /tn \<TaskName>     |                                                                                                                                                                                                                                                                                                               Identifica a tarefa a ser alterado. Insira o nome da tarefa.                                                                                                                                                                                                                                                                                                               |
|     /s \<Computer>      |                                                                                                                                                                                                                                                                               Especifica o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                                                                                                                                                                                               |
|  /u [\<Domain>\]<User>  |                                                                                                                                                                 Executa este comando com as permissões da conta de usuário especificada. O padrão é que as permissões do usuário atual do computador local. A conta de usuário especificado deve ser um membro do grupo Administradores no computador remoto. O **/u** e **/p** parâmetros são válidos somente para alterar uma tarefa em um computador remoto ( **/s**).                                                                                                                                                                  |
|     /p \<Password>      |                                                                                                                                                                                              Especifica a senha da conta de usuário especificada na **/u** parâmetro. Se você usar o **/u** parâmetro, mas omita a **/p** parâmetro ou o argumento de senha, **schtasks** solicitará uma senha.</br>O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       Sistema}                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<senha >     |                                                                                                                                                                                                                                                 Especifica uma nova senha para a conta de usuário existente ou a conta de usuário especificada pela **/ru** parâmetro. Esse parâmetro é ignorado quando usado com a conta sistema local.                                                                                                                                                                                                                                                  |
|     /tr \<TaskRun>      |                                                                                                                                                                                  Altera o programa executado pela tarefa. Insira o nome de arquivo e caminho totalmente qualificado de um arquivo executável, arquivo de script ou arquivo em lotes. Se você omitir o caminho **schtasks** supõe que o arquivo está no \<systemroot > diretório \System32. O programa especificado substitui o programa original executado pela tarefa.                                                                                                                                                                                  |
|    /ST \<Starttime >     |                                                                                                                                                                                                                                                              Especifica a hora de início da tarefa, usando o formato de 24 horas, hh: mm. Por exemplo, um valor de 14:30 é equivalente à hora de 12 horas da 2:30 PM.                                                                                                                                                                                                                                                               |
|     /RI \<intervalo >     |                                                                                                                                                                                                                                                                           Especifica o intervalo de repetição para a tarefa agendada, em minutos. Intervalo válido é de 1-599940 (599940 minutos = 9999 horas).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime>      |                                                                                                                                                                                                                                                               Especifica a hora de término da tarefa, usando o formato de 24 horas, hh: mm. Por exemplo, um valor de 14:30 é equivalente à hora de 12 horas da 2:30 PM.                                                                                                                                                                                                                                                                |
|     /du \<Duration>     |                                                                                                                                                                                                                                                                                                     Especifica para fechar a tarefa durante o \<EndTime > ou <Duration>, se especificado.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Interrompe o programa que a tarefa é executada no momento especificado por **/et** ou **/du**. Sem **/k**, **schtasks** não inicia o programa novamente depois que ele atinge o tempo especificado por **/et** ou **/du**, mas não interrompe o programa se ele ainda está em execução. Esse parâmetro é opcional e é válido somente com um agendamento por hora ou minutos.                                                                                                                                                                   |
|    /SD \<StartDate >     |                                                                                                                                                                                                                                                                                              Especifica a primeira data em que a tarefa deve ser executada. O formato de data é MM/DD/AAAA.                                                                                                                                                                                                                                                                                               |
|     /ED \<EndDate >      |                                                                                                                                                                                                                                                                                                 Especifica a última data na qual a tarefa deve ser executada. O formato é MM/DD/AAAA.                                                                                                                                                                                                                                                                                                  |
|         / HABILITAR         |                                                                                                                                                                                                                                                                                                                       Especifica para habilitar a tarefa agendada.                                                                                                                                                                                                                                                                                                                       |
|        / DESABILITAR         |                                                                                                                                                                                                                                                                                                                      Especifica para desabilitar a tarefa agendada.                                                                                                                                                                                                                                                                                                                       |
|           /it           | Especifica para executar a tarefa agendada somente quando o usuário "Executar como" (a conta de usuário sob a qual a tarefa é executada) fizer logon no computador.</br>Esse parâmetro não tem nenhum efeito nas tarefas que são executados com permissões do sistema ou tarefas que já têm a propriedade interativa apenas definida. Você não pode usar um comando de alteração para remover a propriedade interativa apenas de uma tarefa.</br>Por padrão, o usuário "Executar como" é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pela **/u** parâmetro, se um for usado. No entanto, se o comando inclui o **/ru** parâmetro, o usuário, em seguida, o "Executar como" é a conta especificada pela **/ru** parâmetro. |
|           /z            |                                                                                                                                                                                                                                                                                                          Especifica para excluir a tarefa após a conclusão da sua agenda.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Comentários

-   O **/tn** e **/s** parâmetros identificam a tarefa. O **/TR**, **/ru**, e **/rp** os parâmetros especificam as propriedades da tarefa que você pode alterar.
-   O **/ru**, e **/rp** parâmetros especificam as permissões sob a qual a tarefa é executada. O **/u** e **p** os parâmetros especificam as permissões usadas para alterar a tarefa.
-   Para alterar as tarefas em um computador remoto, o usuário deve fazer logon no computador local com uma conta que seja membro do grupo Administradores no computador remoto.
-   Para executar uma **/alterar** comando com as permissões de um usuário diferente ( **/u**, **/p**), o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio que confia no domínio do computador remoto.
-   A conta do sistema não tem direitos de logon interativo. Os usuários não aparecerem e não podem interagir com programas executados com permissões do sistema.
-   Para identificar as tarefas com o **/it** propriedade, use uma consulta detalhada ( **/consulta /v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o **modo de Logon** campo tem um valor de **interativo apenas**.

### <a name="examples"></a>Exemplos

### <a name="to-change-the-program-that-a-task-runs"></a>Para alterar o programa que executa uma tarefa

O comando a seguir altera o programa que a tarefa de verificação de vírus é executado de VirusCheck.exe VirusCheck2.exe. Esse comando usa o **/tn** parâmetro para identificar a tarefa e o **/TR** parâmetro para especificar o novo programa para a tarefa. (Não é possível alterar o nome da tarefa.)
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
Em resposta, **SchTasks.exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
Como resultado desse comando, a tarefa de verificação de vírus agora executa VirusCheck2.exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Para alterar a senha de uma tarefa remota

O comando a seguir altera a senha da conta de usuário para a tarefa Lembrar_me no computador remoto, Svr01. O comando usa o **/tn** parâmetro para identificar a tarefa e o **/s** parâmetro para especificar o computador remoto. Ele usa o **/rp** parâmetro para especificar a nova senha, p@ssWord3.

Esse procedimento é necessário sempre que a senha para uma conta de usuário expira ou é alterada. Se a senha salva em uma tarefa não é mais válida, em seguida, a tarefa não será executado.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
Em resposta, **SchTasks.exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
Como resultado desse comando, a tarefa Lembrar_me agora executa em sua conta de usuário original, mas com uma nova senha.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Para alterar a conta de usuário e programa para uma tarefa

O comando a seguir altera o programa que executa uma tarefa e as alterações que a conta de usuário na qual a tarefa é executado. Essencialmente, ele usa um agendamento antigo para uma nova tarefa. Esse comando altera a tarefa ChkNews, que inicia o Notepad.exe todas as manhãs às 9h00, para iniciar o Internet Explorer.

O comando usa o **/tn** parâmetro para identificar a tarefa. Ele usa o **/TR** parâmetro para alterar o programa que a tarefa é executada e o **/ru** parâmetro para alterar a conta de usuário sob a qual a tarefa é executada.

O **/ru**, e **/rp** parâmetro, que fornece a senha da conta de usuário, é omitido. Você deve fornecer uma senha para a conta, mas você pode usar o **/ru**, e **/rp** parâmetro e digite a senha em texto descriptografado ou aguarde até **SchTasks.exe** para avisá-lo para um senha e, em seguida, digite a senha em texto obscurecido.
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
Em resposta, **SchTasks.exe** solicitará a senha da conta de usuário. Obscurece o texto digitado, portanto, a senha não é visível.
```
Please enter the password for DomainX\Admin01: 
```
Observe que o **/tn** parâmetro identifica a tarefa e que o **/TR** e **/ru** parâmetros alterar as propriedades da tarefa. Você não pode usar outro parâmetro para identificar a tarefa e você não pode alterar o nome da tarefa.

Em resposta, **SchTasks.exe** exibe a seguinte mensagem de êxito:
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
Como resultado desse comando, a tarefa ChkNews agora executa o Internet Explorer com as permissões de uma conta de administrador.

### <a name="to-change-a-program-to-the-system-account"></a>Para alterar um programa para a conta do sistema

O comando a seguir altera a tarefa Script_de_segurança para que ele seja executado com permissões da conta do sistema. Ele usa o **/ru ""** parâmetro para indicar a conta do sistema.
```
schtasks /change /tn SecurityScript /ru ""
```
Em resposta, **SchTasks.exe** exibe a seguinte mensagem de êxito:
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
Como as tarefas executadas com permissões de conta do sistema não exigem uma senha **SchTasks.exe** não solicitará um.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Para executar um programa apenas quando estou conectado

O comando a seguir adiciona a propriedade interativa apenas a MyApp, uma tarefa existente. Essa propriedade garante que a tarefa é executada somente quando o usuário "Executar como", ou seja, a conta de usuário sob a qual a tarefa é executada, faz logon no computador.

O comando usa o **/tn** parâmetro para identificar a tarefa e o **/it** parâmetro para adicionar a propriedade interativa apenas à tarefa. Como a tarefa já é executado com as permissões da minha conta de usuário, não preciso alterar o **/ru** parâmetro para a tarefa.
```
schtasks /change /tn MyApp /it
```
Em resposta, **SchTasks.exe** exibe a seguinte mensagem de êxito.
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>schtasks run

Inicia uma tarefa agendada imediatamente. O **executar** operação ignora o agendamento, mas usa o local do arquivo de programa, a conta de usuário e a senha salvas na tarefa para executar a tarefa imediatamente.

### <a name="syntax"></a>Sintaxe

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                                       Obrigatório. Identifica a tarefa.                                                                                                                                                        |
|    /s \<Computer>     |                                                                                                           Especifica o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<Domain>\]<User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificado deve ser um membro do grupo Administradores no computador remoto. O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**. |
|    /p \<Password>     |                          Especifica a senha da conta de usuário especificada na **/u** parâmetro. Se você usar o **/u** parâmetro, mas omita a **/p** parâmetro ou o argumento de senha, **schtasks** solicitará uma senha.</br>O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.                           |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

-   Use essa operação para testar suas tarefas. Se uma tarefa não for executado, verifique o log de transação de serviço Agendador de tarefas, \<Systemroot > \SchedLgU.txt. erros.
-   Executar uma tarefa não afeta o agendamento de tarefas e não altera o próximo tempo de execução agendado para a tarefa.
-   Para executar uma tarefa remotamente, a tarefa deve ser agendada no computador remoto. Quando você executá-lo, a tarefa é executada somente no computador remoto. Para verificar se uma tarefa está em execução em um computador remoto, use o Gerenciador de tarefas ou o log de transações do Agendador de tarefas, \<Systemroot > \SchedLgU.txt.

### <a name="examples"></a>Exemplos

### <a name="to-run-a-task-on-the-local-computer"></a>Para executar uma tarefa no computador local

O comando a seguir inicia a tarefa "Script de segurança".
```
schtasks /run /tn "Security Script"
```
Em resposta, **SchTasks.exe** inicia o script associado à tarefa e exibe a seguinte mensagem:
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
Como indica a mensagem, **schtasks** tenta iniciar o programa, mas ele não pode verificar que o programa foi iniciado.

### <a name="to-run-a-task-on-a-remote-computer"></a>Para executar uma tarefa em um computador remoto

O comando a seguir inicia a tarefa de atualização em um computador remoto, Svr01:
```
schtasks /run /tn Update /s Svr01
```
Nesse caso, **SchTasks.exe** exibe a seguinte mensagem de erro:
```
ERROR: Unable to run the scheduled task "Update".
```
Para encontrar a causa do erro, examine o log de transações de tarefas agendadas, C:\Windows\SchedLgU.txt no Serv01. Nesse caso, a seguinte entrada aparece no log:
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Aparentemente, o nome de usuário ou senha na tarefa não é válida no sistema. O seguinte **schtasks /change** comando atualiza o nome de usuário e senha para a tarefa de atualização no Serv01:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Após o **alterar** comando for concluído, o **executar** comando é repetido. Neste momento, o programa de Update.exe inicia e **SchTasks.exe** exibe a seguinte mensagem:
```
SUCCESS: Attempted to run the scheduled task "Update".
```
Como indica a mensagem, **schtasks** tenta iniciar o programa, mas ele não pode verificar que o programa foi iniciado.

## <a name="BKMK_end"></a>schtasks end

Interrompe um programa iniciado por uma tarefa.

### <a name="syntax"></a>Sintaxe

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                               Definição                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                         Obrigatório. Identifica a tarefa que iniciou o programa.                                                                                                                                         |
|    /s \<Computer>     |                                                                                                                        Especifica o nome ou endereço IP de um computador remoto. O padrão é o computador local.                                                                                                                        |
| /u [\<Domain>\]<User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificado deve ser um membro do grupo Administradores no computador remoto. O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**. |
|    /p \<Password>     |                        Especifica a senha da conta de usuário especificada na **/u** parâmetro. Se você usar o **/u** parâmetro, mas omita a **/p** parâmetro ou o argumento de senha, **schtasks** solicitará uma senha.</br>O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.                         |
|          /?           |                                                                                                                                                             Exibe a Ajuda.                                                                                                                                                              |

### <a name="remarks"></a>Comentários

**SchTasks.exe** termina somente as instâncias de um programa iniciado por uma tarefa agendada. Para interromper outros processos, use TaskKill. Para obter mais informações, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Exemplos

### <a name="to-end-a-task-on-a-local-computer"></a>Para finalizar uma tarefa em um computador local

O comando a seguir interrompe a instância do Notepad.exe que foi iniciado pela tarefa meu bloco de notas:
```
schtasks /end /tn "My Notepad"
```
Em resposta, **SchTasks.exe** interrompe a instância do Notepad.exe que a tarefa iniciada e exibirá a seguinte mensagem de êxito:
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Para finalizar uma tarefa em um computador remoto

O comando a seguir interrompe a instância do Internet Explorer foi iniciado pela tarefa InternetOn no computador remoto, Serv01:
```
schtasks /end /tn InternetOn /s Svr01
```
Em resposta, **SchTasks.exe** interrompe a instância do Internet Explorer que a tarefa iniciada e exibirá a seguinte mensagem de êxito:
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks delete

Exclui uma tarefa agendada.

### <a name="syntax"></a>Sintaxe

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /tn {\<TaskName>    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Suprime a mensagem de confirmação. A tarefa é excluída sem aviso.                                                                                                                                  |
|    /s \<Computer>     |                                                                                                           Especifica o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<Domain>\]<User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificado deve ser um membro do grupo Administradores no computador remoto. O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**. |
|    /p \<Password>     |                          Especifica a senha da conta de usuário especificada na **/u** parâmetro. Se você usar o **/u** parâmetro, mas omita a **/p** parâmetro ou o argumento de senha, **schtasks** solicitará uma senha.</br>O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.                           |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

- O **excluir** operação exclui a tarefa do agendamento. Ele não exclui o programa executado pela tarefa nem interrompe um programa em execução.
- O **exclua \\** * comando exclui todas as tarefas agendadas no computador, não apenas as tarefas agendadas pelo usuário atual.

### <a name="examples"></a>Exemplos

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Para excluir uma tarefa do agendamento de um computador remoto

O comando a seguir exclui a tarefa "Iniciar email" do agendamento de um computador remoto. Ele usa o **/s** parâmetro para identificar o computador remoto.
```
schtasks /delete /tn "Start Mail" /s Svr16
```
Em resposta, **SchTasks.exe** exibe a seguinte mensagem de confirmação. Para excluir a tarefa, pressione Y<strong>.</strong> Para cancelar o comando, digite **n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Para excluir todas as tarefas agendadas para o computador local

O comando a seguir exclui todas as tarefas do agendamento do computador local, incluindo tarefas agendadas por outros usuários. Ele usa o **/tn \\** * parâmetro para representar todas as tarefas no computador e o **/f** parâmetro para suprimir a mensagem de confirmação.
```
schtasks /delete /tn * /f
```
Em resposta, **SchTasks.exe** exibe as seguintes mensagens de êxito indicando que a única tarefa agendada, Script_de_segurança, foi excluída.

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>schtasks query

Exibe tarefas agendadas para execução no computador.

### <a name="syntax"></a>Sintaxe

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parâmetros

|         Termo          |                                                                                                                                                                 Definição                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [/query]        |                                                                                                                        O nome da operação é opcional. Digitação **schtasks** sem quaisquer parâmetros executa uma consulta.                                                                                                                         |
|      /FO {tabela       |                                                                                                                                                                    LISTA                                                                                                                                                                     |
|          /nh          |                                                                                                            Omite os cabeçalhos de coluna da exibição de tabela. Esse parâmetro é válido com o **tabela** e **CSV** formatos de saída.                                                                                                             |
|          /v           |                                                                                                         Adiciona propriedades avançadas de tarefas à exibição.</br>Consultas usando **/v** deve ser formatado como **lista** ou **CSV**.                                                                                                          |
|    /s \<Computer>     |                                                                                                           Especifica o nome ou endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local.                                                                                                           |
| /u [\<Domain>\]<User> | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local.</br>A conta de usuário especificado deve ser um membro do grupo Administradores no computador remoto. O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**. |
|    /p \<Password>     |                                        Especifica a senha da conta de usuário especificada na **/u** parâmetro. Se você usar **/u**, mas omita **/p** ou o argumento de senha **schtasks** solicitará uma senha.</br>O **/u** e **/p** parâmetros são válidos somente quando você usa **/s**.                                         |
|          /?           |                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                     |

### <a name="remarks"></a>Comentários

**SchTasks.exe** termina somente as instâncias de um programa iniciado por uma tarefa agendada. Para interromper outros processos, use TaskKill. Para obter mais informações, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Exemplos

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Para exibir as tarefas agendadas no computador local

Os comandos a seguir exibem todas as tarefas agendadas para o computador local. Esses comandos produzem o mesmo resultado e podem ser usados alternadamente.
```
schtasks
schtasks /query
```
Em resposta, **SchTasks.exe** exibe as tarefas do padrão, o formato de tabela simples, conforme mostrado na tabela a seguir:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Para exibir as propriedades avançadas de tarefas agendadas

O comando a seguir solicita uma exibição detalhada das tarefas no computador local. Ele usa o **/v** parâmetro para solicitar uma exibição detalhada de (detalhada) e o **/fo lista** parâmetro para formatar a exibição como uma lista para facilitar a leitura. Você pode usar esse comando para verificar se uma tarefa que você criou tem o padrão de recorrência pretendido.

**schtasks /query /fo LIST /v**

Em resposta, **SchTasks.exe** exibe uma lista de propriedades detalhadas para todas as tarefas. A exibição a seguir mostra a lista de tarefas para uma tarefa agendada para ser executada às 4:00. na última sexta-feira de cada mês:
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

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Para registrar tarefas agendadas em um computador remoto

O comando a seguir solicita uma lista de tarefas agendadas em um computador remoto e adiciona as tarefas em um arquivo de log separados por vírgulas no computador local. Você pode usar esse formato de comando para coletar e acompanhar tarefas que estão agendadas para vários computadores.

O comando usa o **/s** parâmetro para identificar o computador remoto, Reskit16, o **/fo** parâmetro para especificar o formato e o **/nh** parâmetro para suprimir a coluna títulos. O **>>** acrescentar a saída de redirecionamentos de símbolo no log de tarefa, p0102 no computador local, Serv01. Como o comando é executado no computador remoto, o caminho do computador local deve ser totalmente qualificado.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
Em resposta, **SchTasks.exe** adiciona as tarefas agendadas no computador Reskit16 ao arquivo p0102 no computador local, Serv01.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
