---
title: Habilitar-TransportServer
description: Tópico de referência para Enable-TransportServer, que habilita todos os serviços para o servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50cc381b1c178628be7d135868027b4f37787cdd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720916"
---
# <a name="enable-transportserver"></a>Habilitar-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para o servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando[usando o comando](using-the-disable-transportserver-command.md)
Disable-TransportServer[usando o subcomando Get-TransportServer comando](using-the-get-transportserver-command.md)
[: Set-TransportServer](subcommand-set-transportserver.md)
subcomando[: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
