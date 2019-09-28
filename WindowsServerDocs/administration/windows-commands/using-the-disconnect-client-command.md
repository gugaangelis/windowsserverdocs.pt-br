---
title: Usando o comando Disconnect-Client
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: dbb96d64b47ec72ff0710bfb3684257c1bda2d04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363475"
---
# <a name="using-the-disconnect-client-command"></a>Usando o comando Disconnect-Client



Desconecta um cliente de uma transmissão multicast ou de um namespace. A menos que você especifique **/Force**, o cliente voltará para outro método de transferência (se houver suporte pelo cliente).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ClientId: ID do \<Client >|Especifica a ID do cliente a ser desconectado. Para exibir a ID de um cliente, digite **WDSUTIL/Get-MulticastTransmission/show: clients**.|
|[/Server: \<Server nome >]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Force|Interrompe completamente a instalação e não usa um método de fallback. Observe que o Wdsmcast. exe não dá suporte a nenhum mecanismo de fallback. Se você não usar essa opção, o comportamento padrão será o seguinte:</br>-Se você estiver usando o cliente dos serviços de implantação do Windows, o cliente continuará a instalação usando unicasting.</br>-Se você não estiver usando o cliente dos serviços de implantação do Windows, a instalação falhará.</br>Importante: Você deve usar essa opção com cuidado porque a instalação falhará e o computador poderá ser deixado em um estado inutilizável.|

## <a name="BKMK_examples"></a>Disso

Para desconectar um cliente, digite:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Para desconectar um cliente e forçar a falha da instalação, digite:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)