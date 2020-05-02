---
title: Desconectar-cliente
description: Tópico de referência para Disconnect-Client, que desconecta um cliente de uma transmissão multicast ou de um namespace.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 541e2e0acfa51d7b63cf6cfb27ff42874760e37d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720955"
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
|/ClientId:\<ID do cliente>|Especifica a ID do cliente a ser desconectado. Para exibir a ID de um cliente, digite **WDSUTIL/Get-MulticastTransmission/show: clients**.|
|[/Server:\<nome do servidor>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Force|Interrompe completamente a instalação e não usa um método de fallback. Observe que o Wdsmcast. exe não dá suporte a nenhum mecanismo de fallback. Se você não usar essa opção, o comportamento padrão será o seguinte:</br>-Se você estiver usando o cliente dos serviços de implantação do Windows, o cliente continuará a instalação usando unicasting.</br>-Se você não estiver usando o cliente dos serviços de implantação do Windows, a instalação falhará.</br>Importante: você deve usar essa opção com cuidado porque a instalação falhará e o computador poderá ser deixado em um estado inutilizável.|

## <a name="examples"></a>Exemplos

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