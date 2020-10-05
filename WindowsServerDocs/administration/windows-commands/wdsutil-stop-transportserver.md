---
title: WDSUTIL Stop-TransportServer
description: Artigo de referência para Stop-TransportServer
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 800c99cba18ed2158749b1638627b31f8fdbaf5f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729817"
---
# <a name="wdsutil-stop-transportserver"></a>WDSUTIL Stop-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompe todos os serviços em um servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum servidor de transporte for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Disable-TransportServer](wdsutil-disable-transportserver.md)
- [comando WDSUTIL Enable-TransportServer](wdsutil-enable-transportserver.md)
- [comando WDSUTIL Get-TransportServer](wdsutil-get-transportserver.md)
- [comando WDSUTIL Set-TransportServer](wdsutil-set-transportserver.md)
- [comando WDSUTIL Start-TransportServer](wdsutil-start-transportserver.md)
