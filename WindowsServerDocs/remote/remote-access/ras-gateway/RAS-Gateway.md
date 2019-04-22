---
title: Gateway de RAS
description: Este tópico, que é destinado a profissionais de tecnologia da informação (TI), fornece informações gerais sobre o Gateway de RAS, incluindo modos de implantação do Gateway de RAS e recursos no Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: 8fc1c97d7c2a8694e56cc36b5501a82081b3db23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812337"
---
# <a name="ras-gateway"></a>Gateway de RAS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Gateway de RAS é um roteador de software e o gateway que você pode usar no modo de locatário único ou modo multilocatário.  
  
- **Único locatário** modo permite que organizações de qualquer tamanho para implantar o gateway como um exterior, ou para a Internet edge rede virtual privada (VPN) e o servidor DirectAccess. No modo de locatário único, você pode implantar o Gateway de RAS em um servidor físico ou máquina virtual (VM) executando o Windows Server 2016.  
  
- **Modo multilocatário** permite que provedores de serviço de nuvem (CSPs) e empresas a usar o Gateway de RAS para habilitar o datacenter e nuvem roteamento de tráfego de rede entre redes virtuais e físicas, incluindo a Internet. No modo multilocatário, é recomendável que você implante o Gateway de RAS em VMs que estão executando o Windows Server 2016.  
  
> [!NOTE]  
> Gateway de RAS dá suporte a IPv4 e IPv6, incluindo o encaminhamento IPv4 e IPv6. Quando você configura o Gateway de RAS com a conversão de endereço de rede (NAT), somente o NAT44 tem suporte.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>Quem estaria interessado no Gateway de RAS?
  
Se você for um administrador do sistema, arquiteto de rede ou outro profissional de TI, Gateway de RAS interessem a você em uma ou mais das seguintes circunstâncias:  
  
-   Você projeta ou dá suporte à infraestrutura de TI para uma organização que está usando ou planejando usar o Hyper-V para implantar VMs (máquinas virtuais) em redes virtuais.  
  
-   Você projeta ou dá suporte à infraestrutura de TI de uma organização que implantou ou está planejando implantar tecnologias de nuvem.  
  
-   Você deseja fornecer conectividade total de rede entre redes físicas e redes virtuais.  
  
-   Você deseja fornecer aos clientes de sua organização acesso às suas redes virtuais pela Internet.  
  
-   Você deseja fornecer aos funcionários da sua organização acesso remoto à rede da organização.  
  
-   Você deseja se conectar a escritórios em diferentes locais físicos com a Internet.  
 
Este tópico, que é destinado a profissionais de tecnologia da informação (TI), fornece informações gerais sobre o Gateway de RAS, incluindo os recursos e modos de implantação do Gateway de RAS. 
  
