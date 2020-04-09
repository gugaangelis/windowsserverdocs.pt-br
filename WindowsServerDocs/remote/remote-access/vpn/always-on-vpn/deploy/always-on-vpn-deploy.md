---
title: Implantação de VPN Always On para o Windows Server e o Windows 10
description: Você pode usar essa implantação para implantar Always On conexões de rede virtual privada (VPN) para funcionários remotos usando o acesso remoto no Windows Server 2016 ou posterior e Always On perfis de VPN para computadores cliente com Windows 10.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: e107ba0d36a1b59d4bc1bb365fb98aa9285677ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860119"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Implantação de VPN Always On para Windows Server e Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Acesso remoto](../../../Remote-Access.md)<br>
- [**Em seguida:** Saiba mais sobre os recursos e a funcionalidade de VPN Always On](../../vpn-map-da.md)

Always On VPN fornece uma solução única e coesa para acesso remoto e oferece suporte a dispositivos ingressados no domínio, não ingressados no domínio (grupo de trabalho) ou no Azure AD, mesmo em dispositivos de propriedade pessoal. Com a VPN Always On, o tipo de conexão não precisa ser exclusivamente de usuário ou dispositivo, podendo ser uma combinação de ambos. Por exemplo, você pode habilitar a autenticação de dispositivo para o gerenciamento de dispositivo remoto e, em seguida, habilitar a autenticação de usuário para conectividade com serviços e sites internos da empresa.

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Provavelmente, você tem as tecnologias implantadas que podem ser usadas para implantar Always On VPN. Além dos servidores DC/DNS, a implantação de VPN Always On requer um servidor NPS (RADIUS), um servidor de autoridade de certificação (CA) e um servidor de acesso remoto (roteamento/VPN). Depois que a infraestrutura estiver configurada, você deverá registrar clientes e conectar os clientes ao seu local com segurança por meio de várias alterações na rede.

- Active Directory infraestrutura de domínio, incluindo um ou mais servidores DNS (sistema de nomes de domínio). As zonas de DNS (sistema de nomes de domínio) internas e externas são necessárias, o que pressupõe que a zona interna é um subdomínio delegado da zona externa (por exemplo, corp.contoso.com e contoso.com).
- PKI (infraestrutura de chave pública) baseada em Active Directory e serviços de certificados de Active Directory (AD CS).
- Servidor, virtual ou físico, existente ou novo, para instalar o servidor de diretivas de rede (NPS). Se você já tiver servidores NPS em sua rede, poderá modificar uma configuração de servidor NPS existente em vez de adicionar um novo servidor.
- Acesso remoto como um servidor VPN de gateway RAS com um pequeno subconjunto de recursos que dão suporte a conexões VPN IKEv2 e roteamento de LAN.
- Rede de perímetro que inclui dois firewalls.  Verifique se os firewalls permitem o tráfego necessário para que as comunicações VPN e RADIUS funcionem corretamente. Para obter mais informações, consulte [visão geral da tecnologia VPN Always on](../always-on-vpn-technology-overview.md).
- Servidor físico ou VM (máquina virtual) em sua rede de perímetro com dois adaptadores de rede Ethernet físicos para instalar o acesso remoto como um servidor VPN de gateway RAS. As VMs exigem a LAN virtual (VLAN) para o host. 
- A associação em administradores, ou equivalente, é o mínimo necessário.
- Leia a seção de planejamento deste guia para garantir que você está preparado para essa implantação antes de executar a implantação.
- Examine os guias de design e implantação para cada uma das tecnologias usadas. Esses guias podem ajudá-lo a determinar se os cenários de implantação fornecem os serviços e a configuração de que você precisa para a rede da sua organização. Para obter mais informações, consulte [visão geral da tecnologia VPN Always on](../always-on-vpn-technology-overview.md).
- Plataforma de gerenciamento de sua escolha para implantar a configuração de VPN Always On porque o CSP não é específico do fornecedor.

>[!IMPORTANT]
>Para essa implantação, não é um requisito de que seus servidores de infraestrutura, como computadores que executam Active Directory Domain Services, Active Directory serviços de certificados e servidor de diretivas de rede, estejam executando o Windows Server 2016. Você pode usar versões anteriores do Windows Server, como o Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.
>
>Não tente implantar o acesso remoto em uma VM (máquina virtual) no Microsoft Azure. Não há suporte para o uso do acesso remoto no Microsoft Azure, incluindo VPN de acesso remoto e DirectAccess. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>Sobre esta implantação

As instruções fornecidas orientam você pela implantação de acesso remoto como um gateway de RAS VPN de locatário único para conexões VPN ponto a site, usando qualquer um dos cenários mencionados abaixo, para computadores cliente remotos que executam o Windows 10. Você também encontrará instruções para modificar alguns de sua infraestrutura existente para a implantação. Além disso, durante toda essa implantação, você encontrará links para ajudá-lo a saber mais sobre o processo de conexão VPN, os servidores a serem configurados, o nó ProfileXML VPNv2 CSP e outras tecnologias para implantar Always On VPN.

**Cenários de implantação de VPN Always On:**

1. Implante somente Always On VPN.
2. Implante Always On VPN com acesso condicional para conectividade VPN usando o Azure AD.

Para obter mais informações e fluxo de trabalho dos cenários apresentados, consulte [implantar Always on VPN](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>O que não é fornecido nesta implantação

Esta implantação não fornece instruções para:

- Active Directory Domain Services (AD DS).
- Active Directory serviços de certificados (AD CS) e uma PKI (infraestrutura de chave pública).
- Protocolo de configuração dinâmica de hosts (DHCP).
- Hardware de rede, como cabeamento Ethernet, firewalls, comutadores e hubs.
- Recursos de rede adicionais, como servidores de aplicativos e de arquivos, que os usuários remotos podem acessar por meio de uma conexão VPN Always On.
- Conectividade com a Internet ou acesso condicional para conectividade com a Internet usando o Azure AD. Para obter detalhes, consulte [acesso condicional em Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Saiba mais sobre os recursos e a funcionalidade de VPN Always On](../../vpn-map-da.md)

- [Saiba mais sobre os aprimoramentos de VPN Always On](../always-on-vpn-enhancements.md)

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia de VPN Always On](../always-on-vpn-technology-overview.md)

- [Iniciar o planejamento de sua implantação de VPN Always On](always-on-vpn-deploy-deployment.md)