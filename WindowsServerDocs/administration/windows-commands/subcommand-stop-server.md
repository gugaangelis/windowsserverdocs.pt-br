---
title: Subcomando Stop-Server
description: Tópico de referência para o subcomando Stop-Server, que interrompe todos os serviços em um servidor de serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68cc73ac016e2ffded774567034801e1c11944d1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721628"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando[usando o comando](using-the-disable-server-command.md)
Disable-Server[usando o comando](using-the-enable-server-command.md)
Enable-Server[usando o comando](using-the-get-server-command.md)
Get-Server[usando o comando Initialize-Server Command](using-the-initialize-server-command.md)
[subcomando: Set-Server](subcommand-set-server.md)
[subcomando: Start-Server](subcommand-start-server.md)
[a opção não Initialize-Server](the-uninitialize-server-option.md)
