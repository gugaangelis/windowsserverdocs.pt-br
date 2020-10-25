---
title: WDSUTIL aprovar-AutoAddDevices
description: Artigo de referência do comando WDSUTIL Approve-AutoAddDevices, que aprova computadores que estão com aprovação administrativa pendente.
ms.topic: reference
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 31dba6d0a0bb585f61433f86d1aa0ae4388fe484
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524931"
---
# <a name="wdsutil-approve-autoadddevices"></a>WDSUTIL aprovar-AutoAddDevices

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aprova computadores que estão com aprovação administrativa pendente. Quando a política de adição automática está habilitada, a aprovação administrativa é necessária antes que computadores desconhecidos (aqueles que não são pré-configurados) possam instalar uma imagem. Você pode habilitar essa política usando a guia **resposta do PXE** da página de propriedades do servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| RequestId`{Request ID|ALL}` | Especifica a ID da solicitação atribuída ao computador pendente. Especifique **All** para aprovar todos os computadores pendentes. |
| MachineName`<Devicename>` | Especifica o nome do dispositivo a ser adicionado. Você não pode usar essa opção ao aprovar todos os computadores. |
| [/OU: `<DN of OU>` ] | O nome distinto da unidade organizacional onde o objeto de conta de computador deve ser criado. Por exemplo: **ou = MyOU, CN = test, DC = Domain, DC = com**. O local padrão é o contêiner do computador padrão. |
| [/User: `<Domain\User|User@Domain>` ] | Define permissões no objeto de conta de computador para dar ao usuário especificado os direitos necessários para ingressar o computador no domínio. |
| [/JoinRights: `{JoinOnly|Full}` ] | Especifica o tipo de direitos a serem atribuídos ao usuário.<ul><li>**JoinOnly** – requer que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio.</li><li>**Completo** – fornece acesso completo ao usuário, que inclui o direito de ingressar o computador no domínio. |
| [/JoinDomain: `{Yes|No}` ] | Especifica se o computador deve ser ingressado no domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**. |
| [/ReferralServer: `<Servername>` ] | Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando o trivial protocolo FTP (TFTP). |
| [/BootProgram: `<Relativepath>` ] | Especifica o caminho relativo da pasta **remoteInstall** para o programa de inicialização de rede que este computador deve receber. Por exemplo: **boot\x86\pxeboot.com**. |
| [/WdsClientUnattend: `<Relativepath>` ] | Especifica o caminho relativo da pasta **remoteInstall** para o arquivo autônomo que automatiza o cliente dos serviços de implantação do Windows. |
| [/BootImagepath: `<Relativepath>` ] | Especifica o caminho relativo da pasta remoteInstall para a imagem de inicialização que este computador deve receber. |

## <a name="examples"></a>Exemplos

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

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Delete-AutoAddDevices](wdsutil-delete-autoadddevices.md)

- [comando WDSUTIL Get-AutoAddDevices](wdsutil-get-autoadddevices.md)

- [comando WDSUTIL Reject-AutoAddDevices](wdsutil-reject-autoadddevices.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
