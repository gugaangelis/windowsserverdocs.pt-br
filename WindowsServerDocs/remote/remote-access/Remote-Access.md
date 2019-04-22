---
title: Acesso remoto
description: Este tópico fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818357"
---
# <a name="remote-access"></a>Acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

O guia de acesso remoto fornece uma visão geral da função de servidor de acesso remoto no Windows Server 2016 e aborda os seguintes assuntos:

- [Sempre no guia de implantação de VPN](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [Border Gateway Protocol &#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [Gateway de RAS](ras-gateway/RAS-Gateway.md) 
- [Documentação de função de servidor de acesso remoto](ras/Remote-Access-Server-Role-Documentation.md)
- [Gateway RAS para SDN](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [Rede virtual privada (VPN)](vpn/vpn-top.md)
 
Para obter mais informações sobre outras tecnologias de rede, consulte [sistema de rede do Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).

A função de servidor de acesso remoto é um agrupamento lógico dessas tecnologias de acesso de rede relacionados: [O serviço de acesso remoto (RAS)](#bkmk_da), [roteamento](#bkmk_rras), e [Proxy de aplicativo Web](#bkmk_proxy). Essas tecnologias são os *serviços de função* da função do servidor de acesso remoto. Quando você instala a função de servidor de acesso remoto com o **assistente Adicionar funções e recursos** ou o Windows PowerShell, você pode instalar um ou mais dessas três serviços de função.

>[!IMPORTANT]
>Não tente implantar o acesso remoto em uma máquina virtual \(VM\) no Microsoft Azure. Não há suporte para usar o acesso remoto no Microsoft Azure. Você não pode usar o acesso remoto em uma VM do Azure para implantar a VPN, DirectAccess ou qualquer outro recurso de acesso remoto no Windows Server 2016 ou versões anteriores do Windows Server. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="bkmk_da"></a>Serviço de acesso remoto \(RAS\) -Gateway de RAS

Quando você instala o **DirectAccess e VPN (RAS)** serviço de função, você está implantando o Gateway do serviço de acesso remoto \( **Gateway de RAS**\). Você pode implantar o Gateway de RAS uma único locatário Gateway RAS rede virtual privada \(VPN\) server, um servidor VPN de Gateway de RAS multilocatário e como um servidor do DirectAccess.

- **Gateway RAS - locatário único**. Usando o Gateway de RAS, você pode implantar conexões VPN para fornecer aos usuários finais acesso remoto à rede e recursos de sua organização. Se os clientes estão executando o Windows 10, você pode implantar VPN Always On, que mantém uma conexão persistente entre clientes e a rede da sua organização, sempre que os computadores remotos são conectados à Internet. Com o Gateway de RAS, você também pode criar uma conexão de VPN site a site entre dois servidores em locais diferentes, como entre seu escritório principal e uma filial e usar conversão de endereços de rede \(NAT\) , de modo que os usuários dentro do rede possa acessar recursos externos, como a Internet. Além disso, o Gateway de RAS dá suporte a Border Gateway Protocol (BGP), que fornece serviços de roteamento dinâmicos quando seus locais de escritório remoto também possuem gateways de borda que dão suporte a BGP.

- **Gateway RAS - multilocatário**. Você pode implantar o Gateway de RAS como um multilocatário, o roteador e gateway do edge baseado em software quando você estiver usando Hyper\-V virtualização de rede, ou você tem redes VM implantadas com redes locais virtuais \(VLANs\). Com o Gateway de RAS, provedores de serviços de nuvem \(CSPs\) e as empresas podem habilitar o datacenter e na nuvem roteamento de tráfego de rede entre redes virtuais e físicas, incluindo a Internet. Com o Gateway de RAS, seus locatários podem usar conexões de VPN de ponto para site para acessar seus recursos de rede VM no datacenter de qualquer lugar. Você também pode fornecer locatários com conexões de VPN site a site entre seus sites remotos e seu datacenter do CSP. Além disso, você pode configurar o Gateway de RAS com o BGP para roteamento dinâmico, e você pode habilitar a conversão de endereços de rede \(NAT\) para fornecer acesso à Internet para VMs em redes VM.

>[!IMPORTANT]
> O Gateway de RAS com recursos multilocatário também está disponível no Windows Server 2012 R2.

- **VPN Always On**. VPN Always On permite que usuários remotos acessem com segurança os recursos compartilhados, sites da intranet e aplicativos em uma rede interna sem precisar se conectar a uma VPN. 

Para obter mais informações, consulte [Gateway RAS](ras-gateway/RAS-Gateway.md) e [Border Gateway Protocol (BGP)](bgp/Border-Gateway-Protocol-BGP.md).

## <a name="bkmk_rras"></a>Roteamento

Você pode usar o acesso remoto para rotear o tráfego de rede entre sub-redes na sua rede Local. Roteamento fornece suporte para roteadores de conversão de endereço de rede (NAT), roteadores de rede local executando o BGP, Routing Information Protocol (RIP) e os roteadores compatíveis com multicast usando o Internet Group Management Protocol (IGMP). Como um roteador completo, você pode implantar acesso remoto em um computador de servidor ou como uma máquina virtual (VM) em um computador que executa o Hyper-V.

Para instalar o acesso remoto como um roteador de LAN, você pode usar o assistente Adicionar funções e recursos no Gerenciador do servidor e selecionar o **acesso remoto** função de servidor e o **roteamento** serviço de função; ou digite o seguinte comando em um prompt do Windows PowerShell e pressione ENTER.

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Proxy de aplicativo Web

Proxy de aplicativo Web é um serviço de função acesso remoto no Windows Server 2016. O Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. Proxy de aplicativo Web pré-autentica o acesso a aplicativos web usando os serviços de Federação do Active Directory (AD FS) e também funciona como um proxy do AD FS.

Para instalar o acesso remoto como um Proxy de aplicativo Web, você pode usar o assistente Adicionar funções e recursos no Gerenciador do servidor e selecionar o **acesso remoto** função de servidor e o **Proxy de aplicativo Web** serviço de função; ou Digite o seguinte comando em um prompt do Windows PowerShell e pressione ENTER.  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

Para obter mais informações, consulte [Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).


---