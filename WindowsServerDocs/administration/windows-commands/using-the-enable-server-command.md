---
title: Habilitar-servidor
description: O tópico comandos do Windows para Enable-Server, que habilita todos os serviços para os serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b10ff920667cfdbaae5baaf096bf56e11ce880e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831539"
---
# <a name="enable-server"></a>Habilitar-servidor

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando disable-Server](using-the-disable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[usando o comando Initialize-](using-the-initialize-server-command.md) server
subcomando [: Set-Server](subcommand-set-server.md)
[subcomando: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[a opção não Initialize-Server](the-uninitialize-server-option.md)
