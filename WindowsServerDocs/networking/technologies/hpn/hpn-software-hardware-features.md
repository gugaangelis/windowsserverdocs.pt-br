---
title: Software e hardware (SH) integrado recursos e tecnologias
description: Esses recursos têm componentes de hardware e software. O software está intimamente relacionado a recursos de hardware que são necessários para o recurso funcione. Exemplos disso incluem VMMQ, VMQ, descarregamento de soma de verificação do lado de envio IPv4 e RSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: a58d1e14dc8f543f25ef241f2a65054599136031
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817387"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Software e hardware (SH) integrado recursos e tecnologias

Esses recursos têm componentes de hardware e software. O software está intimamente relacionado a recursos de hardware que são necessários para o recurso funcione. Exemplos disso incluem VMMQ, VMQ, descarregamento de soma de verificação do lado de envio IPv4 e RSS.

>[!TIP]
>SH e HO recursos estarão disponíveis se a NIC instalada dá suporte a ele. As descrições de recurso abaixo abordará como saber se a NIC suporta o recurso.

## <a name="converged-nic"></a>NIC convergida 

NIC convergida é uma tecnologia que permite a NICs virtuais no host do Hyper-V para expor serviços RDMA para processos de host. Windows Server 2016 não requer mais NICs separadas para RDMA. O recurso NIC convergida permite que as NICs virtuais na partição Host (vNICs) para expor o RDMA para a partição de host e compartilham a largura de banda das NICs entre o tráfego RDMA e a VM e o outro tráfego TCP/UDP de maneira justa e gerenciável.

