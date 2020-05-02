---
title: Início do subcomando-servidor
description: Tópico de referência para o subcomando Start-Server, que inicia todos os serviços para um servidor de serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a6de007e62bf3be5544f97b53a4fcc13118985
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721648"
---
# <a name="subcommand-start-server"></a>Subcomando: Start-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor a ser iniciado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando[usando o comando Disable-Server](using-the-disable-server-command.md)

[usando o comando Enable-Server](using-the-enable-server-command.md)[usando o comando](using-the-get-server-command.md)
Get-Server[usando o comando Initialize-Server Command](using-the-initialize-server-command.md)
[: Set-Server](subcommand-set-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[a opção não inicializar-Server](the-uninitialize-server-option.md)
