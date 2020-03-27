---
title: Acesso remoto
description: Este tópico fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 05/18/2018
ms.openlocfilehash: dbcd0380dffca29e782be2179024270da73a2c11
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309410"
---
# <a name="remote-access"></a>Acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

O guia de acesso remoto fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016 e cobre os seguintes assuntos:

- [Guia de implantação de VPN Always On](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Border Gateway Protocol &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Gateway de RAS](ras-gateway/RAS-Gateway.md) 
- [Documentação da função de servidor de acesso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Gateway de RAS para SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [VPN (rede virtual privada)](vpn/vpn-top.md)
 
Para obter mais informações sobre outras tecnologias de rede, consulte [Networking in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

A função de servidor de acesso remoto é um agrupamento lógico dessas tecnologias de acesso à rede relacionadas: [RAS (serviço de acesso remoto)](#bkmk_da), [Roteamento](#bkmk_rras)e [proxy de aplicativo Web](#bkmk_proxy). Essas tecnologias são os *serviços de função* da função do servidor de acesso remoto. Ao instalar a função de servidor de acesso remoto com o **Assistente para adicionar funções e recursos** ou o Windows PowerShell, você pode instalar um ou mais desses três serviços de função.

>[!IMPORTANT]
>Não tente implantar o acesso remoto em uma máquina virtual \(\) VM no Microsoft Azure. Não há suporte para o uso do acesso remoto no Microsoft Azure. Você não pode usar o acesso remoto em uma VM do Azure para implantar VPN, DirectAccess ou qualquer outro recurso de acesso remoto no Windows Server 2016 ou versões anteriores do Windows Server. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="remote-access-service-ras---ras-gateway"></a><a name="bkmk_da"></a>Serviço de acesso remoto \(RAS\)-gateway RAS

Ao instalar o serviço de função **DirectAccess e VPN (RAS)** , você está implantando o gateway do serviço de acesso remoto \(o **Gateway RAS**\). Você pode implantar o gateway de RAS uma rede virtual privada de gateway de RAS de locatário único \(servidor VPN\), um servidor VPN de gateway de RAS multilocatário e como um servidor DirectAccess.

- **Gateway RAS-locatário único**. Usando o gateway RAS, você pode implantar conexões VPN para fornecer aos usuários finais acesso remoto à rede e aos recursos da sua organização. Se os clientes estiverem executando o Windows 10, você poderá implantar Always On VPN, que mantém uma conexão persistente entre os clientes e a rede da sua organização sempre que os computadores remotos estiverem conectados à Internet. Com o gateway de RAS, você também pode criar uma conexão VPN site a site entre dois servidores em locais diferentes, como entre o escritório primário e uma filial, e usar a conversão de endereços de rede \(NAT\) para que os usuários dentro da rede possam acessar recursos externos, como a Internet. Além disso, o gateway RAS dá suporte a Border Gateway Protocol (BGP), que fornece serviços de roteamento dinâmico quando seus locais de escritório remotos também têm gateways de borda com suporte para BGP.

- **Gateway RAS-multilocatário**. Você pode implantar o gateway RAS como um gateway de borda baseado em software multilocatário e um roteador quando estiver usando a virtualização de rede Hyper\-V ou se tiver redes VM implantadas com redes de área local virtual \(VLANs\). Com o gateway RAS, os provedores de serviço de nuvem \(CSPs\) e as empresas podem habilitar o roteamento de tráfego de rede de nuvem e de datacenter entre redes físicas e virtuais, incluindo a Internet. Com o gateway RAS, seus locatários podem usar conexões VPN ponto a site para acessar seus recursos de rede VM no datacenter de qualquer lugar. Você também pode fornecer locatários com conexões VPN site a site entre seus sites remotos e seu datacenter CSP. Além disso, você pode configurar o gateway RAS com BGP para roteamento dinâmico e pode habilitar a conversão de endereços de rede \(NAT\) para fornecer acesso à Internet para VMs em redes VM.

>[!IMPORTANT]
> O gateway RAS com recursos multilocatário também está disponível no Windows Server 2012 R2.

- **Always on VPN**. Always On VPN permite que os usuários remotos acessem com segurança recursos compartilhados, sites da intranet e aplicativos em uma rede interna sem se conectar a uma VPN. 

Para obter mais informações, consulte [RAS gateway](ras-gateway/RAS-Gateway.md) and [Border Gateway Protocol (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="routing"></a><a name="bkmk_rras"></a>Zona

Você pode usar o acesso remoto para rotear o tráfego de rede entre sub-redes em sua rede local. O roteamento fornece suporte para roteadores NAT (conversão de endereços de rede), roteadores de LAN executando BGP, RIP (Routing Information Protocol) e roteadores compatíveis com multicast usando o protocolo IGMP (Internet Group Management Protocol). Como um roteador completo, você pode implantar o RAS em um computador servidor ou como uma VM (máquina virtual) em um computador que esteja executando o Hyper-V.

Para instalar o acesso remoto como um roteador de LAN, use o assistente para adicionar funções e recursos no Gerenciador do Servidor e selecione a função de servidor de **acesso remoto** e o serviço de função de **Roteamento** ; ou digite o seguinte comando em um prompt do Windows PowerShell e pressione ENTER.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="web-application-proxy"></a><a name="bkmk_proxy"></a>Proxy de aplicativo Web

O proxy de aplicativo Web é um serviço de função de acesso remoto no Windows Server 2016. O Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. O proxy de aplicativo Web autentica previamente o acesso a aplicativos Web usando Serviços de Federação do Active Directory (AD FS) (AD FS) e também funciona como um proxy AD FS.

Para instalar o acesso remoto como um proxy de aplicativo Web, use o assistente para adicionar funções e recursos no Gerenciador do Servidor e selecione a função de servidor de **acesso remoto** e o serviço de função **proxy de aplicativo Web** ; ou digite o seguinte comando em um prompt do Windows PowerShell e pressione ENTER.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Para obter mais informações, consulte [proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---