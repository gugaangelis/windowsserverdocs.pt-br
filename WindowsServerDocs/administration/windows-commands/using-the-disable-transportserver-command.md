---
title: desabilitar-TransportServer
description: Tópico de referência para Disable-TransportServer, que desabilita todos os serviços de um servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81ae150b4f8e4de577e377a2d10a7a69675adac7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720963"
---
# <a name="disable-transportserver"></a>desabilitar-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita todos os serviços de um servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte a ser desabilitado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome do servidor de transporte for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para desabilitar o servidor, digite:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)[usando o subcomando Get-TransportServer comando](using-the-get-transportserver-command.md)
[: Set-TransportServer](subcommand-set-transportserver.md)
subcomando[: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
