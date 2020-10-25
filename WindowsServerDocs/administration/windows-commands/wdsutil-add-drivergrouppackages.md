---
title: WDSUTIL Add-drivergrouppackages
description: Artigo de referência para o comando WDSUTIL Add-drivergrouppackages, que adiciona pacotes de grupo de driver.
ms.topic: reference
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67e211207f2c715053d734d659d511c61337e393
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524531"
---
# <a name="wdsutil-add-drivergrouppackages"></a>WDSUTIL Add-drivergrouppackages

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona pacotes do grupo de drivers.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica o nome do novo grupo de drivers. |
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| FilterType`<Filtertype>` | Especifica o tipo do pacote de driver a ser pesquisado. Você pode especificar vários atributos em um único comando. Você também deve especificar **/Operator** e **/Value** com essa opção. Os valores válidos incluem:<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>PackageProvider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| Operador`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | Especifica a relação entre o atributo e os valores. Você só pode especificar **Contains** com atributos de cadeia de caracteres. Você só pode especificar **EQUAL**, não **EQUAL**, **GreaterOrEqual** e **LessOrEqual** com atributos Date e Version. |
| Valor`<Value>` | Especifica o valor do cliente correspondente a **/FilterType**. Você pode especificar vários valores para um único **/FilterType**. Os valores disponíveis para cada filtro são:<ul><li>**PackageID** -ESPECIFIQUE um GUID válido. Por exemplo: {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**PackageName** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageEnabled** -especifique **Sim** ou **não**</li><li>**Packagedateadded** -especifique a data no seguinte formato: aaaa/mm/dd</li><li>**PackageInfFilename** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageClass** -especifique um nome de classe ou um GUID de classe válido. Por exemplo: **DiskDrive**, **net**ou {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**Packageprovider** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageArchitecture** -especificar **x86**, **x64**ou **IA64**</li><li>**PackageLocale** -especifique um identificador de idioma válido. Por exemplo: **en-US** ou **es-es**</li><li>**PackageSigned** -especifique **Sim** ou **não**</li><li>**PackagedatePublished** -especifique a data no seguinte formato: aaaa/mm/dd</li><li>**PackageVersion** -especifique a versão no seguinte formato: a.b.x.y. Por exemplo: 6.1.0.0</li><li>**Driverdescription** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverManufacturer** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverHardwareId** -especificar qualquer valor de cadeia de caracteres</li><li>**DrivercompatibleId** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverExcludeId** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverGroupId** -ESPECIFIQUE um GUID válido. Por exemplo: {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**DriverGroupName** -especificar qualquer valor de cadeia de caracteres</li></ul> Para obter mais informações sobre esses valores, consulte [atributos de driver e pacote](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11)). |

## <a name="examples"></a>Exemplos

Para adicionar um pacote de grupo de drivers, digite:

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-DriverPackage](wdsutil-add-driverpackage.md)

- [comando WDSUTIL Add-drivergrouppackage](wdsutil-add-drivergrouppackage.md)

- [comando WDSUTIL Add-AllDriverPackages](wdsutil-add-alldriverpackages.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
