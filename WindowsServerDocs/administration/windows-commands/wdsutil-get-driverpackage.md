---
title: Get-DriverPackage
description: Artigo de referência para Get-DriverPackage, que exibe informações sobre um pacote de driver no servidor.
ms.topic: reference
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 86f8b2cdc7a0bc62f42c02c9ff7c6852e9b159f0
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524431"
---
# <a name="get-driverpackage"></a>Get-DriverPackage

Exibe informações sobre um pacote de driver no servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                           Descrição                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |              Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.               |
| [/DriverPackage: \<Name> ] |                                                        Especifica o nome do pacote de driver a ser mostrado.                                                         |
|    [/PackageId: \<ID> ]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser mostrado. Você deve especificar a ID se o pacote de driver não puder ser identificado exclusivamente pelo nome. |
|     [/Show: {drivers     |                                                                              Arquivos                                                                               |

## <a name="examples"></a>Exemplos

Para exibir informações sobre um pacote de driver, digite um dos seguintes:
```
wdsutil /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
wdsutil /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)