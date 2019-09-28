---
title: Funções, serviços de função e recursos que não estão no Windows Server-Server Core
description: Saiba mais sobre as funções e os recursos não incluídos na opção de instalação do Server Core para o Windows Server.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce8fd0edc426b673f873717a27e6045e3476170f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383359"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Funções, serviços de função e recursos que não estão no Windows Server-Server Core

> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

As funções, os serviços de função e os recursos a seguir foram removidos da opção de instalação Server Core do Windows Server. Use essas informações para ajudar a descobrir se a opção Server Core funciona para seu ambiente.

> [!NOTE]
> Você também pode ver uma lista de funções, serviços de função e recursos que [estão incluídos no Server Core](server-core-roles-and-services.md). É uma lista muito grande, portanto, para obter melhores resultados, pesquise essa lista para a função ou o recurso específico no qual você está interessado.

## <a name="roles-not-in-server-core"></a>Funções que não estão no Server Core

- Servidor de fax (**fax**)
- Serviços do MultiPoint (**MultiPointServerRole**)
- Serviços de acesso e política de rede (**npas**)
- **WDS**(serviços de implantação do Windows) *(antes do windows Server versão 1803)*

## <a name="role-services-not-in-server-core"></a>Serviços de função não estão no Server Core
Observe que alguns serviços de função Área de Trabalho Remota estão incluídos no Server Core (agente de conexão, licenciamento, host de virtualização), mas outros não são (gateway, Host da Sessão RD, Acesso via Web).

- Serviços de Impressão e Documentos \ Servidor de Digitalização Distribuída (**Print-Scan-Server**)
- Serviços de Impressão e Documentos \ Impressão via Internet (**impressão-Internet**)
- Serviços de Área de Trabalho Remota \ Área de Trabalho Remota gateway (**RDS-gateway**)
- Serviços de Área de Trabalho Remota \ Host da Sessão da Área de Trabalho Remota (**RDS-RD-Server**)
- Serviços de Área de Trabalho Remota \ Acesso via Web à Área de Trabalho Remota (**RDS-Web-Access**)
- Servidor Web do serviço de função (IIS) \ ferramentas de gerenciamento \ console de gerenciamento do IIS (**Web-MGMT-console**)
- Servidor Web do serviço de função (IIS) \ ferramentas de gerenciamento \ compatibilidade de gerenciamento do IIS 6 \ console de gerenciamento do IIS 6 (**Web-LGCY-MGMT-console**)
- Serviços de implantação do Windows \ servidor de implantação (**WDS-implantação**)
- Serviços de implantação do Windows \ servidor de transporte (**WDS-Transport**) *(antes do Windows Server versão 1803)*

## <a name="features-not-in-server-core"></a>Recursos que não estão no Server Core
- Serviço de Transferência Inteligente em Segundo Plano (BITS) \ extensão do servidor IIS (**bits-IIS-Ext**)
- Desbloqueio de rede do BitLocker (**BitLocker-NetworkUnlock**)
- Play direto (**Direct-Play**)
- Impressão via Internet cliente (**Internet-impressão-cliente**)
- Monitor de porta LPR (**LPR-Port-Monitor**)
- Enfileiramento de mensagens \ Serviços de enfileiramento de mensagens \ suporte a multicast (**MSMQ-multicast**)
- Kit de administração do Gerenciador de conexões RAS (**CMAK**)
- Assistência remota (**assistência remota**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de recurso \ ferramentas de servidor SMTP (**rsat-SMTP**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de recursos \ Criptografia de Unidade de Disco BitLocker utilitários de administração \ Criptografia de Unidade de Disco BitLocker ferramentas (**rsat-Feature-Tools-BitLocker-RemoteAdminTool**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de recurso \ BITS extensões de servidor ferramentas (**rsat-bits-servidor**)
- Ferramentas de administração de recurso do Ferramentas de Administração de Servidor Remoto \ ferramentas de balanceamento**de carga de rede (RSAT-NLB**)
- Ferramentas de administração de recurso do Ferramentas de Administração de Servidor Remoto \ ferramentas SNMP (**rsat-SNMP**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de recurso \ ferramentas de servidor WINS (**rsat-WINS**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de gerenciamento do Hyper-V \ ferramentas de gerenciamento de GUI do Hyper-V (**Hyper-v-Tools**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Serviços de Área de Trabalho Remota ferramentas \ Área de Trabalho Remota ferramentas de gateway (**rsat-RDS-Tools**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Serviços de Área de Trabalho Remota ferramentas \ Área de Trabalho Remota ferramentas de gateway (**rsat-RDS-gateway**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Serviços de Área de Trabalho Remota ferramentas \ Área de Trabalho Remota ferramentas do diagnosticador de licenciamento (**rsat-RDS-Licensing-diagnóstico-UI**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Serviços de Área de Trabalho Remota ferramentas \ Área de Trabalho Remota ferramentas de licenciamento (**RDS-Licensing-UI**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Windows Server Update Services ferramentas \ console de gerenciamento de interface do usuário (**updateservices-UI**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Active Directory ferramentas de serviços de certificados (**rsat-ADCS**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Active Directory ferramentas de serviços de certificados \ ferramentas de gerenciamento de autoridade de certificação (**rsat-ADCS-MGMT**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Active Directory ferramentas de serviços de certificados \ ferramentas de Respondente Online (**rsat-online-Respondente**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Active Directory Rights Management Services Tools (**rsat-ADRMS**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de servidor de fax (**rsat-fax**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de arquivo (**rsat-File-Services**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de arquivo \ ferramentas de gerenciamento do DFS (**rsat-DFS-MGMT-con**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de arquivos \ servidor de arquivos ferramentas do Gerenciador de recursos (**rsat-FSRM-MGMT**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de arquivo \ Serviços para o sistema de arquivos de rede ferramentas de gerenciamento (**rsat-NFS-admin**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de acesso e política de rede (**rsat-NPAS**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ Serviços de Impressão e Documentos ferramentas (**rsat-Print-Services**)
- Ferramentas de administração de função do Ferramentas de Administração de Servidor Remoto \ ferramentas de ativação de volume (**rsat-VA-Tools**)
- Ferramentas de Administração de Servidor Remoto \ ferramentas de administração de função \ ferramentas de serviços de implantação do Windows (**WDS-Adminpack**)
- Serviços TCP/IP simples (**simples-tcpip**)
- Servidor SMTP (**servidor SMTP**)
- Cliente TFTP (**TFTP-Client**)
- Redirecionador WebDAV (**WebDAV-redirecionador**)
- Windows Biometric Framework (**Biometric-Framework**)
- Recursos do Windows Defender \ GUI para Windows Defender (**Windows-Defender-GUI**)
- Windows Identity Foundation 3,5 (**Windows – Identity-Foundation**)
- Windows PowerShell \ ISE do Windows PowerShell (**PowerShell-ISE**)
- Serviço de Pesquisa do Windows (**serviço de pesquisa**)
- IFilter TIFF do Windows (**Windows-TIFF-IFilter**)
- Serviço de LAN sem fio (**rede sem fio**)
- Visualizador XPS (**Visualizador XPS**)
