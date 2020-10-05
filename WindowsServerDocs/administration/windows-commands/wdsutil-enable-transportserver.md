---
title: WDSUTIL Enable-TransportServer
description: Artigo de referência para WDSUTIL Enable-TransportServer, que habilita todos os serviços para o servidor de transporte.
ms.topic: reference
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4d4a294c0bda291e8340a4d8ffe54a11d487e0dd
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729677"
---
# <a name="wdsutil-enable-transportserver"></a>WDSUTIL Enable-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para o servidor de transporte.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Disable-TransportServer](wdsutil-disable-transportserver.md)
- [comando WDSUTIL Get-TransportServer](wdsutil-get-transportserver.md)
- [comando WDSUTIL Set-TransportServer](wdsutil-set-transportserver.md)
- [comando WDSUTIL Start-TransportServer](wdsutil-start-transportserver.md)
- [comando WDSUTIL Stop-TransportServer](wdsutil-stop-transportserver.md)
