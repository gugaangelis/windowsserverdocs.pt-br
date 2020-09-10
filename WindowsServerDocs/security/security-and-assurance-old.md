---
title: Segurança e garantia
description: Visão geral da segurança no Windows Server 2016
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
ms.author: lizross
author: eross-msft
manager: mtillman
ms.localizationpriority: medium
ms.openlocfilehash: 86011cbe4ba748347e049699a716ec38d462a788
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641201"
---
# <a name="security-and-assurance-in-windows-server"></a>Segurança e garantia no Windows Server

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

>[!TIP]
> Está procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar informações específicas neste site](/search/index?dataSource=previousVersions&search=Windows+Server).

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> Você pode contar com novas camadas de proteção integradas ao sistema operacional para proteger ainda mais contra falhas de segurança. Bloqueie ataques mal-intencionados e aumente a segurança de máquinas virtuais, aplicativos e dados.


### <a name="windows-server-security-blog-post"></a>[Postagem no blog de segurança do Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
Nesta postagem de blog sobre a segurança do Windows Server, a equipe de segurança destaca muitas das melhorias do Windows Server que aumentam a segurança para ambientes de hospedagem e nuvem híbrida.

### <a name="datacenter-and-private-cloud-security-blog"></a>[Blog de segurança de nuvem privada e datacenter](/archive/blogs/datacentersecurity/)
Esse é o site de blog central para conteúdo técnico da equipe de segurança de nuvem privada e datacenter da Microsoft.

### <a name="addressing-emerging-threats-and-landscape-shifts"></a>[Abordar ameaças emergentes e mudanças de paisagem](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
Neste vídeo de seis minutos, Anders Vinberg fornece uma visão geral sobre as estratégias de segurança e garantia da Microsoft, e discute as tendências e mudanças do setor relacionadas à segurança. Em seguida, ele se concentra em iniciativas essenciais da Microsoft para proteger as cargas de trabalho da malha subjacente e proteger contra ataques diretos de contas privilegiadas. Por fim, em caso de violação, ele explica como os novos recursos forenses e de detecção podem ajudar a identificar melhor a ameaça.

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-post"></a>[Postagem do blog sobre como protegendo seu datacenter e nuvem de ameaças emergentes](https://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
Esta postagem de blog descreve como você pode usar as tecnologias da Microsoft para proteger seus investimentos de datacenter e nuvem contra ameaças emergentes.

### <a name="security-and-assurance-overview-session-at-ignite"></a>[Sessão Visão geral sobre garantia e segurança no Ignite](https://channel9.msdn.com/events/ignite/2015/brk2482)
Esta sessão do Ignite aborda as ameaças persistentes, as violações internas, o crime cibernético organizado e a proteção da plataforma de nuvem da Microsoft (serviços locais e conectados com o Azure). Ela inclui cenários para proteger cargas de trabalho, locatários de grandes empresas e provedores de serviços.

## <a name="secure-virtualization-with-shielded-vms"></a>Virtualização segura com VMs blindadas

### <a name="shielded-vm-in-channel-9"></a>[VM blindada no Channel 9](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
Um passo a passo de tecnologia e de benefícios de VM blindada.

### <a name="shielded-vm-demo"></a>[Demonstração de VM blindada](https://www.youtube.com/watch?v=xip5Qtk-7d8)
Este vídeo de quatro minutos descreve o valor de VMs blindadas e as diferenças entre uma VM blindada e uma VM não blindada.

### <a name="shielded-virtual-machines-in-windows-server-video-walkthrough"></a>[Passo a passo em vídeo de máquinas virtuais blindadas no Windows Server](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
Este passo a passo em vídeo mostra como o Serviço Guardião de Host habilita as máquinas virtuais blindadas para que os dados confidenciais sejam protegidos do acesso não autorizado por administradores de host do Hyper-V.

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-video"></a>[Aumentar a proteção da malha: Proteção de segredos de locatário no Hyper-V (vídeo do Ignite)](https://channel9.msdn.com/events/ignite/2015/brk3457)

Esta apresentação da Ignite discute os aprimoramentos feitos no Hyper-V, no Virtual Machine Manager e a nova função Servidor Guardião de Host para habilitar as VMs blindadas.

### <a name="guarded-fabric-deployment-guide"></a>[Guia de implantação de malha protegida](./guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview.md)
Este guia proporciona informações de instalação e validação para o Windows Server e o System Center Virtual Machine Manager para hosts de malha protegida e VMs blindadas.

