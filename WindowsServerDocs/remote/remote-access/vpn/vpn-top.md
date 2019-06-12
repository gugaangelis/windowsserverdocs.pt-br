---
title: Rede Virtual Privada (VPN)
description: Você pode usar este tópico para saber mais sobre a funcionalidade e os recursos do Windows Server 2016 e de VPN do Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bfd00b7a13e9fad113da1191e7ccd33965223070
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749554"
---
# <a name="virtual-private-networking-vpn"></a>Rede Virtual Privada (VPN)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Gateway RAS como um servidor VPN de único locatário

No Windows Server 2016, a função de servidor de acesso remoto é um agrupamento lógico das tecnologias de acesso de rede relacionados a seguir.

- Serviço de acesso remoto (RAS)
- Roteamento
- Proxy de aplicativo Web

Essas tecnologias são os serviços de função da função de servidor de acesso remoto.

Quando você instala a função de servidor de acesso remoto com a adição de funções e Assistente de recursos ou do Windows PowerShell, você pode instalar um ou mais dessas três serviços de função.

Quando você instala o **DirectAccess e VPN (RAS)** serviço de função, você está implantando o Gateway do serviço de acesso remoto (**Gateway RAS**). Você pode implantar o Gateway de RAS como um servidor de rede virtual privada (VPN) de Gateway RAS de locatário único que fornece muitos recursos avançados e funcionalidade aprimorada.

>[!NOTE]
>Você também pode implantar o Gateway de RAS como um servidor VPN multilocatário para uso com o Software Defined Networking (SDN) ou como um servidor DirectAccess. Para obter mais informações, consulte [Gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [Software Defined Networking (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking), e [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Tópicos relacionados
- [Recursos de VPN Always On e a funcionalidade](vpn-map-da.md): Neste tópico, você aprenderá sobre os recursos e funcionalidade de VPN Always On. 

- [Configurar túneis de dispositivo VPN no Windows 10](vpn-device-tunnel-config.md): VPN Always On fornece a capacidade de criar um perfil VPN dedicado para o computador ou dispositivo. Conexões de VPN Always On incluem dois tipos de túneis: _túnel do dispositivo_ e _túnel do usuário_. Túnel do dispositivo é usado para cenários de conectividade de pré-logon e fins de gerenciamento de dispositivo. Túnel do usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

- [Sempre na implantação de VPN para Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): Fornece instruções sobre como implantar o acesso remoto como um único locatário Gateway RAS de VPN para conexões VPN ponto a site que permitem que seus funcionários remotos para se conectar à rede da organização com conexões VPN Always On. É recomendável que você examine os guias de design e implantação para cada uma das tecnologias que são usadas nesta implantação.

- [Guia técnico de VPN do Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Explica as decisões que serão feitas para clientes do Windows 10 em sua solução de VPN da empresa e como configurar sua implantação. Você pode encontrar referências ao provedor de serviços de configuração para VPNv2 (CSP) e fornece gerenciamento de dispositivo móvel usando o Microsoft Intune e o modelo de perfil de VPN para Windows 10 de instruções de configuração (MDM).

- [Como criar perfis VPN no System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): Neste tópico, você aprenderá como criar perfis VPN no System Center Configuration Manager (SCCM).

- [Configurar o cliente do Windows 10 sempre em conexões VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tópico descreve o ProfileXML opções e o esquema e como criar a VPN ProfileXML. Depois de configurar a infraestrutura de servidor, você deve configurar os computadores de cliente do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN.

- [Opções de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): Este tópico descreve as configurações de perfil VPN no Windows 10 e saiba como configurar perfis de VPN usando o Intune ou do SCCM. Você pode configurar todas as configurações de VPN no Windows 10 usando o nó do ProfileXML no VPNv2 CSP.
