---
title: processo de consulta
description: Artigo de referência para o comando de processo de consulta, que exibe informações sobre processos que estão sendo executados em um servidor de Host da Sessão da Área de Trabalho Remota.
ms.topic: reference
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0cf1952be3e7885c4631c229061b4630ef4598c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037224"
---
# <a name="query-process"></a>processo de consulta

Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre os processos que estão sendo executados em um servidor Host da Sessão da Área de Trabalho Remota. Você pode usar esse comando para descobrir quais programas um usuário específico está executando e também quais usuários estão executando um programa específico. Esse comando retorna as informações a seguir:

- Usuário que possui o processo

- Sessão que possui o processo

- ID da sessão

- Nome do processo

- ID do processo

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
query process [*|<processID>|<username>|<sessionname>|/id:<nn>|<programname>] [/server:<servername>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| * | Lista os processos para todas as sessões. |
| `<processID>` | Especifica a ID numérica que identifica o processo que você deseja consultar. |
| `<username>` | Especifica o nome do usuário cujos processos você deseja listar. |
| `<sessionname>` | Especifica o nome da sessão ativa cujos processos você deseja listar. |
| /ID`<nn>` | Especifica a ID da sessão cujos processos você deseja listar. |
| `<programname>` | Especifica o nome do programa cujos processos você deseja consultar. A extensão. exe é necessária. |
| /server:`<servername>` | Especifica o servidor de Host da Sessão da Área de Trabalho Remota cujos processos você deseja listar. Se não for especificado, o servidor no qual você está conectado no momento será usado. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os administradores têm acesso completo a todas as funções de **processo de consulta** .

- Se você não especificar o *nome de usuário* <>, <*SessionName*>, */ID: `<nn>` *, <*ProgramName*> ou parâmetros de *&#42;* , essa consulta exibirá somente os processos que pertencem ao usuário atual.

- Quando o **processo de consulta** retorna informações, um símbolo maior que `(>)` é exibido antes de cada processo que pertence à sessão atual.

## <a name="examples"></a>Exemplos

Para exibir informações sobre os processos que estão sendo usados por todas as sessões, digite:

```
query process *
```

Para exibir informações sobre os processos que estão sendo usados pela *ID de sessão 2*, digite:

```
query process /ID:2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
