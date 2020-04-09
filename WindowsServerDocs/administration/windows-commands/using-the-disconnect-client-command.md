---
title: Desconectar-cliente
description: Tópico de comandos do Windows para Disconnect-Client, que desconecta um cliente de uma transmissão multicast ou de um namespace.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ba40a7e885cfa3e42065b939d3ddb21ead2f866
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831599"
---
# <a name="disconnect-client"></a>Desconectar-cliente

Desconecta um cliente de uma transmissão multicast ou de um namespace. A menos que você especifique **/Force**, o cliente voltará para outro método de transferência (se houver suporte pelo cliente).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ClientId: > ID do cliente do\<|Especifica a ID do cliente a ser desconectado. Para exibir a ID de um cliente, digite **WDSUTIL/Get-MulticastTransmission/show: clients**.|
|[/Server: nome do servidor\<>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Force|Interrompe completamente a instalação e não usa um método de fallback. Observe que o Wdsmcast. exe não dá suporte a nenhum mecanismo de fallback. Se você não usar essa opção, o comportamento padrão será o seguinte:</br>-Se você estiver usando o cliente dos serviços de implantação do Windows, o cliente continuará a instalação usando unicasting.</br>-Se você não estiver usando o cliente dos serviços de implantação do Windows, a instalação falhará.</br>Importante: você deve usar essa opção com cuidado porque a instalação falhará e o computador poderá ser deixado em um estado inutilizável.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para desconectar um cliente, digite:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Para desconectar um cliente e forçar a falha da instalação, digite:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)