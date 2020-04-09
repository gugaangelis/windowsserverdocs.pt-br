---
title: Conjunto de subcomandos-DriverPackage
description: O tópico comandos do Windows para o subcomando set-DriverPackage, que renomeia e/ou habilita ou desabilita um pacote de driver em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d282b3338e67a33a55481658f55db69930b10e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834009"
---
# <a name="subcommand-set-driverpackage"></a>Subcomando: Set-DriverPackage

Renomeia e/ou habilita ou desabilita um pacote de driver em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

### <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server: nome do servidor\<>] |                                                                                                                                                 Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. se um nome de servidor não for especificado, o servidor local será usado.                                                                                                                                                 |
| [/DriverPackage: nome do\<>] |                                                                                                                                                                                       Especifica o nome atual do pacote de driver a ser modificado.                                                                                                                                                                                        |
|    [/PackageId: ID de\<>]    | Especifica a ID dos serviços de implantação do Windows do pacote de driver. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome. Para localizar essa ID para um pacote, clique no grupo de drivers no qual o pacote está (ou no nó **todos os pacotes** ), clique com o botão direito do mouse no pacote e clique em **Propriedades**. A ID do pacote é listada na guia **geral** . Por exemplo: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name:\<novo nome >]    |                                                                                                                                                                                              Especifica o novo nome para o pacote de driver.                                                                                                                                                                                              |
|      [/Enabled: {Sim      |                                                                                                                                                                                                                   foi                                                                                                                                                                                                                    |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para alterar as configurações de um pacote, digite um dos seguintes:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)