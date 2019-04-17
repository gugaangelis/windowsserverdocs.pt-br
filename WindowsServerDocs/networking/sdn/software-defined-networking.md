---
title: Software definidos redes (SDN)
description: Você pode usar este tópico para saber mais sobre as tecnologias de rede definidos Software (SDN) que são fornecidas no Windows Server, System Center e Microsoft Azure.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>Software definidos redes (SDN)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre as tecnologias de rede definidos Software (SDN) que são fornecidas no Windows Server Datacenter edition, System Center 2016 e Microsoft Azure.  
  
> [!NOTE]
> Além de neste tópico, o seguinte conteúdo SDN está disponível.
> 
> - [O que há de novo no SDN para o Windows Server](sdn-whats-new.md)
> - [Introdução ao SDN no Windows Server Datacenter](sdn-intro.md)
> - [Tecnologias SDN](technologies/Software-Defined-Networking-Technologies.md)  
> - [Planeje SDN](plan/Plan-Software-Defined-Networking.md) 
> - [Implantar SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [Gerenciar SDN](manage/manage-sdn.md)
> - [Segurança para SDN](security/sdn-security-top.md)
> - [Solucionar problemas SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [Tecnologias do System Center para SDN](Sc-Tech-for-Sdn.md)
> - [Microsoft Azure e SDN](Azure_and_Sdn.md)
> - [Entrar em contato com a equipe de rede na nuvem e Datacenter](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>Software definidos visão geral de rede

Software de rede definidos \(SDN\) fornece um método para configurar e gerenciar dispositivos de rede físicos e virtuais como gateways em seu datacenter, comutadores e roteadores centralmente. Elementos de rede virtual como comutador Virtual Hyper-V, virtualização de rede do Hyper-V e RAS Gateway são projetados para ser elementos integrantes de sua infraestrutura SDN.

>[!NOTE]
>Para hosts Hyper-V e \(VMs\) máquinas virtuais que executam servidores da infraestrutura SDN, como nós de controlador de rede e balanceamento de carga de Software, você deve instalar o Windows Server 2016 Datacenter edition. Para hosts Hyper-V que contêm apenas carga de trabalho de locatário VMs que são conectados às redes controlada SDN\, você pode executar o Windows Server 2016 Standard edition.

Enquanto você ainda pode usar as opções de físicas existentes, roteadores e outros dispositivos de hardware, você pode conseguir mais profunda integração entre a rede virtual e a rede física se esses dispositivos são projetados para compatibilidade com rede software definido.  
  
SDN é possível porque os planos de rede - os planos de dados, controle e gerenciamento - não são mais associados aos dispositivos de rede propriamente ditas, mas são abstraídos para uso por outras entidades, como um software de gerenciamento de datacenter como System Center 2016.  
  
SDN permite que você gerencie dinamicamente a sua rede datacenter para fornecer uma maneira automatizada e centralizada para atender aos requisitos dos seus aplicativos e cargas de trabalho. Rede de software definida fornece os seguintes recursos.  
  
-   A capacidade de abstrair seus aplicativos e cargas de trabalho da rede física subjacente, que é realizado por meio da virtualização da rede. Assim como com a virtualização de servidor usando o Hyper-V, as abstrações sejam consistentes e trabalho com seus aplicativos e cargas de trabalho de forma ininterrupta. Por exemplo, redes de software definida fornece abstrações virtuais para seus elementos de rede física, como endereços IP, comutadores e balanceadores de carga.  
  
-   As políticas de controle que regem redes físicos e virtuais, incluindo o tráfego e a capacidade de definir centralmente fluem entre esses dois tipos de rede.  
  
-   A capacidade de implementar políticas de rede de maneira consistente em escala, mesmo quando você implantar novas cargas de trabalho ou move cargas de trabalho em redes físicas ou virtuais.  
  
## <a name="bkmk_ws"></a>Tecnologias de servidor do Windows para Software definidos rede  
Windows Server inclui as seguintes tecnologias de rede de software definido.  

### <a name="bkmk_nc"></a>Controlador de rede

Novo no Windows Server 2016, o controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de ambos virtuais e a infraestrutura de rede física em seu datacenter. Usando o controlador de rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos de rede e serviços.  
  
Controlador de rede é uma função de servidor altamente disponíveis e escalável e fornece uma interface (API) - a API Southbound - que permite que o controlador de rede para se comunicar com a rede e uma segunda API - a API em sentido norte - que permite que você se comunique com o controlador de rede de programação de aplicativo.  
  
Usando o Windows PowerShell, a API REST Representational State Transfer () ou um aplicativo de gerenciamento, você pode usar o controlador de rede para gerenciar a infraestrutura de rede físicos e virtuais a seguir.  
  
-   VMs do Hyper-V e comutadores virtuais  
  
-   Opções de rede física  
  
-   Roteadores de rede física  
  
-   Software de firewall  
  
-   Gateways de VPN, incluindo Gateways de Multitenant de serviço (acesso remoto)  
  
-   Balanceadores de carga  
  
Para obter mais informações, consulte [rede controlador](../sdn/technologies/network-controller/Network-Controller.md).  
  
### <a name="bkmk_hv"></a>Virtualização de rede do Hyper-V

Virtualização do Hyper-V rede ajuda você a abstrair seus aplicativos e cargas de trabalho da rede física usando redes virtuais. Redes virtuais fornecem o isolamento de vários locatários necessário durante a execução em uma estrutura de rede física compartilhada, assim aumenta utilização de recursos. Para garantir que você pode levar adiante seus investimentos existentes, você pode configurar redes virtuais na engrenagem de rede existente. Além disso, as redes virtuais são compatíveis com as redes locais virtuais (VLANs).  
  
Para obter mais informações, consulte [virtualização de rede do Hyper-V](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  
  
### <a name="bkmk_switch"></a>Comutador Virtual Hyper-V

O Hyper-V Virtual alternar é um baseada em software-Ethernet rede comutador de camada 2 que está disponível no Gerenciador do Hyper-V depois que você instalou a função de servidor do Hyper-V. A opção inclui recursos por meio de programação abrangente e gerenciados para se conectar a máquinas virtuais para redes virtuais e a rede física. Além disso, o Hyper-V Virtual Switch fornece imposição da política de segurança, isolamento e níveis de serviço.  
  
No Hyper-V comutador Virtual no Windows Server 2016, você também pode implantar agrupamento Embedded Switch (conjunto) e o acesso direto de memória remoto (RDMA). Para obter mais informações, consulte a seção [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](#bkmk_rdma) neste tópico.  
  
Para obter mais informações sobre o Hyper-V Virtual Switch, consulte [Hyper-V Virtual Switch](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="bkmk_idns"></a>Internas serviço DNS #40 iDNS &; 41;

Máquinas virtuais hospedadas \(VMs\) e aplicativos exigem DNS para se comunicar em suas próprias redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nome DNS para o espaço de nome isolado, locais e recursos de Internet.

Para obter mais informações, consulte [serviço DNS interno #40 iDNS &; 41; para SDN](technologies/Idns-for-Sdn.md).
  
### <a name="bkmk_nfv"></a>Virtualização de função de rede

No software de hoje datacenters definidos, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) são cada vez mais sendo virtualizados como dispositivos virtuais. Essa virtualização de função"rede" é uma progressão natural de virtualização de servidor e a virtualização de rede. Dispositivos virtuais são rapidamente emergentes e criação de um mercado totalmente novo. Eles continuam gerar interesse e obter inovação em ambas as plataformas de virtualização e serviços de nuvem.  
  
As seguintes tecnologias de virtualização de função de rede estão disponíveis.  
  
-   **Software balanceador (SLB) e endereço de rede NAT (conversão)**. 4 balanceador de camada do Norte-Sul e Oeste Leste e NAT aumenta a taxa de transferência, dando suporte direto servidor retornar, com o qual o tráfego de rede retorno pode ignorar o balanceamento de carga de multiplexador. Para obter mais informações, consulte [Software carga balanceamento (SLB) para SDN](technologies/network-function-virtualization/software-load-balancing-for-sdn.md).  
  
-   **Datacenter Firewall**. Este firewall distribuído fornece listas de controle granular de acesso (ACLs), permitindo que você aplique políticas de firewall no nível da interface VM ou no nível de sub-rede.  
  
    Para obter mais informações, consulte [visão geral de Firewall de Datacenter](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
-   **Gateway RAS**. Você pode usar gateways para ligando o tráfego entre redes virtuais e redes não virtualizados; Especificamente, você pode implantar-to-site VPN gateways, gateways de encaminhamento e gateways de encapsulamento de roteamento genérico (GRE). Além disso, M + N redundância de gateways é suportada. Para obter mais informações, consulte [RAS Gateway para SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).
  
Para obter mais informações, consulte [rede função virtualização](technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_rdma"></a>Remoto acesso direto à memória (RDMA) e alternar inserido agrupamento (conjunto)  
No Windows Server 2016, você pode habilitar RDMA em adaptadores de rede que são associados a um comutador Virtual do Hyper-V com ou sem alternar Embedded agrupamento (conjunto). Isso permite que você use menos adaptadores de rede quando você quer usar RDMA e conjunto ao mesmo tempo.  
  
CONJUNTO é uma solução alternativa NIC agrupamento que você pode usar em ambientes que incluem o Hyper-V e a pilha de rede definidos Software (SDN) no Windows Server 2016. DEFINIR algumas das funcionalidades NIC agrupamento integra o Hyper-V Virtual Switch.  
  
CONJUNTO permite que você agrupe entre um e oito físicos adaptadores de rede Ethernet em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual fornecem desempenho rápido e tolerância no caso de uma falha de adaptador de rede.  
Todos os adaptadores de rede de membro do conjunto devem ser instalados no host do Hyper-V físico do mesmo deve ser colocado em uma equipe.  
  
Além disso, você pode usar comandos do Windows PowerShell para habilitar a ponte de centro de dados (DCB), crie um comutador Virtual Hyper-V com uma NIC virtual RDMA (vNIC) e criar um comutador Virtual Hyper-V com vNICs conjunto e RDMA.  
  
Para obter mais informações, consulte [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](https://technet.microsoft.com/library/mt403349.aspx).  
  
### <a name="bkmk_rras"></a>Gateway RAS para SDN  
Gateway RAS é um baseada em software, vários locatários, Border Gateway Protocol (BGP) compatíveis com roteador no Windows Server 2016 é projetado para provedores de serviço de nuvem (CSPs) e as empresas que hospedam várias redes virtuais locatário usando a virtualização de rede do Hyper-V.  
  
Gateway RAS fornece pools de gateway, redundância M + N, vários tipos de conexões VPN para site e BGP refletor de rota para fornecer opções de design flexível para sua infraestrutura de gateway.  
  
Para obter mais informações, consulte [RAS Gateway para SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>(SLB) de balanceamento de carga de software  
Provedores de serviço de nuvem (CSPs) e as empresas que estão implantando Software definida de rede (SDN) no Windows Server 2016 podem usar o balanceamento de carga de Software (SLB) para distribuir uniformemente locatário e tráfego de rede de cliente de locatário entre recursos de rede virtual. O Windows Server SLB permite que vários servidores hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.  
  
Para obter mais informações, consulte [balanceamento de carga de Software & #40; SLB & #41; para SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md).

### <a name="windows-server-containers"></a>Contêineres de servidor do Windows

Windows Server contêineres são um método de virtualização de sistema operacional leve usado para separar os serviços ou aplicativos de outros serviços que são executados no mesmo contêiner host. Para habilitar esse recurso, cada contêiner tem seu próprio modo de exibição do sistema operacional, processos, sistema de arquivos, do registro e endereços IP. Com o Windows Server 2016, agora você pode conectar contêineres do Windows Server para redes virtuais. Para obter mais informações, consulte [contêineres do Windows Server](technologies/containers/Container-networking-overview.md).

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Entrar em contato com a equipe do produto Datacenter e de rede na nuvem

Se você estiver interessado em discussão tecnologias SDN com a Microsoft ou outros clientes SDN, há uma variedade de métodos para tomada de contato.

Para obter mais informações, consulte [entre em contato com a equipe de rede na nuvem e Datacenter](contact-sdn-team.md).
