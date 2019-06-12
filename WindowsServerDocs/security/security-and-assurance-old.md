---
title: Segurança e garantia
description: Visão geral da segurança no Windows Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: fed0587b74873005f14a216bac22f952bcc65a4f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447288"
---
# <a name="security-and-assurance-in-windows-server"></a>Segurança e garantia no Windows Server 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> Você pode contar com novas camadas de proteção integradas ao sistema operacional para proteger ainda mais contra falhas de segurança. Bloqueie ataques mal-intencionados e aumente a segurança de máquinas virtuais, aplicativos e dados.


### <a name="windows-server-security-blog-posthttpsblogstechnetmicrosoftcomwindowsserver20160425ten-reasons-youll-love-windows-server-2016-8-security"></a>[Postagem do Blog de segurança do Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
Esta postagem no blog da equipe de segurança do Windows Server destaca muitas das melhorias feitas no Windows Server que aumentam a segurança de ambientes de nuvem de hospedagem e híbridos.

### <a name="datacenter-and-private-cloud-security-bloghttpsblogstechnetmicrosoftcomdatacentersecurity"></a>[Blog de segurança de nuvem privada e Datacenter](https://blogs.technet.microsoft.com/datacentersecurity/)
Esse é o site de blog central para conteúdo técnico da equipe de segurança de nuvem privada e datacenter da Microsoft.                                    

### <a name="addressing-emerging-threats-and-landscape-shiftshttpswwwyoutubecomwatchvb5jmyxywx1kfeatureyoutube"></a>[Desloca abordar ameaças emergentes e paisagem](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
Neste vídeo de seis minutos, Anders Vinberg fornece uma visão geral sobre as estratégias de segurança e garantia da Microsoft, e discute as tendências e mudanças do setor relacionadas à segurança. Em seguida, ele se concentra em iniciativas essenciais da Microsoft para proteger as cargas de trabalho da malha subjacente e proteger contra ataques diretos de contas privilegiadas. Por fim, em caso de violação, ele explica como os novos recursos forenses e de detecção podem ajudar a identificar melhor a ameaça.

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-posthttpblogstechnetcombwindowsserverarchive20151118protecting-your-datacenter-and-cloud-november-updateaspx"></a>[Protegendo seu Datacenter e nuvem contra ameaças emergentes postagem de blog](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
Esta postagem de blog descreve como você pode usar as tecnologias da Microsoft para proteger seus investimentos de datacenter e nuvem contra ameaças emergentes.                   

### <a name="security-and-assurance-overview-session-at-ignitehttpchannel9msdncomeventsignite2015brk2482"></a>[Visão geral sobre garantia e segurança de sessão no Ignite](http://channel9.msdn.com/events/ignite/2015/brk2482)
Esta sessão do Ignite aborda as ameaças persistentes, as violações internas, o crime cibernético organizado e a proteção da plataforma de nuvem da Microsoft (serviços locais e conectados com o Azure). Ela inclui cenários para proteger cargas de trabalho, locatários de grandes empresas e provedores de serviços.                                                                   

## <a name="secure-virtualization-with-shielded-vms"></a>Virtualização segura com VMs blindadas

### <a name="shielded-vm-in-channel-9httpchannel9msdncomshowsmechanicsintroduction-to-shielded-virtual-machines-in-windows-server-2016"></a>[VM blindada no Channel 9](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
Um passo a passo de tecnologia e de benefícios de VM blindada.                           

### <a name="shielded-vm-demohttpswwwyoutubecomwatchvxip5qtk-7d8"></a>[Demonstração de VM blindada](https://www.youtube.com/watch?v=xip5Qtk-7d8)
Este vídeo de quatro minutos descreve o valor de VMs blindadas e as diferenças entre uma VM blindada e uma VM não blindada.                                   

### <a name="shielded-virtual-machines-in-windows-server-video-walkthroughhttpmicrosoft-cloudcloudguidescomguidesshielded-virtual-machines-in-windows-serverhtm"></a>[Máquinas virtuais blindadas no Windows Server vídeo passo a passo](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
Este passo a passo em vídeo mostra como o Serviço Guardião de Host habilita as máquinas virtuais blindadas para que os dados confidenciais sejam protegidos do acesso não autorizado por administradores de host do Hyper-V.

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-videohttpchannel9msdncomeventsignite2015brk3457"></a>[Proteção da malha: Proteger segredos do locatário no Hyper-V (vídeo do Ignite)](http://channel9.msdn.com/events/ignite/2015/brk3457)

Esta apresentação da Ignite discute os aprimoramentos feitos no Hyper-V, no Gerenciador de Máquina Virtual e a nova função Servidor Guardião de Host para habilitar as VMs blindadas.                

### <a name="guarded-fabric-deployment-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-deploying-hgs-overview"></a>[Guia de implantação de malha protegida](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
Este guia proporciona informações de instalação e validação para o Windows Server e o System Center Virtual Machine Manager para hosts de malha protegida e VMs blindadas.

