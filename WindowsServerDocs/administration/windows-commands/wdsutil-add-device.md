---
title: Adicionar dispositivo do WDSUTIL
description: Artigo de referência do comando WDSUTIL Add-Device, que prepara previamente um computador no Active Directory Domain Services.
ms.topic: reference
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: de05d5510e61f5cba0813a7e11215935fd380968
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524632"
---
# <a name="wdsutil-add-device"></a>Adicionar dispositivo do WDSUTIL

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pré-prepara um computador no Active Directory Domain Services (AD DS). Os computadores pré-configurados também são chamados de *computadores conhecidos*. Isso permite que você configure Propriedades para controlar a instalação do cliente. Por exemplo, você pode configurar o programa de inicialização de rede e o arquivo autônomo que o cliente deve receber, bem como o servidor do qual o cliente deve baixar o programa de inicialização de rede.

## <a name="syntax"></a>Sintaxe

```
wdsutil /add-Device /Device:<Devicename> /ID:<UUID | MAC address> [/ReferralServer:<Servername>] [/BootProgram:<Relativepath>] [/WdsClientUnattend:<Relativepath>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relativepath>] [/OU:<DN of OU>] [/Domain:<Domain>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| Vice`<Devicename>` | Especifica o nome do dispositivo a ser adicionado. |
| Sessão`<UUID|MAC address>` | Especifica o GUID/UUID ou o endereço MAC do computador. Um GUID/UUID deve estar em um dos dois formatos: cadeia de caracteres binária ( `/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6` ) ou cadeia de caracteres GUID ( `/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6` ). Um endereço MAC deve estar no seguinte formato: **00B056882FDC** (sem traços) ou **00-B0-56-88-2F-DC** (com traços) |
| [/ReferralServer: `<Servername>` ] | Especifica o nome do servidor a ser contatado para baixar o programa de inicialização de rede e a imagem de inicialização usando o trivial protocolo FTP (TFTP). |
| [/BootProgram: `<Relativepath>` ] | Especifica o caminho relativo da pasta **remoteInstall** para o programa de inicialização de rede que este computador deve receber. Por exemplo: `boot\x86\pxeboot.com` |
| [/WdsClientUnattend: `<Relativepath>` ] | Especifica o caminho relativo da pasta **remoteInstall** para o arquivo de instalação autônoma que automatiza as telas de instalação do cliente dos serviços de implantação do Windows. |
| [/User: `<Domain\User|User@Domain>` ] | Define permissões no objeto de conta de computador para dar ao usuário especificado os direitos necessários para ingressar o computador no domínio. |
| [/JoinRights: `{JoinOnly|Full}` ] | Especifica o tipo de direitos a serem atribuídos ao usuário.<ul><li>**JoinOnly** – requer que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio.</li><li>**Completo** – fornece acesso completo ao usuário, que inclui o direito de ingressar o computador no domínio. |
| [/JoinDomain: `{Yes|No}` ] | Especifica se o computador deve ser ingressado no domínio como esta conta de computador durante a instalação do sistema operacional. O valor padrão é **Sim**. |
| [/BootImagepath: `<Relativepath>` ] | Especifica o caminho relativo da pasta **remoteInstall** para a imagem de inicialização que esse computador deve usar. |
| [/OU: `<DN of OU>` ] | O nome distinto da unidade organizacional onde o objeto de conta de computador deve ser criado. Por exemplo: **ou = MyOU, CN = test, DC = Domain, DC = com**. O local padrão é o contêiner do computador padrão. |
| [/Domain: `<Domain>` ] | O domínio em que o objeto de conta de computador deve ser criado. O local padrão é o domínio local. |

## <a name="examples"></a>Exemplos

Para adicionar um computador usando um endereço MAC, digite:

```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```

Para adicionar um computador usando uma cadeia de caracteres GUID, digite:

```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Get-meus dispositivos](wdsutil-get-alldevices.md)

- [comando de Get-Device do WDSUTIL](wdsutil-get-device.md)

- [WDSUTIL Set-comando de dispositivo](wdsutil-set-device.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)

- [New-WdsClient](/powershell/module/wds/New-WdsClient)
