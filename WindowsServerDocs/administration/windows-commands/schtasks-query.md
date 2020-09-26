---
title: consulta Schtasks
description: Artigo de referência para o comando de consulta Schtasks, que lista todas as tarefas agendadas para execução no computador.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: c1bff666f762b21e8dbcf2bff59383d49cab6409
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390674"
---
# <a name="schtasks-query"></a>consulta Schtasks

Lista todas as tarefas agendadas para execução no computador.

Sintaxe

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /Query | Opcionalmente, especifica o nome da operação. Usar essa consulta sem parâmetros executa uma consulta. |
| /Fo `<format>` | Especifica o formato de saída. Os valores válidos são *tabela*, *lista*ou *CSV*. |
| /NH | Remove títulos de coluna da exibição da tabela. Esse parâmetro é válido com os formatos de saída de *tabela* ou *CSV* . |
| /v | Adiciona as propriedades avançadas da tarefa à exibição. Esse parâmetro é válido com os formatos de saída de *lista* ou *CSV* . |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para listar todas as tarefas agendadas para o computador local, digite:

```
schtasks
schtasks /query
```

Esses comandos produzem o mesmo resultado e podem ser usados de forma intercambiável.

Para solicitar uma exibição detalhada das tarefas no computador local, digite:

```
schtasks /query /fo LIST /v
```

Esse comando usa o parâmetro **/v** para solicitar uma exibição detalhada (detalhada) e o parâmetro **/fo List** para formatar a exibição como uma lista para facilitar a leitura. Você pode usar esse comando para verificar se uma tarefa criada tem o padrão de recorrência pretendido.

Para solicitar uma lista de tarefas agendadas para um computador remoto e adicionar as tarefas a um arquivo de log separado por vírgula no computador local, digite:

```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```

Você pode usar esse formato de comando para coletar e controlar tarefas que estão agendadas para vários computadores. Esse comando usa o parâmetro **/s** para identificar o computador remoto, *Reskit16*, o parâmetro **/fo** para especificar o formato e o parâmetro **/NH** para suprimir os cabeçalhos de coluna. O **>>** símbolo de acréscimo redireciona a saída para o log de tarefas, *p0102.csv*, no computador local, *SVR01*. Como o comando é executado no computador remoto, o caminho do computador local deve ser totalmente qualificado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de alteração de Schtasks](schtasks-change.md)

- [comando SCHTASKS CREATE](schtasks-create.md)

- [comando de exclusão de Schtasks](schtasks-delete.md)

- [comando de término de Schtasks](schtasks-end.md)

- [comando Schtasks execute](schtasks-run.md)