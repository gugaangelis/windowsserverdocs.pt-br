---
title: Início do subcomando-TransportServer
description: Tópico de referência para subcomando Start-TransportServer, que inicia todos os serviços para um servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92bd68421883c49ec29dfb78f06121bff880b01e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721645"
---
# <a name="subcommand-start-transportserver"></a>Subcomando: Start-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando usando o
[comando Disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)[usando o subcomando Get-TransportServer comando](using-the-get-transportserver-command.md)
[: Set-TransportServer](subcommand-set-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
