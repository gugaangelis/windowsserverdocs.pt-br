---
title: tsdiscon
description: Artigo de referência para tsdiscon, que desconecta uma sessão de um servidor de Host da Sessão da Área de Trabalho Remota.
ms.topic: reference
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b7126e2e9d1f5185ea64566843c523bf0570891
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156382"
---
# <a name="tsdiscon"></a>tsdiscon

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta uma sessão de um servidor Host da Sessão da Área de Trabalho Remota. Se você não especificar uma ID de sessão ou um nome de sessão, esse comando desconectará a sessão atual.

> [!IMPORTANT]
> Você deve ter permissão de **acesso controle total** ou **Desconectar permissão de acesso especial** para desconectar outro usuário de uma sessão.

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
tsdiscon [<sessionID> | <sessionname>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<sessionID>` | Especifica a ID da sessão a ser desconectada. |
| `<sessionname>` | Especifica o nome da sessão a ser desconectada. |
| /server:`<servername>` | Especifica o servidor de terminal que contém a sessão que você deseja desconectar. Caso contrário, o servidor de Host da Sessão da Área de Trabalho Remota atual será usado. Esse parâmetro será necessário somente se você executar o comando **tsdiscon** a partir de um servidor remoto. |
| /v | Exibe informações sobre as ações que estão sendo executadas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Todos os aplicativos em execução quando você desconectou a sessão são automaticamente executados quando você se reconecta a essa sessão sem perda de dados. Você pode usar o [comando Redefinir sessão](reset-session.md) para encerrar os aplicativos em execução da sessão desconectada, mas isso pode resultar na perda de dados na sessão.

- A sessão do console não pode ser desconectada.

## <a name="examples"></a>Exemplos

Para desconectar a sessão atual, digite:

```
tsdiscon
```

Para desconectar a *sessão 10*, digite:

```
tsdiscon 10
```

Para desconectar a sessão chamada *TERM04*, digite:

```
tsdiscon TERM04
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)

- [comando Redefinir sessão](reset-session.md)
