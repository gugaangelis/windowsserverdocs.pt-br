---
title: Rede Virtual Privada (VPN)
description: Você pode usar este tópico para aprender sobre recursos e funcionalidades de VPN do Windows Server 2016 e do Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.openlocfilehash: 2f8362b5581dbd6c08f3f708f435e8625784e8fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818719"
---
# <a name="virtual-private-networking-vpn"></a>Rede Virtual Privada (VPN)

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Gateway RAS como um servidor VPN de locatário único

No Windows Server 2016, a função de servidor de acesso remoto é um agrupamento lógico das tecnologias de acesso à rede relacionadas a seguir.

- Serviço de acesso remoto (RAS)
- Roteamento
- Proxy de Aplicativo Web

Essas tecnologias são os serviços de função da função de servidor de acesso remoto.

Ao instalar a função de servidor de acesso remoto com o assistente para adicionar funções e recursos ou o Windows PowerShell, você pode instalar um ou mais desses três serviços de função.

Ao instalar o serviço de função **DirectAccess e VPN (RAS)** , você está implantando o gateway de serviço de acesso remoto (**Gateway de Ras**). Você pode implantar o gateway RAS como um servidor de rede virtual privada (VPN) de gateway de RAS de locatário único que fornece muitos recursos avançados e funcionalidade aprimorada.

>[!NOTE]
>Você também pode implantar o gateway RAS como um servidor VPN multilocatário para uso com o SDN (rede definida pelo software) ou como um servidor DirectAccess. Para obter mais informações, consulte [Gateway de Ras](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [rede definida por software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)e [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Tópicos relacionados
- [Always on recursos e funcionalidades de VPN](vpn-map-da.md): neste tópico, você aprende sobre os recursos e a funcionalidade de Always on VPN. 

- [Configurar túneis de dispositivo VPN no Windows 10](vpn-device-tunnel-config.md): Always on VPN oferece a capacidade de criar um perfil VPN dedicado para o dispositivo ou computador. Always On conexões VPN incluem dois tipos de túneis: _túnel de dispositivo_ e túnel de _usuário_. O túnel de dispositivo é usado para cenários de conectividade pré-login e para fins de gerenciamento de dispositivos. O túnel do usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

- [Always on implantação de VPN para Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): fornece instruções sobre como implantar o acesso remoto como um gateway de Ras VPN de locatário único para conexões VPN ponto a site que permitem que seus funcionários remotos se conectem à rede da sua organização com conexões VPN Always on. É recomendável revisar os guias de design e implantação para cada uma das tecnologias usadas nesta implantação.

- [Guia técnico de VPN do Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): orienta você pelas decisões que você fará para clientes do Windows 10 em sua solução de VPN corporativa e como configurar sua implantação. Você pode encontrar referências ao provedor de serviços de configuração do VPNv2 (CSP) e fornece instruções de configuração de MDM (gerenciamento de dispositivo móvel) usando Microsoft Intune e o modelo de perfil VPN para Windows 10.

- [Como criar perfis VPN no Configuration Manager](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles): neste tópico, você aprende a criar perfis vpn no Configuration Manager.

- [Configurar conexões VPN Always on cliente do Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tópico descreve as opções e o esquema do ProfileXML e como criar a VPN ProfileXML. Depois de configurar a infraestrutura do servidor, você deve configurar os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN.

- [Opções de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): Este tópico descreve as configurações de perfil VPN no Windows 10 e saiba como configurar perfis VPN usando o Intune ou Configuration Manager. Você pode definir todas as configurações de VPN no Windows 10 usando o nó ProfileXML no CSP VPNv2.
