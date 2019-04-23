---
title: Usando o comando get-dispositivo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871027"
---
# <a name="using-the-get-device-command"></a>Usando o comando get-dispositivo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações de serviços de implantação do Windows sobre um computador pré-testado (ou seja, um computador físico que tem sido embutido em uma conta de computador nos serviços de domínio do Active Directory.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/ Dispositivo:<Device name>|Especifica o nome do computador (SAMAccountName).|
|/ID:<MAC or UUID>|Especifica o endereço MAC ou o UUID (GUID) do computador, conforme mostrado nos exemplos a seguir. Observe que um GUID válido deve ser em um dos dois formatos de cadeia de caracteres binária ou de cadeia de caracteres GUID<br /><br />-   **Cadeia de caracteres binária**: /ID:ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Endereço MAC**: 00B056882FDC (sem traços) ou 00-B0-56-88-2F-DC (com traços)<br />-   **Cadeia de caracteres GUID**: /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain:<Domain>]|Especifica o domínio a ser pesquisado para o computador pré-testado. O valor padrão para esse parâmetro é o domínio local.|
|[/ floresta: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows deve pesquisar toda a floresta ou domínio local. O valor padrão é **não**, que significa que somente o domínio local será pesquisado.|
## <a name="BKMK_examples"></a>Exemplos
Para obter informações usando o nome do computador, digite:
```
wdsutil /Get-Device /Device:computer1
```
Para obter informações usando o endereço MAC, digite:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Para obter informações usando a cadeia de caracteres do GUID, digite:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: dispositivo conjunto](subcommand-set-device.md)
[usando o comando add-Device](using-the-add-device-command.md)
[usando o Comando Get-AllDevices](using-the-get-alldevices-command.md)
