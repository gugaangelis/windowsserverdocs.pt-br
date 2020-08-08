---
title: Tecnologias de SDN
description: Os tópicos nesta seção fornecem uma visão geral e informações técnicas sobre as tecnologias de rede definidas pelo software que estão incluídas no Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: anpaul
author: AnirbanPaul
ms.date: 02/14/2019
ms.openlocfilehash: 69e01630cf34a588b6861c833015076bd4a31ef4
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996467"
---
# <a name="sdn-technologies"></a>Tecnologias de SDN

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Os tópicos nesta seção fornecem uma visão geral e informações técnicas sobre as tecnologias de rede definidas pelo software que estão incluídas no Windows Server 2016.

## <a name="network-controller"></a>[Controlador de rede](network-controller/Network-Controller.md)

O controlador de rede fornece um ponto de automação centralizado e programável para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. Com o controlador de rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual de dispositivos e serviços de rede.

O controlador de rede é um servidor altamente disponível e escalonável e fornece duas interfaces de programação de aplicativo (APIs):

1. **API Southbound** – permite que o controlador de rede se comunique com a rede.
2. **API Northbound** – permite que você se comunique com o controlador de rede.

Você pode usar o Windows PowerShell, a API REST, ou um aplicativo de gerenciamento para gerenciar a seguinte infraestrutura de rede física e virtual:

- VMs e comutadores virtuais do Hyper-V
- Comutadores de rede física
- Roteadores de rede física
- Software de firewall
- Gateways de VPN, incluindo gateways de multilocatários RAS (serviço de acesso remoto)
- Balanceadores de Carga

## <a name="hyper-v-network-virtualization"></a>[Virtualização de rede Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

A HNV (virtualização de rede) do Hyper-V ajuda você a abstrair seus aplicativos e cargas de trabalho da rede física usando redes virtuais. Redes virtuais fornecem o isolamento multilocatário necessário durante a execução em uma malha de rede física compartilhada, aumentando assim a utilização de recursos. Para garantir que você possa encaminhar seus investimentos existentes, você pode configurar redes virtuais no equipamento de rede existente. Além disso, as redes virtuais são compatíveis com VLANs (redes locais virtuais).

## <a name="hyper-v-virtual-switch"></a>[Comutador Virtual do Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

O comutador virtual do Hyper-V é um comutador de rede Ethernet de camada 2 baseado em software que está disponível no Gerenciador do Hyper-V após a instalação da função de servidor Hyper-V. O comutador inclui funcionalidades programaticamente gerenciadas e extensíveis para conectar máquinas virtuais a redes físicas e a redes virtuais. Além disso, o comutador virtual do Hyper-V fornece imposição de políticas para segurança, isolamento e níveis de serviço.

Você também pode implantar o comutador virtual do Hyper-V com o switch Embedded teaming (SET) e acesso remoto direto à memória (RDMA). Para obter mais informações, consulte a seção [RDMA (acesso remoto direto à memória) e comutador inserido de equipe (Set)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) neste tópico.

## <a name="internal-dns-service-idns-for-sdn"></a>[Serviço DNS interno (iDNS) para SDN](Idns-for-Sdn.md)

As VMs (máquinas virtuais) hospedadas e os aplicativos exigem que o DNS se comunique em suas redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nomes DNS para namespaces e recursos locais isolados e de Internet.

## <a name="network-function-virtualization"></a>[Virtualização de função de rede](network-function-virtualization/Network-Function-Virtualization.md)

Dispositivos de hardware, como balanceadores de carga, firewalls, roteadores e comutadores estão se tornando cada vez mais dispositivos virtuais. A Microsoft tem redes virtualizadas, comutadores, gateways, NATs, balanceadores de carga e firewalls. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Os dispositivos virtuais estão surgindo rapidamente e criando um mercado totalmente novo. Eles continuam gerando interesse e conquistam impulso nas plataformas de virtualização e nos serviços de nuvem.

As tecnologias de virtualização de função de rede a seguir estão disponíveis.

