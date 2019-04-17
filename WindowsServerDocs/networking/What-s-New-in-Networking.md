---
title: Novidades na rede
description: Este tópico fornece uma visão geral sobre novos recursos e tecnologias de rede no Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85b1a573e8ac724a2a9e22863f890db668423cad
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-networking"></a>Novidades na rede

>Aplica-se a: Windows Server 2016

A seguir estão as tecnologias de rede novas ou avançadas no Windows Server 2016.  
  
Este tópico contém as seções a seguir.  
  
-   [Novos recursos de rede e tecnologias](#bkmk_features)  
  
-   [Novos recursos para as tecnologias de rede adicionais](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Novos recursos de rede e tecnologias

Rede é uma parte fundamental da plataforma do Software definidos Datacenter (SDDC) e Windows Server 2016 fornece novas e aprimoradas tecnologias de rede definidos Software (SDN) para ajudá-lo a mover para uma solução SDDC totalmente realizada para sua organização.  
  
Ao gerenciar redes como um recurso definido de software, você pode descrever os requisitos de infraestrutura de um aplicativo uma vez e escolha onde o aplicativo é executado - no local ou na nuvem.  Essa consistência significa que seus aplicativos agora são mais fáceis de escala e perfeitamente você pode executar aplicativos, em qualquer lugar e com confiança igual ao redor de segurança, desempenho e qualidade de serviço e a disponibilidade.  
  
As seções a seguir contêm informações sobre esses novos recursos e tecnologias de rede.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestrutura de rede definidos do software

A seguir estão as tecnologias de infraestrutura SDN novas ou melhoradas.  
  
-   **O controlador de rede**. Novo no Windows Server 2016, controlador de rede oferece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede físicos e virtuais em seu datacenter. Usando o controlador de rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos de rede e serviços. Para obter mais informações, consulte [rede controlador](sdn/technologies/network-controller/Network-Controller.md) e [implantar Software definidos redes usando scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Comutador Virtual Hyper-V**. O Hyper-V Virtual Switch é executado em hosts Hyper-V e permite que você crie troca distribuído e roteamento e uma camada de imposição de política que é alinhado e compatíveis com o Microsoft Azure. Para obter mais informações, consulte [Hyper-V Virtual Switch ](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualização de função (NFV) de rede**. No software de hoje datacenters definidos, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) estão cada vez mais está sendo implantados como dispositivos virtuais. Essa virtualização de função"rede" é uma progressão natural de virtualização de servidor e a virtualização de rede. Dispositivos virtuais são rapidamente emergentes e criação de um mercado totalmente novo. Eles continuam gerar interesse e obter inovação em ambas as plataformas de virtualização e serviços de nuvem. As seguintes tecnologias NFV Network estão disponíveis no Windows Server 2016.  
  
    -   **Datacenter Firewall**. Este firewall distribuído fornece listas de controle granular de acesso (ACLs), permitindo que você aplique políticas de firewall no nível da interface VM ou no nível de sub-rede.  
  
        Para obter mais informações, consulte [visão geral de Firewall de Datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Gateway RAS**. Você pode usar RAS Gateway para o tráfego de roteamento entre redes virtuais e redes físicas, incluindo conexões de VPN-to-site de seu datacenter na nuvem para locais remotos seu locatários. Especificamente, você pode implantar Internet Key Exchange versão 2 (IKEv2)-to-site redes virtuais privadas (VPNs), VPN Layer 3 (L3) e gateways de encapsulamento de roteamento genérico (GRE). Além disso, pools de gateway e redundância M + N de gateways agora têm suporte; e Border Gateway Protocol (BGP) com recursos de refletor de rota fornece roteamento dinâmico entre redes para todos os cenários de gateway (IKEv2 VPN, GRE VPN e L3 VPN).  
  
        Para obter mais informações, consulte [What's New em Gateway RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [RAS Gateway para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Software balanceador (SLB) e endereço de rede NAT (conversão)**. 4 balanceador de camada do Norte-Sul e Oeste Leste e NAT aumenta a taxa de transferência, dando suporte direto servidor retornar, com o qual o tráfego de rede retorno pode ignorar o balanceamento de carga de multiplexador.  
       Para obter mais informações, consulte [balanceamento de carga de Software & #40; SLB & #41; para SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obter mais informações, consulte [rede função virtualização](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Padronizada protocolos**. Controlador de rede usa REST Representational State Transfer () em sua interface em sentido norte com cargas de JavaScript Object Notation (JSON). A interface southbound do controlador de rede usa vSwitch aberto protocolo de gerenciamento de banco de dados (OVSDB).  
  
-   **Tecnologias de encapsulamento flexível**. Essas tecnologias operam no plano de dados e suportam LAN Extensible Virtual (VxLAN) e rede virtualização genérico de roteamento de encapsulamento (NVGRE). Para obter mais informações, consulte [GRE encapsulamento no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para saber mais sobre SDN, consulte [Software de rede definidos & #40; SDN & #41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Conceitos básicos de escala de nuvem
 
Os seguintes fundamentos de escala de nuvem agora estão disponíveis.  
  
-   **Convergidos placa de Interface de rede (NIC)**. O NIC convergido permite que você use um único adaptador de rede para gerenciamento de armazenamento remoto RDMA direta de acesso à memória habilitado e o tráfego do locatário. Isso reduz as despesas que estão associadas a cada servidor em seu datacenter, porque você precisa de adaptadores de rede menos para gerenciar diferentes tipos de tráfego por servidor.  
  
-   **Pacote Direta**.  Pacote Direta oferece uma taxa de transferência de tráfego de rede alto e infraestrutura de processamento do pacote de baixa latência.  
  
-   **Alternar inserido agrupamento (conjunto)**.        CONJUNTO é uma solução de agrupamento de NIC que está integrada no Hyper-V Virtual Switch. CONJUNTO permite o agrupamento de até oito placas NIC físicas em uma única equipe conjunto, que melhora a disponibilidade e fornece failover. No Windows Server 2016, você pode criar equipes de conjunto que são restritas ao uso do servidor SMB Message Block () e RDMA. Além disso, você pode usar o conjunto de equipes para distribuir o tráfego de rede para a virtualização de rede do Hyper-V. Para obter mais informações, consulte [remoto acesso direto à memória & #40; RDMA & #41; e alternar agrupamento Embedded & #40; Definir & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Novos recursos para as tecnologias de rede adicionais

Esta seção contém informações sobre novos recursos para tecnologias de rede familiares.
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP é um padrão de Internet Engineering Task Force (IETF) que foi projetado para reduzir a sobrecarga administrativa e a complexidade de configurar hosts em uma rede baseada em TCP/IP, como uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração TCP/IP nos clientes DHCP é automático.  
  
Para obter mais informações, consulte [What's New em DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS é um sistema que é usado em redes TCP/IP para nomear computadores e serviços de rede. Os nomes DNS localiza computadores e serviços por meio de nomes amigáveis. Quando um usuário insere um nome DNS em um aplicativo, os serviços DNS podem resolver o nome para outras informações que está associadas com o nome, como um endereço IP.  
  
A seguir encontra informações sobre o cliente DNS e servidor DNS.  
  
### <a name="bkmk_dnsc"></a>Cliente DNS  
A seguir estão as tecnologias do cliente DNS novas ou melhoradas.  
  
-   **Associação de serviço do cliente DNS**. No Windows 10, o serviço de cliente DNS oferece suporte aprimorado para computadores com mais de uma interface de rede.  
  
Para obter mais informações, consulte [What's New no cliente DNS no Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Servidor DNS  
A seguir estão as tecnologias de servidor DNS novas ou melhoradas.  
  
-   **Políticas DNS**.  Você pode configurar políticas DNS para especificar como um servidor DNS responde às consultas do DNS. Respostas DNS podem ser baseadas em endereço IP do cliente (local), hora do dia e vários outros parâmetros. As políticas DNS permitem DNS reconhecimento de local, gerenciamento de tráfego, balanceamento de carga, uma DNS e outros cenários.  
  
-   **Suporte de servidor Nano para arquivos com base em DNS**, você pode implantar o servidor DNS no Windows Server 2016 em uma imagem do servidor Nano. Essa opção de implantação está disponível para você se você estiver usando o arquivo com base em DNS. Pelo servidor DNS em execução em uma imagem do servidor Nano, você pode executar seus servidores DNS com espaço reduzido, inicialização rápida e aplicação de patch minimizado.  
  
    > [!NOTE]   
    > DNS integrado ao Active Directory não é compatível com o servidor Nano.  
  
-   **Resposta classificar limitação (RRL)**.  Você pode habilitar a limitação de taxa de resposta em seus servidores DNS. Ao fazer isso, você deve evitar a possibilidade de sistemas mal-intencionado usando seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.  
  
-   **Autenticação baseada em DNS de entidades nomeadas (DANE)**.   Você pode usar registros TLSA (Transport Layer Security autenticação) para fornecer informações aos clientes DNS afirma que autoridade de certificação (CA) devem esperar um certificado de seu nome de domínio. Isso impede que ataques man-in-the-middle, em que alguém pode corromper o cache do DNS para apontar para o site ganha e fornecer um certificado que eles emitido por uma autoridade de certificação diferente.  
  
-   **Suporte de registro desconhecido**.   
     Você pode adicionar os registros que não são suportados explicitamente pelo servidor DNS do Windows usando a funcionalidade de registro desconhecido.  
  
-   **Dicas de raiz IPv6**.   
     Você pode usar o IPV6 nativo dicas de raiz dão suporte para executar a resolução de nomes de internet usando os servidores de raiz IPV6.  
  
-   **Suporte do Windows PowerShell aprimorado**.   
      Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.  
  
Para obter mais informações, consulte [What's New no servidor DNS no Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Encapsulamento de GRE  
Gateway RAS agora dá suporte a alta disponibilidade genérico de roteamento de encapsulamento () túneis para conexões de site e redundância M + N de gateways. GRE é um protocolo de encapsulamento leve que pode encapsular uma ampla variedade de protocolos de camadas de rede dentro de links ponto a ponto virtual através de uma interconexão de protocolo de Internet.  
  
Para obter mais informações, consulte [GRE encapsulamento no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualização de rede do Hyper-V  
Introduzida no Windows Server 2012, a virtualização de rede do Hyper-V (HNV) permite a virtualização de redes de cliente na parte superior de uma infraestrutura de rede física compartilhada. Com alterações mínimas necessárias na malha de rede física, HNV oferece provedores de serviço a agilidade para implantar e migrar cargas de trabalho de locatário em qualquer lugar entre três nuvens: a nuvem de provedor de serviço, a nuvem privada ou a nuvem pública do Microsoft Azure.  
  
Para obter mais informações, consulte [What's New na virtualização de rede do Hyper-V no Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM fornece recursos de monitoramento e administrativos altamente personalizáveis para o endereço IP e a infraestrutura DNS em uma rede corporativa. Usando IPAM, você pode monitorar, auditar e gerenciar servidores que executam o Dynamic Host Configuration Protocol (DHCP) e sistema de nome de domínio (DNS).  
  
-   **Gerenciamento de endereço IP aprimorado**.  
     Recursos IPAM aperfeiçoados para cenários como lidar com sub-redes /32 IPv4 e IPv6 /128 e localização de sub-redes gratuitos de endereço IP e intervalos de em um bloco de endereço IP.  
  
-   **Gerenciamento de serviço DNS aprimorado**.  
     IPAM dá suporte a registro de recurso DNS, o encaminhador condicional e o gerenciamento de zona DNS para os servidores DNS integrada ao Active Directory e feito o arquivo ingressado no domínio.  
  
-   **Gerenciamento de endereço (DDI) DNS, DHCP e IP integrado**.  
     Várias novas experiências e gerenciamento de ciclo de vida integrado operações forem habilitadas, como visualizar todos os registros de recurso DNS que pertencem a um endereço IP, automatizada inventário dos endereços IP com base em registros de recurso DNS e gerenciamento de ciclo de vida de endereços IP para operações de DNS e DHCP.  
  
-   **Suporte a vários floresta do Active Directory**.  
     Você pode usar IPAM para gerenciar os servidores DHCP e DNS de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta onde IPAM está instalado e cada uma das florestas remotas.  
  
-   **Suporte do Windows PowerShell para controle de acesso com base em função**.  
     Você pode usar o Windows PowerShell para definir os escopos de acesso em objetos IPAM.  
  
Para obter mais informações, consulte [What's New em IPAM](technologies/ipam/What-s-New-in-IPAM.md) e [gerenciar IPAM](technologies/ipam/Manage-IPAM.md).  
  

