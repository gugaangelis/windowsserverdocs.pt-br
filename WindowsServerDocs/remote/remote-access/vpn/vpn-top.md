---
title: Rede Virtual Privada (VPN)
description: Você pode usar este tópico para saber mais sobre a funcionalidade e os recursos do Windows Server 2016 e VPN do Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066964"
---
# \(VPN\) de rede virtual privada

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

## Gateway RAS como um servidor VPN único locatário

No Windows Server 2016, a função de servidor de acesso remoto é um agrupamento lógico das tecnologias de acesso de rede relacionados a seguir.

- Serviço de acesso remoto (RAS)
- Roteamento
- Proxy de aplicativo Web

Essas tecnologias são os serviços de função da função de servidor de acesso remoto.

Quando você instala a função de servidor de acesso remoto com a adição de funções e recursos do assistente ou do Windows PowerShell, você pode instalar um ou mais desses três serviços de função.

Quando você instala o serviço de função do **DirectAccess e VPN (RAS)** , você está implantando o Gateway de serviço de acesso remoto \ (**Gateway de RAS**\). Você pode implantar o Gateway de RAS como um servidor \(VPN\) de rede virtual privada de Gateway de RAS único locatário que fornece muitos recursos avançados e funcionalidade aprimorada.

>[!NOTE]
>Você também pode implantar o Gateway de RAS como um servidor VPN multilocatário para uso com \(SDN\) rede definida pelo Software, ou como um servidor DirectAccess. Para obter mais informações, consulte o [Gateway de RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [Rede definida pelo Software (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)e [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## Tópicos relacionados
- [Funcionalidade e os recursos de VPN always On](vpn-map-da.md): neste tópico, você saberá mais sobre os recursos e funcionalidades de VPN Always On. 

- [Configurar túneis de dispositivo de VPN no Windows 10](vpn-device-tunnel-config.md): VPN Always On oferece a capacidade de criar um perfil de VPN dedicado para o dispositivo ou computador. Conexões VPN Always On incluem dois tipos de encapsulamentos: _encapsulamento do dispositivo_ e _encapsulamento do usuário_. Encapsulamento do dispositivo é usado para fins de gerenciamento de dispositivo e cenários de conectividade de pré-login. Encapsulamento de usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

- [Sempre na implantação de VPN para Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): fornece instruções sobre como implantar o acesso remoto como um único locatário Gateway de RAS VPN para conexões VPN point\-to\-site que permitem que seus funcionários remotos para se conectar à sua organização rede com conexões VPN Always On. É recomendável que você consulte os guias de design e implantação para cada uma das tecnologias que são usadas nesta implantação.

- [Guia de técnico de VPN do Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): orienta as decisões que você deverá tomar para clientes do Windows 10 em sua solução VPN empresarial e como configurar sua implantação. Você pode encontrar referências para o provedor de serviços de configuração de VPNv2 (CSP) e fornece gerenciamento de dispositivo móvel instruções de configuração (MDM) usando o Microsoft Intune e o modelo de perfil de VPN para o Windows 10.

- [Como criar VPN perfis no System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): neste tópico, você aprenderá a criar perfis VPN no System Center Configuration Manager (SCCM).

- [Configurar o Windows 10 sempre em conexões VPN de cliente](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Este tópico descreve o ProfileXML opções e esquema e como criar o ProfileXML VPN. Depois de configurar a infraestrutura de servidor, você deve configurar os computadores de cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. 

- [Opções de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): Este tópico descreve as configurações de perfil VPN no Windows 10 e saiba como configurar perfis VPN usando o Intune ou o SCCM. Você pode definir todas as configurações de VPN no Windows 10 usando o nó ProfileXML no CSP VPNv2.

---