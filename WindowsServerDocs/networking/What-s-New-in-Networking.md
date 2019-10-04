---
title: Novidades na rede
description: Este tópico fornece informações de visão geral sobre os novos recursos e as tecnologias de rede no Windows Server 2016
ms.prod: windows-server
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da2166d28edda5662797824d9b26ad930f51083c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406752"
---
# <a name="whats-new-in-networking"></a>Novidades na rede

>Aplica-se a: Windows Server 2016

A seguir estão as tecnologias de rede novas ou aprimoradas no Windows Server 2016.  
  UPD este tópico contém as seções a seguir.  
  
-   [Novos recursos e tecnologias de rede](#bkmk_features)  
  
-   [Novos recursos para tecnologias de rede adicionais](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Novos recursos e tecnologias de rede

A rede é uma parte fundamental da plataforma do Data Center (SDDC) definida pelo software, e o Windows Server 2016 fornece tecnologias de SDN (rede definida pelo software) novas e aprimoradas para ajudá-lo a migrar para uma solução SDDC totalmente realizada para sua organização.  
  
Ao gerenciar redes como um recurso de software definido, você pode descrever os requisitos de infraestrutura de um aplicativo uma vez e, em seguida, escolher onde o aplicativo é executado no local ou na nuvem.  Essa consistência significa que agora os aplicativos são mais fáceis de dimensionar e você pode executar aplicativos de forma simples, em qualquer lugar, com confiança igual em relação à segurança, ao desempenho, à qualidade do serviço e à disponibilidade.  
  
As seções a seguir contêm informações sobre esses novos recursos e tecnologias de rede.  
  
### <a name="software-defined-networking-infrastructure"></a>Infraestrutura de rede definida pelo software

A seguir estão as tecnologias de infraestrutura de SDN novas ou aprimoradas.  
  
-   **Controlador de rede**. Novidade no Windows Server 2016, o controlador de rede fornece um ponto de automação centralizado e programável para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. Usando o Controlador de Rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos e dos serviços de rede. Para obter mais informações, consulte [controlador de rede](sdn/technologies/network-controller/Network-Controller.md) e [implantar redes definidas por software usando scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Comutador virtual do Hyper-V**. O comutador virtual do Hyper-V é executado em hosts Hyper-V e permite que você crie comutação distribuída e roteamento e uma camada de imposição de política que esteja alinhada e compatível com Microsoft Azure. Para saber mais, consulte [Comutador Virtual do Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **NFV (virtualização de função de rede)** . Nos data centers definidos pelo software de hoje, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores etc.) são cada vez mais implantadas como dispositivos virtuais. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Os dispositivos virtuais estão surgindo rapidamente e criando um mercado totalmente novo. Eles continuam gerando interesse e conquistam impulso nas plataformas de virtualização e nos serviços de nuvem. As tecnologias NFV a seguir estão disponíveis no Windows Server 2016.  
  
    -   **Firewall do datacenter**. Esse firewall distribuído fornece ACLs (listas de controle de acesso) granulares, permitindo que você aplique políticas de firewall no nível da interface da VM ou no nível da sub-rede.  
  
        Para obter mais informações, consulte [visão geral do firewall do datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Gateway de Ras**. Você pode usar o gateway de RAS para roteamento de tráfego entre redes virtuais e redes físicas, incluindo conexões VPN site a site de seu datacenter de nuvem para sites remotos de seus locatários. Especificamente, você pode implantar protocolo IKE a IKEv2 (redes virtuais privadas) site a site (VPNs), VPN de camada 3 (L3) e gateways de túnel de roteamento genérico (GRE). Além disso, agora há suporte para pools de gateway e redundância M + N de gateways; e Border Gateway Protocol (BGP) com recursos de refletor de rota fornecem roteamento dinâmico entre redes para todos os cenários de gateway (VPN IKEv2, VPN GRE e VPN L3).  
  
        Para obter mais informações, consulte [What ' s New in RAS gateway](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) and [RAS gateway for Sdn](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Load Balancer de software (SLB) e conversão de endereços de rede (NAT)** . O balanceador de carga da camada 4-Sul e leste-oeste e o NAT aprimoram a taxa de transferência ao dar suporte ao retorno direto de servidor, com o qual o tráfego de rede de retorno pode ignorar o multiplexador de balanceamento de carga.  
       Para obter mais informações, consulte [ &#40;SLB&#41; de balanceamento de carga de software para Sdn](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Para obter mais informações, consulte [virtualização de função de rede](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Protocolos padronizados**. O controlador de rede usa a transferência de estado de reapresentação (REST) em sua interface Northbound com cargas JavaScript Object Notation (JSON). A interface Southbound do controlador de rede usa o protocolo de gerenciamento de banco de dados vSwitch (OVSDB) aberto.  
  
-   **Tecnologias de encapsulamento flexíveis**. Essas tecnologias operam no plano de dados e dão suporte tanto ao VxLAN (virtual Extensible LAN) quanto ao NVGRE (encapsulamento de roteamento genérico de virtualização de rede). Para obter mais informações, consulte [túnel GRE no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Para obter mais informações sobre SDN, consulte [rede &#40;definida pelo&#41;software Sdn](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Conceitos básicos de escala de nuvem
 
Os conceitos básicos de escala de nuvem a seguir agora estão disponíveis.  
  
-   **NIC (placa de interface de rede) convergida**. A NIC convergida permite que você use um único adaptador de rede para gerenciamento, armazenamento habilitado para RDMA (acesso remoto direto à memória) e tráfego de locatário. Isso reduz as despesas de capital associadas a cada servidor em seu datacenter, pois você precisa de menos adaptadores de rede para gerenciar diferentes tipos de tráfego por servidor.  
  
-   **Pacote direto**.  O pacote direto fornece uma alta taxa de transferência de tráfego de rede e uma infra-estrutura de processamento de pacotes de baixa latência.  
  
-   **Alterne a equipe inserida (Set)** .        O conjunto é uma solução de agrupamento NIC integrada ao comutador virtual Hyper-V. SET permite o agrupamento de até oito NICS físicas em uma única equipe de conjunto, o que melhora a disponibilidade e fornece failover. No Windows Server 2016, você pode criar equipes definidas que são restritas ao uso de protocolo SMB e RDMA. Além disso, você pode usar o SET Teams para distribuir o tráfego de rede para virtualização de rede Hyper-V. Para obter mais informações,consulte [Acesso remoto direto à memória &#40;Access&#41; RDMA e troca de equipe inserida &#40;Integration&#41;Set](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Novos recursos para tecnologias de rede adicionais

Esta seção contém informações sobre os novos recursos para tecnologias de rede familiares.
  
## <a name="bkmk_dhcp"></a>DHCP  
O DHCP é um padrão IETF (Internet Engineering Task Force) projetado para reduzir a carga administrativa e a complexidade da configuração de hosts em uma rede baseada em TCP/IP, por exemplo, uma intranet privada. Usando o serviço de servidor DHCP, o processo de configuração de TCP/IP em clientes DHCP é automático.  
  
Para obter mais informações, consulte [What ' s New in DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS é um sistema usado em redes TCP/IP para nomear computadores e serviços de rede. A nomeação DNS localiza computadores e serviços por meio de nomes simples. Quando um usuário insere um nome DNS em um aplicativo, os serviços DNS podem resolvê-lo para outra informação associada a ele, como um endereço IP.  
  
Veja a seguir informações sobre o cliente DNS e o servidor DNS.  
  
### <a name="bkmk_dnsc"></a>Cliente DNS  
A seguir estão as tecnologias de cliente DNS novas ou aprimoradas.  
  
-   **Associação de serviço do cliente DNS**. No Windows 10, o serviço cliente DNS oferece suporte aprimorado para computadores com mais de uma interface de rede.  
  
Para obter mais informações, consulte [o que há de novo no cliente DNS no Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Servidor DNS  
A seguir estão as tecnologias de servidor DNS novas ou aprimoradas.  
  
-   **Políticas de DNS**.  Você pode configurar as políticas de DNS para especificar como um servidor DNS responde a consultas DNS. As respostas DNS podem ser baseadas no endereço IP do cliente (local), na hora do dia e em vários outros parâmetros. As políticas de DNS habilitam o DNS com reconhecimento de local, o gerenciamento de tráfego, o balanceamento de carga, o DNS de divisão e outros cenários.  
  
-   **Suporte do nano Server para DNS baseado em arquivo**, você pode implantar o servidor DNS no Windows Server 2016 em uma imagem do nano Server. Essa opção de implantação estará disponível se você estiver usando DNS baseado em arquivo. Ao executar o servidor DNS em uma imagem do nano Server, você pode executar seus servidores DNS com espaço reduzido, inicialização rápida e aplicação de patch minimizada.  
  
    > [!NOTE]   
    > Não há suporte para o DNS integrado Active Directory no nano Server.  
  
-   **Limitação de taxa de resposta (RRL)** .  Você pode habilitar a limitação da taxa de resposta em seus servidores DNS. Ao fazer isso, você evita a possibilidade de sistemas mal-intencionados que usam seus servidores DNS para iniciar um ataque de negação de serviço em um cliente DNS.  
  
-   **Autenticação baseada em DNS de entidades nomeadas (sundanês)** .   Você pode usar os registros TLSA (autenticação de segurança de camada de transporte) para fornecer informações aos clientes DNS que afirmam qual autoridade de certificação (CA) eles devem esperar um certificado para seu nome de domínio. Isso impede ataques man-in-the-Middle, em que alguém pode corromper o cache DNS para apontar para seu próprio site e fornecer um certificado emitido por uma autoridade de certificação diferente.  
  
-   **Suporte a registro desconhecido**.   
     Você pode adicionar registros que não são explicitamente suportados pelo servidor DNS do Windows usando a funcionalidade de registro desconhecida.  
  
-   **Dicas de raiz IPv6**.   
     Você pode usar o suporte de dicas de raiz IPV6 nativo para executar a resolução de nomes da Internet usando os servidores raiz IPV6.  
  
-   **Suporte aprimorado do Windows PowerShell**.   
      Novos cmdlets do Windows PowerShell estão disponíveis para o servidor DNS.  
  
Para obter mais informações, consulte [o que há de novo no servidor DNS no Windows server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Túnel GRE  
O gateway de RAS agora dá suporte a túneis GRE (encapsulamento de roteamento genérico) de alta disponibilidade para conexões site a site e a redundância M + N de gateways. GRE é um protocolo de túnel leve que pode encapsular uma ampla variedade de protocolos de camada de rede em links de ponto a ponto virtuais por meio de uma ligação entre redes de protocolo de Internet.  
  
Para obter mais informações, consulte [túnel GRE no Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualização de rede Hyper-V  
Introduzido no Windows Server 2012, a HNV (virtualização de rede do Hyper-V) permite a virtualização de redes de clientes sobre uma infraestrutura de rede física compartilhada. Com as alterações mínimas necessárias na malha de rede física, o HNV dá aos provedores de serviços a agilidade para implantar e migrar cargas de trabalho de locatário em qualquer lugar nas três nuvens: a nuvem do provedor de serviços, a nuvem privada ou a nuvem pública Microsoft Azure.  
  
Para obter mais informações, consulte [o que há de novo na virtualização de rede Hyper-V no Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
O IPAM fornece recursos de monitoramento e administração altamente personalizáveis para o endereço IP e a infraestrutura de DNS em uma rede da organização. Usando o IPAM, você pode monitorar, auditar e gerenciar servidores que estão executando o protocolo DHCP e o DNS (sistema de nomes de domínio).  
  
-   **Gerenciamento de endereços IP aprimorado**.  
     Os recursos do IPAM são aprimorados para cenários como tratar sub-redes IPv4/32 e IPv6/128 e localizar sub-redes de endereço IP e intervalos livres em um bloco de endereço IP.  
  
-   **Gerenciamento de serviço DNS avançado**.  
     O IPAM dá suporte ao registro de recurso DNS, encaminhador condicional e gerenciamento de zona DNS para servidores DNS integrados ao domínio Active Directory e com backup em arquivo.  
  
-   **DNS integrado, DHCP e gerenciamento de endereço IP (DDI)** .  
     Várias experiências novas e operações de gerenciamento de ciclo de vida integradas são habilitadas, como visualização de todos os registros de recursos DNS que pertencem a um endereço IP, inventário automatizado de endereços IP com base em registros de recursos DNS e gerenciamento de ciclo de vida de endereço IP para operações de DNS e DHCP.  
  
-   **Suporte a florestas múltiplas Active Directory**.  
     Você pode usar o IPAM para gerenciar os servidores DNS e DHCP de várias florestas do Active Directory quando há uma relação de confiança bidirecional entre a floresta em que o IPAM está instalado e cada uma das florestas remotas.  
  
-   **Suporte do Windows PowerShell para controle de acesso baseado em função**.  
     Você pode usar o Windows PowerShell para definir escopos de acesso em objetos IPAM.  
  
Para obter mais informações, consulte [o que há de novo no IPAM](technologies/ipam/What-s-New-in-IPAM.md) e [gerenciar o IPAM](technologies/ipam/Manage-IPAM.md).  
  

