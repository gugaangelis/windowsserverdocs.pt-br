---
title: execução de Schtasks
description: Artigo de referência do comando Schtasks Run, que
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 9304e41c21899ce49bd6d4d4409ae3b47d3746aa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390673"
---
# <a name="schtasks-run"></a>execução de Schtasks

Inicia uma tarefa agendada imediatamente. A operação de execução ignora o agendamento, mas usa o local do arquivo de programa, a conta de usuário e a senha salvos na tarefa para executar a tarefa imediatamente. A execução de uma tarefa não afeta a agenda da tarefa e não altera o tempo de execução seguinte agendado para a tarefa.

## <a name="syntax"></a>Sintaxe

```
schtasks /run /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /TN `<taskname>` | Identifica a tarefa a ser iniciada. Este parâmetro é necessário. |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Use esta operação para testar suas tarefas. Se uma tarefa não for executada, verifique se há erros no log de transações do serviço Agendador de Tarefas `<Systemroot>\SchedLgU.txt` .

- Para executar uma tarefa remotamente, a tarefa deve ser agendada no computador remoto. Quando você executa a tarefa, ela é executada somente no computador remoto. Para verificar se uma tarefa está em execução em um computador remoto, use o Gerenciador de tarefas ou o log de transações do serviço Agendador de Tarefas, `<Systemroot>\SchedLgU.txt` .

## <a name="examples"></a>Exemplos

Para iniciar a tarefa *script de segurança* , digite:

```
schtasks /run /tn Security Script
```

Para iniciar a tarefa de *atualização* em um computador remoto, SVR01, digite:

```
schtasks /run /tn Update /s Svr01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de alteração de Schtasks](schtasks-change.md)

- [comando SCHTASKS CREATE](schtasks-create.md)

- [comando de exclusão de Schtasks](schtasks-delete.md)

- [comando de término de Schtasks](schtasks-end.md)

- [comando de consulta Schtasks](schtasks-query.md)