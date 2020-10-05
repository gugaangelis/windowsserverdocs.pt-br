---
title: WDSUTIL Initialize-Server
description: Artigo de referência para WDSUTIL Initialize-Server, que configura um servidor de serviços de implantação do Windows para uso inicial após a instalação da função de servidor.
ms.topic: reference
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 42d579b7139f7ff516d9ff239c535ecd2be42474
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729633"
---
# <a name="wdsutil-initialize-server"></a>WDSUTIL Initialize-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura um servidor dos serviços de implantação do Windows para uso inicial após a instalação da função de servidor. Depois de executar esse comando, você deve usar o comando [wdsutiladd-Image](wdsutil-add-image.md) para adicionar imagens ao servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|remInst<Full path>|Especifica o caminho completo e o nome da pasta remoteInstall. Se a pasta especificada ainda não existir, essa opção a criará quando o comando for executado. Você sempre deve inserir um caminho local, mesmo no caso de um computador remoto. Por exemplo: **D:\remoteInstall**.|
|/Authorize|Autoriza o servidor no protocolo de controle de host dinâmico (DHCP). Essa opção será necessária somente se a detecção de invasor não autorizado estiver habilitada, o que significa que o servidor PXE dos serviços de implantação do Windows deve ser autorizado no DHCP antes que os computadores cliente possam ser atendidos. Observe que a detecção não autorizada de DHCP está desabilitada por padrão.|
## <a name="examples"></a>Exemplos
Para inicializar o servidor e definir a pasta compartilhada remoteInstall para a unidade F:, digite.
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
Para inicializar o servidor e definir a pasta compartilhada remoteInstall para a unidade C:, digite.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [WDSUTIL Disable – comando do servidor](wdsutil-disable-server.md)
- [WDSUTIL Enable-Server comando](wdsutil-enable-server.md)
- [comando WDSUTIL Get-Server](wdsutil-get-server.md)
- [WDSUTIL Set-Server Command](wdsutil-set-server.md)
- [comando Start-Server do WDSUTIL](wdsutil-start-server.md)
- [comando WDSUTIL Stop-Server](wdsutil-stop-server.md)
- [WDSUTIL não inicializar-comando de servidor](wdsutil-uninitialize-server.md)
