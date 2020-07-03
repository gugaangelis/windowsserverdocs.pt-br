---
title: Get-TransportServer
description: Artigo de referência para Get-TransportServer, que exibe informações sobre um servidor de transporte especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115942290679decd8b8c660e4113576efb30123d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932171"
---
# <a name="get-transportserver"></a>Get-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um servidor de transporte especificado.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {config}|Retorna informações de configuração sobre o servidor de transporte especificado.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre o servidor, digite:
```
wdsutil /Get-TransportServer /Show:Config
```
Para exibir informações de configuração, digite:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usando o comando](using-the-enable-transportserver-command.md) 
 Enable-TransportServer [Subcomando: Set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Start-TransportServer](subcommand-start-transportserver.md) 
 [Subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
