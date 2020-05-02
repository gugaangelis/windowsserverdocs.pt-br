---
title: Habilitar-servidor
description: Tópico de referência para Enable-Server, que habilita todos os serviços para serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf7bf57c0784fa16719b9f77da50212bca0ef850
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720928"
---
# <a name="enable-server"></a>Habilitar-servidor

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando[usando o comando](using-the-disable-server-command.md)
Disable-Server[usando o comando](using-the-get-server-command.md)
Get-Server[usando o comando Initialize-Server Command](using-the-initialize-server-command.md)
: o subcomando[set-](subcommand-set-server.md)
Server: o subcomando[Start-Server](subcommand-start-server.md)
[: Stop-Server](subcommand-stop-server.md)
[a opção não inicializar-Server](the-uninitialize-server-option.md)
