---
title: Implantação de VPN Always On para o Windows Server e o Windows 10
description: Você pode usar essa implantação para implantar conexões sempre em Virtual VPN (rede privada) para funcionários remotos usando perfis de VPN Always On e acesso remoto no Windows Server 2016 ou posterior para computadores cliente com Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859617"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Implantação de VPN Always On para Windows Server e Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Anterior:** Acesso remoto](../../../Remote-Access.md)<br>
&#187; [**Next:** Saiba mais sobre os recursos de VPN Always On e funcionalidade](../../vpn-map-da.md)


VPN Always On fornece uma solução única e coesa para acesso remoto e dá suporte ao domínio, ingressado fora do domínio (grupo de trabalho) ou ingressados no AD – dispositivos do Azure, até mesmo pessoalmente de dispositivos de propriedade.  Com VPN Always On, o tipo de conexão não precisará ser exclusivamente, usuário ou dispositivo, mas pode ser uma combinação de ambos. Por exemplo, você pode habilitar a autenticação de dispositivo para o gerenciamento de dispositivo remoto e, em seguida, habilitar a autenticação de usuário para conectividade com serviços e sites internos da empresa.



## <a name="prerequisites"></a>Pré-requisitos

Você provavelmente tem as tecnologias implantado que você pode usar para implantar a VPN Always On. Diferente de seus servidores DNS/DC, a implantação de VPN Always On exige um servidor de NPS (RADIUS), um servidor de autoridade de certificação (CA) e um servidor de acesso remoto (roteamento/VPN). Depois que a infra-estrutura estiver configurada, você deve registrar os clientes e, em seguida, conecte-se os clientes para o seu local com segurança por meio de várias alterações de rede.

- Domínio infra-estrutura do Active Directory, incluindo um ou mais servidores de sistema de nome de domínio (DNS). Zonas de sistema de nome de domínio (DNS) internos e externos são necessárias, que presume que a zona interna é um subdomínio delegado da zona externo (por exemplo, "corp.contoso.com" e "contoso.com").
- Active Directory com base no diretório de infraestrutura de chave pública (PKI) e os serviços de certificados do Active Directory (AD CS).
- Servidor virtual ou física, existente ou novo, para instalar o servidor de diretivas de rede (NPS). Se você já tiver servidores NPS em sua rede, você pode modificar uma configuração de servidor existente do NPS em vez de adicionar um novo servidor.
- Acesso remoto como um servidor de VPN de Gateway de RAS com um pequeno subconjunto de recursos que dão suporte a conexões VPN IKEv2 e roteamento de rede local.
- Rede de perímetro que inclui dois firewalls.  Verifique se os firewalls permitem o tráfego que é necessário para comunicações de VPN e RADIUS funcionar corretamente. Para obter mais informações, consulte [sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).
- Servidor físico ou máquina virtual (VM) em sua rede de perímetro com dois adaptadores de rede Ethernet físicas para instalar o acesso remoto como um servidor VPN de Gateway de RAS. As VMs necessitam de LAN virtual (VLAN) para o host. 
- Associação administradores ou equivalente, é o mínimo necessário.
- Leia a seção de planejamento deste guia para garantir que você esteja preparado para essa implantação antes de executar a implantação.
- Examine os guias de design e implantação para cada uma das tecnologias usadas. Esses guias podem ajudá-lo a determinar se os cenários de implantação fornecem os serviços e configurações que você precisa para a rede da sua organização. Para obter mais informações, consulte [sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).
- Plataforma de gerenciamento de sua escolha para implantar a configuração de VPN Always On, porque o CSP não é específico do fornecedor.


>[!IMPORTANT]
>Para essa implantação, não é um requisito de que seus servidores de infraestrutura, como computadores que executam o Active Directory Domain Services, serviços de certificados do Active Directory e servidor de políticas de rede estão executando o Windows Server 2016. Você pode usar versões anteriores do Windows Server, como o Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.
>
>Não tente implantar o acesso remoto em uma máquina virtual \(VM\) no Microsoft Azure. Usando o acesso remoto no Microsoft Azure não há suporte, incluindo acesso remoto VPN e o DirectAccess. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>Sobre esta implantação

As instruções fornecidas orientarão-lo pela implantação de acesso remoto como um único locatário VPN Gateway de RAS para ponto\-para\-conexões de VPN, usando qualquer um dos cenários mencionados a seguir, para computadores cliente remotos que estão executando o Windows do site 10. Você também pode encontrar instruções para modificar algumas das sua infraestrutura existente para a implantação. Também em toda esta implantação, você encontrar links para ajudá-lo a saber mais sobre o processo de conexão de VPN, servidores para configurar, nó ProfileXML VPNv2 CSP e outras tecnologias para implantar a VPN Always On.

**Cenários de implantação de VPN Always On:**

1. Implante sempre VPN apenas.
2. Implante VPN Always On com acesso condicional para conectividade VPN usando o Azure AD.


Para obter mais informações e fluxo de trabalho dos cenários apresentados, consulte [implantar VPN Always On](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>O que não é fornecido nessa implantação

Essa implantação não fornece instruções para:

- Serviços de domínio do Active Directory \(AD DS\).
- Serviços de certificados do Active Directory \(AD CS\) e uma infraestrutura de chave pública \(PKI\).
- Dynamic Host Configuration Protocol \(DHCP\). 
- Hardware de rede, como firewalls, cabeamento Ethernet, comutadores e hubs.
- Recursos de rede adicionais, como servidores de arquivos e aplicativos, o que os usuários remotos podem acessar ao longo de uma conexão VPN Always On.
- Conectividade com a Internet ou acesso condicional para conectividade com a Internet usando o Azure AD. Para obter detalhes, consulte [acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre os recursos de VPN Always On e funcionalidade](../../vpn-map-da.md)

- [Saiba mais sobre os aprimoramentos de VPN Always On](../always-on-vpn-enhancements.md)

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia VPN Always On](../always-on-vpn-technology-overview.md)

- [Comece a planejar sua implantação de VPN Always On](always-on-vpn-deploy-deployment.md)


---
