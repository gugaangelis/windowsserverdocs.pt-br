---
title: copiar-controlador de driver
description: O tópico de comandos do Windows para o grupo de drivers de cópia, que duplica um grupo de drivers existente no servidor, incluindo os filtros, pacotes de driver e status habilitado/desabilitado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 277903150a25555b03b51c980436250656c597b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831729"
---
# <a name="copy-drivergroup"></a>copiar-controlador de driver

Duplica um grupo de drivers existente no servidor, incluindo os filtros, os pacotes de driver e o status habilitado/desabilitado.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server: nome do servidor\<>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/DriverGroup: nome do grupo de origem\<>|Especifica o nome do grupo de drivers de origem.|
|/GroupName:\<nome do novo grupo >|Especifica o nome do novo grupo de drivers.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para copiar um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)