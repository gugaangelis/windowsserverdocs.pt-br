---
title: obter dispositivo
description: O tópico de comandos do Windows para Get-Device, que recupera as informações dos serviços de implantação do Windows sobre um computador pré-configurado (ou seja, um computador físico que foi alinhado a uma conta de computador nos serviços de domínio Active Directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b9554ed6236d02be0be3502f42552bafbbfe1cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831109"
---
# <a name="get-device"></a>obter dispositivo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera as informações dos serviços de implantação do Windows sobre um computador pré-configurado (ou seja, um computador físico que foi embutido em uma conta de computador nos serviços de domínio Active Directory.

## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Device:<Device name>|Especifica o nome do computador (SAMAccountName).|
|/ID:<MAC or UUID>|Especifica o endereço MAC ou o UUID (GUID) do computador, conforme mostrado nos exemplos a seguir. Observe que um GUID válido deve estar em um dos dois formatos de cadeia de caracteres binária ou de GUID<p>-   **cadeia de caracteres binária**:/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**endereço -   Mac**: 00B056882FDC (sem traços) ou 00-B0-56-88-2F-DC (com traços)<br />**cadeia de caracteres GUID**-   :/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain:<Domain>]|Especifica o domínio a ser procurado para o computador pré-configurado. O valor padrão para esse parâmetro é o domínio local.|
|[/Forest: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows devem Pesquisar toda a floresta ou o domínio local. O valor padrão é **não**, o que significa que somente o domínio local será pesquisado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para obter informações usando o nome do computador, digite:
```
wdsutil /Get-Device /Device:computer1
```
Para obter informações usando o endereço MAC, digite:
```
wdsutil /verbose /Get-Device /ID:00-B0-56-88-2F-DC /Domain:MyDomain
```
Para obter informações usando a cadeia de caracteres GUID, digite:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: set-Device](subcommand-set-device.md)
[usando o comando Add-Device](using-the-add-device-command.md)
[usando o comando Get-meus dispositivos](using-the-get-alldevices-command.md)
