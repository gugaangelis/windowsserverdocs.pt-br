---
title: Início do subcomando-servidor
description: O tópico comandos do Windows para o subcomando Start-Server, que inicia todos os serviços para um servidor de serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3e0d11348dd6b531671e62139f2ce2c884c5ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833739"
---
# <a name="subcommand-start-server"></a>Subcomando: Start-Server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor a ser iniciado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o comando [disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: Set-Server](subcommand-set-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[a opção não Initialize-Server](the-uninitialize-server-option.md)
