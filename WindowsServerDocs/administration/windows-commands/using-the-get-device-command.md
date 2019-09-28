---
title: Usando o comando Get-Device
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6451e0a55a72fc88867a3f3be0e1317d881391aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363201"
---
# <a name="using-the-get-device-command"></a>Usando o comando Get-Device

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera as informações dos serviços de implantação do Windows sobre um computador pré-configurado (ou seja, um computador físico que foi embutido em uma conta de computador nos serviços de domínio Active Directory.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Device: <Device name>|Especifica o nome do computador (SAMAccountName).|
|/ID: <MAC or UUID>|Especifica o endereço MAC ou o UUID (GUID) do computador, conforme mostrado nos exemplos a seguir. Observe que um GUID válido deve estar em um dos dois formatos de cadeia de caracteres binária ou de GUID<br /><br />**cadeia de caracteres binária**-   :/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**endereço MAC**-   : 00B056882FDC (sem traços) ou 00-B0-56-88-2F-DC (com traços)<br />**cadeia de caracteres de GUID**-   :/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain: <Domain>]|Especifica o domínio a ser procurado para o computador pré-configurado. O valor padrão para esse parâmetro é o domínio local.|
|[/Forest: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows devem Pesquisar toda a floresta ou o domínio local. O valor padrão é **não**, o que significa que somente o domínio local será pesquisado.|
## <a name="BKMK_examples"></a>Disso
Para obter informações usando o nome do computador, digite:
```
wdsutil /Get-Device /Device:computer1
```
Para obter informações usando o endereço MAC, digite:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Para obter informações usando a cadeia de caracteres GUID, digite:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: set-Device](subcommand-set-device.md)
[usando o comando Add-Device](using-the-add-device-command.md)
[usando o comando Get-meus dispositivos](using-the-get-alldevices-command.md)
