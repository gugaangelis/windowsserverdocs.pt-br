---
title: Início do subcomando-servidor
description: Artigo de referência para o subcomando Start-Server, que inicia todos os serviços para um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d79ef2689f15a0f6fdc67cf5c068372f6f448c7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024740"
---
# <a name="subcommand-start-server"></a>Subcomando: Start-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor a ser iniciado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-server-command.md) 
 Disable-Server [Usando o comando](using-the-enable-server-command.md) 
 Enable-Server [Usando o comando](using-the-get-server-command.md) 
 Get-Server [Usando o comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: Set-Server](subcommand-set-server.md) 
 [Subcomando: Stop-Server](subcommand-stop-server.md) 
 [A opção Uninitialize-Server](the-uninitialize-server-option.md)
