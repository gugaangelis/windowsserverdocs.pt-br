---
title: Conjunto de subcomandos – dispositivo
description: Tópico de referência para subcomando set-Device, que altera os atributos de um computador pré-configurado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a587b809b02da775594e795730799bf9af578bc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721754"
---
# <a name="subcommand-set-device"></a>Subcomando: Set-Device

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de um computador pré-configurado. Um computador pré-configurado é um computador que foi vinculado a um objeto de conta de computador nos servidores de domínio Active Directory (AD DS). Os clientes pré-configurados também são chamados de computadores conhecidos. Você pode configurar propriedades na conta do computador para controlar a instalação do cliente do. Por exemplo, você pode configurar o programa de inicialização de rede e o arquivo autônomo que o cliente deve receber, bem como o servidor do qual o cliente deve baixar o programa de inicialização de rede.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|Vice<computer name>|Especifica o nome do computador (SAM-Account-Name).|
|[/ID: <UUID &#124; endereço MAC>]|Especifica o GUID/UUID ou o endereço MAC do computador. Esse valor deve estar em um dos três formatos a seguir:<p>-Cadeia de caracteres binária: **/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID cadeia de caracteres:/ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Endereço MAC: **00B056882FDC** (sem traços) ou **00-B0-56-88-2F-DC** (com traços)|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando o trivial protocolo FTP (TFTP).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que o computador especificado receberá. Por exemplo: **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o arquivo autônomo que automatiza as telas de instalação do cliente dos serviços de implantação do Windows.|
|[/User: <domínio \ User@Domain usuário &#124;>]|Define permissões no objeto de conta de computador para dar ao usuário especificado os direitos necessários para ingressar o computador no domínio.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica o tipo de direitos a serem atribuídos ao usuário.<p>-   O **JoinOnly** exige que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio.<br />-   **Completo** dá acesso completo ao usuário, incluindo o direito de ingressar o computador no domínio.|
|[/JoinDomain: {Sim &#124; não}]|Especifica se o computador deve ou não ingressar no domínio como esta conta de computador durante uma instalação dos serviços de implantação do Windows. A configuração padrão é **Sim**.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para a imagem de inicialização que o computador usará.|
|[/Domain:<Domain>]|Especifica o domínio a ser procurado para o computador pré-configurado. O valor padrão é o domínio local.|
|[/resetAccount]|Redefine as permissões no computador especificado para que qualquer pessoa com as permissões apropriadas possa ingressar no domínio usando essa conta.|
## <a name="examples"></a>Exemplos
Para definir o programa de inicialização de rede e o servidor de referência para um computador, digite:
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Para definir várias configurações para um computador, digite:
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando usando o
[comando Adicionar dispositivo](using-the-add-device-command.md)
[usando o comando Get-meus dispositivos](using-the-get-alldevices-command.md)[usando o comando Get-Device](using-the-get-device-command.md)
