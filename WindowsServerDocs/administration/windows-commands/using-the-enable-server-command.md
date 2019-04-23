---
title: Usando o comando enable-servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fdf03a778a6c646aa79c2f844212b1728c5c73eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852717"
---
# <a name="using-the-enable-server-command"></a>Usando o comando enable-servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que todos os serviços para os serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Exemplos
Para habilitar os serviços no servidor, execute um destes procedimentos:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando get-Server](using-the-get-server-command.md)
[usando o Comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: Iniciar servidor](subcommand-start-server.md) 
 [ Subcomando: stop-Server](subcommand-stop-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
