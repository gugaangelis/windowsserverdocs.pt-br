---
title: WDSUTIL Start-TransportServer
description: Artigo de referência para o subcomando Start-TransportServer, que inicia todos os serviços para um servidor de transporte.
ms.topic: reference
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3aa948fb86b1b69448ac131c6894ff0c41f548e8
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729843"
---
# <a name="wdsutil-start-transportserver"></a>WDSUTIL Start-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Disable-TransportServer](wdsutil-disable-transportserver.md)
- [comando WDSUTIL Enable-TransportServer](wdsutil-enable-transportserver.md)
- [comando WDSUTIL Get-TransportServer](wdsutil-get-transportserver.md)
- [comando WDSUTIL Set-TransportServer](wdsutil-set-transportserver.md)
- [comando WDSUTIL Stop-TransportServer](wdsutil-stop-transportserver.md)
