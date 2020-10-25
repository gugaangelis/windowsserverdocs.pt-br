---
title: WDSUTIL Add-imagedriverpackages
description: Artigo de referência para o comando WDSUTIL Add-imagedriverpackages, que adiciona pacotes de driver do repositório de drivers a uma imagem de inicialização.
ms.topic: reference
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 74029ca7b2243603c832b74f59a9248353cea911
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524471"
---
# <a name="wdsutil-add-imagedriverpackages"></a>WDSUTIL Add-imagedriverpackages

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona pacotes de driver do repositório de drivers a uma imagem de inicialização.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome de servidor não for especificado, o servidor local será usado. |
| [mídia: `<Imagename>` ] | Especifica o nome da imagem à qual adicionar o driver. |
| [MediaType: inicialização] | Especifica o tipo de imagem ao qual adicionar o driver. Os pacotes de driver só podem ser adicionados a imagens de inicialização. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica a arquitetura da imagem de inicialização. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada. |
| [/Filename:`<Filename>`] | Especifica o nome do arquivo. Se a imagem não puder ser identificada exclusivamente pelo nome, o nome do arquivo deverá ser especificado. |
| FilterType`<Filtertype>` | Especifica o atributo do pacote de driver a ser pesquisado. Você pode especificar vários atributos em um único comando. Você também deve especificar **/Operator** e **/Value** com essa opção. Os valores válidos incluem:<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>PackageProvider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| Operador`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | Especifica a relação entre o atributo e os valores. Você só pode especificar **Contains** com atributos de cadeia de caracteres. Você só pode especificar **GreaterOrEqual** e **LessOrEqual** com atributos de data e versão. |
| Valor`<Value>` | Especifica o valor a ser pesquisado em relação ao especificado `<attribute>` . Você pode especificar vários valores para um único **/FilterType**. Os valores disponíveis para cada filtro são:<ul><li>**PackageID** -ESPECIFIQUE um GUID válido. Por exemplo: {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**PackageName** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageEnabled** -especifique **Sim** ou **não**</li><li>**Packagedateadded** -especifique a data no seguinte formato: aaaa/mm/dd</li><li>**PackageInfFilename** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageClass** -especifique um nome de classe ou um GUID de classe válido. Por exemplo: **DiskDrive**, **net**ou {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**Packageprovider** -especificar qualquer valor de cadeia de caracteres</li><li>**PackageArchitecture** -especificar **x86**, **x64**ou **IA64**</li><li>**PackageLocale** -especifique um identificador de idioma válido. Por exemplo: **en-US** ou **es-es**</li><li>**PackageSigned** -especifique **Sim** ou **não**</li><li>**PackagedatePublished** -especifique a data no seguinte formato: aaaa/mm/dd</li><li>**PackageVersion** -especifique a versão no seguinte formato: a.b.x.y. Por exemplo: 6.1.0.0</li><li>**Driverdescription** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverManufacturer** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverHardwareId** -especificar qualquer valor de cadeia de caracteres</li><li>**DrivercompatibleId** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverExcludeId** -especificar qualquer valor de cadeia de caracteres</li><li>**DriverGroupId** -ESPECIFIQUE um GUID válido. Por exemplo: {4D36E972-E325-11CE-BFC1-08002BE10318}</li><li>**DriverGroupName** -especificar qualquer valor de cadeia de caracteres</li></ul> Para obter mais informações sobre esses valores, consulte [atributos de driver e pacote](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11)). |

## <a name="examples"></a>Exemplos

Para adicionar pacotes de driver a uma imagem de inicialização, digite um dos seguintes:

```
wdsutil /add-ImageDriverPackagemedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```

```
wdsutil /verbose /add-ImageDriverPackagemedia: WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

```
wdsutil /verbose /add-ImageDriverPackagemedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-
- [comando WDSUTIL Add-ImageDriverPackage](wdsutil-add-imagedriverpackage.md)
-
- [comando WDSUTIL Add-AllDriverPackages](wdsutil-add-alldriverpackages.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
