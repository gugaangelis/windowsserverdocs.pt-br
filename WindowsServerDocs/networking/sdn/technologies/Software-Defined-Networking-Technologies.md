---
title: SDN Technologies
description: Os tópicos nesta seção fornecem informações técnicas sobre as tecnologias de rede definida pelo Software que estão incluídos no Windows Server 2016 e visão geral.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: 0fc076eaeacc98c5554f44ae6177a3a8b8286647
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034641"
---
# <a name="sdn-technologies"></a>SDN Technologies

>Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

Os tópicos nesta seção fornecem informações técnicas sobre as tecnologias de rede definida pelo Software que estão incluídos no Windows Server 2016 e visão geral.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Controlador de rede](network-controller/Network-Controller.md)

O controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de ambos virtuais e infraestrutura de rede física em seu datacenter. Com o controlador de rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos de rede e serviços. 

O controlador de rede é um servidor altamente disponível e escalonável e fornece duas interfaces de programação de aplicativo (APIs):

1. **A API southbound** – permite que o controlador de rede para se comunicar com a rede.
2. **A API northbound** – permite que você se comunique com o controlador de rede.

Você pode usar o Windows PowerShell, a API REST Representational State Transfer () ou um aplicativo de gerenciamento para gerenciar a infraestrutura de rede física e virtual a seguir:

- VMs e comutadores virtuais do Hyper-V 
- Comutadores de rede física 
- Roteadores de rede física 
- Software de firewall 
- Gateways de VPN, incluindo os Gateways multilocatário do serviço de acesso remoto (RAS) 
- Balanceadores de carga 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualização de rede Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualização de rede do Hyper-V (HNV) ajuda a abstrair seus aplicativos e cargas de trabalho da rede física usando redes virtuais. Redes virtuais fornecem o isolamento multilocatário necessário durante a execução em uma malha de rede física compartilhada, aumentando assim a utilização de recursos. Para garantir que você possa transferir os investimentos existentes, você pode configurar redes virtuais em equipamentos de rede existentes. Além disso, as redes virtuais são compatíveis com as redes locais virtuais (VLANs).
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Comutador Virtual do Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

O comutador Virtual Hyper-V é um software com base em camada 2 Ethernet comutador de rede que está disponível no Gerenciador do Hyper-V, depois de ter instalado a função de servidor Hyper-V. O comutador inclui funcionalidades programaticamente gerenciadas e extensíveis para conectar máquinas virtuais a redes físicas e a redes virtuais. Além disso, o comutador Virtual Hyper-V fornece imposição de política de segurança, isolamento e níveis de serviço.
  
Você também pode implantar o comutador Virtual do Hyper-V com o Switch Embedded Teaming (SET) e Direct memória RDMA (acesso remoto). Para obter mais informações, consulte a seção [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) neste tópico.

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Serviço DNS interno (iDNS) para SDN](Idns-for-Sdn.md)

As máquinas virtuais hospedadas (VMs) e aplicativos exigem DNS se comuniquem em suas redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nome DNS para o namespace local, isolado e recursos da Internet. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualização de função de rede](network-function-virtualization/Network-Function-Virtualization.md)

Dispositivos de hardware, como balanceadores de carga, firewalls, roteadores e switches estão se tornando dispositivos virtuais. Microsoft tem virtualizados redes, comutadores, gateways, NATs, balanceadores de carga e firewalls. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Soluções de virtualização são emergentes e criando um novo mercado rapidamente. Eles continuam a gerar interesse e obter um impulso em ambas as plataformas de virtualização e serviços de nuvem. 
  
As seguintes tecnologias de virtualização de função de rede estão disponíveis.  
  
-   **O balanceador de carga de software (SLB) e endereços de rede NAT (conversão)** . Melhore a taxa de transferência, que dão suporte a retorno direto de servidor no qual o tráfego de rede de retorno pode ignorar o balanceamento de carga multiplexador. Para obter mais detalhes, consulte [/(SLB/) de balanceamento de carga de Software para SDN](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **Firewall do Datacenter**. Fornecem listas de controle de acesso granular (ACLs), permitindo que você aplique políticas de firewall no nível da interface VM ou no nível de sub-rede. Para obter mais detalhes, consulte [visão geral do Firewall do Datacenter](network-function-virtualization/Datacenter-Firewall-Overview.md).
  
-   **Gateway RAS para SDN**. Rotear o tráfego de rede entre a rede física e os recursos de rede VM, independentemente do local. Você pode rotear o tráfego de rede no mesmo local físico ou em vários locais diferentes. Para obter mais detalhes, consulte [Gateway de RAS para SDN](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acesso Remoto Direto à Memória (RDMA) e Agrupamento incorporado do comutador (SET)  
No Windows Server 2016, você pode habilitar o RDMA nos adaptadores de rede que estão associados a um comutador Virtual do Hyper-V com ou sem Switch Embedded Teaming (SET). Isso permite que você use menos adaptadores de rede quando você deseja usar RDMA e SET ao mesmo tempo.  
  
CONJUNTO é uma solução alternativa do agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a pilha de Software Defined Networking (SDN) no Windows Server 2016. CONJUNTO integra algumas das funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V.  
  
CONJUNTO permite que você agrupe entre uma e oito Ethernet adaptadores de rede física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.  
Todos os adaptadores de rede do conjunto de membro devem ser instalados no mesmo host do Hyper-V físico a ser colocado em uma equipe.  
  
Além disso, você pode usar comandos do Windows PowerShell para habilitar ponte DCB (Data Center), crie um comutador Virtual do Hyper-V com uma NIC virtual de RDMA (vNIC) e criar um comutador Virtual do Hyper-V com vNICs RDMA e SET. Para obter mais informações, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[BGP (Border Gateway Protocol)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Border Gateway Protocol (BGP) é um protocolo de roteamento dinâmico que automaticamente aprende rotas entre sites que usam conexões de VPN site a site. Portanto, o BGP reduz a configuração manual de roteadores.   Quando você configura o Gateway de RAS, o BGP permite que você gerencie o roteamento de tráfego de rede entre redes VM e sites remotos de seus locatários.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[SLB (balanceamento de carga do software) para SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Provedores de serviço de nuvem (CSPs) e empresas que implantam SDN podem usar o balanceamento de carga de Software (SLB) para distribuir uniformemente o tráfego de rede de cliente do locatário entre os recursos de rede virtual e locatário. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Contêineres do Windows Server](Containers/Container-networking-overview.md)

Contêineres do Windows Server são um método de virtualização do sistema operacional leve separar aplicativos ou serviços de outros serviços em execução no mesmo host do contêiner. Cada contêiner tem seu próprio sistema operacional, processos, sistema de arquivos, registro e endereços IP, que você pode se conectar às redes virtuais. 

## <a name="system-center"></a>System Center

Implantar e gerenciar a infraestrutura SDN com [gerenciamento de máquina Virtual (VMM)](https://docs.microsoft.com/system-center/vmm/) e [Operations Manager](https://docs.microsoft.com/system-center/scom/). Com o VMM, provisionar e gerenciar os recursos necessários para criar e implantar máquinas virtuais e serviços em nuvens privadas.  Com o Operations Manager, você monitorar serviços, dispositivos e operações em toda a empresa para identificar problemas para ação imediata. 


---