---
title: Subcomando /InfFile conjunto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24acd672184b8df235e8de843961ac4adb2bd412
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441136"
---
# <a name="subcommand-set-driverpackage"></a>Subcommand: set-DriverPackage



Renomeia e/ou habilita ou desabilita um pacote de driver em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |                                                                                                                                                 Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se não for especificado um nome de servidor, o servidor local será usado.                                                                                                                                                 |
| [/DriverPackage:\<Name>] |                                                                                                                                                                                       Especifica o nome atual do pacote de driver para modificar.                                                                                                                                                                                        |
|    [/PackageId:\<ID>]    | Especifica a ID de serviços de implantação do Windows do pacote de driver. Você deve especificar essa opção se o pacote de driver não pode ser identificado exclusivamente pelo nome. Para localizar essa ID para um pacote, clique no grupo de driver que o pacote está em (ou o **todos os pacotes** nó), o pacote com o botão direito e, em seguida, clique em **propriedades**. A ID do pacote está listada na **geral** guia. Por exemplo: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name:\<novo nome >]    |                                                                                                                                                                                              Especifica o novo nome para o pacote de driver.                                                                                                                                                                                              |
|      [/ Habilitado: {Sim      |                                                                                                                                                                                                                   Não}                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>Exemplos

Para alterar as configurações de um pacote, digite um dos seguintes:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)