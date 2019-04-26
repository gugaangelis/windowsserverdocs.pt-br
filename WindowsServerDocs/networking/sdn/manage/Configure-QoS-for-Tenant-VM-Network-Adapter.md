---
title: Configurar qualidade de serviço (QoS) para um adaptador de rede VM de locatário
description: Quando você configura a QoS para um adaptador de rede VM de locatário, você tem uma opção entre o Data Center Bridging \(DCB\)ou a rede definida pelo Software \(SDN\) QoS.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880597"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurar qualidade de serviço (QoS) para um adaptador de rede VM de locatário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando você configura a QoS para um adaptador de rede VM de locatário, você tem uma opção entre o Data Center Bridging \(DCB\)ou a rede definida pelo Software \(SDN\) QoS.

1.  **DCB**. Você pode configurar o DCB usando os cmdlets do Windows PowerShell NetQoS. Para obter um exemplo, consulte a seção "Habilitar ponte de Data Center" no tópico [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS de SDN**. Você pode habilitar a QoS de SDN usando o controlador de rede, que pode ser definida para limitar a largura de banda em uma interface virtual para impedir que uma VM de alto tráfego bloqueando outros usuários.  Você também pode configurar o QoS de SDN para reservar uma quantidade específica de largura de banda para uma VM garantir que a VM está acessível independentemente da quantidade de tráfego de rede.  

Aplica todas as configurações de QoS de SDN por meio das configurações de porta das propriedades do adaptador de rede. Consulte a tabela abaixo para obter mais detalhes.

|Nome do elemento|Descrição|
|------------|-----------| 
|macSpoofing| Permite que as VMs alterar o controle de acesso de mídia de origem \(MAC\) endereço em pacotes de saída para um endereço MAC não está atribuído à VM.<p>Valores permitidos:<ul><li>Habilitado – Use um endereço MAC diferente.</li><li>Desabilitado – Use apenas o endereço MAC atribuído a ele.</li></ul>|
|arpGuard| Permite que o protetor de ARP somente endereços especificados na ArpFilter passar através da porta.<p>Valores permitidos:<ul><li>Habilitado – permitido</li><li>Desativado – não permitido</li></ul>|
|dhcpGuard| Permite ou descarta todas as mensagens DHCP de uma máquina virtual que alega ser um servidor DHCP. <p>Valores permitidos:<ul><li>Habilitado – descartes de mensagens DHCP porque o servidor DHCP virtualizado é considerado não confiável.</li><li>Desabilitado – permite que as mensagens sejam recebidas porque o servidor DHCP virtualizado é considerado confiável.</li></ul>|
|stormLimit| O número de pacotes (unicast de difusão, multicast e desconhecido) por segundo de uma VM tem permissão para enviar por meio do adaptador de rede virtual. Pacotes além do limite durante esse intervalo de um segundo são descartados. Um valor de zero \(0\) significa que não há nenhum limite...|
|portFlowLimit| O número máximo de fluxos permitido para execução para a porta. Um valor de espaço em branco ou zero \(0\) significa que não há nenhum limite. |
|vmqWeight| O peso relativo descreve a afinidade do adaptador de rede virtual para usar a fila de máquina virtual (VMQ). O intervalo de valor é de 0 a 100.<p>Valores permitidos:<ul><li>0 – desabilita a VMQ no adaptador de rede virtual.</li><li>1 a 100 – habilita a VMQ no adaptador de rede virtual.</li></ul>|
|iovWeight| O peso relativo define a afinidade do adaptador de rede virtual para a virtualização de e/s de raiz única atribuída \(SR-IOV\) função virtual. <p>Valores permitidos:<ul><li>0 – desabilita a SR-IOV no adaptador de rede virtual.</li><li>1 a 100 – permite SR-IOV no adaptador de rede virtual.</li></ul>|
|iovInterruptModeration|<p>Valores permitidos:<ul><li>padrão – configuração do fornecedor de adaptador de rede física determina o valor.</li><li>adaptável- </li><li>configurações </li><li>Baixa</li><li>médio</li><li>Alta</li></ul><p>Se você escolher **padrão**, configuração do fornecedor de adaptador de rede física determina o valor.  Se você escolher, **adaptável**, o tráfego de tempo de execução padrão determins a taxa de moderação de interrupção.|
|iovQueuePairsRequested| O número de pares de fila de hardware alocados para uma função virtual SR-IOV. Se o receive-side dimensionando \(RSS\) é necessária e se o adaptador de rede física que associa ao comutador virtual dá suporte a RSS em funções virtuais de SR-IOV, mais de um par de fila será necessário. <p>Valores permitidos: 1 a 4294967295.|
|QosSettings| Defina as seguintes configurações de Qos, que são opcionais: <ul><li>**outboundReservedValue** - se outboundReservedMode for "absoluto", em seguida, o valor indica a largura de banda em Mbps, a garantia de porta virtual para transmissão (saída). Se outboundReservedMode é "peso", em seguida, o valor indica a parte ponderada da largura de banda garantida.</li><li>**outboundMaximumMbps** -indica que o máximo permitido envio largura de banda em Mbps, para a porta virtual (saída).</li><li>**InboundMaximumMbps** -indica que o máximo permitido de largura de banda do lado de recebimento para a porta virtual (entrada) em Mbps.</li></ul> |

---