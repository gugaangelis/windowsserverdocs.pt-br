---
title: Usando o comando Disable-TransportServer
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7531f8a638ac8fabdad08cc0134dbc63873505de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363460"
---
# <a name="using-the-disable-transportserver-command"></a>Usando o comando Disable-TransportServer

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita todos os serviços de um servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte a ser desabilitado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome do servidor de transporte for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para desabilitar o servidor, digite:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando enable-TransportServer](using-the-enable-transportserver-command.md)
[usando o comando Get-TransportServer](using-the-get-transportserver-command.md)@no__t o[subcomando-5: Set-TransportServer](subcommand-set-transportserver.md)
[subcomando: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
