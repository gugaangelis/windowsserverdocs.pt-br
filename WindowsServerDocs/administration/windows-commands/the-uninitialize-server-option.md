---
title: cancelar inicialização-servidor
description: Tópico de referência para Uninitialize-Server, que reverte as alterações feitas no servidor durante a configuração inicial do servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d7747a44172b7382bd22a7d48ccc717a89ccacb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721391"
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
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando
[usando o comando Disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)[usando o comando Initialize-Server Command](using-the-initialize-server-command.md)
subcomando[: Set-Server](subcommand-set-server.md)
subcomando[: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
