---
title: Usando o comando AutoaddDevices aprovar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886817"
---
# <a name="using-the-approve-autoadddevices-command"></a>Usando o comando AutoaddDevices aprovar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aprova os computadores que estão aguardando aprovação administrativa. Quando a política de adição automática está habilitada, é necessária aprovação administrativa antes que os computadores desconhecidos (aquelas que não foram pré-testados) possam instalar uma imagem. Você pode habilitar essa política usando o **resposta do PXE** guia da página de propriedades de servidor s.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
|/RequestId:{Request ID &#124; ALL}|Especifica a ID da solicitação atribuída ao computador pendente. Especificar **todos os** para aprovar todos os computadores pendentes.|
|[/MachineName:<Device name>]|Especifica o nome do computador a ser adicionado. Você não pode usar essa opção ao aprovar todos os computadores.|
|[/OU:<DN of OU>]|Especifica o nome diferenciado da unidade organizacional (UO) em que o objeto de conta de computador deve ser criado. Por exemplo:  **OU=MyOU,CN=Test,DC=Domain,DC=com**. O local padrão é o contêiner do computador padrão.|
|[/ Usuário: < domínio \ usuário &#124; User@Domain>]|Define as permissões no objeto de conta de computador para atribuir o usuário especificado os direitos necessários.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica o tipo de direitos a ser atribuído ao usuário especificado.<br /><br />-   **JoinOnly** exige que o administrador redefinir a conta de computador antes do usuário pode ingressar o computador ao domínio.<br />-   **Completo** fornece acesso completo para o usuário, que inclui o direito de ingressar o computador no domínio.|
|[/JoinDomain:{Yes &#124; No}]|Especifica se ou não o computador deve estar associado ao domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**.|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar a imagem de inicialização e o programa de inicialização rede usando o protocolo TFTP (tftp).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que deve receber a este computador. Por exemplo: **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o arquivo autônomo que automatiza o cliente dos serviços de implantação do Windows.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall à imagem de inicialização que este computador deve receber.|
## <a name="BKMK_examples"></a>Exemplos
Para aprovar o computador com uma ID de solicitação de 12, digite:
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Para aprovar o computador com uma ID de solicitação de 20 e implantar a imagem com as configurações especificadas, digite:
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Para aprovar todos os computadores pendentes, digite:
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando delete AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando o comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Usando o comando AutoaddDevices de rejeição](using-the-reject-autoadddevices-command.md)
