---
title: Rede
description: Este tópico fornece uma visão geral das tecnologias Rede definida pelo software e Plataforma de Rede, disponíveis no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: anpaul
author: AnirbanPaul
ms.localizationpriority: medium
ms.openlocfilehash: 052e81125f95bba6e6bdd76279dec6b23b759cbf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861769"
---
# <a name="networking"></a>Rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

       [!TIP]
        Looking for information about older versions of Windows Server? Check out our other [Windows Server libraries](/previous-versions/windows/) on docs.microsoft.com. You can also [search this site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) for specific information.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> A rede é uma parte fundamental do datacenter definido pelo software \(a plataforma\) do SDDC, e o Windows Server 2016 fornece uma nova e aprimorada rede definida pelo software \(as tecnologias SDN\) para ajudá-lo a migrar para uma solução SDDC totalmente realizada para sua organização.

Ao gerenciar redes como um recurso de software definido, você pode descrever os requisitos de infraestrutura de um aplicativo uma vez e, em seguida, escolher onde o aplicativo é executado no local ou na nuvem. 

Essa consistência significa que seus aplicativos agora são mais fáceis de serem escalados e você pode executar perfeitamente os aplicativos, em qualquer lugar, com a mesma confiança em relação à segurança, ao desempenho, à qualidade de serviço e à disponibilidade.

