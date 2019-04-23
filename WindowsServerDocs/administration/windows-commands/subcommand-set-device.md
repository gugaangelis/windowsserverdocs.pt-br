---
title: Subcomando o conjunto de dispositivo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80bb9144936cf493784603bcbdb8a0d1e5c870bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848057"
---
# <a name="subcommand-set-device"></a>Subcomando: o conjunto de dispositivo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de um computador pré-testado. Um computador pré-testado é um computador que tenha sido vinculado a um objeto de conta de computador em servidores de domínio do Active Directory (AD DS). A clientes pré-testados também é chamada de computadores conhecidos. Você pode configurar propriedades na conta de computador para controlar a instalação do cliente. Por exemplo, você pode configurar o programa de inicialização de rede e o arquivo autônomo que o cliente deve receber, bem como o servidor do qual o cliente deve baixar o programa de inicialização de rede.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/ Dispositivo:<computer name>|Especifica o nome do computador (nome de conta SAM).|
|[/ID:<UUID &#124; MAC address>]|Especifica o GUID/UUID ou o endereço MAC do computador. Esse valor deve estar em um dos três formatos a seguir:<br /><br />-Cadeia de caracteres binária: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-Cadeia de caracteres GUID/UUID: / ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Endereço MAC: **00B056882FDC** (sem traços) ou **00-B0-56-88-2F-DC** (com traços)|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar a imagem rede de inicialização e o programa de inicialização usando Trivial FTP (tftp).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que receberão o computador especificado. Por exemplo: **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o arquivo autônomo que automatiza as telas de instalação para o cliente dos serviços de implantação do Windows.|
|[/ Usuário: < domínio \ usuário &#124; User@Domain>]|Define as permissões no objeto de conta de computador para que o usuário especificado os direitos necessários para ingressar o computador ao domínio.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica o tipo de direitos a ser atribuído ao usuário.<br /><br />-   **JoinOnly** exige que o administrador redefinir a conta de computador antes do usuário pode ingressar o computador ao domínio.<br />-   **Completo** fornece acesso completo para o usuário, inclusive o direito de ingressar o computador no domínio.|
|[/JoinDomain:{Yes &#124; No}]|Especifica se o computador deve estar associado ao domínio como esta conta de computador durante uma instalação de serviços de implantação do Windows. A configuração padrão é **Sim**.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall à imagem de inicialização que usará o computador.|
|[/Domain:<Domain>]|Especifica o domínio a ser pesquisado para o computador pré-testado. O valor padrão é o domínio local.|
|[/resetAccount]|Redefine as permissões no computador especificado para que qualquer pessoa com as permissões apropriadas pode ingressar no domínio usando essa conta.|
## <a name="BKMK_examples"></a>Exemplos
Para definir o servidor de referência e o programa de inicialização rede para um computador, digite:
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Para definir várias configurações para um computador, digite:
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Device](using-the-add-device-command.md)
[usando o comando get-AllDevices](using-the-get-alldevices-command.md)
[usando o Comando Get-dispositivo](using-the-get-device-command.md)