### <a name="shielded-vm-and-guarded-fabric-in-branch-officeshttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-manage-branch-office"></a>[VM blindada e malha protegida em filiais](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
Este guia fornece práticas recomendadas para a execução de máquinas virtuais blindadas em filiais e em outros cenários remotos onde os hosts do Hyper-V podem ter períodos de tempo com conectividade limitada para HGS.

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-troubleshoot-overview"></a>[VM blindada e malha protegida, guia de solução de problemas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
Este guia fornece informações sobre como resolver problemas que você pode encontrar em seu ambiente de VM blindada.

### <a name="shielded-vm-articlehttpwindowsitprocomhyper-vsuper-secure-hyper-v-environments-shielded-vms-2016"></a>[Artigo de VM blindada](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
Este white paper proporciona uma visão geral sobre como as VMs blindadas aumentam a segurança geral para evitar a violação.                                         

## <a name="privileged-access-management"></a>Gerenciamento de Acesso Privilegiado
### <a name="securing-privileged-accesshttpstechnetmicrosoftcomwindows-server-docssecuritysecuring-privileged-accesssecuring-privileged-access"></a>[Como proteger o acesso privilegiado](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
Um roteiro para proteger o acesso privilegiado. Esse roteiro foi criado com base na experiência combinada da equipe de segurança de servidores, a TI da Microsoft, a equipe do Azure e os serviços de consultoria da Microsoft                           

### <a name="just-in-time-administration-with-microsoft-identity-managerhttpstechnetmicrosoftcomlibrarymt150258aspx"></a>[Administração just in Time com o Microsoft Identity Manager](https://technet.microsoft.com/library/mt150258.aspx)
Este artigo descreve os recursos e as funcionalidades incluídos no Microsoft Identity Manager, incluindo suporte para Gerenciamento de Acesso Privilegiado Just In Time (JIT).                                                                    

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-managementhttpchannel9msdncomeventsignite2015brk3873"></a>[Protegendo o Windows e o Microsoft Azure Active Directory com o Privileged Access Management](http://channel9.msdn.com/events/ignite/2015/brk3873)
Esta apresentação do Ignite aborda a estratégia e os investimentos da Microsoft no Windows Server, no PowerShell, no Active Directory, no Identity Manager e no Azure Active Directory para lidar com os riscos de acesso do administrador por meio de autenticação mais forte e o gerenciamento do acesso usando a Administração Just In Time e Just Enough.

### <a name="just-enough-administration-articlehttpakamsjea"></a>[Artigo Just Enough Administration](http://aka.ms/JEA)
Este documento compartilha a visão e os detalhes técnicos da Administração Just Enough (JEA), um kit de ferramentas do PowerShell projetado para ajudar as organizações a reduzirem o risco restringindo os operadores apenas ao acesso necessário para executar tarefas específicas.

### <a name="just-enough-administration-demo-videohttpswwwyoutubecomwatchvxnbrbky9p20"></a>[Apenas o vídeo de demonstração Enough Administration](https://www.youtube.com/watch?v=xnBrbkY9P20)
Passo a passo da demonstração da Administração Just Enough.                                                                                                                  
## <a name="credential-protection"></a>Proteção de credenciais

### <a name="protect-derived-domain-credentials-with-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectioncredential-guardcredential-guard"></a>[Proteger as credenciais de domínio derivadas com o Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
O Credential Guard usa segurança baseada em virtualização para isolar segredos para que apenas o software de sistema privilegiado possa acessá-los. O acesso não autorizado a esses segredos pode levar a ataques de roubo de credenciais, como os ataques Pass-the-Hash ou Pass-The-Ticket. O Credential Guard evita esses ataques, protegendo hashes de senha NTLM e Tíquetes de Concessão de Tíquetes Kerberos.

### <a name="protect-remote-desktop-credentials-with-remote-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectionremote-credential-guard"></a>[Proteger as credenciais de Área de Trabalho Remota com o Remote Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
O Remote Credential Guard ajuda a proteger suas credenciais através de uma Conexão de Área de Trabalho Remota ao redirecionar as solicitações de Kerberos para o dispositivo que está solicitando a conexão. Ele também fornece experiências de logon único para sessões de Área de Trabalho Remota.                                                                                                        |
### <a name="credential-guard-demo-videohttpswwwyoutubecomwatchveupkogsl7yk"></a>[Credential Guard vídeo de demonstração](https://www.youtube.com/watch?v=eUpKOGSl7yk)
Este vídeo de cinco minutos demonstra o Credential Guard e o Remote Credential Guard.         

## <a name="hardening-the-os-and-applications"></a>Proteção do sistema operacional e de aplicativos
### <a name="windows-defender-application-control-wdac-deployment-guidehttpsdocsmicrosoftcomwindowssecuritythreat-protectionwindows-defender-application-controlwindows-defender-application-control"></a>[Guia de implantação do controle (WDAC) de aplicativos do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
O WDAC é uma política de integridade de código (CI) configurável que ajuda as empresas a controlar quais aplicativos serão executados em seu ambiente e não possui nenhum requisito específico de hardware ou software diferente do que esteja executando o Windows 10.

