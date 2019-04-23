---
title: O opção de servidor uninitialize
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73f1ff67331ae41fa0d88cb3a16df5095e0b6d66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873977"
---
# <a name="the-uninitialize-server-option"></a>O opção de servidor uninitialize

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

reverte as alterações feitas no servidor durante a configuração inicial do servidor. Isso inclui as alterações feitas por qualquer um de **/initialize-server** opção ou mmc snap-in de serviços de implantação do Windows. Observe que esse comando redefine o servidor para um estado não configurado. Esse comando não modifica o conteúdo da pasta remoteInstall compartilhada. Em vez disso, ele redefine o estado do servidor, de modo que você pode reinicializar o servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Exemplos
Para reinicializar o servidor, digite o seguinte:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando Get-Server](using-the-get-server-command.md)
[usando o comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md) 
 [ Subcomando: start-Server](subcommand-start-server.md)
[subcomando: parar o servidor](subcommand-stop-server.md)
