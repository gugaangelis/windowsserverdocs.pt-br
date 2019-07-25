---
title: Funções, serviços de função e recursos que não estão no Windows Server-Server Core
description: Saiba mais sobre as funções e os recursos não incluídos na opção de instalação do Server Core para o Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476512"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Funções, serviços de função e recursos que não estão no Windows Server-Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

As funções, os serviços de função e os recursos a seguir foram removidos da opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para seu ambiente.

> [!NOTE]
> Você também pode ver uma lista de funções, serviços de função e recursos que [estão incluídos no Server Core](server-core-roles-and-services.md). É uma lista muito grande, portanto, para obter melhores resultados, pesquise essa lista para a função ou o recurso específico no qual você está interessado.

## <a name="roles-not-in-server-core"></a>Funções que não estão no Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Serviços de função não estão no Server Core
Observe que alguns serviços de função Área de Trabalho Remota estão incluídos no Server Core (agente de conexão, licenciamento, host de virtualização), mas outros não são (gateway, Host da Sessão RD, Acesso via Web).

- Print-Scan-Server
- Impressão-Internet
- RDS-gateway
- RDS-RD-Server
- RDS-acesso via Web
- Web-MGMT-console
- Web-LGCY-MGMT-console
- WDS-implantação
- WDS-Transport *(antes do Windows Server versão 1803)*

## <a name="features-not-in-server-core"></a>Recursos que não estão no Server Core

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Execução direta
- Internet-impressão-cliente
- LPR-monitor de porta
- MSMQ-multicast
- DO
- Assistência remota
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-bits-servidor
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-ferramentas
- RSAT-RDS-Tools
- RSAT-RDS-gateway
- RSAT-RDS-Licensing-diagnóstico-interface do usuário
- RDS-Licensing-interface do usuário
- Updateservices-interface do usuário
- RSAT-ADCS
- RSAT-ADCS-MGMT
- RSAT-online-Respondente
- RSAT-ADRMS
- RSAT-fax
- RSAT-arquivos-serviços
- RSAT-DFS-MGMT-con
- RSAT-FSRM-MGMT
- RSAT-NFS-admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-ferramentas
- WDS-AdminPack
- Servidor SMTP
- TFTP-cliente
- WebDAV – redirecionador
- Biometria-estrutura
- Windows-Defender-GUI
- Windows-Identity-Foundation
- PowerShell – ISE
- Serviço de pesquisa
- Windows-TIFF-IFilter
- Sem fio-rede
- XPS-Viewer

