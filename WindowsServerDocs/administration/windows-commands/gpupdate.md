---
title: gpupdate
description: Artigo de referência para o comando gpupdate, que atualiza Política de Grupo configurações.
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc17be77c17ad45dfa1ce8d8d112f86a46f67262
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888532"
---
# <a name="gpupdate"></a>gpupdate

Atualiza Política de Grupo configurações.

## <a name="syntax"></a>Sintaxe

```
gpupdate [/target:{computer | user}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| /target:`{computer|user}` | Especifica que apenas as configurações de política de usuário ou de computador são atualizadas. Por padrão, as configurações de diretiva de usuário e de computador são atualizadas. |
| /Force | Reaplica todas as configurações de política. Por padrão, somente as configurações de política que foram alteradas são aplicadas. |
| /Wait`<VALUE>` | Define o número de segundos a aguardar até que o processamento da política seja concluído antes de retornar ao prompt de comando. Quando o limite de tempo é excedido, o prompt de comando é exibido, mas o processamento da política continua. O valor padrão é de 600 segundos. O valor **0** significa não aguardar. O valor **-1** significa aguardar indefinidamente.<p>Em um script, usando esse comando com um limite de tempo especificado, você pode executar o **gpupdate** e continuar com comandos que não dependem da conclusão do **gpupdate**. Como alternativa, você pode usar esse comando sem um limite de tempo especificado para permitir que o **gpupdate** conclua a execução antes que outros comandos que dependem dele sejam executados. |
| /logoff | Causa um logoff após a atualização das configurações de Política de Grupo. Isso é necessário para as extensões Política de Grupo do lado do cliente que não processam a política em um ciclo de atualização em segundo plano, mas processam a política quando um usuário faz logon. Os exemplos incluem instalação de software direcionado pelo usuário e redirecionamento de pasta. Essa opção não terá efeito se não houver extensões chamadas que exijam um logoff. |
| /boot | Causa uma reinicialização do computador após a aplicação das configurações de Política de Grupo. Isso é necessário para as extensões Política de Grupo do lado do cliente que não processam a política em um ciclo de atualização em segundo plano, mas processam a política na inicialização do computador. Os exemplos incluem instalação de software destinado a computador. Essa opção não terá efeito se não houver extensões chamadas que exijam uma reinicialização. |
| /sync | Faz com que o próximo aplicativo de política de primeiro plano seja executado de forma síncrona. A política de primeiro plano é aplicada na inicialização do computador e no logon do usuário. Você pode especificar isso para o usuário, computador ou ambos, usando o parâmetro **/target** . Os parâmetros **/Force** e **/Wait** serão ignorados se você especificá-los. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para forçar uma atualização em segundo plano de todas as configurações de Política de Grupo, independentemente de terem sido alteradas, digite:

```
gpupdate /force
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)