---
title: Usando o comando Enable-TransportServer
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 732a021b02193a3bfb5cb573a33879dbecb840b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363426"
---
# <a name="using-the-enable-transportserver-command"></a>Usando o comando Enable-TransportServer

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para o servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Get-TransportServer](using-the-get-transportserver-command.md)@no__t o[subcomando-5: Set-TransportServer](subcommand-set-transportserver.md)
[subcomando: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
