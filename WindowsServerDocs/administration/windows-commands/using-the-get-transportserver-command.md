---
title: Get-TransportServer
description: O tópico de comandos do Windows para Get-TransportServer, que exibe informações sobre um servidor de transporte especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549eb32536bda201a81e5f40ef408359fae39f4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830809"
---
# <a name="get-transportserver"></a>Get-TransportServer

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um servidor de transporte especificado.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {config}|Retorna informações de configuração sobre o servidor de transporte especificado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre o servidor, digite:
```
wdsutil /Get-TransportServer /Show:Config
```
Para exibir informações de configuração, digite:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[subcomando: Set-TransportServer](subcommand-set-transportserver.md)
subcomando [: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
