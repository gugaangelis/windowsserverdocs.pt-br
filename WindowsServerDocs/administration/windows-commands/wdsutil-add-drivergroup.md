---
title: WDSUTIL adicionar-controlador de driver
description: Artigo de referência do comando WDSUTIL Add-Driver, que adiciona um grupo de drivers ao servidor.
ms.topic: reference
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 68583ba35c141df03cbc5e351da6c4dc8ea4e161
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524611"
---
# <a name="wdsutil-add-drivergroup"></a>WDSUTIL adicionar-controlador de driver

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um grupo de drivers ao servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-DriverGroup /DriverGroup:<Groupname>\n\ [/Server:<Servername>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filtertype> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica o nome do novo grupo de drivers. |
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| Habilitado`{Yes|No}` | Habilita ou desabilita o pacote. |
| Aplicabilidade`{Matched|All}` | Especifica quais pacotes instalar se os critérios de filtro forem atendidos. **MATCHED** significa instalar apenas os pacotes de driver que correspondem a um hardware de cliente. **Tudo** significa instalar todos os pacotes para clientes, independentemente de seu hardware. |
| FilterType`<Filtertype>` | Especifica o tipo do filtro a ser adicionado ao grupo. Você pode especificar vários tipos de filtro em um único comando. Cada tipo de filtro deve ser seguido por **/Policy** e pelo menos um **/Value**. Os valores válidos incluem:<ul><li>BiosVendor</li><li>Biosversion</li><li>Chassistype</li><li>Fabricante</li><li>Uuid</li><li>OSVersion</li><li>Osedition</li><li>OsLanguage</li></ul> Para obter informações sobre como obter valores para todos os outros tipos de filtro, consulte [filtros de grupo de driver](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |
| [/Policy: `{Include|Exclude}` ] | Especifica a política a ser definida no filtro. Se **/Policy** estiver definido como **include**, os computadores cliente que correspondem ao filtro terão permissão para instalar os drivers nesse grupo. Se **/Policy** for definido como **Exclude**, os computadores cliente que corresponderem ao filtro não terão permissão para instalar os drivers nesse grupo. |
| [/Value: `<Value>` ] | Especifica o valor do cliente que corresponde a **/FilterType**. Você pode especificar vários valores para um único tipo. Para obter informações sobre valores de tipo de filtro aceitáveis, consulte [filtros de grupo de driver](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |

## <a name="examples"></a>Exemplos

Para adicionar um grupo de drivers, digite:

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-drivergrouppackage](wdsutil-add-drivergrouppackage.md)

- [comando WDSUTIL Add-drivergrouppackages](wdsutil-add-drivergrouppackages.md)

- [comando WDSUTIL Add-drivergroupfilter](wdsutil-add-drivergroupfilter.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
