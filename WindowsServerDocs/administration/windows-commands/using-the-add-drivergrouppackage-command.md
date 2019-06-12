---
title: Usando o comando add-DriverGroupPackage
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad9d180e2cf2110d36ebc82211af3a495a0e0b6b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440734"
---
# <a name="using-the-add-drivergrouppackage-command"></a>Usando o comando add-DriverGroupPackage

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um pacote de driver para um grupo de drivers.
## <a name="syntax"></a>Sintaxe
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                                                                                               Descrição                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 Especifica o nome do grupo de drivers.                                                                                                                                 |
|   /Server:<Server name>   |                                                                                  Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                  |
|   /DriverPackage:<Name>   |                                                                      Especifica o nome do pacote de driver a ser adicionado ao grupo. Você deve especificar essa opção se o pacote de driver não pode ser identificado exclusivamente pelo nome.                                                                       |
|      /PackageId:<ID>      | Especifica a ID de um pacote. Para localizar a ID do pacote, clique no grupo de driver que o pacote está em (ou o **todos os pacotes** nó), o pacote com o botão direito e, em seguida, clique em **propriedades**. A ID do pacote está listada na **gerais** guia, por exemplo: **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}** . |

## <a name="BKMK_examples"></a>Exemplos
Para adicionar um pacote de driver, digite o seguinte:
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[usando o comando add-/InfFile](using-the-add-driverpackage-command.md) 
 [Usando o subcomando add AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
