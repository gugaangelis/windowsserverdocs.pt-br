---
title: exclusão de Schtasks
description: Artigo de referência para o comando SCHTASKS Delete, que exclui uma tarefa agendada do agendamento.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: a5b090ac9520aeae5e603e0c47c0c225b8bd0eff
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390679"
---
# <a name="schtasks-delete"></a>exclusão de Schtasks

Exclui uma tarefa agendada da agenda. Esse comando não exclui o programa que a tarefa executa ou interrompe um programa em execução.

## <a name="syntax"></a>Sintaxe

```
schtasks /delete /tn {<taskname> | *} [/f] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /TN `{<taskname> | *}` | Identifica a tarefa a ser excluída. Se você usar o `*` , esse comando excluirá todas as tarefas agendadas para o computador, não apenas as tarefas agendadas pelo usuário atual. |
| /f | Suprime a mensagem de confirmação. A tarefa é excluída sem aviso. |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para excluir a tarefa *Iniciar email* do agendamento de um computador remoto.

```
schtasks /delete /tn Start Mail /s Svr16
```

Esse comando usa o parâmetro **/s** para identificar o computador remoto.

Para excluir todas as tarefas do agendamento do computador local, incluindo tarefas agendadas por outros usuários.

```
schtasks /delete /tn * /f
```

Esse comando usa o parâmetro **/tn &#42;** para representar todas as tarefas no computador e o parâmetro **/f** para suprimir a mensagem de confirmação.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de alteração de Schtasks](schtasks-change.md)

- [comando SCHTASKS CREATE](schtasks-create.md)

- [comando de término de Schtasks](schtasks-end.md)

- [comando de consulta Schtasks](schtasks-query.md)

- [comando Schtasks execute](schtasks-run.md)
