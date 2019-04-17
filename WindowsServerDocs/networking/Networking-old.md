---
title: Rede
description: Este tópico fornece uma visão geral das tecnologias Rede definida pelo software e Plataforma de Rede, disponíveis no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 7caa99f1b6b9e25e5a6f2c4333b033fb3088195d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066832"
---
# Rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar este site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> A rede é uma parte fundamental da plataforma SDDC \(Datacenter definido pelo software\), e o Windows Server 2016 oferece tecnologias SDN \(Rede definida pelo software\) novas e melhores para ajudar você a mudar para uma solução SDDC totalmente realizada para a sua organização.

Quando você gerencia redes como um recurso definido pelo software, você pode descrever os requisitos de infraestrutura de um aplicativo uma vez e, em seguida, escolher onde o aplicativo é executado – localmente ou na nuvem. 

Essa consistência significa que seus aplicativos agora são mais fáceis de serem escalados e você pode executar perfeitamente os aplicativos, em qualquer lugar, com a mesma confiança em relação à segurança, ao desempenho, à qualidade de serviço e à disponibilidade.

>[!Note]
> Para baixar o Windows Server, consulte [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

O Windows Server 2016 adiciona as novas tecnologias de rede a seguir:

- Rede definida pelo software: o controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. O controlador de rede permite que você use a virtualização de função de rede para implantar facilmente VMs \(máquinas virtuais\) para SLB \(Balanceamento de Carga de Software\) para otimizar cargas de tráfego de rede para seus locatários e Gateways de RAS para fornecer aos locatários as opções de conectividade de que eles precisam entre recursos de nuvem, locais e Internet. Você também pode usar o controlador de rede para gerenciar o firewall do datacenter em VMs e hosts Hyper-V.

- Plataforma de rede: usando novos recursos para tecnologias de plataforma de rede existentes, você pode usar a política DNS para personalizar as respostas do seu servidor DNS a consultas, usar uma NIC convergida que manipula o tráfego Acesso Remoto Direto à Memória \(RDMA\) e Ethernet, usar o Agrupamento incorporado do comutador \(SET\) para criar comutadores virtuais Hyper-V conectados a NICs de RDMA e usar o Gerenciamento de Endereço IP \(IPAM\) para gerenciar os servidores e zonas DNS, bem como endereços IP e DHCP.

Para obter mais informações, consulte [Cenários de rede com suporte do Windows Server](windows-server-supported-networking-scenarios.md).

As seções a seguir fornecem informações sobre as tecnologias SDN e as tecnologias de Plataforma de Rede.

## Tecnologias de rede definidas pelo software

### [SDN &#40;Rede definida pelo software&#41;](sdn/software-defined-networking.md)

Você pode usar este tópico para saber mais sobre as tecnologias SDN fornecidas no Windows Server, no System Center e no Microsoft Azure.

>[!NOTE]
>Para os hosts Hyper-V e as máquinas virtuais \(VMs\) que executam servidores da infraestrutura SDN, como nós de Controlador de Rede e Balanceamento de Carga de Software, você deverá instalar o Windows Server 2016 Datacenter edition. Para hosts Hyper-V que contenham apenas VMs de carga de trabalho de locatário conectadas às redes controladas por SDN\, você poderá executar o Windows Server 2016 Standard edition.

### [Implantar uma infraestrutura de rede definida pelo software usando scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Este guia fornece instruções sobre como implantar o controlador de rede com redes virtuais e gateways em um ambiente de laboratório de teste.

### [Controlador de rede](sdn/technologies/network-controller/Network-Controller.md)

O controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter.

### [SLB &#40;Balanceamento de carga de software&#41; para SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Os Provedores de Serviço de Nuvem \(CSPs\) e empresas que estão implantando o SDN (Rede definida pelo software) no Windows Server 2016 podem usar o Balanceamento de Carga de Software \(SLB\) para distribuir uniformemente o tráfego de rede do locatário e do cliente do locatário entre recursos de rede virtual. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.

### [Gateway de RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

O Gateway de RAS, um roteador baseado em software, multilocatário, compatível com o Border Gateway Protocol \(BGP\) no Windows Server 2016, foi criado para Provedores de Serviço de Nuvem \(CSPs\) e empresas que hospedam várias redes virtuais de locatário usando a Virtualização de Rede Hyper-V. 

### [Virtualização de função de rede](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Em datacenters definidos por software, as funções de rede que estão sendo executadas por dispositivos de hardware \(como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante\) estão cada vez mais sendo virtualizadas como soluções de virtualização. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede.

### [Visão geral do firewall do datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

O firewall do datacenter é um firewall multilocatário, de camada de rede, com 5 tuplas (protocolo, números de porta de origem e de destino, endereços IP de origem e de destino) e com monitoração de estado.

## <a name="bkmk_networking"></a>Tecnologias de rede

A tabela a seguir fornece links para algumas das tecnologias de rede no Windows Server 2016.

### [Novidades na Rede](../networking/What-s-New-in-Networking.md)

Você pode usar as seções a seguir para descobrir novas tecnologias de rede e novos recursos para tecnologias existentes no Windows Server 2016.

### [BranchCache](branchcache/BranchCache.md)

O BranchCache é uma tecnologia de otimização de largura de banda de \(WAN\). Para otimizar a largura de banda de WAN quando os usuários acessam conteúdo em servidores remotos, o BranchCache busca o conteúdo dos servidores de conteúdo da matriz ou da nuvem hospedada e o armazena em cache nas filiais, permitindo que os computadores cliente das filiais acessem o conteúdo localmente e não pela WAN.

### [Guia da rede principal do Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Saiba como implantar uma rede do Windows Server com o Guia de Rede Principal, bem como adicionar recursos para a implantação de rede com Guias Complementares de Rede Principal.

### [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

O DirectAccess permite a conectividade de usuários remotos aos recursos de rede da organização. 

A documentação do DirectAccess agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016, em [Acesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obter mais informações, consulte [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### [DNS &#40;Sistema de nomes de domínio&#41;](dns/dns-top.md)

O \(DNS\) é um do pacote de protocolos padrão do setor que compõem o TCP/IP e, juntos, o cliente DNS e o servidor DNS fornecem serviços de resolução de nomes para endereço IP de computador para computadores e usuários.

### [Protocolo DHCP](technologies/dhcp/dhcp-top.md)

O protocolo \(DHCP\) é um protocolo de cliente/servidor que fornece automaticamente um host \(IP\) com seu endereço IP e outras informações de configuração relacionadas, como a máscara de sub-rede e o gateway padrão.

### [Virtualização de Rede Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

A Virtualização de Rede Hyper-V \(HNV\) permite a virtualização de redes de clientes em uma infraestrutura de rede física compartilhada.

### [Comutador Virtual Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

O Comutador Virtual Hyper-V é um comutador de rede de Ethernet de camada 2 baseado em software que está disponível no Gerenciador Hyper-V quando a função de servidor Hyper-V é instalada. O comutador inclui funcionalidades programaticamente gerenciadas e extensíveis para conectar máquinas virtuais a redes físicas e a redes virtuais. Além disso, o Comutador Virtual Hyper-V fornece imposição de política para níveis de segurança, de isolamento e de serviço. 

A documentação do Comutador Virtual do Hyper-V agora está localizada na seção **Virtualização** do sumário do Windows Server 2016. Para saber mais, consulte [Comutador Virtual do Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### [IPAM &#40;Gerenciamento de Endereço IP&#41;](technologies/ipam/ipam-top.md)

O Gerenciamento de Endereço IP \(IPAM\) é um pacote integrado de ferramentas que permitem o planejamento, a implantação, o gerenciamento e o monitoramento de ponta a ponta de sua infraestrutura de endereços IP, com uma rica experiência de usuário. O IPAM descobre automaticamente os servidores de infraestrutura de endereços IP e servidores \(DNS\) na sua rede e permite que você os gerencie com base em uma interface central.

### [Balanceamento de Carga de Rede](technologies/Network-Load-Balancing.md)

O Balanceamento de Carga de Rede \(NLB\) distribui o tráfego entre vários servidores usando o protocolo de rede TCP/IP. Para implantações não SDN, o NLB garante que aplicativos sem monitoração de estado, como servidores Web que executam Serviços de Informações da Internet \(IIS\),, são escalonáveis, adicionando mais servidores conforme a carga aumenta.

### [Rede de Alto Desempenho](technologies/hpn/hpn-top.md)

O descarregamento e a otimização de tecnologias de rede no Windows Server 2016 incluem recursos e tecnologias Somente Software (SO), recursos e tecnologias integrados de Software e Hardware (SH) e recursos e tecnologias Somente Hardware (HO).

A documentação de tecnologia de descarregamento e otimização a seguir também está disponível.

- [Guia de Configuração de Adaptador de Rede (NIC) Convergido](technologies/conv-nic/cnic-top.md)
- [DCB (Data Center Bridging)](technologies/dcb/dcb-top.md)
- [Virtual Receive Side Scaling (vRSS)](technologies/vrss/vrss-top.md)


### [Servidor de Políticas de Rede](technologies/nps/nps-top.md)

O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão.

### [Shell de Rede (Netsh)](technologies/netsh/netsh.md)

Você pode usar o utilitário de rede Shell de Rede \(netsh\) para gerenciar as tecnologias de rede no Windows Server 2016 e no Windows 10.

### [Ajuste de desempenho do subsistema de rede](technologies/network-subsystem/net-sub-performance-top.md)

Este tópico fornece informações sobre como escolher o adaptador de rede certo para a carga de trabalho do seu servidor, como ordenar os adaptadores de rede, os contadores de desempenho relacionados à rede e os adaptadores de rede de ajuste de desempenho e tecnologias de rede relacionadas, como o Receive Side Scaling \(RSS\) e o Receive Side Coalescing \(RSC\), entre outros.

### [Agrupamento NIC](technologies/nic-teaming/NIC-Teaming.md)

O Agrupamento NIC permite que você agrupe adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.

### [Política de Qualidade de Serviço (QoS)](technologies/qos/qos-policy-top.md)

Você pode usar a Política de QoS como um ponto central de gerenciamento de largura de banda de rede em toda sua infraestrutura do Active Directory por meio da criação de perfis de QoS, cujas configurações são distribuídas com a Política de Grupo.

### [Acesso remoto](../remote/remote-access/remote-access.md)

Você pode usar tecnologias de Acesso Remoto, como o DirectAccess e a Rede Privada Virtual \(VPN\) para fornecer a trabalhadores remotos a conectividade aos recursos de rede internos. Além disso, você pode usar o Acesso Remoto para roteamento de \(LAN\) e para o Proxy de Aplicativo Web. que fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa.

A documentação do Acesso Remoto agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016. Para obter mais informações, consulte [Acesso Remoto](../remote/remote-access/remote-access.md).

Para obter mais informações sobre o Proxy de Aplicativo Web, que é um serviço da função de servidor de Acesso Remoto, consulte [Proxy de aplicativo Web no Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### [Rede Virtual Privada (VPN)](../remote/remote-access/vpn/vpn-top.md)

No Windows Server 2016, **DirectAccess e VPN** é um serviço da função de servidor **Acesso Remoto**.

Quando você instala o Acesso Remoto como um servidor VPN, pode usar a \(VPN\) para fornecer aos seus funcionários remotos conexões à rede da organização por meio da Internet - mantendo também a privacidade de informações com conexões criptografadas.

Com a VPN de Acesso Remoto do Windows Server 2016 — e com computadores cliente Windows 10 - agora você pode implantar uma VPN Always On. A VPN Always On oferece a capacidade de gerenciar clientes VPN remotos que sempre estão conectados, fornecendo também conveniência para os funcionários remotos, que não precisam mais se conectar e desconectar manualmente da VPN à rede da organização.

Para obter mais informações, consulte [Guia de implantação de VPN Always On de Acesso Remoto para o Windows Server 2016 e o Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>A documentação da VPN agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016, em [Acesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obter mais informações sobre VPN, consulte [Rede Virtual Privada (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### [Rede de Contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

A Rede de Contêineres do Windows permite que você crie e gerencie redes para a conexão de pontos de extremidade de contêiner em hosts do Windows 10 e do Windows Server usando fluxos de trabalho e ferramentas padrão do setor. As redes de contêineres do Windows dão suporte a várias topologias, incluindo privada, L2 plana e L3 roteada.

As sobreposições também têm suporte que você pode criar localmente no host usando o Docker, o Kubernetes ou o Windows PowerShell por meio de plug-ins que se comunicam com o Windows Host Networking Service \(HNS\). Você pode criar e gerenciar redes de cluster de vários\-nós por meio de sistemas de coordenação de nível superior por meio da comunicação por um agente local para o HNS de cada nó.

### [Serviço de Cadastramento na Internet do Windows (WINS)](technologies/wins/wins-top.md)

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP. O uso do DNS é recomendável em vez do WINS.

## Recursos adicionais

Os recursos de rede para sistemas operacionais anteriores ao Windows Server 2016 estão disponíveis nos seguintes locais.

- [Visão geral da rede](https://technet.microsoft.com/library/hh831357.aspx) do Windows Server 2012 e do Windows Server 2012 R2
- [Rede](https://technet.microsoft.com/library/cc753940) do Windows Server 2008 e do Windows Server 2008 R2
- Windows Server 2003 [Conteúdo desativado do Windows Server 2003/2003 R2 ](https://www.microsoft.com/download/details.aspx?id=53314)
