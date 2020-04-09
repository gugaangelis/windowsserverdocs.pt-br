---
title: Get-DriverPackage
description: O tópico de comandos do Windows para Get-DriverPackage, que exibe informações sobre um pacote de driver no servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1906a109d22b24b5a44227d56c726996e6532bd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831049"
---
# <a name="get-driverpackage"></a>Get-DriverPackage

Exibe informações sobre um pacote de driver no servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                           Descrição                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server: nome do servidor\<>] |              Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.               |
| [/DriverPackage: nome do\<>] |                                                        Especifica o nome do pacote de driver a ser mostrado.                                                         |
|    [/PackageId: ID de\<>]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser mostrado. Você deve especificar a ID se o pacote de driver não puder ser identificado exclusivamente pelo nome. |
|     [/Show: {drivers     |                                                                              Files                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir informações sobre um pacote de driver, digite um dos seguintes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)