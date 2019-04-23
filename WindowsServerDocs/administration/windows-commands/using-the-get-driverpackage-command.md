---
title: Usando o comando get-/InfFile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6574177f1f0a8ead0cc2fa596380eb2c980f8d1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888517"
---
# <a name="using-the-get-driverpackage-command"></a>Usando o comando get-/InfFile



Exibe informações sobre um pacote de driver no servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/DriverPackage:\<Name>]|Especifica o nome do pacote de driver para mostrar.|
|[/PackageId:\<ID>]|Especifica a ID dos serviços de implantação do Windows do pacote de driver para mostrar. Se o pacote de driver não pode ser identificado exclusivamente pelo nome, você deve especificar a ID.|
|[/Show: {Drivers | Arquivos | All}]|Indica quais informações a ser exibido (se especificado). Se **/Mostrar** não for especificado, o padrão é retornar somente o driver de metadados do pacote. **Drivers** exibe todos os drivers no pacote. **Arquivos** exibe a lista de arquivos no pacote. **Todos os** exibe drivers, arquivos e metadados.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações sobre um pacote de driver, digite o seguinte:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)