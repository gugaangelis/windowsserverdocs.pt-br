---
title: quser
description: Artigo de referência para o comando quser, que exibe informações sobre sessões de usuário em um servidor Host da Sessão da Área de Trabalho Remota.
ms.topic: reference
ms.assetid: 8056204f-ed11-4c91-bb1d-c799283a48a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 89820143ffaf656b2f022e1f19796a03576b615b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637430"
---
# <a name="quser"></a>quser

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre sessões de usuário em um servidor Host da Sessão da Área de Trabalho Remota. Você pode usar esse comando para descobrir se um usuário específico está conectado a um servidor Host da Sessão da Área de Trabalho Remota específico. Esse comando retorna as informações a seguir:

- Nome do usuário

- Nome da sessão no servidor de Host da Sessão da Área de Trabalho Remota

- ID da sessão

- Estado da sessão (ativa ou desconectada)

- Tempo ocioso (o número de minutos desde o último pressionamento da tecla ou o movimento do mouse na sessão)

- Data e hora em que o usuário fez logon

> [!NOTE]
> Esse comando é o mesmo que o [comando Query User](query-user.md). Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
quser [<username> | <sessionname> | <sessionID>] [/server:<servername>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<username>` | Especifica o nome de logon do usuário que você deseja consultar. |
| `<sessionname>` | Especifica o nome da sessão que você deseja consultar. |
| `<sessionID>` | Especifica a ID da sessão que você deseja consultar. |
| /server:`<servername>` | Especifica o servidor de Host da Sessão da Área de Trabalho Remota que você deseja consultar. Caso contrário, o servidor de Host da Sessão da Área de Trabalho Remota atual será usado. Esse parâmetro só será necessário se você estiver usando esse comando de um servidor remoto. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Para usar esse comando, você deve ter permissão de controle total ou permissão de acesso especial.

- Se você não especificar um usuário usando o <*nome* de usuário>, <*SessionName*> ou parâmetros *SessionID* , uma lista de todos os usuários que estão conectados ao servidor será retornada. Como alternativa, você também pode usar o comando **Query Session** para exibir uma lista de todas as sessões em um servidor.

- Quando **quser** retorna informações, um símbolo maior que `(>)` é exibido antes da sessão atual.

### <a name="examples"></a>Exemplos

Para exibir informações sobre todos os usuários conectados ao sistema, digite:

```
quser
```

Para exibir informações sobre o usuário *Usuário1* no servidor *Server1*, digite:

```
quser USER1 /server:Server1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de consulta do usuário](query-user.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
