---
title: Parada do subcomando-TransportServer
description: Tópico de comandos do Windows para Stop-TransportServer
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44d62332307ffda4dcfa6af286c7b95cb12423dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833699"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: Stop-TransportServer

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum servidor de transporte for especificado, o servidor local será usado.|
## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[usando o comando Get-TransportServer,](using-the-get-transportserver-command.md)
subcomando [: Set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Start-TransportServer](subcommand-start-transportserver.md)
