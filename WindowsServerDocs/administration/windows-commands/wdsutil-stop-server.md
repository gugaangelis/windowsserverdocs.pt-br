---
title: WDSUTIL Stop-Server
description: Artigo de referência para o subcomando Stop-Server, que interrompe todos os serviços em um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0c1288900cdc5eaa46c27b6eb05fd64b9a1b16a4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729600"
---
# <a name="wdsutil-stop-server"></a>WDSUTIL Stop-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [WDSUTIL Disable – comando do servidor](wdsutil-disable-server.md)
- [WDSUTIL Enable-Server comando](wdsutil-enable-server.md)
- [comando WDSUTIL Get-Server](wdsutil-get-server.md)
- [comando do servidor de inicialização WDSUTIL](wdsutil-initialize-server.md)
- [WDSUTIL Set-Server Command](wdsutil-set-server.md)
- [comando Start-Server do WDSUTIL](wdsutil-start-server.md)
- [WDSUTIL não inicializar-comando de servidor](wdsutil-uninitialize-server.md)

