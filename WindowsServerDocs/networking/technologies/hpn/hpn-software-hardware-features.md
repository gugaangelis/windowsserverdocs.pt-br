---
title: Software e hardware (SH) integrado recursos e tecnologias
description: Esses recursos têm componentes de software e hardware. O software está intimamente ligado a recursos de hardware que são necessários para que o recurso funcione. Exemplos desses incluem VMMQ, VMQ, descarregamento de soma de verificação do IPv4 do lado do envio e RSS.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/12/2018
ms.openlocfilehash: 3c1b414acaf7487b0a435cfea2891903646c869f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995269"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Software e hardware (SH) integrado recursos e tecnologias

Esses recursos têm componentes de software e hardware. O software está intimamente ligado a recursos de hardware que são necessários para que o recurso funcione. Exemplos desses incluem VMMQ, VMQ, descarregamento de soma de verificação do IPv4 do lado do envio e RSS.

>[!TIP]
>Os recursos SH e HO estarão disponíveis se a NIC instalada der suporte a ele. As descrições de recurso a seguir abordarão como saber se a NIC dá suporte ao recurso.

## <a name="converged-nic"></a>NIC convergida

A NIC convergida é uma tecnologia que permite que as NICs virtuais no host Hyper-V exponham serviços RDMA para hospedar processos. O Windows Server 2016 não requer mais NICs separadas para RDMA. O recurso NIC convergida permite que as NICs virtuais na partição de host (vNICs) exponham RDMA à partição de host e compartilhem a largura de banda das NICs entre o tráfego RDMA e a VM e outro tráfego TCP/UDP de uma maneira justa e gerenciável.

