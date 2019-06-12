---
title: Usando o comando remove-/InfFile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 217ff23b8724464670520d0b2d5b196df5a4af47
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440304"
---
# <a name="using-the-remove-driverpackage-command"></a>Usando o comando remove-/InfFile

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um pacote de driver do servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                            Descrição                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se não for especificado um nome de servidor, o servidor local será usado.              |
| [/DriverPackage:<Name>] |                                                        Especifica o nome do pacote de driver a ser removido.                                                         |
|    [/PackageId:<ID>]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver para remover. Se o pacote de driver não pode ser identificado exclusivamente pelo nome, você deve especificar a ID. |

## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre as imagens, digite o seguinte:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando remove-DriverPackages](using-the-remove-driverpackages-command.md)
