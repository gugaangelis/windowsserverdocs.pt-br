---
title: Usando o comando disable-TransportServer
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d653aacf225333ac8a8b090ef53fb6826e84ab5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836977"
---
# <a name="using-the-disable-transportserver-command"></a>Usando o comando disable-TransportServer

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita todos os serviços para um servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte a ser desabilitado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor de transporte for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Exemplos
Para desativar o servidor, digite:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando enable-TransportServer](using-the-enable-transportserver-command.md)
[usando o comando get-TransportServer](using-the-get-transportserver-command.md) 
 [Subcomando: set-TransportServer](subcommand-set-transportserver.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
[subcomando: stop-TransportServer](subcommand-stop-transportserver.md)
