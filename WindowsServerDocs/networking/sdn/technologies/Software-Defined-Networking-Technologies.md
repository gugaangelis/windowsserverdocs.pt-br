---
title: Tecnologias SDN
description: Os tópicos nesta seção fornecem informações técnicas sobre as tecnologias de Software definido de rede que estão incluídos no Windows Server 2016 e visão geral.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>Tecnologias SDN

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Os tópicos nesta seção fornecem informações técnicas sobre as tecnologias de Software definido de rede que estão incluídos no Windows Server 2016 e visão geral.  
  
> [!NOTE]  
> Para obter outras documentações de Software de rede definidos, você pode usar as seguintes seções de biblioteca.  
>   
> - [Planeje SDN](../plan/Plan-Software-Defined-Networking.md)
> - [Implantar SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [Gerenciar SDN](../manage/manage-sdn.md)
> - [Segurança para SDN](../security/sdn-security-top.md)
> - [Solucionar problemas SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

Há várias tecnologias que trabalham juntos para criar soluções de Software definido de rede (SDN) da Microsoft, incluindo o seguinte:  
  
-   **[Protocolo de Gateway de borda & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    Quando configurada em um Windows Server 2016 acesso serviço RAS () Gateway remoto, Border Gateway Protocol (BGP) fornece a capacidade de gerenciar o roteamento de tráfego de rede entre redes VM seu locatários e seus locais remotos. BGP reduz a necessidade de configuração manual rota nos roteadores porque ele é um protocolo de roteamento dinâmico e aprende automaticamente rotas entre sites que estão conectados por meio de conexões VPN ao site.  
  
-   **[Visão geral sobre Datacenter Firewall](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Datacenter Firewall é um novo serviço incluído no Windows Server 2016. É uma camada de rede, 5 tuplas (protocolo, os números de porta de origem e de destino, endereços IP de origem e de destino), firewall com monitoração de estado, vários locatários. Quando implantado e oferecido como um serviço pelo provedor de serviço, os administradores locatários podem instalar e configurar as políticas de firewall para ajudar a proteger suas redes virtuais de indesejado tráfego originado da Internet e redes de intranet.  
  
  
-   **[Virtualização de rede do Hyper-V](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    Virtualização de rede do Hyper-V (HNV) permite a virtualização de redes de cliente na parte superior de uma infraestrutura de rede física compartilhada.  
  
- **[Internas serviço DNS #40 iDNS &; 41; para SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    Máquinas virtuais hospedadas \(VMs\) e aplicativos exigem DNS para se comunicar em suas próprias redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nome DNS para o espaço de nome isolado, locais e recursos de Internet.

-   **[Controlador de rede](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    O controlador de rede oferece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede físicos e virtuais no seu datacenter.  
  
-   **[Virtualização de função de rede](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    Funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) são cada vez mais sendo virtualizadas como dispositivos virtuais.  
  
    Microsoft tem virtualizados redes, comutadores, gateways, NATs, balanceadores de carga e firewalls.  

-   **[Gateway RAS para SDN](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    Gateway RAS é um baseada em software, vários locatários, Border Gateway Protocol (BGP) compatíveis com roteador no Windows Server 2016 é projetado para provedores de serviço de nuvem (CSPs) e as empresas que hospedam várias redes virtuais locatário usando a virtualização de rede do Hyper-V.  
      
- **[Remoto acesso direto à memória & #40; RDMA & #41; e alternar agrupamento Embedded & #40; Definir & #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    Você pode usar uma NIC convergida combinar tráfego RDMA e Ethernet usando um único adaptador de rede. O NIC convergido permite que você use um único adaptador de rede para gerenciamento de armazenamento remoto RDMA direta de acesso à memória habilitado e o tráfego do locatário. Isso reduz as despesas que estão associadas a cada servidor em seu datacenter, porque você precisa de adaptadores de rede menos para gerenciar diferentes tipos de tráfego por servidor.  
  
    CONJUNTO é uma solução de agrupamento de NIC que está integrada no Hyper-V Virtual Switch. CONJUNTO permite o agrupamento de até oito placas NIC físicas em uma única equipe conjunto, que melhora a disponibilidade e fornece failover. No Windows Server 2016, você pode criar equipes de conjunto que são restritas ao uso do servidor SMB Message Block () e RDMA.
  

-   **[Balanceamento de carga de software & #40; SLB & #41; para SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    Provedores de serviço de nuvem (CSPs) e as empresas que estão implantando Software definida de rede (SDN) no Windows Server 2016 podem usar o balanceamento de carga de Software (SLB) para distribuir uniformemente locatário e tráfego de rede de cliente de locatário entre recursos de rede virtual. O Windows Server SLB permite que vários servidores hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.
  
-   **[System Center](../../sdn/Sc-Tech-for-Sdn.md) ** você pode usar o System Center 2016 Virtual Machine Manager (VMM) e Operations Manager para implantar e gerenciar a infraestrutura SDN, incluindo controladores de rede, balanceadores de carga de software e gateways. Você também pode usar VMM centralmente definir e controlar as políticas de rede virtual e vincular as políticas para seus aplicativos ou cargas de trabalho.
  
- **[Contêineres do Windows](../technologies/Containers/Container-networking-overview.md)**
    
    Windows Server contêineres são um método de virtualização de sistema operacional leve usado para separar os serviços ou aplicativos de outros serviços que são executados no mesmo contêiner host. Para habilitar esse recurso, cada contêiner tem seu próprio modo de exibição do sistema operacional, processos, sistema de arquivos, do registro e endereços IP. Com o Windows Server 2016, agora você pode conectar contêineres do Windows Server para redes virtuais. Função de contêineres do Windows da mesma forma para máquinas virtuais em relação à rede. Cada contêiner tem um adaptador de rede virtual que está conectado a um comutador virtual, por meio da qual tráfego de entrada e saído é encaminhado. Para impor o isolamento entre contêineres no mesmo host, um compartimento de rede é criado para cada Windows Server e Hyper-V contêiner no qual o adaptador de rede para o contêiner está instalado. Contêineres do Windows Server usam um vNIC Host para anexar ao switch virtual. Contêineres do Hyper-V use uma NIC VM sintéticos (não exposta a VM utilitário) para anexar ao switch virtual.

