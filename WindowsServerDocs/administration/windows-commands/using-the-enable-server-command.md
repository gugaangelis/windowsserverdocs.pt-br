---
title: Usando o comando Enable-Server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b90621ec14c6cf451d7a05eace79f2e0679b2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363447"
---
# <a name="using-the-enable-server-command"></a>Usando o comando Enable-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-Server](using-the-disable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: Set-Server](subcommand-set-server.md)
[ Subcomando: Start-Server](subcommand-start-server.md)1[subcomando: Stop-Server](subcommand-stop-server.md)3[a opção Uninitialize-Server](the-uninitialize-server-option.md)