### <a name="device-guard-demo-videohttpswwwyoutubecomwatchvf-ptkesjkhi"></a>[Vídeo de demonstração do protetor de dispositivo](https://www.youtube.com/watch?v=F-pTkesjkhI)
O Device Guard é uma combinação de WDAC e HVCI (integridade de código protegida pelo Hipervisor). Este vídeo de sete minutos apresenta o Device Guard e seu uso no Windows Server.

### <a name="transport-layer-security-registry-settingshttpsdocsmicrosoftcomwindows-serversecuritytlstls-registry-settings"></a>[Configurações de registro de segurança de camada de transporte](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
Informações de configuração do Registro com suporte para a implementação do Windows do protocolo TLS (Transport Layer Security) e do protocolo SSL (Secure Sockets Layer).

### <a name="control-flow-guardhttpsdocsmicrosoftcomwindowsdesktopsecbpcontrol-flow-guard"></a>[Proteção de fluxo de controle](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
A Proteção de Fluxo de Controle oferece proteção incorporada contra algumas classes de ataque de corrupção de memória.

### <a name="windows-defenderhttpstechnetmicrosoftcomwindows-server-docssecuritywindows-defenderwindows-defender-overview-windows-server"></a>[Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
O Windows Defender proporciona recursos de detecção ativa para bloquear malware conhecido. O Windows Defender é ativado por padrão e é otimizado para oferecer suporte a várias funções de servidor no Windows Server.

## <a name="detecting-and-responding-to-threats"></a>Detectar e responder a ameaças
### <a name="security-threat-analysis-using-microsoft-operations-management-suitehttpschannel9msdncomeventsignite2015brk3464"></a>[Análise de ameaças de segurança usando o Microsoft Operations Management Suite](https://channel9.msdn.com/events/ignite/2015/brk3464)
Esta apresentação do Ignite discute como você pode usar informações operacionais para executar análise de riscos de segurança.

### <a name="microsoft-operations-management-suite-omshttpswwwmicrosoftcomen-usserver-cloudoperations-management-suiteoverviewaspx"></a>[Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
A solução de segurança e auditoria do Microsoft Operations Management Suite (OMS) processa logs de segurança e eventos de firewall de ambientes locais e de nuvem para analisar e detectar comportamentos mal-intencionados.

### <a name="oms-and-windows-serverhttpswwwyoutubecomwatchvsadw1dry2k"></a>[OMS e o Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
Este vídeo de 3 minutos mostra como o OMS pode ajudar a detectar comportamentos possivelmente mal-intencionados bloqueados pelo Windows Server.  

### <a name="microsoft-advanced-threat-analyticshttpblogstechnetcombadarchive20150722microsoft-advanced-threat-analytics-coming-next-monthaspx"></a>[Microsoft Advanced Threat Analytics](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
Esta postagem do blog aborda Microsoft Advanced ameaça Analytics, um produto no local que usa dados SIEM e tráfego de rede do Active Directory para descobrir e alerta contra ameaças potenciais.

### <a name="microsoft-advanced-threat-analyticshttpswwwyoutubecomwatchv0na9fetrzfwlistpl8nfc9hageb5izgm8hvmrozethrpbdksw"></a>[Microsoft Advanced Threat Analytics](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
Este vídeo de três minutos apresenta uma visão geral de como a Microsoft está adicionando recursos de análise de ameaças no Windows Server.                                                                                 |

## <a name="network-security"></a>Segurança de rede

### <a name="datacenter-firewall-overviewhttpstechnetmicrosoftcomlibrarydn920240aspx"></a>[Visão geral do Firewall do Datacenter](https://technet.microsoft.com/library/dn920240.aspx)
Esta visão geral discute o Firewall do Datacenter, uma camada de rede, 5 tuplas (protocolo, números de porta de origem e destino, endereços IP de origem e destino), firewall multilocatário e com monitoração de estado.

### <a name="whats-new-in-dns-in-windows-serverhttpstechnetmicrosoftcomwindows-server-docsnetworkingdnswhat-s-new-in-dns-server"></a>[O que há de novo no DNS no Windows Server](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
Este tópico de visão geral fornece descrições breves dos novos recursos de DNS, com links para saber mais.                                                                           

## <a name="mapping-security-features-to-compliance-regulations"></a>Mapear recursos de segurança com regulamentos de conformidade

A conformidade é um aspecto importante de recursos de segurança. É melhor que os seus conselheiros especialistas em conformidade recomendem como atingir a conformidade e ajudem a definir o que é conformidade para você, mas queremos fornecer um mapeamento inicial que você pode usar ao avaliar o Windows Server.

-   [White paper sobre mapeamento de conformidade de VMs Blindadas de Hyper-V](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [Whitepaper de mapeamento de conformidade de JEA e JIT](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade proteção do dispositivo](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Credential Guard white paper sobre mapeamento de conformidade](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [White paper sobre mapeamento de conformidade do Windows Defender](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)
