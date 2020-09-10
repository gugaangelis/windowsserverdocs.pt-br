---
title: Parada do subcomando-TransportServer
description: Artigo de referência para Stop-TransportServer
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7fb17b885708fb039f85fe88a7b4d04f2f0a63f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639459"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: Stop-TransportServer

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
## <a name="examples"></a><a name="BKMK_examples"></a>Exemplos
Para interromper os serviços, digite um dos seguintes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usando o comando](using-the-enable-transportserver-command.md) 
 Enable-TransportServer [Usando o comando](using-the-get-transportserver-command.md) 
 Get-TransportServer [Subcomando: Set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Start-TransportServer](subcommand-start-transportserver.md)
