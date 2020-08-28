---
title: Subcomando Stop-Server
description: Artigo de referência para o subcomando Stop-Server, que interrompe todos os serviços em um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fd4a2e249b5bbf52cce9d35fcb07821b793fb23
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024660"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-server-command.md) 
 Disable-Server [Usando o comando](using-the-enable-server-command.md) 
 Enable-Server [Usando o comando](using-the-get-server-command.md) 
 Get-Server [Usando o comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: Set-Server](subcommand-set-server.md) 
 [Subcomando: Start-Server](subcommand-start-server.md) 
 [A opção Uninitialize-Server](the-uninitialize-server-option.md)