Este tópico contém as seguintes seções.  
  
  
-   [Modos de implantação do Gateway RAS](#bkmk_modes)  
  
-   [Clustering do Gateway de RAS para alta disponibilidade](#bkmk_clustering)  
  
-   [Recursos de Gateway RAS](#bkmk_features)  
  
-   [Cenários de implantação do Gateway RAS](#bkmk_deploy)  
  
-   [Ferramentas de gerenciamento de Gateway RAS](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>Modos de implantação do Gateway RAS  
Gateway de RAS inclui os seguintes modos de implantação.  
  
### <a name="single-tenant-mode"></a>Modo de locatário único  
Para a maioria das organizações, usando o Gateway de RAS no modo de locatário único é a configuração normal. No modo de locatário único, você pode implantar o Gateway de RAS como um servidor de VPN de borda, um servidor do DirectAccess de borda ou ambos simultaneamente. Nessa configuração, o Gateway de RAS fornece funcionários remotos com conectividade à sua rede usando conexões VPN ou DirectAccess. Além disso, o modo de locatário único permite que você conecte escritórios em locais físicos diferentes da Internet.  
  
### <a name="multitenant-mode"></a>Modo multilocatário  
Se sua organização for um CSP ou uma empresa com vários locatários, você pode implantar o Gateway de RAS no modo multilocatário para fornecer roteamento de tráfego de rede para e de redes virtuais e físicas.  
  
A multilocação é a capacidade de uma infraestrutura de nuvem para oferecer suporte as cargas de trabalho de máquina virtual de vários locatários, ainda isolá-las umas das outras, embora todas as cargas de trabalho executados na mesma infraestrutura. As várias cargas de trabalho de um locatário individual podem se interconectar e serem gerenciadas remotamente, mas esses sistemas não interconectam com as cargas de trabalho de outros locatários, nem outros locatários podem gerenciá-las remotamente.  
  
Por exemplo, uma Empresa pode ter várias sub-redes virtuais diferentes, cada uma dedicada a servir um departamento específico, como Pesquisa e Desenvolvimento ou Contabilidade. Em outro exemplo, um CSP tem muitos locatários com sub-redes virtuais isoladas existentes no mesmo datacenter físico. Em ambos os casos, o Gateway de RAS pode rotear o tráfego de e para cada locatário, mantendo o isolamento projetado de cada locatário. Esse recurso torna o Gateway de RAS locatários.  
  
Redes virtuais são criadas usando a virtualização de rede Hyper-V, que é uma tecnologia que foi introduzida no Windows Server 2012 e foi aprimorada no Windows Server 2016. Gateway de RAS é integrado com a virtualização de rede do Hyper-V e é capaz de rotear o tráfego de rede com eficácia em ocasiões em que há muitos clientes diferentes - ou locatários - que tenham isolados virtual redes no mesmo datacenter.  
  
Virtualização de rede do Hyper-V fornece a capacidade de implantar uma rede de máquina virtual (VM) que é independente da rede física subjacente. Com redes VM, que são compostos de uma ou mais sub-redes virtuais, a localização física exata de uma sub-rede IP é separada da topologia da rede virtual. Como resultado, você pode mover facilmente suas sub-redes diante do local para a nuvem - enquanto preserva seu IP existente endereços e topologia na nuvem. Essa capacidade de preservar a infraestrutura permite que os serviços existentes continuem funcionando, sem reconhecimento do local físico das sub-redes. Ou seja, a Virtualização de Rede Hyper-V permite uma nuvem híbrida integrada.  
  
> [!NOTE]  
> Virtualização de rede do Hyper-V é uma tecnologia de sobreposição de rede usando a rede virtualização de encapsulamento de roteamento genérico ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)), que permite que os locatários ativem seus próprios espaços de endereço e permite aos CSPs melhor escalabilidade do que é é possível usando VLANs para isolamento de locatários.  
  
No Windows Server 2016, Gateway de RAS roteia o tráfego de rede entre a rede física e os recursos de rede VM, independentemente de onde os recursos estão localizados. Você pode usar o Gateway de RAS para rotear o tráfego de rede entre redes físicas e virtuais no mesmo local físico ou em vários locais físicos diferentes.  
  
Por exemplo, se você tiver uma rede física e uma rede virtual no mesmo local físico, você pode implantar um computador que executa o Hyper-V está configurado com uma VM do Gateway de RAS para atuar como um gateway de encaminhamento e rotear o tráfego entre a física e virtual redes.  
  
Em outro exemplo, se suas redes virtuais existirem na nuvem, seu CSP poderá implantar um Gateway RAS para que você possa criar uma conexão site a site de rede virtual privada (VPN) entre o servidor VPN e o Gateway de RAS do CSP; Quando esse link é estabelecido você pode se conectar aos recursos virtuais na nuvem sobre a conexão VPN.  
  
Para obter mais informações, consulte [alta disponibilidade do Gateway de RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_clustering"></a>Clustering do Gateway de RAS para alta disponibilidade  
Gateway de RAS é implantado em um computador dedicado que executa o Hyper-V e que está configurado com uma máquina virtual. A VM, em seguida, é configurada como um Gateway de RAS.  
  
Para alta disponibilidade de recursos de rede, você pode implantar o Gateway de RAS com failover usando dois servidores de host físico que executa o Hyper-V que são cada um também executando uma máquina virtual (VM) que é configurada como um gateway. Em seguida, as VMs do gateway são configuradas como um cluster para fornecer proteção de failover contra interrupções da rede e falha de hardware.  
  
Por exemplo, se sua organização é uma empresa com uma implantação de nuvem privada, talvez seja necessário apenas duas VMs de Gateway de RAS, cada um dos quais está instalada em um computador diferente que executa o Hyper-V. Nesse cenário, as VMs de Gateway de RAS são adicionadas a um cluster para fornecer alta disponibilidade.  
  
Em outro exemplo, se sua organização for um provedor de serviços de nuvem (CSP) com 200 locatários em seu datacenter, você pode usar oito VMs de Gateway de RAS, com cada par de VMs de Gateway RAS clusterizadas fornecendo serviços de roteamento para 50 locatários. Nesse cenário, dois computadores que executam o Hyper-V cada tem quatro máquinas virtuais configuradas como Gateways de RAS. Você, em seguida, configurar quatro clusters de VM de Gateway de RAS, cada cluster que contém uma VM de cada computador que executa o Hyper-V.  
  
Quando você implanta o Gateway de RAS, os servidores de host que executam o Hyper-V e as VMs que você configura como gateways devem estar executando o Windows Server 2012 R2 ou Windows Server 2016.  
  
## <a name="bkmk_features"></a>Recursos de Gateway RAS  
Gateway de RAS inclui os seguintes recursos.  
  
-   **VPN site a site**. Esse recurso de Gateway de RAS permite que você conecte duas redes em locais físicos diferentes da Internet por meio de uma conexão de VPN site a site. Se você tiver um escritório principal e várias filiais, você pode implantar uma Gateway de RAS em cada local de borda e criar conexões site a site para fornecer fluxo de tráfego de rede entre os locais. Para os CSPs que hospedam muitos locatários em seus datacenters, o Gateway de RAS fornece uma solução de gateway multilocatário que permite que seus locatários acessar e gerenciar seus recursos em conexões de VPN site a site de sites remotos, e que permite que o fluxo do tráfego de rede entre recursos virtuais no seu datacenter e sua rede física.  
  
-   **VPN ponto a site**. Esse recurso de Gateway de RAS permite que os funcionários da organização ou os administradores para se conectar à rede da sua organização de locais remotos. Para implantações de único locatário do Gateway de RAS, funcionários remotos podem se conectar à rede da organização, usando uma conexão VPN. Essa conexão permite que eles usem os recursos de rede interna, como sites da intranet e servidores de arquivos. Para implantações de multilocatários, os administradores de rede do locatário podem usar conexões de VPN ponto a site para acessar recursos de rede virtual no datacenter do CSP.  
  
-   **Roteamento dinâmico com o Border Gateway Protocol (BGP)**. O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente. Se sua organização tiver vários sites que estão conectados por meio de roteadores BGP habilitado, como o Gateway de RAS, o BGP permite que os roteadores calcular automaticamente e usar as rotas válidas entre si em caso de interrupção de rede ou a falha. Para obter mais informações, consulte [4271 RFC](https://tools.ietf.org/html/rfc4271).  
  
-   **Conversão de endereço (NAT) de rede**. Conversão de endereços de rede (NAT) permite que você compartilhe uma conexão à Internet pública por meio de uma única interface com um único endereço IP público. Os computadores na rede privada usam endereços privados e não roteável. NAT mapeia os endereços privados para o endereço público. Esse recurso de Gateway de RAS permite que os funcionários da organização com implantações de único locatário acessar recursos da Internet por trás do gateway. Para os CSPs, esse recurso permite que aplicativos que estão em execução em VMs para acessar a Internet do locatário. Por exemplo, um VM que está configurado como um servidor Web de locatário pode contatar os recursos financeiros externos para processar transações de cartão de crédito.  

  
## <a name="bkmk_deploy"></a>Cenários de implantação do Gateway RAS  
A seguir estão os cenários de implantação recomendada para o Gateway de RAS.  
  
-   **Enterprise Edge - implantação de único locatário**. Com o implantação corporativa de locatário único, você pode conectar uma física para vários outros locais físicos pela Internet usando VPN site a site de recursos – e Border Gateway Protocol (BGP) permite que você use o roteamento dinâmico. Você também pode fornecer acesso de funcionários remotos à rede da organização com conexões VPN ponto a site e conexões do DirectAccess. (As conexões do DirectAccess estão sempre ativados e também oferecem a vantagem de que você pode gerenciar facilmente os computadores que estão conectados usando DirectAccess, porque eles são conectados sempre que estiverem em e conectado à Internet.) Você também pode configurar Gateways de RAS do locatário único empresarial com NAT, para que os computadores na sua Intranet facilmente podem se comunicar com a Internet.  
  
-   **Borda do provedor de serviço - implantação de multilocatário de nuvem**. Implantação de multilocatário do Gateway de RAS para os CSPs permite que você ofereça a seus locatários todos os recursos que estão disponíveis com a implantação de único locatário de borda da empresa. Conexões de VPN site a site entre redes virtuais de locatário em seu datacenter e os locais de rede do locatário entre a média de Internet que locatários tenham acesso contínuo aos seus recursos de nuvem o tempo todo. Acesso a VPN ponto a site para locatários significa que os administradores de inquilinos sempre podem se conectar às suas redes virtuais em seu datacenter para gerenciar seus recursos. BGP fornece roteamento dinâmico e mantém locatários conectados aos seus ativos, mesmo quando problemas de rede ocorrem na Internet ou em outro lugar. E o NAT permite que locatários VMs para se conectar aos recursos na Internet, como recursos de processamento de cartão de crédito.  
  
## <a name="bkmk_manage"></a>Ferramentas de gerenciamento de Gateway RAS  
A seguir estão as ferramentas de gerenciamento para o Gateway de RAS.  
  
-   No Windows Server 2016, para implantar um roteador de Gateway de RAS, você deve usar comandos do Windows PowerShell. Para obter mais informações, consulte [Cmdlets de acesso remoto](https://technet.microsoft.com/library/hh918399.aspx) para Windows Server 2016 e Windows 10.  
  
-   No System Center 2012 R2 Virtual Machine Manager (VMM), o Gateway de RAS é chamado de Gateway do Windows Server. Um conjunto limitado de opções de configuração do Border Gateway Protocol (BGP) estão disponíveis na interface de software do VMM, incluindo **endereço IP do BGP Local** e **números de sistema autônomo (ASN)**,  **Lista de endereços de IP de par de BGP**, e **valores ASN**. No entanto, você pode usar os comandos BGP do Windows PowerShell de Acesso Remoto para configurar todos os outros recursos do Gateway do Windows Server. Para obter mais informações, consulte [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) e [Cmdlets de acesso remoto](https://technet.microsoft.com/library/hh918399.aspx) para Windows Server 2016 e Windows 10.  
  
## <a name="related-topics"></a>Tópicos relacionados
- [Alta disponibilidade do Gateway RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Túnel de GRE no Windows Server](gre-tunneling-windows-server.md)
- [Desempenho e taxa de transferência túnel GRE de Gateway RAS](RAS-Gateway-GRE-Perf.md)