![NIC convergida com SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Você pode gerenciar a operação de NIC convergida por meio do VMM ou do Windows PowerShell. Os cmdlets do PowerShell são os mesmos cmdlets usados para RDMA (veja abaixo).

Para usar a capacidade de NIC convergida:

1.  Certifique-se de definir o host para DCB.

2.  Certifique-se de habilitar o RDMA na NIC ou, no caso de uma equipe definida, as NICs estão associadas à opção do Hyper-V.

3.  Certifique-se de habilitar o RDMA no vNICs designado para RDMA no host.

Para obter mais detalhes sobre o RDMA e o conjunto, consulte [acesso remoto direto à memória (RDMA) e alternância inserida de equipe (Set)](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="data-center-bridging-dcb"></a>DCB (Data Center Bridging)

O DCB é um conjunto de padrões de IEEE (Institute of Electrical and Electronics Engineers) que habilitam malhas convergentes em data centers. O DCB fornece gerenciamento de largura de banda baseado em fila de hardware em um host com cooperação do comutador adjacente. Todo o tráfego de armazenamento, rede de dados, IPC (comunicação entre processos de cluster) e gerenciamento compartilham a mesma infraestrutura de rede Ethernet. No Windows Server 2016, DCB pode ser aplicado a qualquer NIC individualmente e a NICs associadas ao comutador do Hyper-V.

Para DCB, o Windows Server usa o controle de fluxo baseado em prioridade (PFC), padronizado no IEEE 802.1 QBB. O PFC cria uma malha de rede (quase) sem perdas, impedindo o estouro nas classes de tráfego. O Windows Server também usa a seleção de transmissão avançada (ETS), padronizada em IEEE 802.1 Qaz. O ETS permite a divisão da largura de banda em partes reservadas para até oito classes de tráfego. Cada classe de tráfego tem sua própria fila de transmissão e, por meio do uso de PFC, pode iniciar e parar a transmissão dentro de uma classe.

Para obter mais informações, consulte [ponte do Data Center (DCB)](../dcb/dcb-top.md).

## <a name="hyper-v-network-virtualization"></a>Virtualização de Rede Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **V1 (HNVv1)**       |                     Introduzido no Windows Server 2012, a HNV (virtualização de rede do Hyper-V) permite a virtualização de redes de clientes sobre uma infraestrutura de rede física compartilhada. Com as alterações mínimas necessárias na malha de rede física, o HNV dá aos provedores de serviços a agilidade para implantar e migrar cargas de trabalho de locatário em qualquer lugar nas três nuvens: a nuvem do provedor de serviços, a nuvem privada ou a nuvem pública Microsoft Azure.                     |
| **v2 NVGRE (HNVv2 NVGRE)** | No Windows Server 2016 e System Center Virtual Machine Manager, a Microsoft fornece uma solução de virtualização de rede de ponta a ponta que inclui gateway de RAS, balanceamento de carga de software, controlador de rede e muito mais. Para obter mais informações, consulte [visão geral da virtualização de rede Hyper-V no Windows Server 2016](../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server.md). |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        No Windows Server 2016, faz parte da extensão SDN, que você gerencia por meio do controlador de rede.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Descarregamento de tarefa IPsec (IPsecto)

O descarregamento de tarefa IPsec é um recurso NIC que permite que o sistema operacional use o processador na NIC para o trabalho de criptografia IPsec.

>[!IMPORTANT]
>O descarregamento de tarefa IPsec é uma tecnologia herdada que não é suportada pela maioria dos adaptadores de rede e onde ela existe, está desabilitada por padrão.

## <a name="private-virtual-local-area-network-pvlan"></a>Rede de área local virtual privada (PVLAN).

PVLANs permitir a comunicação somente entre máquinas virtuais no mesmo servidor de virtualização. Uma rede virtual privada não está associada a um adaptador de rede física. Uma rede virtual privada é isolada de todo o tráfego de rede externo no servidor de virtualização, bem como qualquer tráfego de rede entre o sistema operacional de gerenciamento e a rede externa. Esse tipo de rede é útil quando você precisa criar um ambiente isolado de rede, como um domínio de teste isolada. As pilhas do Hyper-V e do SDN dão suporte apenas ao modo de porta isolada PVLAN.

Para obter detalhes sobre o isolamento de PVLAN, consulte [System Center: blog de engenharia de Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>Acesso Remoto Direto à Memória (RDMA)

O RDMA é uma tecnologia de rede que fornece comunicação de baixa latência e alta taxa de transferência que minimiza o uso da CPU. O RDMA dá suporte à rede de cópia zero, permitindo que o adaptador de rede transfira dados diretamente para ou da memória do aplicativo. Compatível com RDMA significa que a NIC (física ou virtual) é capaz de expor RDMA a um cliente RDMA. Por outro lado, habilitado para RDMA significa que uma NIC compatível com RDMA está expondo a interface RDMA na pilha.

Para obter mais detalhes sobre o RDMA, consulte [acesso remoto direto à memória (RDMA) e comutador inserido de equipe (Set)](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="receive-side-scaling-rss"></a>RSS (Receive Side Scaling)

RSS é um recurso de NIC que separa diferentes conjuntos de fluxos e os entrega a processadores diferentes para processamento. O RSS paralelize o processamento de rede, permitindo que um host seja dimensionado para taxas de dados muito altas.

Para obter mais detalhes, consulte [RSS (receber dimensionamento lateral)](/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualização de entrada/saída de raiz única (SR-IOV)

O SR-IOV permite que o tráfego da VM seja movido diretamente da NIC para a VM sem passar pelo host do Hyper-V. O SR-IOV é uma melhoria incrível no desempenho de uma VM, mas não tem a capacidade de o host gerenciar esse pipe. Use somente Sr-IOV quando a carga de trabalho estiver bem comparada, confiável e geralmente a única VM no host.

O tráfego que usa o SR-IOV ignora o comutador do Hyper-V, o que significa que qualquer política, por exemplo, ACLs ou gerenciamento de largura de banda não será aplicada. O tráfego de SR-IOV também não pode ser transmitido por meio de qualquer recurso de virtualização de rede; portanto, o encapsulamento VxLAN, GRE ou não pode ser aplicado. Use somente SR-IOV para cargas de trabalho bem confiáveis em situações específicas. Além disso, você não pode usar as políticas de host, gerenciamento de largura de banda e tecnologias de virtualização.

No futuro, duas tecnologias permitiriam SR-IOV: GFT (tabelas de fluxo genérico) e descarga de QoS de hardware (gerenciamento de largura de banda na NIC) – uma vez que as NICs em nosso ecossistema dão suporte a elas. A combinação dessas duas tecnologias tornaria o SR-IOV útil para todas as VMs, permitiria que políticas, virtualização e regras de gerenciamento de largura de banda fossem aplicadas e poderia resultar em grandes saltos no aplicativo geral de SR-IOV.

Para obter mais detalhes, consulte [visão geral de Sr-IOV (virtualização de e/s de raiz única)](/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Descarregamento de Chimney TCP

O descarregamento de Chimney TCP, também conhecido como TOE (TCP Engine Offload), é uma tecnologia que permite que o host descarregue todo o processamento TCP para a NIC. Como a pilha TCP do Windows Server é quase sempre mais eficiente do que o mecanismo TOE, não é recomendável usar o descarregamento de Chimney TCP.

>[!IMPORTANT]
>O descarregamento de Chimney TCP é uma tecnologia preterida. Recomendamos que você não use o descarregamento de Chimney TCP, pois a Microsoft pode parar de dar suporte a ele no futuro.

## <a name="virtual-local-area-network-vlan"></a>Rede de área local virtual (VLAN)

VLAN é uma extensão para o cabeçalho de quadro de Ethernet para habilitar o particionamento de uma LAN em várias VLANs, cada uma usando seu próprio espaço de endereço. No Windows Server 2016, as VLANs são definidas em portas do comutador do Hyper-V ou definindo interfaces de equipe em equipes de agrupamento NIC. Para obter mais informações, consulte [agrupamento NIC e redes locais virtuais (VLANs)](../nic-teaming/nic-teaming.md).

## <a name="virtual-machine-queue-vmq"></a>VMQ (Fila de Máquina Virtual)

VMQs é um recurso de NIC que aloca uma fila para cada VM. Sempre que você tiver habilitado o Hyper-V; Você também deve habilitar a VMQ. No Windows Server 2016, VMQs use o comutador NIC vPorts com uma única fila atribuída ao vPort para fornecer a mesma funcionalidade. Para obter mais informações, consulte [vRSS (recebimento virtual)](../vrss/vrss-top.md) e [agrupamento NIC](../nic-teaming/nic-teaming.md).

## <a name="virtual-machine-multi-queue-vmmq"></a>VMMQ (várias filas de máquina virtual)

VMMQ é um recurso de NIC que permite que o tráfego de uma VM se espalhe por várias filas, cada uma processada por um processador físico diferente. O tráfego é passado para vários LPs na VM como seria em vRSS, o que permite a entrega de largura de banda de rede substancial para a VM.

---