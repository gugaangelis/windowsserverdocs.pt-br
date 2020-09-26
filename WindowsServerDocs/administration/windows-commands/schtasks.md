---
title: comandos Schtasks
description: Artigo de referência para os comandos Schtasks, que programa os comandos e programas para execução periódica ou em um momento específico, adiciona e remove tarefas da agenda, inicia e interrompe as tarefas sob demanda e exibe e altera as tarefas agendadas.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 50b0bd7f7a55a7aa5889c39e4bf9f4e582af7f3b
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388632"
---
# <a name="schtasks-commands"></a>comandos Schtasks

Agenda comandos e programas para serem executados periodicamente ou em um momento específico, adiciona e remove tarefas da agenda, inicia e interrompe as tarefas sob demanda e exibe e altera as tarefas agendadas.

> [!NOTE]
> A ferramenta de **schtasks.exe** executa as mesmas operações que **as tarefas agendadas** no **painel de controle**. Você pode usar essas ferramentas juntas e de forma intercambiável.

## <a name="required-permissions"></a>Permissões necessárias

- Para agendar, exibir e alterar todas as tarefas no computador local, você deve ser um membro do grupo Administradores.

- Para agendar, exibir e alterar todas as tarefas no computador remoto, você deve ser um membro do grupo Administradores no computador remoto ou deve usar o parâmetro **/u** para fornecer as credenciais de um administrador do computador remoto.

- Você pode usar o parâmetro **/u** em uma operação **/Create** ou **/Change** se os computadores locais e remotos estiverem no mesmo domínio ou se o computador local estiver em um domínio no qual o domínio do computador remoto confia. Caso contrário, o computador remoto não poderá autenticar a conta de usuário especificada e não conseguirá verificar se a conta é membro do grupo Administradores.

- A tarefa que você planeja executar deve ter a permissão apropriada; essas permissões variam por tarefa. Por padrão, as tarefas são executadas com as permissões do usuário atual do computador local ou com as permissões do usuário especificado pelo parâmetro **/u** , se houver uma incluída. o executar uma tarefa com permissões de uma conta de usuário diferente ou com permissões do sistema, use o parâmetro **/ru** .

## <a name="syntax"></a>Sintaxe

```
schtasks change
schtasks create
schtasks delete
schtasks end
schtasks query
schtasks run
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [troca de Schtasks](schtasks-change.md) | Altera uma ou mais das seguintes propriedades de uma tarefa:<ul><li>O programa que a tarefa executa (/TR)</li><li>A conta de usuário sob a qual a tarefa é executada (/ru)</li><li>A senha da conta de usuário (/RP)</li><li>Adiciona a propriedade somente interativa à tarefa (/IT)</li></ul> |
| [criar Schtasks](schtasks-create.md) | Agenda uma nova tarefa.|
| [exclusão de Schtasks](schtasks-delete.md) | Exclui uma tarefa agendada. |
| [término de Schtasks](schtasks-end.md) | Interrompe um programa iniciado por uma tarefa. |
| [consulta Schtasks](schtasks-query.md) | Exibe as tarefas agendadas para execução no computador. |
| [execução de Schtasks](schtasks-run.md) | Inicia uma tarefa agendada imediatamente. A operação de **execução** ignora o agendamento, mas usa o local do arquivo de programa, a conta de usuário e a senha salvos na tarefa para executar a tarefa imediatamente. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
