---
title: Virtualização de função de rede
description: Você pode usar este tópico para saber mais sobre a virtualização de função de rede, que permite que você implantar dispositivos redes virtuais como Datacenter Firewall, vários locatários Gateway RAS e Software carga balanceamento (SLB) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>Virtualização de função de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre a virtualização de função de rede, que permite que você implantar dispositivos redes virtuais como Datacenter Firewall, multitenant RAS Gateway e balanceamento de carga de Software \(SLB\) \(MUX\) multiplexador.
  
>[!NOTE]  
>Além de neste tópico, a seguinte documentação de virtualização de função de rede está disponível.  
> - [Visão geral sobre Datacenter Firewall](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Gateway RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software balanceamento de carga (SLB) para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
No software de hoje datacenters definidos, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) são cada vez mais sendo virtualizados como dispositivos virtuais. Essa virtualização de função"rede" é uma progressão natural de virtualização de servidor e a virtualização de rede. Dispositivos virtuais são rapidamente emergentes e criação de um mercado totalmente novo. Eles continuam gerar interesse e obter inovação em ambas as plataformas de virtualização e serviços de nuvem.  
  
Microsoft incluiu um gateway autônomo como um dispositivo virtual a partir do Windows Server 2012 R2. Para obter mais informações, consulte [Windows servidor Gateway](https://technet.microsoft.com/library/dn313101.aspx). Agora com o Windows Server 2016 Microsoft continua expandir e investir no mercado de virtualização de função de rede.  
  
## <a name="virtual-appliance-benefits"></a>Benefícios do dispositivo virtual  
Um dispositivo virtual é dinâmico e fácil de alterar porque é uma máquina virtual pré-construídos e personalizada. Ele pode ser um ou mais máquinas virtuais empacotado, atualizados e mantidos como uma unidade. Juntos com o software de rede (SDN) definido, você obtém a agilidade e a flexibilidade necessária na infraestrutura de baseado em nuvem de hoje. Por exemplo:  
  
-   SDN apresenta a rede como um recurso de pool e dinâmico.  
  
-   SDN facilita o isolamento de locatário.  
  
-   SDN maximiza a escala e desempenho.  
  
-   Dispositivos virtuais permitem mobilidade perfeita de capacidade de expansão e a carga de trabalho.  
  
-   Dispositivos virtuais minimizar a complexidade operacional.  
  
-   Dispositivos virtuais permitem que os clientes facilmente adquirir, implantar e gerenciar soluções previamente integradas.  
  
    -   Os clientes podem facilmente mover o dispositivo virtual em qualquer lugar na nuvem.  
  
    -   Os clientes podem escalar dispositivos virtuais ou para baixo dinamicamente com base em sob demanda.  
  
Para saber mais sobre o Microsoft SDN veja [Software de rede definidos](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Quais funções de rede estão sendo virtualizadas?  
O marketplace para funções de rede virtualizado está crescendo rapidamente. As seguintes funções de rede estão sendo virtualizadas:  
  
-   **Segurança**  
  
    -   Firewall  
  
    -   Antivírus  
  
    -   DDoS (distribuída negação de serviço)  
  
    -   IPS/IDS (sistema de detecção de invasões/sistema de prevenção contra invasões)  
  
-   **Otimizadores do aplicativo/WAN**  
  
-   **Borda**  
  
    -   Gateway to-site  
  
    -   Gateways L3  
  
    -   Roteadores  
  
    -   Switches  
  
    -   NAT  
  
    -   Balanceadores de carga (não necessariamente na borda)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Por que a Microsoft é uma ótima plataforma para dispositivos virtuais  
![Pilha de rede virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
A plataforma Microsoft foi projetada para ser uma ótima plataforma para criar e implantar dispositivos virtuais. Aqui estão as respostas:  
  
-   A Microsoft fornece funções de chave de rede virtualizado com o Windows Server 2016.  
  
-   Você pode implantar um dispositivo virtual do fornecedor de sua escolha.  
  
-   Você pode implantar, configurar e gerenciar seus dispositivos virtuais com o controlador de rede Microsoft que vem com o Windows Server 2016. Para saber mais sobre o controlador de rede, consulte [rede controlador](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V pode hospedar os sistemas operacionais convidados principais que você precisa.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualização de função de rede no Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funções de dispositivos virtuais fornecidas pela Microsoft  
Os seguintes dispositivos virtuais são fornecidos com o Windows Server 2016:  
  
**Balanceador de software**  
  
Um camada 4 balanceador operando em escala de datacenter. Esta é uma versão semelhante de Balanceador de carga do Azure que tenha sido implantada em escala no ambiente Azure. Para saber mais sobre o balanceador de carga de Software da Microsoft, consulte [Software carga balanceamento (SLB) para SDN](https://technet.microsoft.com/library/mt632286.aspx). Para obter mais informações sobre os serviços de balanceamento de carga do Microsoft Azure, consulte [serviços de balanceamento de carga do Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Gateway**. Gateway RAS fornece todas as combinações das seguintes funções de gateway.  
  
-   **Gateway to-Site**  
  
    Gateway RAS fornece uma Border Gateway Protocol (BGP)-capaz, vários locatários gateway que permite que seu locatários acessar e gerenciar seus recursos em conexões de VPN-to-site de locais remotos, e que permite o fluxo de tráfego de rede entre recursos virtuais em nuvem e locatário redes físicas. Para obter mais informações sobre o Gateway RAS, consulte [RAS Gateway alta disponibilidade](https://technet.microsoft.com/library/mt631692.aspx) e [RAS Gateway](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Encaminhamento de gateway**  
  
    Gateway RAS encaminha o tráfego entre redes virtuais e a rede física do provedor de hospedagem. Por exemplo, se locatários criar uma ou mais redes virtuais e precisam de acesso a recursos compartilhados na rede física no provedor de hospedagem, o gateway de encaminhamento pode rotear o tráfego entre a rede virtual e a rede física para fornecer aos usuários trabalhando em uma rede virtual com os serviços que eles precisam. Para obter mais informações, consulte [RAS Gateway alta disponibilidade](https://technet.microsoft.com/library/mt631692.aspx) e [RAS Gateway](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateways de túnel GRE**  
  
    GRE com base em túneis habilitar conectividade entre redes virtuais locatário e redes externas. Como o protocolo GRE é leve e suporte para GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para encapsulamento onde a criptografia de dados não é necessária. Suporte GRE em túneis do Site para outro (S2S) resolve o problema de encaminhamento entre redes virtuais locatário e locatário redes externo usando um gateway de vários locatário. Para saber mais sobre túneis, consulte [GRE encapsulamento no Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plano de controle de roteamento com BGP**  
  
Controle de roteamento de virtualização de rede do Hyper-V (HNV) é a entidade lógica e centralizada no plano de controle, que carrega todas as rotas de plano de endereço do cliente e aprende dinamicamente e atualiza os distribuído roteadores Gateway RAS da rede virtual. Para obter mais informações, consulte [RAS Gateway alta disponibilidade](https://technet.microsoft.com/library/mt631692.aspx) e [RAS Gateway](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall distribuído de vários locatário**  
  
O firewall protege a camada de rede de redes virtuais. As políticas são impostas na porta SDN vSwitch de cada locatário VM. Ele protege todos os fluxos de tráfego: Leste-Oeste e Sul do Norte. As políticas são enviadas por meio do portal do locatário e o controlador de rede distribui para todos os hosts aplicáveis. Para saber mais sobre o firewall de vários locatário distribuído, consulte [visão geral de Firewall de Datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


