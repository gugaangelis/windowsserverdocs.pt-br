---
title: A opção Uninitialize-Server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c63e09738871c5b74c1b564a83c35ad28f4fa80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385595"
---
# <a name="the-uninitialize-server-option"></a>A opção Uninitialize-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

reverte as alterações feitas no servidor durante a configuração inicial do servidor. Isso inclui as alterações feitas pela opção **/Initialize-Server** ou o snap-in MMC dos serviços de implantação do Windows. Observe que esse comando redefine o servidor para um estado não configurado. Esse comando não modifica o conteúdo da pasta compartilhada remoteInstall. Em vez disso, ele redefine o estado do servidor para que você possa reinicializar o servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para reinicializar o servidor, digite um dos seguintes:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
[ Subcomando: Set-Server](subcommand-set-server.md)1[subcomando: Start-Server](subcommand-start-server.md)3[Subcommand: Stop-Server](subcommand-stop-server.md)
