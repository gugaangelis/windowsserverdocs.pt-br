---
title: Habilitar-TransportServer
description: O tópico de comandos do Windows para Enable-TransportServer, que habilita todos os serviços para o servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ee5f1206633fb47c4a4b066c099fbf9c7020fb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831479"
---
# <a name="enable-transportserver"></a>Habilitar-TransportServer

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para o servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome for especificado, o servidor local será usado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [A chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Get-TransportServer](using-the-get-transportserver-command.md)
subcomando [: Set-TransportServer](subcommand-set-transportserver.md)
subcomando [: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