### <a name="shielded-vm-and-guarded-fabric-in-branch-offices"></a>[VM blindada e malha protegida em filiais](./guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md)
Este guia fornece práticas recomendadas para a execução de máquinas virtuais blindadas em filiais e em outros cenários remotos em que os hosts do Hyper-V podem ter períodos de tempo com conectividade limitada para HGS.

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guide"></a>[Guia de solução de problemas de VM blindada e malha protegida](./guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview.md)
Este guia fornece informações sobre como resolver problemas que você pode encontrar em seu ambiente de VM blindada.

### <a name="shielded-vm-article"></a>[Artigo de VM blindada](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
Este white paper proporciona uma visão geral sobre como as VMs blindadas aumentam a segurança geral para evitar a violação.

## <a name="privileged-access-management"></a>Gerenciamento de Acesso Privilegiado
### <a name="securing-privileged-access"></a>[Como proteger o acesso privilegiado](../identity/securing-privileged-access/securing-privileged-access.md)
Um roteiro para proteger o acesso privilegiado. Esse roteiro foi criado com base na experiência combinada da equipe de segurança de servidores, a TI da Microsoft, a equipe do Azure e os serviços de consultoria da Microsoft

### <a name="just-in-time-administration-with-microsoft-identity-manager"></a>[Administração Just in Time com o Microsoft Identity Manager](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)
Este artigo descreve os recursos e as funcionalidades incluídos no Microsoft Identity Manager, incluindo suporte para Gerenciamento de Acesso Privilegiado Just In Time (JIT).

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-management"></a>[Proteger o Windows e o Microsoft Azure Active Directory com Gerenciamento de Acesso Privilegiado](https://channel9.msdn.com/events/ignite/2015/brk3873)
Esta apresentação do Ignite aborda a estratégia e os investimentos da Microsoft no Windows Server, no PowerShell, no Active Directory, no Identity Manager e no Azure Active Directory para lidar com os riscos de acesso do administrador por meio de autenticação mais forte e o gerenciamento do acesso usando a Administração Just In Time e Just Enough.

### <a name="just-enough-administration-article"></a>[Artigo sobre JEA (Administração Just Enough)](https://aka.ms/JEA)
Este documento compartilha a visão e os detalhes técnicos da Administração Just Enough (JEA), um kit de ferramentas do PowerShell projetado para ajudar as organizações a reduzirem o risco restringindo os operadores apenas ao acesso necessário para executar tarefas específicas.

### <a name="just-enough-administration-demo-video"></a>[Vídeo de demonstração sobre JEA (Administração Just Enough)](https://www.youtube.com/watch?v=xnBrbkY9P20)
Passo a passo da demonstração da Administração Just Enough.
## <a name="credential-protection"></a>Proteção de credenciais

### <a name="protect-derived-domain-credentials-with-credential-guard"></a>[Proteger as credenciais de domínio derivadas com o Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)
O Credential Guard usa segurança baseada em virtualização para isolar segredos para que apenas o software de sistema privilegiado possa acessá-los. O acesso não autorizado a esses segredos pode levar a ataques de roubo de credenciais, como Pass-the-Hash ou Pass-the-Ticket. O Credential Guard evita esses ataques protegendo os hashes de senha de NTLM e concessão de tickets de Kerberos.

### <a name="protect-remote-desktop-credentials-with-remote-credential-guard"></a>[Proteger as credenciais de Área de Trabalho Remota com o Remote Credential Guard](/windows/security/identity-protection/remote-credential-guard)
O Remote Credential Guard ajuda a proteger suas credenciais através de uma Conexão de Área de Trabalho Remota ao redirecionar as solicitações de Kerberos para o dispositivo que está solicitando a conexão. Ele também fornece experiências de logon único para sessões de Área de Trabalho Remota.                                                                                                        |
### <a name="credential-guard-demo-video"></a>[Vídeo de demonstração do Credential Guard](https://www.youtube.com/watch?v=eUpKOGSl7yk)
Este vídeo de cinco minutos demonstra o Credential Guard e o Remote Credential Guard.

