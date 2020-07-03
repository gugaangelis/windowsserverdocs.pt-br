---
title: fazer logoff
description: Artigo de referência para o comando logoff, que faz logoff de um usuário de uma sessão em um servidor de Host da Sessão da Área de Trabalho Remota e exclui a sessão.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d154b767302f5c536e0a7efb30d99ac0a8e087d5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927162"
---
# <a name="logoff"></a>fazer logoff

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz logoff de um usuário de uma sessão em um servidor Host da Sessão da Área de Trabalho Remota e exclui a sessão.

## <a name="syntax"></a>Sintaxe
```
logoff [<sessionname> | <sessionID>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<sessionname>` | Especifica o nome da sessão. Isso deve ser uma sessão ativa.|
| `<sessionID>` | Especifica a ID numérica que identifica a sessão para o servidor. |
| /server:`<servername>` | Especifica o servidor de Host da Sessão da Área de Trabalho Remota que contém a sessão cujo usuário você deseja fazer logoff. Se não for especificado, o servidor no qual você está ativo no momento será usado. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você sempre pode fazer logoff da sessão na qual está conectado no momento. No entanto, você deve ter permissão **controle total** para fazer logoff de usuários de outras sessões.

- Fazer logoff de um usuário de uma sessão sem aviso pode resultar em perda de dados na sessão do usuário. Você deve enviar uma mensagem para o usuário usando o comando **msg** para avisar o usuário antes de executar esta ação.

- Se `<sessionID>` ou `<sessionname>` não for especificado, **logoff fará logoff** do usuário da sessão atual.

- Depois de fazer logoff de um usuário, todos os processos são encerrados e a sessão é excluída do servidor.

- Não é possível fazer logoff de um usuário da sessão de console.

### <a name="examples"></a>Exemplos

Para fazer logoff de um usuário da sessão atual, digite:

```
logoff
```

Para fazer logoff de um usuário de uma sessão usando a ID da sessão, por exemplo, *sessão 12*, digite:

```
logoff 12
```

Para fazer logoff de um usuário de uma sessão usando o nome da sessão e do servidor, por exemplo, Session *TERM04* no *Server1*, digite:

```
logoff TERM04 /server:Server1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
