---
title: Remove-DriverGroupPackage
description: Artigo de referência para Remove-DriverGroupPackage, que remove um pacote de driver de um grupo de drivers em um servidor.
ms.topic: reference
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2cddeca84ba4993d8d77a4b55062d6ba4255abf9
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729620"
---
# <a name="remove-drivergrouppackage"></a>Remove-DriverGroupPackage



Remove um pacote de driver de um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|
|[/DriverPackage: \<Name> ]|Especifica o nome do pacote de driver a ser removido.|
|[/PackageId: \<ID> ]|Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser removido. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome.|

## <a name="examples"></a>Exemplos

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)