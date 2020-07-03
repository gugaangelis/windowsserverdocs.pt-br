---
title: Get-DriverPackage
description: Artigo de referência para Get-DriverPackage, que exibe informações sobre um pacote de driver no servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0ca307b9f42d0921c896df2fe622c5b0f8a853d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932254"
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
| [/Server:\<Server name>] |              Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.               |
| [/DriverPackage: \<Name> ] |                                                        Especifica o nome do pacote de driver a ser mostrado.                                                         |
|    [/PackageId: \<ID> ]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser mostrado. Você deve especificar a ID se o pacote de driver não puder ser identificado exclusivamente pelo nome. |
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