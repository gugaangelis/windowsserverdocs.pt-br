---
title: cancelar inicialização-servidor
description: Artigo de referência para Uninitialize-Server, que reverte as alterações feitas no servidor durante a configuração inicial do servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbe391a7335c347f05f9f9c06bbade3474fa30e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935876"
---
# <a name="uninitialize-server"></a>cancelar inicialização-servidor

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reverte as alterações feitas no servidor durante a configuração inicial do servidor. Isso inclui as alterações feitas pela opção **/Initialize-Server** ou o snap-in MMC dos serviços de implantação do Windows. Observe que esse comando redefine o servidor para um estado não configurado. Esse comando não modifica o conteúdo da pasta compartilhada remoteInstall. Em vez disso, ele redefine o estado do servidor para que você possa reinicializar o servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para reinicializar o servidor, digite um dos seguintes:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-server-command.md) 
 Disable-Server [Usando o comando](using-the-enable-server-command.md) 
 Enable-Server [Usando o comando](using-the-get-server-command.md) 
 Get-Server [Usando o comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: Set-Server](subcommand-set-server.md) 
 [Subcomando: Start-Server](subcommand-start-server.md) 
 [Subcomando: Stop-Server](subcommand-stop-server.md)