-   **Load Balancer de software (SLB) e conversão de endereços de rede (NAT)**. Aprimore a taxa de transferência ao dar suporte ao retorno de servidor direto no qual o tráfego de rede de retorno pode ignorar o multiplexador de balanceamento de carga. Para obter mais detalhes, consulte [balanceamento de carga de software/(SLB/) para Sdn](network-function-virtualization/software-load-balancing-for-sdn.md).

-   **Firewall do datacenter**. Forneça ACLs (listas de controle de acesso) granulares, permitindo que você aplique políticas de firewall no nível da interface da VM ou no nível da sub-rede. Para obter mais detalhes, consulte [visão geral do firewall do datacenter](network-function-virtualization/Datacenter-Firewall-Overview.md).

-   **Gateway de RAS para Sdn**. Rotear o tráfego de rede entre a rede física e os recursos da rede VM, independentemente do local. Você pode rotear o tráfego de rede no mesmo local físico ou em vários locais diferentes. Para obter mais detalhes, consulte [Gateway de RAS para Sdn](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acesso Remoto Direto à Memória (RDMA) e Agrupamento incorporado do comutador (SET)
No Windows Server 2016, você pode habilitar o RDMA em adaptadores de rede que estão vinculados a um comutador virtual do Hyper-V com ou sem o switch Embedded Integration (SET). Isso permite que você use menos adaptadores de rede quando desejar usar RDMA e definido ao mesmo tempo.

O conjunto é uma solução de agrupamento NIC alternativa que você pode usar em ambientes que incluem o Hyper-V e a pilha de SDN (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas das funcionalidades de agrupamento NIC ao comutador virtual Hyper-V.

SET permite que você agrupe entre um e oito adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.
O conjunto de adaptadores de rede de membros deve ser instalado no mesmo host do Hyper-V físico a ser colocado em uma equipe.

Além disso, você pode usar comandos do Windows PowerShell para habilitar a ponte do Data Center (DCB), criar um comutador virtual do Hyper-V com uma NIC virtual RDMA (vNIC) e criar um comutador virtual do Hyper-V com SET e RDMA vNICs. Para obter mais informações, consulte [acesso remoto direto à memória (RDMA) e Comutador incorporado (Set)](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgp"></a>[BGP (Border Gateway Protocol)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)

O Border Gateway Protocol (BGP) é um protocolo de roteamento dinâmico que aprende automaticamente as rotas entre sites que usam conexões VPN site a site. Portanto, o BGP reduz a configuração manual dos roteadores.   Quando você configura o gateway RAS, o BGP permite gerenciar o roteamento do tráfego de rede entre as redes de VM e os sites remotos de seus locatários.

## <a name="software-load-balancing-slb-for-sdn"></a>[SLB (balanceamento de carga do software) para SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Os CSPs (provedores de serviços de nuvem) e as empresas que implantam SDN podem usar o SLB (balanceamento de carga de software) para distribuir uniformemente o tráfego de rede do cliente locatário e locatário entre os recursos de rede virtual. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.

## <a name="windows-server-containers"></a>[Contêineres do Windows Server](Containers/Container-networking-overview.md)

Os contêineres do Windows Server são um método leve de virtualização do sistema operacional que separa aplicativos ou serviços de outros serviços em execução no mesmo host do contêiner. Cada contêiner tem seu próprio sistema operacional, processos, sistema de arquivos, registro e endereços IP, os quais você pode se conectar a redes virtuais.

## <a name="system-center"></a>System Center

Implante e gerencie a infraestrutura de SDN com o [VMM (gerenciamento de máquinas virtuais)](/system-center/vmm/) e [Operations Manager](/system-center/scom/). Com o VMM, você provisiona e gerencia os recursos necessários para criar e implantar máquinas virtuais e serviços em nuvens privadas.  Com o Operations Manager, você monitora serviços, dispositivos e operações em toda a empresa para identificar problemas para ação imediata.


---