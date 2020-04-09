---
title: remover-controlador de driver
description: Tópico de comandos do Windows para Remove-Group-Driver, que remove um grupo de drivers de um servidor.
ms.prod: windows-server
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56622c30b8b0af88a57c476eb4f03d598703d603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830519"
---
# <a name="remove-drivergroup"></a>remover-controlador de driver

Remove um grupo de drivers de um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/DriverGroup: nome do grupo de\<>|Especifica o nome do grupo de drivers a ser removido.|
|[/Server: nome do servidor\<>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. se um nome de servidor não for especificado, o servidor local será usado.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para remover um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)