---
title: Parar o subcomando-servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ddb681234cfcbe6d02e56f2e366167faeeb25280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834727"
---
# <a name="subcommand-stop-server"></a>Subcommand: stop-Server

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
## <a name="BKMK_examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando Get-Server](using-the-get-server-command.md)
[usando o comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md) 
 [ Subcomando: start-Server](subcommand-start-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
