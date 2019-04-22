---
title: Funções, serviços de função e recursos não no Windows Server - Server Core
description: Saiba mais sobre as funções e recursos não incluídos na opção de instalação Server Core do Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825527"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Funções, serviços de função e recursos não no Windows Server - Server Core

> Aplica-se a: Windows Server (canal semestral) e Windows Server 2016

Funções, serviços de função e recursos a seguir foram retirados a opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para o seu ambiente.

> [!NOTE]
> Você também pode ver uma lista de funções, serviços de função e recursos que [estão incluídos no Server Core](server-core-roles-and-services.md). É uma lista muito grande, então, para obter melhores resultados, pesquise essa lista para a função ou recurso específico que está interessado.

## <a name="roles-not-in-server-core"></a>Funções não estão no núcleo do servidor

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Serviços de função não está no núcleo do servidor
Observe que alguns serviços de função da área de trabalho remota estão incluídos no Server Core (Conexão Broker, licenciamento, Host de virtualização), mas outros não são (Gateway, o Host de sessão de área de trabalho remota, acesso via Web).

- Servidor de digitalização de impressão
- Internet de impressão
- RDS-Gateway
- RDS-RD-Server
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- WDS-Deployment
- Transporte do WDS *(antes da versão 1803 do Windows Server)*

## <a name="features-not-in-server-core"></a>Recursos não estão no núcleo do servidor

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct-Play
- Cliente de impressão via Internet
- Monitor de porta LPR
- Multicast do MSMQ
- CMAK
- Assistência remota
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-Bits-Server
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-Tools
- RSAT-RDS-Tools
- RSAT-RDS-Gateway
- RSAT-RDS-Licensing-Diagnosis-UI
- RDS-Licensing-UI
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- RSAT-File-Services
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-Tools
- WDS-AdminPack
- SMTP-Server
- Cliente TFTP
- WebDAV-Redirector
- Biometric Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- Search-Service
- Windows-TIFF-IFilter
- Wireless-Networking
- XPS-Viewer

