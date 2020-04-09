---
title: Aprovar-AutoaddDevices
description: Tópico de comandos do Windows para Approve-AutoaddDevices, que aprova computadores que estão com aprovação administrativa pendente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f63faabf37337136cad186fd252915adf1a743
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831829"
---
# <a name="approve-autoadddevices"></a>Aprovar-AutoaddDevices

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aprova computadores que estão com aprovação administrativa pendente. Quando a política de adição automática está habilitada, a aprovação administrativa é necessária antes que computadores desconhecidos (aqueles que não são pré-configurados) possam instalar uma imagem. Você pode habilitar essa política usando a guia **resposta do PXE** da página de propriedades do servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
|/RequestId: {ID &#124; da solicitação All}|Especifica a ID da solicitação atribuída ao computador pendente. Especifique **All** para aprovar todos os computadores pendentes.|
|[/MachineName:<Device name>]|Especifica o nome do computador a ser adicionado. Você não pode usar essa opção ao aprovar todos os computadores.|
|[/OU:<DN of OU>]|Especifica o nome distinto da UO (unidade organizacional) em que o objeto de conta de computador deve ser criado. Por exemplo: **ou = MyOU, CN = test, DC = Domain, DC = com**. O local padrão é o contêiner do computador padrão.|
|[/User: < domínio &#124; \ usuário User@Domain>]|Define permissões no objeto de conta de computador para atribuir os direitos necessários ao usuário especificado.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica o tipo de direitos a serem atribuídos ao usuário especificado.<p>-   **JoinOnly** exige que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio.<br />-   **completo** dá acesso completo ao usuário, que inclui o direito de ingressar o computador no domínio.|
|[/JoinDomain: {Sim &#124; não}]|Especifica se o computador deve ou não ingressar no domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**.|
|[/ReferralServer:<Server name>]|Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando o trivial protocolo FTP (TFTP).|
|[/BootProgram:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o programa de inicialização de rede que este computador deve receber. Por exemplo: **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para o arquivo autônomo que automatiza o cliente dos serviços de implantação do Windows.|
|[/BootImagepath:<Relative path>]|Especifica o caminho relativo da pasta remoteInstall para a imagem de inicialização que este computador deve receber.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para aprovar o computador com um RequestId de 12, digite:
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Para aprovar o computador com um RequestID de 20 e implantar a imagem com as configurações especificadas, digite:
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:OU=Test,CN=company,DC=Domain,DC=Com /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Para aprovar todos os computadores pendentes, digite:
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando o comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[usando o comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