![NIC convergida com SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Você pode gerenciar a operação de NIC convergida por meio do VMM ou o Windows PowerShell. Os cmdlets do PowerShell são os mesmos cmdlets usados para RDMA (veja abaixo).

Para usar o recurso NIC convergido:

1.  Certifique-se de definir o host para DCB.

2.  Certifique-se de habilitar o RDMA no NIC ou, no caso de uma equipe de conjunto, as NICs estão associadas ao comutador do Hyper-V. 

3.  Certifique-se de habilitar o RDMA no vNICs designado para RDMA no host. 

Para obter mais detalhes sobre o RDMA e SET, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>DCB (Data Center Bridging) 

O DCB é um conjunto de padrões do Institute de Electrical and Electronics Engineers (IEEE) que permitem estruturas convergidas em data centers. O DCB fornece gerenciamento de largura de banda baseada em fila de hardware em um host com a cooperação do comutador adjacente. Todo o tráfego de armazenamento, dados de rede, gerenciamento e comunicação entre processos (IPC) de cluster compartilham a mesma infraestrutura de rede Ethernet. No Windows Server 2016, o DCB pode ser aplicado a qualquer NIC individualmente e às NICs associadas para o Hyper-V alternar.

Para DCB, Windows Server usa a prioridade baseada em fluxo de controle (PFC), padronizado no IEEE 802.1Qbb. PFC cria uma malha de rede (quase) sem perdas, evitando estouro dentro de classes de tráfego. Windows Server também usa aprimorada transmissão seleção (ETS), padronizado no IEEE 802.1Qaz. ETS permite que a divisão da largura de banda em partes reservadas para até oito classes de tráfego. Cada classe de tráfego tem seu próprio transmissão da fila e, por meio do uso PFC, pode iniciar e parar a transmissão em uma classe.

Para obter mais informações, consulte [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualização de Rede Hyper-V

| | |
|---|---|
| **v1 (HNVv1)**             | Introduzido no Windows Server 2012, a virtualização de rede do Hyper-V (HNV) habilita a virtualização de redes de cliente em uma infraestrutura de rede física compartilhada. Com mínimas alterações necessárias na malha de rede física, HNV fornece provedores de serviço a agilidade para implantar e migrar cargas de trabalho de locatário em qualquer lugar em três nuvens: a nuvem do provedor de serviço, a nuvem privada ou a nuvem pública do Microsoft Azure.                                         |
| **v2 NVGRE (HNVv2 NVGRE)** | No Windows Server 2016 e no System Center Virtual Machine Manager, a Microsoft fornece uma solução de virtualização de rede de ponta a ponta que inclui o Gateway de RAS, balanceamento de carga de Software, controlador de rede e muito mais. Para obter mais informações, consulte [visão geral de virtualização de rede do Hyper-V no Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **v2 VxLAN (HNVv2 VxLAN)** | No Windows Server 2016, é parte da SDN-extensão, que podem ser gerenciados por meio do controlador de rede.    |
---

## <a name="ipsec-task-offload-ipsecto"></a>Descarregamento de tarefa IPsec IPsecTO) 

Descarregamento de tarefa IPsec é um recurso NIC que permite ao sistema operacional usar o processador na NIC para o trabalho de criptografia de IPsec.

>[!IMPORTANT] 
>Descarregamento de tarefa IPsec é uma tecnologia herdada que não é compatível com a maioria dos adaptadores de rede e, em que ele existe, ele é desabilitado por padrão.

## <a name="private-virtual-local-area-network-pvlan"></a>Privada virtual rede Local (PVLAN). 

PVLANs permitem a comunicação somente entre máquinas virtuais no mesmo servidor de virtualização. Uma rede virtual privada não está associada a um adaptador de rede física. Uma rede virtual privada é isolada de todo o tráfego de rede externo no servidor de virtualização, bem como qualquer tráfego de rede entre o sistema operacional de gerenciamento e a rede externa. Esse tipo de rede é útil quando você precisa criar um ambiente isolado de rede, como um domínio de teste isolada. O Hyper-V e as pilhas de SDN dão suporte somente no modo PVLAN isolado porta.

Para obter detalhes sobre o isolamento de PVLAN, consulte [System Center: Blog de engenharia do Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>Acesso Remoto Direto à Memória (RDMA) 

O RDMA é uma tecnologia de rede que fornece alta taxa de transferência e baixa latência de comunicação que minimiza o uso da CPU. RDMA dá suporte à cópia de zero a rede, permitindo que o adaptador de rede transferir dados diretamente ou da memória do aplicativo. Compatível com RDMA significa que a NIC (física ou virtual) é capaz de expor RDMA a um cliente RDMA. Compatível com RDMA, por outro lado, significa que uma NIC compatível com RDMA expõe a interface RDMA na pilha.

Para obter mais detalhes sobre o RDMA, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

O RSS é um recurso NIC que separa os conjuntos diferentes de fluxos e entrega a processadores diferentes para processamento. RSS processa o processamento, permitindo que um host dimensionar para muito altas taxas de dados de rede. 

Para obter mais detalhes, consulte [RSS Receive Side Scaling ()](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualização de entrada e saída de raiz única (SR-IOV) 

SR-IOV permite o tráfego VM para mover diretamente da NIC para a VM sem passar por meio do host do Hyper-V. SR-IOV é uma melhoria incrível no desempenho de uma VM, mas não tem a capacidade do host gerenciar esse pipe. Só use SR-IOV quando a carga de trabalho é bem comportada, confiável e em geral, a única VM no host.

O tráfego que usa o SR-IOV ignora o comutador do Hyper-V, que significa que todas as políticas, por exemplo, ACLs, ou gerenciamento de largura de banda não será aplicado. Tráfego de SR-IOV também não pode ser passado por meio de qualquer recurso de virtualização de rede, para que o encapsulamento de GRE NV ou VxLAN não pode ser aplicado. Use somente o SR-IOV para cargas de trabalho confiáveis bem em situações específicas. Além disso, é possível usar as políticas de host, gerenciamento de largura de banda e as tecnologias de virtualização.

No futuro, duas tecnologias permitiria que a SR-IOV: Tabelas de fluxo (GFT) genérica e descarregamento de QoS de Hardware (gerenciamento de largura de banda na NIC) – depois que as NICs em nosso ecossistema dão suporte a eles. A combinação dessas duas tecnologias que tornariam SR-IOV útil para todas as VMs, permitiria que as políticas, virtualização e regras de gerenciamento de largura de banda a serem aplicadas e poderia resultar em um grande salto para a frente no geral aplicação de SR-IOV.

Para obter mais detalhes, consulte [visão geral de e/s virtualização de raiz única (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Descarregamento de Chimney TCP

O TCP Chimney Offload, também conhecido como TCP Offload TOE (Engine), é uma tecnologia que permite que o host descarregar todos os TCP processamento à NIC. Como a pilha de TCP do Windows Server quase sempre é mais eficiente do que o mecanismo TOE, usar o descarregamento de Chimney TCP não é recomendado.

>[!IMPORTANT]
>Descarregamento de Chimney de TCP é uma tecnologia preterida. É recomendável que você não usar o descarregamento de Chimney de TCP como a Microsoft poderá parar de dar suporte a ele no futuro.

## <a name="virtual-local-area-network-vlan"></a>Rede Local virtual (VLAN) 

VLAN é uma extensão do cabeçalho do quadro Ethernet para habilitar o particionamento de uma LAN em várias VLANs, cada um usando seu próprio espaço de endereço. No Windows Server 2016, as VLANs são definidas nas portas de comutador do Hyper-V ou definindo interfaces da equipe em equipes de agrupamento NIC. Para obter mais informações, consulte [agrupamento NIC e redes locais virtuais (VLANs)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>VMQ (Fila de Máquina Virtual) 

VMQs é um recurso NIC que aloca uma fila para cada VM. Sempre que tiver o Hyper-V habilitado; Você também deve habilitar VMQ. No Windows Server 2016, VMQs usam vPorts NIC Switch com uma única fila atribuída para o vPort para fornecer a mesma funcionalidade. Para obter mais informações, consulte [Virtual Receive Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) e [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Fila de várias máquinas virtuais (VMMQ) 

VMMQ é um recurso NIC que permita o tráfego para uma VM distribuídas em várias filas, cada processado por um processador físico diferente. O tráfego é então passado para vários LPs na VM, como seria o vRSS, que permite o fornecimento de largura de banda de rede considerável para a máquina virtual.

---