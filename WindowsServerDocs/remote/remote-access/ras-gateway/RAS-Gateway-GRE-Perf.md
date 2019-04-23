---
title: Desempenho e produtividade de túnel GRE de Gateway RAS
description: Este tópico, que é destinado a profissionais de tecnologia da informação (TI), fornece informações de desempenho de taxa de transferência sobre túneis RAS Gateway Generic Routing Encapsulation (GRE).
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880757"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Desempenho e produtividade de túnel GRE de Gateway RAS

>Aplica-se a: Windows Server \(canal semestral\)

Você pode usar este tópico para saber mais sobre o servidor de acesso remoto \(RAS\) Gateway de encapsulamento de roteamento genérico \(GRE\) desempenho no Windows Server, versão 1709, em uma não - rede definida pelo Software detúnel\( SDN\) teste baseado no ambiente.

Gateway de RAS é um roteador de software e o gateway que você pode usar no modo de locatário único ou modo multilocatário. Este tópico discute um modo de locatário único, a configuração de alta disponibilidade com o Clustering de Failover. As estatísticas de desempenho de túnel GRE que são apresentadas neste tópico são válidas para o Gateway de RAS em modos de multilocatários e de locatário singele.

>[!NOTE]
>Clustering de failover é um recurso do Windows Server que permite a você agrupar vários servidores em um cluster e tolerante a falhas. Para obter mais informações, consulte [Clustering de Failover](../../../failover-clustering/failover-clustering-overview.md)

Modo de locatário único permite que organizações de qualquer tamanho para implantar o gateway como um exterior ou da Internet\-voltado para a rede virtual privada da borda \(VPN\) server. No modo de locatário único, você pode implantar o Gateway de RAS em um servidor físico ou máquina virtual \(VM\). Este tópico descreve a implantação do Gateway de RAS em duas máquinas virtuais \(VMs\) que são configurados em um cluster de failover.

>[!IMPORTANT]
>Como túneis GRE fornecem o encapsulamento, mas não a criptografia, você não deve usar o Gateway de RAS configurado com GRE como um gateway de borda de Internet. Para saber mais sobre os principais usos para o Gateway de RAS com túneis GRE, consulte [túnel GRE no Windows Server](gre-tunneling-windows-server.md).

GRE é uma leve que pode encapsular uma ampla variedade de protocolos de camada de rede virtual ponto dentro de protocolo de encapsulamento\-para\-aponte links através de uma interconexão de protocolo de Internet. A implementação da Microsoft GRE encapsula IPv4 e IPv6.

