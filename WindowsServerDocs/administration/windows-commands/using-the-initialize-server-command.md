---
title: Usando o comando Initialize-Server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9e95972838fc70ee1e617d1e299c9e35db5b979
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392097"
---
# <a name="using-the-initialize-server-command"></a>Usando o comando Initialize-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura um servidor dos serviços de implantação do Windows para uso inicial após a instalação da função de servidor. Depois de executar esse comando, você deve usar o comando [usando o comando Add-Image](using-the-add-image-command.md) para adicionar imagens ao servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/remInst: "<Full path>"|Especifica o caminho completo e o nome da pasta remoteInstall. Se a pasta especificada ainda não existir, essa opção a criará quando o comando for executado. Você sempre deve inserir um caminho local, mesmo no caso de um computador remoto. Por exemplo: **D:\remoteInstall**.|
|/Authorize|Autoriza o servidor no protocolo de controle de host dinâmico (DHCP). Essa opção será necessária somente se a detecção de invasor não autorizado estiver habilitada, o que significa que o servidor PXE dos serviços de implantação do Windows deve ser autorizado no DHCP antes que os computadores cliente possam ser atendidos. Observe que a detecção não autorizada de DHCP está desabilitada por padrão.|
## <a name="BKMK_examples"></a>Disso
Para inicializar o servidor e definir a pasta compartilhada remoteInstall para a unidade F:, digite.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Para inicializar o servidor e definir a pasta compartilhada remoteInstall para a unidade C:, digite.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Get-Server](using-the-get-server-command.md)
[subcomando: Set-Server](subcommand-set-server.md)
[subcomando: Start-Server](subcommand-start-server.md)1[subcomando: Stop-Server](subcommand-stop-server.md)3[a opção Uninitialize-Server](the-uninitialize-server-option.md)
