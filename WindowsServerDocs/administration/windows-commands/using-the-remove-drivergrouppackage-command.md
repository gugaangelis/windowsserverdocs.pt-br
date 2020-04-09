---
title: Remove-DriverGroupPackage
description: O tópico de comandos do Windows para Remove-DriverGroupPackage, que remove um pacote de driver de um grupo de drivers em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dd30f430bd91bf2680cd1138270526bce965f9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830459"
---
# <a name="remove-drivergrouppackage"></a>Remove-DriverGroupPackage



Remove um pacote de driver de um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server: nome do servidor\<>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. se um nome de servidor não for especificado, o servidor local será usado.|
|[/DriverPackage: nome do\<>]|Especifica o nome do pacote de driver a ser removido.|
|[/PackageId: ID de\<>]|Especifica a ID dos serviços de implantação do Windows do pacote de driver a ser removido. Você deve especificar essa opção se o pacote de driver não puder ser identificado exclusivamente pelo nome.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)