Para obter mais informações, consulte a seção **cenários de implantação do Gateway RAS** no tópico [Gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

Nesse cenário de teste, que é abordado na ilustração a seguir, o fluxo de tráfego é medido move de organização Intranet 2-out a 1 de Intranet da organização. VMs de carga de trabalho de locatário enviar o tráfego de rede da Intranet 2 para 1 de Intranet por meio do Gateway RAS.

![Visão geral do cenário de teste de túnel GRE de Gateway de RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configuração do ambiente de teste

Esta seção fornece informações sobre o ambiente de teste e a configuração de Gateway de RAS.

O ambiente de teste, as VMs de Gateway de RAS são implantadas em Hyper\-hosts V em um failover de cluster de alta disponibilidade.

### <a name="hyper-v-host-configuration"></a>Hyper\-V configuração do Host

Dois Hyper\-hosts V são configurados para suportar o cenário de teste da seguinte maneira. 

- Dois duplos\-hospedados, computadores físicos são configurados com o Windows Server, versão 1709
- Os dois adaptadores de rede física em cada um dos dois servidores estão conectados a sub-redes diferentes - que representam as sub-redes de uma intranet da organização. Redes e suporte de hardware tem uma capacidade de 10 GBps.
- Hyperthreading nos servidores físicos está desabilitado. Isso fornece a taxa de transferência máxima das placas de rede físicas.
- O Hyper\-função de servidor V está instalada em ambos os servidores e configurada com dois externo Hyper\-comutadores virtuais V, um para cada adaptador de rede física.
- Como ambos os servidores estão conectados à mesma intranet, os servidores podem se comunicar entre si.
- O Hyper\-hosts V são configurados em um cluster de failover na rede da intranet. 

>[!NOTE]
>Para saber mais, consulte [Comutador Virtual do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configuração da VM

Duas VMs são configuradas para suportar o cenário de teste da seguinte maneira.

- Em cada servidor de que uma VM é instalada, que é executando o Windows Server, versão 1709. Cada VM é configurado com 10 núcleos e 8 GB de RAM.
- Cada VM também é configurado com dois adaptadores de rede virtual. Um adaptador de rede virtual está conectado ao comutador virtual 1 de Intranet e o outro adaptador de rede virtual está conectado ao comutador virtual de 2 de Intranet.
- Cada VM tem um Gateway de RAS instalado e configurado como um GRE\-com base em servidor VPN.
- As VMs de gateway são configuradas em um cluster de failover. Quando o cluster, uma máquina virtual está ativa e a outra VM é passiva.

### <a name="workload-hyper-v-hosts-and-vms"></a>Carga de trabalho Hyper\-V Hosts e VMs

Para este teste, duas cargas de trabalho Hyper\-hosts V estão instalados na intranet, e cada host tem uma VM instalada. Se você está duplicando este teste em seu próprio ambiente de teste, você pode instalar quantos servidores de carga de trabalho e máquinas virtuais conforme apropriado para suas finalidades.

- Carga de trabalho Hyper\-hosts V tem um adaptador de rede física instalado que é conectado à Intranet da organização.
- No Hyper\-V Virtual Switch, um comutador virtual é criado em cada host. O comutador é externo e está associado o um adaptador de rede conectado à intranet.
- As VMs da carga de trabalho são configuradas com 2 GB de RAM e 2 núcleos.
- As VMs de carga de trabalho de cada um tem um adaptador de rede virtual que está conectado ao comutador virtual da intranet.

### <a name="traffic-generator-tool"></a>Ferramenta de gerador de tráfego

A ferramenta de gerador de tráfego que é usada neste teste é a ferramenta de ctsTraffic. O repositório Git para essa ferramenta está localizado em [ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Desempenho do Gateway de RAS

As ilustrações nesta seção descrevem o Gerenciador de tarefas exibe de taxa de transferência de túnel GRE com várias conexões TCP.

Você pode obter uma taxa de transferência de até 2,0 Gbps em várias\-núcleos de VMs que são configuradas como Gateways de RAS do GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Desempenho de túnel GRE com várias sessões TCP

Com várias sessões TCP, a utilização da CPU atinge 100% e a taxa de transferência máxima no túnel de GRE é 2.0 Gbps.

A ilustração a seguir ilustra a utilização da CPU em ambas as VMs de Gateway de RAS. VM do Active Directory, n º de VM de Gateway de RAS 1, está à esquerda, enquanto a VM passiva, n º de VM de Gateway de RAS 2, está à direita.

![Utilização de CPU da VM do gateway no Gerenciador de tarefas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

A ilustração a seguir mostra a taxa de transferência de rede Ethernet nas VMs de Gateway de RAS. VM do Active Directory, n º de VM de Gateway de RAS 1, está à esquerda, enquanto a VM passiva, n º de VM de Gateway de RAS 2, está à direita.

![Taxa de transferência da rede de VM Ethernet gateway no Gerenciador de tarefas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Desempenho de túnel GRE com uma Conexão de TCP

Com a configuração de teste foi alterada de várias sessões TCP para uma única sessão TCP, apenas um núcleo de CPU atinge a capacidade máxima em que as VMs de Gateway de RAS.

A taxa de transferência máxima no túnel de GRE é entre 400 a 500 Mbps.

A ilustração a seguir ilustra a utilização da CPU em ambas as VMs de Gateway de RAS. VM do Active Directory, n º de VM de Gateway de RAS 1, está à esquerda, enquanto a VM passiva, n º de VM de Gateway de RAS 2, está à direita.

![Utilização de CPU da VM do gateway no Gerenciador de tarefas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


A ilustração a seguir mostra a taxa de transferência de rede Ethernet nas VMs de Gateway de RAS. VM do Active Directory, n º de VM de Gateway de RAS 1, está à esquerda, enquanto a VM passiva, n º de VM de Gateway de RAS 2, está à direita.

![Taxa de transferência da rede de VM Ethernet gateway no Gerenciador de tarefas](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Para obter mais informações sobre o desempenho do Gateway de RAS, consulte [Gateway de HNV ajustar o desempenho em redes definidas por Software](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).