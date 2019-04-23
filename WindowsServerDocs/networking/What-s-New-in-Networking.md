---
title: Novidades na rede
description: Este tópico fornece uma visão geral sobre novos recursos e tecnologias de rede no Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43ce6290f6559be7cb078032b79519d1681506d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829187"
---
# <a name="whats-new-in-networking"></a>Novidades na rede

>Aplica-se a: Windows Server 2016

A seguir estão as tecnologias de rede novas ou aprimoradas no Windows Server 2016.  
  UPD este tópico contém as seções a seguir.  
  
-   [Novas tecnologias e recursos de rede](#bkmk_features)  
  
-   [Novos recursos para tecnologias de rede adicionais](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Novas tecnologias e recursos de rede

A rede é uma parte fundamental da plataforma de Software definidas SDDC (Datacenter) e Windows Server 2016 fornece tecnologias de Software Defined Networking (SDN) novas e aprimoradas para ajudá-lo a mover para uma solução SDDC totalmente para a sua organização.  
  
Quando você gerencia redes como um recurso definida pelo software, você pode descrever os requisitos de infraestrutura do aplicativo uma vez e, em seguida, escolha onde o aplicativo é executado – localmente ou na nuvem.  Essa consistência significa que seus aplicativos agora são mais fáceis de escala e você pode executar perfeitamente os aplicativos, em qualquer lugar, com confiança igual em torno de segurança, desempenho, qualidade de serviço e disponibilidade.  
  
As seções a seguir contêm informações sobre esses novos recursos e tecnologias de rede.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestrutura de rede definida pelo software

A seguir estão as tecnologias de infraestrutura SDN novas ou aprimoradas.  
  
-   **Controlador de rede**. Novo no Windows Server 2016, o controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. Usando o Controlador de Rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos e dos serviços de rede. Para obter mais informações, consulte [controlador de rede](sdn/technologies/network-controller/Network-Controller.md) e [implantar redes definidas por Software usando scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Comutador Virtual Hyper-V**. O comutador Virtual do Hyper-V é executado nos hosts Hyper-V e permite que você crie distribuído comutação e roteamento, e uma camada de imposição de política que é compatível com o Microsoft Azure e alinhado. Para saber mais, consulte [Comutador Virtual do Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualização de função (NFV) de rede**. No software de hoje datacenters definidos, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) são cada vez mais sendo implantados como soluções de virtualização. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Soluções de virtualização são emergentes e criando um novo mercado rapidamente. Eles continuam a gerar interesse e obter um impulso em ambas as plataformas de virtualização e serviços de nuvem. As seguintes tecnologias NFV estão disponíveis no Windows Server 2016.  
  
    -   **Firewall do Datacenter**. Esse firewall distribuído fornece listas de controle de acesso granular (ACLs), permitindo que você aplique políticas de firewall no nível da interface VM ou no nível de sub-rede.  
  
        Para obter mais informações, consulte [visão geral do Firewall do Datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Gateway de RAS**. Você pode usar o Gateway de RAS para rotear o tráfego entre redes virtuais e redes físicas, incluindo conexões de VPN site a site do seu datacenter de nuvem para locais remotos de seus locatários. Especificamente, você pode implantar o protocolo IKE versão 2 (IKEv2) site a site redes virtuais privadas (VPNs), VPN de camada 3 (L3) e gateways de encapsulamento de roteamento genérico (GRE). Além disso, os pools de gateway e redundância M + N dos gateways agora têm suporte; e Border Gateway Protocol (BGP) com recursos de refletor de rota fornece roteamento dinâmico entre as redes para todos os cenários de gateway (IKEv2 VPN, VPN GRE e L3 VPN).  
  
        Para obter mais informações, consulte [o que há de novo no Gateway RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [Gateway RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **O balanceador de carga de software (SLB) e endereços de rede NAT (conversão)**. O norte-Sul e Leste-Oeste da camada 4 balanceador de carga e NAT melhora a taxa de transferência, oferecendo suporte a retorno de servidor direto, com a qual o tráfego de rede de retorno pode ignorar o balanceamento de carga multiplexador.  
       Para obter mais informações, consulte [balanceamento de carga de Software &#40;SLB&#41; para SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obter mais informações, consulte [a virtualização de rede função](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Padronizado protocolos**. Controlador de rede usa REST Representational State Transfer () em sua interface northbound com cargas de notação JSON (JavaScript Object). A interface de southbound do controlador de rede usa vSwitch aberto protocolo de gerenciamento de banco de dados (OVSDB).  
  
-   **Tecnologias de encapsulamento flexível**. Essas tecnologias operam no plano de dados e dar suporte a LAN Virtual extensível (VxLAN) e rede virtualização genérico roteamento NVGRE (encapsulamento). Para obter mais informações, consulte [túnel GRE no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para obter mais informações sobre o SDN, consulte [rede definida pelo Software &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Conceitos básicos de escala de nuvem
 
Os fundamentos de escala de nuvem seguir agora estão disponíveis.  
  
-   **Convergido placa de Interface de rede (NIC)**. A NIC convergida permite que você use um único adaptador de rede para o gerenciamento de armazenamento habilitados para RDMA de acesso de memória direto remoto e o tráfego de locatário. Isso reduz as despesas de capital que estão associadas a cada servidor em seu datacenter, porque você precisa de menos adaptadores de rede para gerenciar diferentes tipos de tráfego por servidor.  
  
-   **Pacote direto**.  Pacote direto fornece uma infraestrutura de processamento de pacote de baixa latência e taxa de transferência de tráfego de rede de alta.  
  
-   **Switch Embedded Teaming (SET)**.        CONJUNTO é uma solução de agrupamento NIC que está integrada no comutador Virtual Hyper-V. CONJUNTO permite que o agrupamento de até oito NICS físicas em uma única equipe de conjunto, o que melhora a disponibilidade e fornece o failover. No Windows Server 2016, você pode criar um conjunto de equipes que são restritos ao uso de RDMA e o bloco de mensagens de servidor (SMB). Além disso, você pode usar o conjunto de equipes para distribuir o tráfego de rede para virtualização de rede do Hyper-V. Para obter mais informações, consulte [remotos acesso direto à memória &#40;RDMA&#41; e agrupamento incorporado do comutador &#40;definir&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Novos recursos para tecnologias de rede adicionais

Esta seção contém informações sobre os novos recursos para tecnologias de rede familiares.
  
## <a name="bkmk_dhcp"></a>DHCP  
O DHCP é um padrão IETF (Internet Engineering Task Force) projetado para reduzir a carga administrativa e a complexidade da configuração de hosts em uma rede baseada em TCP/IP, por exemplo, uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração de TCP/IP em clientes DHCP é automático.  
  
Para obter mais informações, consulte [o que há de novo no DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS é um sistema usado em redes TCP/IP para nomear computadores e serviços de rede. A nomeação DNS localiza computadores e serviços por meio de nomes simples. Quando um usuário insere um nome DNS em um aplicativo, os serviços DNS podem resolvê-lo para outra informação associada a ele, como um endereço IP.  
  
Veja a seguir informações sobre o cliente DNS e servidor DNS.  
  
### <a name="bkmk_dnsc"></a>Cliente DNS  
A seguir estão as tecnologias de cliente DNS novas ou aprimoradas.  
  
-   **Associação de serviço cliente DNS**. No Windows 10, o serviço cliente DNS oferece suporte avançado para computadores com mais de uma interface de rede.  
  
Para obter mais informações, consulte [que há de novo no cliente DNS no Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Servidor DNS  
A seguir estão as tecnologias de servidor DNS novas ou aprimoradas.  
  
-   **Políticas de DNS**.  Você pode configurar políticas DNS para especificar como um servidor DNS responde a consultas DNS. As respostas DNS podem ser baseadas em endereço IP do cliente (local), hora do dia e vários outros parâmetros. Políticas de DNS permitem DNS reconhecimento de local, o gerenciamento de tráfego, balanceamento de carga, o DNS com partição de rede e outros cenários.  
  
-   **Suporte de Nano Server para o arquivo com base em DNS**, você pode implantar o servidor DNS no Windows Server 2016 em uma imagem do Nano Server. Essa opção de implantação está disponível para você, se você estiver usando o arquivo com base em DNS. Pelo servidor DNS em execução em uma imagem do Nano Server, você pode executar seus servidores DNS com o volume reduzido, inicialização rápida e aplicação de patch minimizados.  
  
    > [!NOTE]   
    > DNS integrado ao Active Directory não tem suporte no Nano Server.  
  
-   **(RRL) limitação de taxa de resposta**.  Você pode habilitar a limitação de taxa de resposta nos servidores DNS. Ao fazer isso, você deve evitar a possibilidade de sistemas mal-intencionado usando seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.  
  
-   **Autenticação baseada em DNS de entidades nomeadas (painel)**.   Você pode usar registros TLSA (autenticação de segurança de camada de transporte) para fornecer informações para os clientes DNS que o estado de qual autoridade de certificação (CA), eles devem esperar um certificado de nome de domínio. Isso impede ataques man-in-the-middle, onde alguém pode corromper o cache do DNS para apontar para seu próprio site e fornecer um certificado emitidos por eles de uma autoridade de certificação diferente.  
  
-   **Suporte de registro desconhecido**.   
     Você pode adicionar os registros que não são explicitamente suportados pelo servidor DNS do Windows usando a funcionalidade de registro desconhecido.  
  
-   **Dicas de raízes de IPv6**.   
     Você pode usar o IPV6 nativo, suporte a dicas de raiz para executar a resolução de nome de internet usando os servidores raiz de IPV6.  
  
-   **Suporte do Windows PowerShell aprimorado**.   
      Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.  
  
Para obter mais informações, consulte [que há de novo no servidor DNS no Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Túnel GRE  
Gateway de RAS agora dá suporte a túneis de encapsulamento de roteamento genérico (GRE) de alta disponibilidade para redundância M + N dos gateways e conexões de site a site. GRE é um protocolo de túnel leve que pode encapsular uma ampla variedade de protocolos de camada de rede em links de ponto a ponto virtuais por meio de uma ligação entre redes de protocolo de Internet.  
  
Para obter mais informações, consulte [túnel GRE no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualização de rede Hyper-V  
Introduzido no Windows Server 2012, a virtualização de rede do Hyper-V (HNV) habilita a virtualização de redes de cliente em uma infraestrutura de rede física compartilhada. Com mínimas alterações necessárias na malha de rede física, HNV fornece provedores de serviço a agilidade para implantar e migrar cargas de trabalho de locatário em qualquer lugar em três nuvens: a nuvem do provedor de serviço, a nuvem privada ou a nuvem pública do Microsoft Azure.  
  
Para obter mais informações, consulte [Novidades na virtualização de rede do Hyper-V no Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
O IPAM fornece funcionalidades de administração e monitoramento altamente personalizáveis para o endereço IP e a infraestrutura DNS em uma rede da organização. O IPAM pode monitorar, auditar e gerenciar servidores que executam o protocolo de configuração de Host dinâmico (DHCP) e o sistema de nome de domínio (DNS).  
  
-   **Aprimorado o gerenciamento de endereço IP**.  
     Funcionalidades do IPAM são aprimoradas para cenários como o tratamento de sub-redes /32 IPv4 e IPv6 /128 e Localizando livres sub-redes de endereço IP e intervalos em um bloco de endereço IP.  
  
-   **Aprimorado o gerenciamento de serviços DNS**.  
     O IPAM dá suporte a registro de recurso DNS encaminhador condicional e gerenciamento de zona DNS para os servidores DNS integrado ao Active Directory e o backup em arquivo ingressado no domínio.  
  
-   **Gerenciamento de endereço (DDI) do DNS, DHCP e IP integrado**.  
     Várias novas experiências e operações de gerenciamento do ciclo de vida integrada estão habilitadas, como visualizar todos os recursos DNS registros que pertencem a um IP endereço, automatizada de inventário de endereços IP com base em registros de recursos DNS e o gerenciamento de ciclo de vida do endereço IP para operações de DNS e DHCP.  
  
-   **Suporte a várias florestas do Active Directory**.  
     Você pode usar o IPAM para gerenciar os servidores DNS e DHCP de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta em que o IPAM é instalado e cada uma das florestas remotas.  
  
-   **Suporte do Windows PowerShell para controle de acesso baseado em função**.  
     Você pode usar o Windows PowerShell para definir escopos de acesso nos objetos do IPAM.  
  
Para obter mais informações, consulte [Novidades no IPAM](technologies/ipam/What-s-New-in-IPAM.md) e [Manage IPAM](technologies/ipam/Manage-IPAM.md).  
  

