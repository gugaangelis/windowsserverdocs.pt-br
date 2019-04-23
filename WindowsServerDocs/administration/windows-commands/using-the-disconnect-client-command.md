---
title: Usando o comando de desconexão de cliente
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841747"
---
# <a name="using-the-disconnect-client-command"></a>Usando o comando de desconexão de cliente



Desconecta um cliente de uma transmissão multicast ou um namespace. A menos que você especifique **/Force**, o cliente fará o fallback para outro método de transferência (se for suportado pelo cliente).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ClientId:\<Client ID>|Especifica a ID do cliente a ser desconectado. Para exibir a ID de um cliente, digite **WDSUTIL /get-multicasttransmission /show:clients**.|
|[/Server:\<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/Force]|Interrompe a instalação completamente e não usa um método de fallback. Observe que Wdsmcast.exe não dá suporte a qualquer mecanismo de fallback. Se você não usar essa opção, o comportamento padrão é da seguinte maneira:</br>– Se você estiver usando o cliente dos serviços de implantação do Windows, o cliente continuará a instalação usando unicast.</br>– Se você não estiver usando o cliente dos serviços de implantação do Windows, a instalação falhará.</br>Importante: Você deve usar essa opção com cuidado, porque a instalação falhará e o computador pode ser deixado em estado inutilizável.|

## <a name="BKMK_examples"></a>Exemplos

Para desconectar um cliente, digite:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Para desconectar um cliente e forçar a instalação falhar, digite:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)