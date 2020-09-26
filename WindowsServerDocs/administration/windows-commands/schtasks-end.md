---
title: término de Schtasks
description: Artigo de referência para o comando schtasks end, que interrompe apenas as instâncias de um programa iniciadas por uma tarefa agendada.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 7075c8689a39c7bc9eba917298678977916e29fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390675"
---
# <a name="schtasks-end"></a>término de Schtasks

Interrompe apenas as instâncias de um programa iniciadas por uma tarefa agendada. Para interromper outros processos, você deve usar o comando [Taskkill](taskkill.md) .

## <a name="syntax"></a>Sintaxe

```
schtasks /end /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /TN `<taskname>` | Identifica a tarefa que iniciou o programa. Este parâmetro é necessário. |
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (com ou sem barras invertidas). O padrão é o computador local. |
| /u `[<domain>]` | Executa este comando com as permissões da conta de usuário especificada. Por padrão, o comando é executado com as permissões do usuário atual do computador local. A conta de usuário especificada deve ser um membro do grupo Administradores no computador remoto. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /p `<password>` | Especifica a senha da conta de usuário especificada no parâmetro **/u** . Se você usar o parâmetro **/u** sem o parâmetro **/p** ou o argumento password, Schtasks irá solicitar uma senha. Os parâmetros **/u** e **/p** são válidos somente quando você usa **/s**. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para interromper a instância do Notepad.exe iniciado pela tarefa *meu bloco de notas* , digite:

```
schtasks /end /tn "My Notepad"
```

Para interromper a instância do Internet Explorer iniciada pela tarefa de *Internet* no computador remoto, *SVR01*, digite:

```
schtasks /end /tn InternetOn /s Svr01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de alteração de Schtasks](schtasks-change.md)

- [comando SCHTASKS CREATE](schtasks-create.md)

- [comando de exclusão de Schtasks](schtasks-delete.md)

- [comando de consulta Schtasks](schtasks-query.md)

- [comando Schtasks execute](schtasks-run.md)