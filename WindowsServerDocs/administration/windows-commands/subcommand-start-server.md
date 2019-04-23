---
title: Subcomando start-servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a89e3f17aeed7eb3156e28997206517342be109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852037"
---
# <a name="subcommand-start-server"></a>Subcommand: start-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor a ser iniciado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Exemplos
Para iniciar o servidor, digite o seguinte:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando Get-Server](using-the-get-server-command.md)
[usando o comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md) 
 [ Subcomando: stop-Server](subcommand-stop-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
