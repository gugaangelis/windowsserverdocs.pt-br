---
title: Implantação de VPN Always On para o Windows Server e o Windows 10
description: Você pode usar essa implantação para implantar conexões sempre em particular VPN (rede Virtual) para funcionários remotos usando o acesso remoto no Windows Server 2016 ou posterior e perfis de VPN Always On para computadores de cliente do Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981126"
---
# Implantação de VPN Always On para Windows Server e Windows 10

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** acesso remoto](../../../Remote-Access.md)<br>
& #187; [ **Próximo:** Saiba mais sobre os recursos de VPN Always On e funcionalidade](../../vpn-map-da.md)


VPN Always On oferece uma única solução para acesso remoto e dá suporte à ingressado no domínio coesa, ingressado fora (grupo de trabalho) ou AD – dispositivos associados ao Azure, dispositivos de propriedade até mesmo pessoal.  Com a VPN Always On, o tipo de conexão não precisa ser exclusivamente usuário ou dispositivo, mas pode ser uma combinação de ambos. Por exemplo, você pode habilitar a autenticação de dispositivo para o gerenciamento de dispositivo remoto e, em seguida, habilitar a autenticação do usuário para conectividade com a empresa interna sites e serviços.



## Pré-requisitos

Você provavelmente terá as tecnologias implantado que você pode usar para implantar VPN Always On. Diferente de seus servidores DNS do DC, a implantação de VPN Always On requer um servidor NPS (RAIO), um servidor de autoridade de certificação (CA) e um servidor de acesso remoto (roteamento/VPN). Depois que a infraestrutura estiver configurada, você deve registrar clientes e conecte os clientes para o local com segurança por meio de várias alterações de rede.

- Domínio infra-estrutura do Active Directory, incluindo um ou mais servidores de sistema de nome de domínio (DNS). Zonas de sistema de nome de domínio (DNS) internos e externos são necessárias, que assume que a zona interna é um subdomínio delegado da zona externo (por exemplo, corp.contoso.com e contoso.com).
- Active Directory com base em infraestrutura de chave pública (PKI) e os serviços de certificados do Active Directory (AD CS).
- Servidor virtual ou físico, existente ou nova, para instalar o servidor de política de rede (NPS). Se você já tiver servidores NPS em sua rede, você pode modificar uma configuração do servidor NPS existente em vez de adicionar um novo servidor.
- Acesso remoto como um servidor de Gateway de RAS VPN com um pequeno subconjunto de recursos com suporte para conexões VPN IKEv2 e roteamento de rede local.
- Rede de perímetro que inclui dois firewalls.  Certifique-se de que os firewalls permitem o tráfego que é necessário para comunicações VPN e RADIUS funcionar corretamente. Para obter mais informações, consulte [Sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).
- Servidor físico ou máquina virtual (VM) em sua rede de perímetro com dois adaptadores de rede Ethernet físicas para instalar o acesso remoto como um servidor VPN de Gateway de RAS. As VMs necessitam de LAN virtual (VLAN) para o host. 
- A associação ao grupo Administradores, ou equivalente, é o mínimo necessário.
- Leia a seção de planejamento deste guia para garantir que você está preparado para essa implantação antes de executar a implantação.
- Examine os guias de design e implantação para cada uma das tecnologias usadas. Esses guias podem ajudar você a determinar se os cenários de implantação fornecem os serviços e a configuração que você precisa para a rede da organização. Para obter mais informações, consulte [Sempre em VPN visão geral da tecnologia](../always-on-vpn-technology-overview.md).
- Plataforma de gerenciamento de sua preferência para implantar a configuração de VPN Always On porque o CSP não é específica do fornecedor.


>[!IMPORTANT]
>Para essa implantação, não é um requisito de que os servidores de infraestrutura, como computadores que executam o Active Directory Domain Services, serviços de certificados do Active Directory e servidor de políticas de rede, estão executando o Windows Server 2016. Você pode usar as versões anteriores do Windows Server, como o Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.
>
>Não tente implantar o acesso remoto em uma máquina virtual \(VM\) no Microsoft Azure. Usando o acesso remoto no Microsoft Azure não é compatível, incluindo o acesso remoto VPN e DirectAccess. Para obter mais informações, consulte o [software de servidor da Microsoft de suporte para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>Sobre essa implantação

As instruções fornecidas percorrer a implantação de acesso remoto como um único locatário Gateway de RAS VPN para conexões VPN de point\-to\-site, usando qualquer um dos cenários mencionados abaixo, para computadores clientes remotos que executam o Windows 10. Você também pode encontrar instruções para modificar algumas das sua infraestrutura existente para a implantação. Também em todo esse implantação, encontre links para ajudá-lo a saber mais sobre o processo de conexão VPN, servidores para configurar, nó ProfileXML CSP de VPNv2 e outras tecnologias para implantar VPN Always On.

**Cenários de implantação de VPN Always On:**

1. Implante sempre VPN somente.
2. Implante a VPN sempre ativa com acesso condicional para conectividade VPN usando o Azure AD.


Para obter mais informações e fluxo de trabalho dos cenários apresentados, consulte [Implantar VPN Always On](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>O que não é fornecido nesta implantação

Essa implantação não fornece instruções para:

- \(AD DS\) de serviços de domínio do Active Directory.
- Serviços de certificados do Active Directory \(AD CS\) e uma infraestrutura de chave pública \(PKI\).
- Dynamic Host Configuration Protocol \(DHCP\). 
- Hardware de rede, como o cabeamento Ethernet, firewalls, switches e hubs.
- Recursos de rede adicionais, como servidores de arquivos e de aplicativo, que os usuários remotos podem acessar ao longo de uma conexão VPN Always On.
- Conectividade com a Internet ou acesso condicional para conectividade com a Internet usando o Azure AD. Para obter detalhes, consulte [condicional acessar no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## Próximas etapas

- [Saiba mais sobre os recursos de VPN Always On e funcionalidade](../../vpn-map-da.md)

- [Saiba mais sobre os aprimoramentos de VPN Always On](../always-on-vpn-enhancements.md)

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia de VPN Always On](../always-on-vpn-technology-overview.md)

- [Começar a planejar sua implantação de VPN Always On](always-on-vpn-deploy-deployment.md)


---
