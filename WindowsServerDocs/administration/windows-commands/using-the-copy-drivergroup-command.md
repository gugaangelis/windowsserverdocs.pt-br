---
title: Usando o comando de cópia DriverGroup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d9c6f4ca78991bb4c286042a6172211161dd1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842077"
---
# <a name="using-the-copy-drivergroup-command"></a>Usando o comando de cópia DriverGroup



Duplica a um grupo de driver existente no servidor, incluindo os filtros, pacotes de driver e status de habilitado/desabilitado.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ DriverGroup:\<nome do grupo de origem >|Especifica o nome do grupo de drivers de fonte.|
|/ GroupName:\<novo nome do grupo >|Especifica o nome do novo grupo de driver.|

## <a name="BKMK_examples"></a>Exemplos

Para copiar um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)