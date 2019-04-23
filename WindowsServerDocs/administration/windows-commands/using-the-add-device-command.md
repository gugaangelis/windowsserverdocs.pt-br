---
title: Usando o comando Adicionar dispositivo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9ef4445b4dabbe85c2397d62b06756e17879d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878537"
---
# <a name="using-the-add-device-command"></a>Usando o comando Adicionar dispositivo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pré-configura um computador nos serviços de domínio do Active Directory. Computadores pré-configurados também são chamados de computadores conhecidos. Isso permite que você configure as propriedades para controlar a instalação do cliente. Por exemplo, você pode configurar o programa de inicialização de rede e o arquivo autônomo que o cliente deve receber, bem como o servidor do qual o cliente deve baixar o programa de inicialização de rede.
Para obter exemplos de como você pode usar esse comando, consulte [exemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxe
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/ Dispositivo:<computer name>|Especifica o nome do computador a ser adicionado.|
|/ ID: < UUID &#124; endereço MAC >|Especifica o GUID/UUID ou o endereço MAC do computador. Um GUID/UUID deve estar em uma cadeia de caracteres de dois formatos binários ou de cadeia de caracteres GUID. Por exemplo: <br /><br />Cadeia de caracteres binária: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br /><br />Cadeia de caracteres GUID: **/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br /><br />Um endereço MAC deve ser no seguinte formato: **00B056882FDC** (sem traços) ou **00-B0-56-88-2F-DC** (com traços)|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando Trivial FTP (tftp).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que deve receber a este computador. Por exemplo: "boot\x86\pxeboot.com"|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall até o arquivo de instalação autônoma que automatiza as telas de instalação do cliente dos serviços de implantação do Windows.|
|[/ Usuário: < domínio \ usuário &#124; User@Domain>]|Define as permissões no objeto de conta de computador para que o usuário especificado os direitos necessários para ingressar o computador ao domínio.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica o tipo de direitos a ser atribuído ao usuário.<br /><br />-   **JoinOnly** exige que o administrador redefinir a conta de computador antes do usuário pode ingressar o computador ao domínio.<br />-   **Completo** fornece acesso completo para o usuário, que inclui o direito de ingressar o computador no domínio.|
|[/JoinDomain:{Yes &#124; No}]|Especifica se ou não o computador deve estar associado ao domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall à imagem de inicialização que este computador deve usar.|
|[/OU:<DN of OU>]|O nome diferenciado da unidade organizacional em que o objeto de conta de computador deve ser criado. Por exemplo:  **OU=MyOU,CN=Test, DC=Domain,DC=com**. O local padrão é o contêiner do computador padrão.|
|[/Domain:<Domain>]|O domínio no qual o objeto de conta de computador deve ser criado. O local padrão é o domínio local.|
## <a name="BKMK_examples"></a>Exemplos
Para adicionar um computador usando um endereço MAC, digite:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Para adicionar um computador usando uma cadeia de caracteres do GUID, digite:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:"OU=MyOU,CN=Test,DC=Domain,DC=com"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllDevices](using-the-get-alldevices-command.md)
[usando o comando get-dispositivo](using-the-get-device-command.md)
[subcomando: set-Device](subcommand-set-device.md)
[WdsClient novo](https://technet.microsoft.com/library/dn283430.aspx)