## <a name="hardening-the-os-and-applications"></a>Proteção do sistema operacional e de aplicativos
### <a name="windows-defender-application-control-wdac-deployment-guide"></a>[Guia de implantação do WDAC (Controle de Aplicativos do Windows Defender)](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
O WDAC é uma política de CI (integridade de código) configurável que ajuda as empresas a controlar quais aplicativos serão executados em seu ambiente e não tem nenhum requisito específico de hardware ou software diferente do que esteja executando o Windows 10.

### <a name="device-guard-demo-video"></a>[Vídeo de demonstração do Device Guard](https://www.youtube.com/watch?v=F-pTkesjkhI)
O Device Guard é uma combinação de WDAC e HVCI (integridade de código protegida pelo Hipervisor). Este vídeo de sete minutos apresenta o Device Guard e seu uso no Windows Server.

### <a name="transport-layer-security-registry-settings"></a>[Configurações do Registro do protocolo TLS](./tls/tls-registry-settings.md)
Informações de configuração do Registro com suporte para a implementação do Windows do protocolo TLS (Transport Layer Security) e do protocolo SSL (Secure Sockets Layer).

### <a name="control-flow-guard"></a>[Proteção de fluxo de controle](/windows/desktop/SecBP/control-flow-guard)
A Proteção de Fluxo de Controle oferece proteção incorporada contra algumas classes de ataque de corrupção de memória.

### <a name="windows-defender"></a>[Windows Defender](./windows-defender/windows-defender-overview-windows-server.md)
O Windows Defender proporciona recursos de detecção ativa para bloquear malware conhecido. O Windows Defender é ativado por padrão e é otimizado para oferecer suporte a várias funções de servidor no Windows Server.

## <a name="detecting-and-responding-to-threats"></a>Detectar e responder a ameaças
### <a name="security-threat-analysis-using-microsoft-operations-management-suite"></a>[Análise de ameaças de segurança usando o Microsoft Operations Management Suite](https://channel9.msdn.com/events/ignite/2015/brk3464)
Esta apresentação do Ignite discute como você pode usar informações operacionais para executar análise de riscos de segurança.

### <a name="microsoft-operations-management-suite-oms"></a>[Microsoft OMS (Operations Management Suite)](https://www.microsoft.com/server-cloud/operations-management-suite/overview.aspx)
A solução de segurança e auditoria do Microsoft Operations Management Suite (OMS) processa logs de segurança e eventos de firewall de ambientes locais e de nuvem para analisar e detectar comportamentos mal-intencionados.

### <a name="oms-and-windows-server"></a>[OMS e Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
Este vídeo de 3 minutos mostra como o OMS pode ajudar a detectar comportamentos possivelmente mal-intencionados que são bloqueado pelo Windows Server.

### <a name="microsoft-advanced-threat-analytics"></a>[Microsoft Advanced Threat Analytics](https://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
Esta postagem do blog aborda Microsoft Advanced ameaça Analytics, um produto no local que usa dados SIEM e tráfego de rede do Active Directory para descobrir e alerta contra ameaças potenciais.

### <a name="microsoft-advanced-threat-analytics"></a>[Microsoft Advanced Threat Analytics](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
Este vídeo de três minutos apresenta uma visão geral de como a Microsoft está adicionando recursos de análise de ameaças no Windows Server.                                                                                 |

## <a name="network-security"></a>Segurança de rede

### <a name="datacenter-firewall-overview"></a>[Visão geral do firewall do datacenter](/previous-versions/windows/server/dn920240(v=ws.12))
Esta visão geral discute o Firewall do Datacenter, uma camada de rede, 5 tuplas (protocolo, números de porta de origem e destino, endereços IP de origem e destino), firewall multilocatário e com monitoração de estado.

### <a name="whats-new-in-dns-in-windows-server"></a>[Novidades em DNS no Windows Server](../networking/dns/what-s-new-in-dns-server.md)
Este tópico de visão geral fornece descrições breves dos novos recursos de DNS, com links para saber mais.

## <a name="mapping-security-features-to-compliance-regulations"></a>Mapear recursos de segurança com regulamentos de conformidade

A conformidade é um aspecto importante de recursos de segurança. É melhor que os seus conselheiros especialistas em conformidade recomendem como atingir a conformidade e ajudem a definir o que é conformidade para você, mas queremos fornecer um mapeamento inicial que você pode usar ao avaliar o Windows Server.

-   [White paper sobre mapeamento da conformidade de VMs blindadas no Hyper-V](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade com JEA e JIT](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade do Device Guard](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade do Credential Guard](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade do Windows Defender](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)