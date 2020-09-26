---
title: troca de Schtasks
description: Artigo de referência para o comando SCHTASKS Change, que agenda comandos e programas para execução periódica ou em um momento específico, adiciona e remove tarefas da agenda, inicia e interrompe as tarefas sob demanda e exibe e altera as tarefas agendadas.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 06cbceaaa0e428743a725127d9495759dd0097e1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390680"
---
# <a name="schtasks-change"></a>troca de Schtasks

Altera uma ou mais das seguintes propriedades de uma tarefa:

- O programa que a tarefa executa (**/TR**)

- A conta de usuário sob a qual a tarefa é executada (**/ru**)

- A senha da conta de usuário (**/RP**)

- Adiciona a propriedade somente interativa à tarefa (**/it**)

## <a name="required-permissions"></a>Permissões necessárias

- Para agendar, exibir e alterar todas as tarefas no computador local, você deve ser um membro do grupo Administradores.

- Para agendar, exibir e alterar todas as tarefas no computador remoto, você deve ser um membro do grupo Administradores no computador remoto ou deve usar o parâmetro **/u** para fornecer as credenciais de um administrador do computador remoto.

- Você pode usar o parâmetro **/u** em uma operação **/Create** ou **/Change** se os computadores locais e remotos estiverem no mesmo domínio ou se o computador local estiver em um domínio no qual o domínio do computador remoto confia. Caso contrário, o computador remoto não poderá autenticar a conta de usuário especificada e não conseguirá verificar se a conta é membro do grupo Administradores.

- A tarefa que você planeja executar deve ter a permissão apropriada; essas permissões variam por tarefa. Por padrão, as tarefas são executadas com as permissões do usuário atual do computador local ou com as permissões do usuário especificado pelo parâmetro **/u** , se houver uma incluída. o executar uma tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema, use o parâmetro **/ru** .

## <a name="syntax"></a>Sintaxe

