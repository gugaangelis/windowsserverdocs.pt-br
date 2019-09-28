---
title: Subcomando Stop-Server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7584dcbca5bfc52d303f187f62be24cbad407416
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383737"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
[ Subcomando: Set-Server](subcommand-set-server.md)1[subcomando: Start-Server](subcommand-start-server.md)3[a opção Uninitialize-Server](the-uninitialize-server-option.md)
