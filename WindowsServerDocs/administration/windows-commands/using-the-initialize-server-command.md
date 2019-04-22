---
title: Usando o comando Inicializar servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825187"
---
# <a name="using-the-initialize-server-command"></a>Usando o comando Inicializar servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura um servidor de serviços de implantação do Windows para uso inicial depois que a função de servidor foi instalada. Depois de executar esse comando, você deve usar o [usando a imagem de adicionar comando](using-the-add-image-command.md) comando para adicionar imagens ao servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/remInst:"<Full path>"|Especifica o caminho completo e o nome da pasta remoteInstall. Se a pasta especificada ainda não existir, essa opção criará-lo quando o comando é executado. Você sempre deve inserir um caminho local, mesmo no caso de um computador remoto. Por exemplo:  **D:\remoteInstall**.|
|[/ Autorizar]|Autoriza o servidor no controle de protocolo DHCP (Dynamic Host). Esta opção é necessária somente se a detecção de invasor do DHCP está habilitada, o que significa que o PXE de serviços de implantação do Windows server deve estar autorizado no DHCP antes de computadores cliente podem ser atendidos. Observe que a detecção de invasor do DHCP é desabilitada por padrão.|
## <a name="BKMK_examples"></a>Exemplos
Para inicializar o servidor e definir a pasta remoteInstall compartilhada para a unidade f:, digite.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Para inicializar o servidor e definir a pasta remoteInstall compartilhada para a unidade c:, digite.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando Get-Server](using-the-get-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: Iniciar servidor](subcommand-start-server.md)
[subcomando: parar o servidor](subcommand-stop-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
