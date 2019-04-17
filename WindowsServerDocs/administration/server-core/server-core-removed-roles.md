---
title: Funções, serviços de função e recursos não no Windows Server - núcleo do servidor
description: Saiba mais sobre as funções e recursos não são incluídos na opção de instalação Server Core do Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2018
ms.locfileid: "2604784"
---
# Funções, serviços de função e recursos não no Windows Server - núcleo do servidor

> Aplica-se a: Windows Server (canal delimitadas anual) e Windows Server 2016

As seguintes funções, serviços de função e recursos foram removidos da opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para seu ambiente.

> [!NOTE]
> Você também pode ver uma lista das funções, serviços de função e recursos que [estão incluídos no núcleo do servidor](server-core-roles-and-services.md). É uma lista muito grande, portanto para obter melhores resultados, procure essa lista para a função específica ou recurso que você está interessado em.

## Funções não no núcleo do servidor

- Fax
- MultiPointServerRole
- NPAS
- WDS

## Serviços de função não no núcleo do servidor
Observe que alguns serviços de função de área de trabalho remota estão incluídos na Server Core (Conexão Broker, licenciamento, Host de virtualização), mas outros não são (Gateway, o Host de sessão RD, Web Access).

- Servidor de verificação de impressão
- Imprimir à Internet
- Gateway do RDS
- Servidor de RD RDS
- Acesso de Web ao RDS
- Console de gerenciamento da Web
- Console da Web-Lgcy-Mgmt
- Implantação do WDS
- WDS-Transport *(antes do Windows Server versão 1803)*

## Recursos que não núcleo do servidor

- BITS-IIS-Ext
- O BitLocker-NetworkUnlock
- Direto-Play
- Cliente de impressão via Internet
- Monitor de porta LPR
- MSMQ-multicast
- CMAK
- Assistência remota
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- Servidor RSAT-Bits
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Ferramentas Hyper-V
- Ferramentas do RSAT RDS
- RSAT-RDS-Gateway
- RSAT-RDS-licenciamento-diagnóstico-UI
- UI de licenciamento RDS
- UI de UpdateServices
- RSAT-DACS
- RSAT-DACS-Mgmt
- RSAT--Respondente Online
- RSAT-ADRMS
- RSAT-Fax
- Serviços de arquivo RSAT
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- Serviços de impressão RSAT
- Ferramentas de VA RSAT
- WDS-AdminPack
- Servidor SMTP
- Cliente TFTP
- Redirecionador de WebDAV
- Biométrica-Framework
- Gui do Windows Defender
- Windows Identity-Foundation
- PowerShell ISE
- Serviço de pesquisa
- IFilter de TIFF do Windows
- Rede de telefonia móvel
- Visualizador XPS

