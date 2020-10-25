---
title: WDSUTIL Add-drivergrouppackage
description: Artigo de referência para o comando WDSUTIL Add-drivergrouppackage, que adiciona um pacote de driver a um grupo de drivers.
ms.topic: reference
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4bf7b7c6cae4551e09fa5aa7d0244e5e4f0407e4
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524541"
---
# <a name="wdsutil-add-drivergrouppackage"></a>WDSUTIL Add-drivergrouppackage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um pacote de driver a um grupo de drivers.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica o nome do novo grupo de drivers. |
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| /DriverPackage:`<Name>` | Especifica o nome do pacote de driver a ser adicionado ao grupo. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome. |
| PackageId`<ID>` | Especifica a ID de um pacote. Para localizar a ID do pacote, selecione o grupo de drivers no qual o pacote está (ou o nó **todos os pacotes** ), clique com o botão direito do mouse no pacote e selecione **Propriedades**. A ID do pacote é listada na guia **geral** , por exemplo: **{DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}**. |

## <a name="examples"></a>Exemplos

Para adicionar um pacote de grupo de drivers, digite:

```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```

```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-drivergroupfilter](wdsutil-add-drivergroupfilter.md)

- [comando WDSUTIL Add-drivergrouppackages](wdsutil-add-drivergrouppackages.md)

- [comando WDSUTIL Add-driver](wdsutil-add-drivergroup.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
