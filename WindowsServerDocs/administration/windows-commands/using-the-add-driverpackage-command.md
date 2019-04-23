---
title: Usando o comando add-/InfFile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d00b020269bb47ba23810883941be1a9ebfe4e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828057"
---
# <a name="using-the-add-driverpackage-command"></a>Usando o comando add-/InfFile



Adiciona um pacote de driver ao servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|InfFile:\<caminho do arquivo Inf >|Especifica o caminho completo do arquivo. inf para adicionar.|
|/Server:\<Server name>|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Architecture:{x86 | ia64 | x64}|Especifica a arquitetura do pacote de driver.|
|[/DriverGroup:\<Group Name>]|Especifica o nome do grupo de drivers para o qual o pacote deve ser adicionado.|
|[/Name:\<nome amigável >]|Declara o nome amigável para o pacote de driver.|

## <a name="BKMK_examples"></a>Exemplos

Para adicionar um pacote de driver, digite o seguinte:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver�?
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

