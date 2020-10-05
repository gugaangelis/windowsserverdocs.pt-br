---
title: WDSUTIL não inicializar-servidor
description: Artigo de referência para Uninitialize-Server, que reverte as alterações feitas no servidor durante a configuração inicial do servidor.
ms.topic: reference
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4f8f7623c31f355ece5df389af474001d996e755
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729596"
---
# <a name="wdsutil-uninitialize-server"></a>WDSUTIL não inicializar-servidor

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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [WDSUTIL Disable – comando do servidor](wdsutil-disable-server.md)
- [WDSUTIL Enable-Server comando](wdsutil-enable-server.md)
- [comando WDSUTIL Get-Server](wdsutil-get-server.md)
- [comando do servidor de inicialização WDSUTIL](wdsutil-initialize-server.md)
- [WDSUTIL Set-Server Command](wdsutil-set-server.md)
- [comando Start-Server do WDSUTIL](wdsutil-start-server.md)
- [comando WDSUTIL Stop-Server](wdsutil-stop-server.md)
