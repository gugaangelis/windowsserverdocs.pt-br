---
title: Get-DriverPackage
description: Tópico de referência para Get-DriverPackage, que exibe informações sobre um pacote de driver no servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fc6bc327b46f8219a7c40fa47e85cc94b6fc749
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719942"
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
| [/Server:\<nome do servidor>] |              Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.               |
| [/DriverPackage:\<nome>] |                                                        Especifica o nome do pacote de driver a ser mostrado.                                                         |
|    [/PackageId:\<ID>]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser mostrado. Você deve especificar a ID se o pacote de driver não puder ser identificado exclusivamente pelo nome. |
|     [/Show: {drivers     |                                                                              Arquivos                                                                               |

## <a name="examples"></a>Exemplos

Para exibir informações sobre um pacote de driver, digite um dos seguintes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)