>[!Note]
> Para baixar o Windows Server, consulte [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

O Windows Server 2016 adiciona as novas tecnologias de rede a seguir:

- Rede definida pelo software: o controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. O controlador de rede permite que você use a virtualização de função de rede para implantar facilmente máquinas virtuais \(VMs\) para o balanceamento de carga de software \(SLB\) para otimizar as cargas de tráfego de rede para seus locatários e gateways de RAS para fornecer aos locatários as opções de conectividade necessárias entre a Internet, o local e os recursos de nuvem. Você também pode usar o controlador de rede para gerenciar o firewall do datacenter em VMs e hosts Hyper-V.

- Plataforma de rede: usando novos recursos para tecnologias de plataforma de rede existentes, você pode usar a política DNS para personalizar as respostas do servidor DNS para consultas, usar uma NIC convergida que lida com acesso de memória direta remota combinado \(RDMA\) e tráfego Ethernet, use o switch Embedded Integration \(definir\) para criar comutadores virtuais Hyper-V conectados a NICs RDMA e usar o gerenciamento de endereço IP \(IPAM\) para gerenciar zonas e servidores DNS, bem como endereços IP e

Para obter mais informações, consulte [Cenários de rede com suporte do Windows Server](windows-server-supported-networking-scenarios.md).

As seções a seguir fornecem informações sobre as tecnologias SDN e as tecnologias de Plataforma de Rede.

## <a name="software-defined-networking-technologies"></a>Tecnologias de rede definidas pelo software

### <a name="software-defined-networking-40sdn41"></a>[Sdn de rede &#40;definida pelo software&#41;](sdn/software-defined-networking.md)

Você pode usar este tópico para saber mais sobre as tecnologias SDN fornecidas no Windows Server, no System Center e no Microsoft Azure.

>[!NOTE]
>Para hosts Hyper-V e máquinas virtuais \(VMs\) que executam servidores de infraestrutura SDN, como nós de controlador de rede e de balanceamento de carga de software, você deve instalar o Windows Server 2016 Datacenter Edition. Para hosts Hyper-V que contêm somente VMs de carga de trabalho de locatário que estão conectadas a SDN\-redes controladas, você pode executar o Windows Server 2016 Standard Edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>[Implantar uma infraestrutura de rede definida pelo software usando scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Este guia fornece instruções sobre como implantar o controlador de rede com redes virtuais e gateways em um ambiente de laboratório de teste.

### <a name="network-controller"></a>[Controlador de rede](sdn/technologies/network-controller/Network-Controller.md)

O controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter.

### <a name="software-load-balancing-40slb41-for-sdn"></a>[SLB&#41; de balanceamento &#40;de carga de software para Sdn](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Provedores de serviço de nuvem \(CSPs\) e as empresas que estão implantando SDN (rede definida pelo software) no Windows Server 2016 podem usar o balanceamento de carga de software \(SLB\) para distribuir uniformemente o tráfego de rede do locatário e do cliente de locatário entre os recursos de rede virtual. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.

### <a name="ras-gateway-for-sdn"></a>[Gateway de RAS para SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

O gateway de RAS, que é um roteador com capacidade de multilocatário, baseado em software Border Gateway Protocol \(BGP\) no Windows Server 2016, foi projetado para provedores de serviços de nuvem \(CSPs\) e empresas que hospedam várias redes virtuais de locatário usando a virtualização de rede Hyper-V. 

### <a name="network-function-virtualization"></a>[Virtualização de função de rede](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Em data centers definidos por software, as funções de rede que estão sendo executadas por dispositivos de hardware \(como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante\) são cada vez mais virtualizadas como dispositivos virtuais. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede.

### <a name="datacenter-firewall-overview"></a>[Visão geral do firewall do datacenter](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

O firewall do datacenter é um firewall multilocatário, de camada de rede, com 5 tuplas (protocolo, números de porta de origem e de destino, endereços IP de origem e de destino) e com monitoração de estado.

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Tecnologias de rede

A tabela a seguir fornece links para algumas das tecnologias de rede no Windows Server 2016.

### <a name="whats-new-in-networking"></a>[O que há de novo na rede](../networking/What-s-New-in-Networking.md)

Você pode usar as seções a seguir para descobrir novas tecnologias de rede e novos recursos para tecnologias existentes no Windows Server 2016.

### <a name="branchcache"></a>[BranchCache](branchcache/BranchCache.md)

O BranchCache é uma rede de longa distância \(tecnologia de otimização de largura de banda\) WAN. Para otimizar a largura de banda de WAN quando os usuários acessam conteúdo em servidores remotos, o BranchCache busca o conteúdo dos servidores de conteúdo da matriz ou da nuvem hospedada e o armazena em cache nas filiais, permitindo que os computadores cliente das filiais acessem o conteúdo localmente e não pela WAN.

### <a name="core-network-guide-for-windows-server-2016"></a>[Guia de rede principal do Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Saiba como implantar uma rede do Windows Server com o Guia de Rede Principal, bem como adicionar recursos para a implantação de rede com Guias Complementares de Rede Principal.

### <a name="directaccess"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

O DirectAccess permite a conectividade de usuários remotos aos recursos de rede da organização. 

A documentação do DirectAccess agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016, em [Acesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Para obter mais informações, consulte [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41"></a>[DNS do sistema &#40;de nomes de domínio&#41;](dns/dns-top.md)

O DNS \(Sistema de nomes de domínio\) é um do pacote de protocolos padrão do setor que compõem o TCP/IP e, juntos, o cliente DNS e o servidor DNS fornecem nomes do computador para endereço IP que mapeiam os serviços de resolução de nomes para computadores e usuários.

### <a name="dynamic-host-configuration-protocol-40dhcp41"></a>[DHCP do protocolo &#40;de configuração de host dinâmico&#41;](technologies/dhcp/dhcp-top.md)

O protocolo DHCP\(\) é um protocolo de cliente/servidor que fornece automaticamente um host IP \(protocolo de Internet\) com seu endereço IP e outras informações de configuração relacionadas, como a máscara de sub-rede e o gateway padrão.

### <a name="hyper-v-network-virtualization"></a>[Virtualização de rede Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

A virtualização de rede Hyper-V \(HNV\) permite a virtualização de redes de clientes sobre uma infraestrutura de rede física compartilhada. "" "

### <a name="hyper-v-virtual-switch"></a>[Comutador Virtual do Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

O Comutador Virtual Hyper-V é um comutador de rede de Ethernet de camada 2 baseado em software que está disponível no Gerenciador Hyper-V quando a função de servidor Hyper-V é instalada. O comutador inclui funcionalidades programaticamente gerenciadas e extensíveis para conectar máquinas virtuais a redes físicas e a redes virtuais. Além disso, o Comutador Virtual Hyper-V fornece imposição de política para níveis de segurança, de isolamento e de serviço. 

A documentação do Comutador Virtual do Hyper-V agora está localizada na seção **Virtualização** do sumário do Windows Server 2016. Para saber mais, consulte [Comutador Virtual do Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41"></a>[IPAM de gerenciamento &#40;de endereço IP&#41;](technologies/ipam/ipam-top.md)

O IPAM \(Gerenciamento de Endereço IP\) é um pacote integrado de ferramentas que permitem o planejamento, a implantação, o gerenciamento e o monitoramento de ponta a ponta de sua infraestrutura de endereços IP, com uma rica experiência de usuário. O IPAM descobre automaticamente os servidores de infraestrutura de endereço IP e o sistema de nomes de domínio \(servidores DNS\) em sua rede e permite que você os gerencie a partir de uma interface central.

### <a name="network-load-balancing"></a>[Balanceamento de Carga de Rede](technologies/Network-Load-Balancing.md)

O NLB \(Balanceamento de Carga de Rede\) distribui o tráfego entre vários servidores usando o protocolo de rede TCP/IP. Para implantações não SDN, o NLB garante que aplicativos sem monitoração de estado, como servidores Web que executam IIS \(Serviços de Informações da Internet \), são escalonáveis, adicionando mais servidores conforme a carga aumenta.

### <a name="high-performance-networking"></a>[Rede de alto desempenho](technologies/hpn/hpn-top.md)

O descarregamento e a otimização de tecnologias de rede no Windows Server 2016 incluem recursos e tecnologias Somente Software (SO), recursos e tecnologias integrados de Software e Hardware (SH) e recursos e tecnologias Somente Hardware (HO).

A documentação de tecnologia de descarregamento e otimização a seguir também está disponível.

- [Guia de configuração da NIC (placa de interface de rede) convergido](technologies/conv-nic/cnic-top.md)
- [Ponte do Data Center (DCB)](technologies/dcb/dcb-top.md)
- [vRSS (Virtual Receive Side Scaling)](technologies/vrss/vrss-top.md)


### <a name="network-policy-server"></a>[Servidor de políticas de rede](technologies/nps/nps-top.md)

O Servidor de Políticas de Rede (NPS) permite que você crie e aplique políticas de acesso de rede em toda a organização para autenticação e autorização de solicitações de conexão.

### <a name="network-shell-netsh"></a>[Shell de Rede (Netsh)](technologies/netsh/netsh.md)

Você pode usar o Shell de rede \(o utilitário de rede netsh\) para gerenciar as tecnologias de rede no Windows Server 2016 e no Windows 10.

### <a name="network-subsystem-performance-tuning"></a>[Ajuste de desempenho do subsistema de rede](technologies/network-subsystem/net-sub-performance-top.md)

Este tópico fornece informações sobre como escolher o adaptador de rede correto para a carga de trabalho do servidor, ordenar os adaptadores de rede, os contadores de desempenho relacionados à rede e as tecnologias de rede de ajuste de desempenho, como receber o dimensionamento lateral \(RSS\), receber a União \(RSC\)e outros.

### <a name="nic-teaming"></a>[Agrupamento NIC](technologies/nic-teaming/NIC-Teaming.md)

O Agrupamento NIC permite que você agrupe adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.

### <a name="quality-of-service-qos-policy"></a>[Política de QoS (qualidade de serviço)](technologies/qos/qos-policy-top.md)

Você pode usar a Política de QoS como um ponto central de gerenciamento de largura de banda de rede em toda sua infraestrutura do Active Directory por meio da criação de perfis de QoS, cujas configurações são distribuídas com a Política de Grupo.

### <a name="remote-access"></a>[Acesso remoto](../remote/remote-access/remote-access.md)

Você pode usar tecnologias de acesso remoto, como o DirectAccess e a rede virtual privada \(VPN\) para fornecer aos funcionários remotos conectividade aos recursos internos da rede. Além disso, você pode usar o acesso remoto para rede local \(roteamento de\) LAN e para o proxy de aplicativo Web. que fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa.

A documentação do Acesso Remoto agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016. Para obter mais informações, consulte [Acesso Remoto](../remote/remote-access/remote-access.md).

Para obter mais informações sobre o Proxy de Aplicativo Web, que é um serviço da função de servidor de Acesso Remoto, consulte [Proxy de aplicativo Web no Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpn"></a>[VPN (rede virtual privada)](../remote/remote-access/vpn/vpn-top.md)

No Windows Server 2016, **DirectAccess e VPN** é um serviço da função de servidor **Acesso Remoto**.

Ao instalar o acesso remoto como um servidor VPN, você pode usar a rede privada virtual \(\) VPN para fornecer aos seus funcionários remotos conexões à rede da sua organização pela Internet, mantendo também a privacidade das informações com conexões criptografadas.

Com a VPN de Acesso Remoto do Windows Server 2016 — e com computadores cliente Windows 10 - agora você pode implantar uma VPN Always On. A VPN Always On oferece a capacidade de gerenciar clientes VPN remotos que sempre estão conectados, fornecendo também conveniência para os funcionários remotos, que não precisam mais se conectar e desconectar manualmente da VPN à rede da organização.

Para obter mais informações, consulte [Guia de implantação de VPN Always On de Acesso Remoto para o Windows Server 2016 e o Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>A documentação da VPN agora está localizada na seção [Gerenciamento de acesso remoto e de servidor](https://docs.microsoft.com/windows-server/remote/) do sumário do Windows Server 2016, em [Acesso remoto](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Para obter mais informações sobre VPN, consulte [Rede Virtual Privada (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networking"></a>[Rede de contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

A Rede de Contêineres do Windows permite que você crie e gerencie redes para a conexão de pontos de extremidade de contêiner em hosts do Windows 10 e do Windows Server usando fluxos de trabalho e ferramentas padrão do setor. As redes de contêineres do Windows dão suporte a várias topologias, incluindo privada, L2 plana e L3 roteada.

Também há suporte para sobreposições que você pode criar localmente no host usando o Docker, o kubernetes ou o Windows PowerShell por meio de plug-ins que se comunicam com o serviço de rede de host do Windows \(\)de falha. Você pode criar e gerenciar várias redes de cluster de nó de\-por meio de sistemas de orquestração de nível superior comunicando-se por meio de um agente local para o HNS de cada nó.

### <a name="windows-internet-name-service-wins"></a>[WINS (Serviço de Cadastramento na Internet do Windows)](technologies/wins/wins-top.md)

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP. O uso do DNS é recomendável em vez do WINS.

## <a name="additional-resources"></a>Recursos adicionais

Os recursos de rede para sistemas operacionais anteriores ao Windows Server 2016 estão disponíveis nos seguintes locais.

- [Visão geral da rede](https://technet.microsoft.com/library/hh831357.aspx) do Windows Server 2012 e do Windows Server 2012 R2
- [Rede](https://technet.microsoft.com/library/cc753940) do Windows Server 2008 e do Windows Server 2008 R2
- Conteúdo desativado do Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
