---
title: Adicionar dispositivo
description: O tópico comandos do Windows para adicionar dispositivo, que pré-testar um computador nos serviços de domínio Active Directory. Os computadores pré-configurados também são chamados de computadores conhecidos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0acccb75d5e378a556e4b7ca8b37f805a8743dae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832159"
---
# <a name="add-device"></a>Adicionar dispositivo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pré-teste de um computador nos serviços de domínio Active Directory. Os computadores pré-configurados também são chamados de computadores conhecidos. Isso permite que você configure Propriedades para controlar a instalação do cliente. Por exemplo, você pode configurar o programa de inicialização de rede e o arquivo autônomo que o cliente deve receber, bem como o servidor do qual o cliente deve baixar o programa de inicialização de rede.
para obter exemplos de como você pode usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Device:<computer name>|Especifica o nome do computador a ser adicionado.|
|/ID: < endereço &#124; MAC UUID >|Especifica o GUID/UUID ou o endereço MAC do computador. Um GUID/UUID deve estar em um dos dois formatos de cadeia de caracteres binária ou de GUID. Por exemplo:<p>Cadeia de caracteres binária: **/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>Cadeia de caracteres GUID: **/ID: E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>Um endereço MAC deve estar no seguinte formato: **00B056882FDC** (sem traços) ou **00-B0-56-88-2F-DC** (com traços)|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando o trivial protocolo FTP (TFTP).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que este computador deve receber. Por exemplo: boot\x86\pxeboot.com|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o arquivo de instalação autônoma que automatiza as telas de instalação do cliente dos serviços de implantação do Windows.|
|[/User: < domínio &#124; \ usuário User@Domain>]|Define permissões no objeto de conta de computador para dar ao usuário especificado os direitos necessários para ingressar o computador no domínio.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica o tipo de direitos a serem atribuídos ao usuário.<p>-   **JoinOnly** exige que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio.<br />-   **completo** dá acesso completo ao usuário, que inclui o direito de ingressar o computador no domínio.|
|[/JoinDomain: {Sim &#124; não}]|Especifica se o computador deve ou não ingressar no domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para a imagem de inicialização que esse computador deve usar.|
|[/OU:<DN of OU>]|O nome distinto da unidade organizacional onde o objeto de conta de computador deve ser criado. Por exemplo: **ou = MyOU, CN = test, DC = Domain, DC = com**. O local padrão é o contêiner do computador padrão.|
|[/Domain:<Domain>]|O domínio em que o objeto de conta de computador deve ser criado. O local padrão é o domínio local.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para adicionar um computador usando um endereço MAC, digite:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Para adicionar um computador usando uma cadeia de caracteres GUID, digite:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Get-mydevices](using-the-get-alldevices-command.md)
[usando o comando Get-Device](using-the-get-device-command.md)
o [subcomando: Set-Device](subcommand-set-device.md)
[New-WdsClient](https://technet.microsoft.com/library/dn283430.aspx)