```
schtasks /change /tn <Taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru <username>] [/rp <password>] [/tr <Taskrun>] [/st <Starttime>] [/ri <interval>] [{/et <Endtime> | /du <duration>} [/k]] [/sd <Startdate>] [/ed <Enddate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /TN `<Taskname>` | Identifica a tarefa a ser alterada. Insira o nome da tarefa. |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /ru `<username>` | Altera o nome de usuário sob o qual a tarefa agendada deve ser executada. Para a conta do sistema, os valores válidos são *""*, *"NT AUTHORITY\SYSTEM"* ou *"System"*. |
| /RP `<password>` | Especifica uma nova senha para a conta de usuário existente ou a conta de usuário especificada pelo parâmetro **/ru** . Esse parâmetro é ignorado com o usado com a conta sistema local. |
| /TR `<Taskrun>` | Altera o programa que a tarefa executa. Insira o caminho totalmente qualificado e o nome de arquivo de um arquivo executável, arquivo de script ou arquivo em lotes. Se você não adicionar o caminho, **Schtasks** assumirá que o arquivo está no `<systemroot>\System32` diretório. O programa especificado substitui o programa original executado pela tarefa. |
| /St `<Starttime>` | Especifica a hora de início para a tarefa, usando o formato de 24 horas, HH: mm. Por exemplo, um valor de 14:30 é equivalente ao tempo de 12 horas de 2:30 PM. |
| /ri `<interval>` | Especifica o intervalo de repetição para a tarefa agendada, em minutos. O intervalo válido é 1-599940 (599940 minutos = 9999 horas). Se os parâmetros **/et** ou **/du** forem especificados, o padrão será **10 minutos**. |
| /et `<Endtime>` | Especifica a hora de término da tarefa, usando o formato de 24 horas, HH: mm. Por exemplo, um valor de 14:30 é equivalente ao tempo de 12 horas de 2:30 PM. |
| /du `<duration>` | Um valor que especifica a duração da execução da tarefa. O formato de hora é HH: mm (hora de 24 horas). Por exemplo, um valor de 14:30 é equivalente ao tempo de 12 horas de 2:30 PM. |
| /k | Interrompe o programa que a tarefa executa no momento especificado por **/et** ou **/du**. Sem **/k**, Schtasks não inicia o programa novamente depois que ele atingir o tempo especificado por **/et** ou **/du** , e não interromperá o programa se ele ainda estiver em execução. Esse parâmetro é opcional e válido somente com um agendamento de minuto ou hora. |
| /SD `<Startdate>` | Especifica a primeira data em que a tarefa deve ser executada. O formato de data é MM/DD/AAAA. |
| /Ed `<Enddate>` | Especifica a última data em que a tarefa deve ser executada. O formato é MM/DD/AAAA. |
| /ENABLE | Especifica a habilitação da tarefa agendada. |
| /DISABLE | Especifica a desabilitação da tarefa agendada. |
| /It | Especifica a execução da tarefa agendada somente quando o usuário executar como (a conta de usuário sob a qual a tarefa é executada) está conectado ao computador. Esse parâmetro não tem nenhum efeito nas tarefas que são executadas com permissões do sistema ou tarefas que já têm a propriedade somente interativa definida. Você não pode usar um comando Change para remover a propriedade somente interativa de uma tarefa. Por padrão, executar como usuário é o usuário atual do computador local quando a tarefa é agendada ou a conta especificada pelo parâmetro **/u** , se uma for usada. No entanto, se o comando incluir o parâmetro **/ru** , o usuário executar como será a conta especificada pelo parâmetro **/ru** . |
| /z | Especifica a exclusão da tarefa após a conclusão de sua agenda. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os parâmetros **/TN** e **/s** identificam a tarefa. Os parâmetros **/TR**, **/ru**e **/RP** especificam as propriedades da tarefa que você pode alterar.

- Os parâmetros **/ru** e **/RP** especificam as permissões sob as quais a tarefa é executada. Os parâmetros **/u** e **/p** especificam as permissões usadas para alterar a tarefa.

- Para alterar tarefas em um computador remoto, o usuário deve estar conectado ao computador local com uma conta que seja membro do grupo de administradores no computador remoto.

- Para executar um comando **/Change** com as permissões de um usuário diferente (**/u**, **/p**), o computador local deve estar no mesmo domínio que o computador remoto ou deve estar em um domínio no qual o domínio do computador remoto confia.

- A conta do sistema não tem direitos de logon interativos. Os usuários não veem e não podem interagir com o, os programas são executados com permissões do sistema.
Para identificar tarefas com a propriedade **/it** , use uma consulta detalhada (**/Query/v**). Em uma exibição de consulta detalhada de uma tarefa com **/it**, o campo de modo de logon tem um valor somente interativo.

## <a name="examples"></a>Exemplos

Para alterar o programa em que a tarefa de verificação de vírus é executada *VirusCheck.exe* *VirusCheck2.exe*, digite:

```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```

Esse comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/TR** para especificar o novo programa para a tarefa. (Não é possível alterar o nome da tarefa.)

Para alterar a senha da conta de usuário para a tarefa *RemindMe* no computador remoto, *SVR01*, digite:

```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```

Esse procedimento é necessário sempre que a senha de uma conta de usuário expirar ou for alterada. Se a senha salva em uma tarefa não for mais válida, a tarefa não será executada. O comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/s** para especificar o computador remoto. Ele usa o parâmetro **/RP** para especificar a nova senha, *p@ssWord3* .

Para alterar a tarefa ChkNews, que inicia Notepad.exe todas as manhãs às 9:00 A.M., para iniciar o Internet Explorer, digite:

```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```

O comando usa o parâmetro **/TN** para identificar a tarefa. Ele usa o parâmetro **/TR** para alterar o programa que a tarefa executa e o parâmetro/ru para alterar a conta de usuário na qual a tarefa é executada. Os parâmetros **/ru** e **/RP** , que fornecem a senha para a conta de usuário, não são usados. Você deve fornecer uma senha para a conta, mas pode usar o parâmetro **/ru** e **/RP** e digitar a senha em texto não criptografado, ou aguardar SchTasks.exe solicitar uma senha e, em seguida, digitar a senha em um texto obscuro.

Para alterar a tarefa SecurityScript para que ela seja executada com permissões da conta do sistema, digite:

```
schtasks /change /tn SecurityScript /ru
```

O comando usa o parâmetro **/ru** para indicar a conta do sistema. Como as tarefas executadas com permissões de conta do sistema não exigem uma senha, SchTasks.exe não solicita uma.

Para adicionar a propriedade somente interativa a MyApp, uma tarefa existente, digite:

```
schtasks /change /tn MyApp /it
```

Essa propriedade garante que a tarefa seja executada somente quando o usuário executar como, ou seja, a conta de usuário sob a qual a tarefa é executada, está conectado ao computador. O comando usa o parâmetro **/TN** para identificar a tarefa e o parâmetro **/it** para adicionar a propriedade interativa somente à tarefa. Como a tarefa já é executada com as permissões da minha conta de usuário, você não precisa alterar o parâmetro **/ru** para a tarefa.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando SCHTASKS CREATE](schtasks-create.md)

- [comando de exclusão de Schtasks](schtasks-delete.md)

- [comando de término de Schtasks](schtasks-end.md)

- [comando de consulta Schtasks](schtasks-query.md)

- [comando Schtasks execute](schtasks-run.md)
