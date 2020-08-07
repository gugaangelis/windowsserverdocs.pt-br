---
title: Configurar a QoS (qualidade de serviço) para um adaptador de rede de VM de locatário
description: Ao configurar a QoS para um adaptador de rede de VM de locatário, você tem uma opção entre a ponte de data center \( DCB \) ou a rede definida pelo software \( QoS de Sdn \) .
manager: grcusanz
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 95307dafd077f1efc3b44131494ced709334a2d8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962220"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurar a QoS (qualidade de serviço) para um adaptador de rede de VM de locatário

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Ao configurar a QoS para um adaptador de rede de VM de locatário, você tem uma opção entre a ponte de data center \( DCB \) ou a rede definida pelo software \( QoS de Sdn \) .

1.    **DCB**. Você pode configurar o DCB usando os cmdlets do NetQoS do Windows PowerShell. Para obter um exemplo, consulte a seção "habilitar ponte do Data Center" no tópico [RDMA (acesso remoto direto à memória) e no conjunto inserido de equipe (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.    **QoS de Sdn**. Você pode habilitar o QoS de SDN usando o controlador de rede, que pode ser definido para limitar a largura de banda em uma interface virtual para impedir que uma VM de tráfego alto bloqueie outros usuários.  Você também pode configurar o QoS de SDN para reservar uma quantidade específica de largura de banda para uma VM a fim de garantir que a VM esteja acessível, independentemente da quantidade de tráfego de rede.

Aplique todas as configurações de QoS de SDN pelas configurações de porta das propriedades da interface de rede. Consulte a tabela abaixo para obter mais detalhes.

|Nome do elemento|Descrição|
|------------|-----------|
|macSpoofing| Permite que as VMs alterem o endereço MAC do controle de acesso à mídia \( \) de origem em pacotes de saída para um endereço MAC não atribuído à VM.<p>Valores permitidos:<ul><li>Habilitado – use um endereço MAC diferente.</li><li>Desabilitado – Use apenas o endereço MAC atribuído a ele.</li></ul>|
|arpGuard| Permite que somente os endereços de proteção do ARP especificados em ArpFilter passem pela porta.<p>Valores permitidos:<ul><li>Habilitado-permitido</li><li>Desabilitado – não permitido</li></ul>|
|dhcpGuard| Permite ou descarta todas as mensagens DHCP de uma VM que alega ser um servidor DHCP. <p>Valores permitidos:<ul><li>Habilitado – descarta mensagens DHCP porque o servidor DHCP virtualizado é considerado não confiável.</li><li>Disabled – permite que a mensagem seja recebida porque o servidor DHCP virtualizado é considerado confiável.</li></ul>|
|stormLimit| O número de pacotes (difusão, multicast e unicast desconhecido) por segundo que uma VM tem permissão para enviar por meio do adaptador de rede virtual. Os pacotes além do limite durante esse intervalo de um segundo são removidos. Um valor igual A zero \( 0 \) significa que não há limite...|
|portFlowLimit| O número máximo de fluxos que podem ser executados para a porta. Um valor de em branco ou zero \( 0 \) significa que não há limite. |
|vmqWeight| O peso relativo descreve a afinidade do adaptador de rede virtual para usar a fila de máquina virtual (VMQ). O intervalo de valor é de 0 a 100.<p>Valores permitidos:<ul><li>0 – desabilita a VMQ no adaptador de rede virtual.</li><li>1-100 – habilita a VMQ no adaptador de rede virtual.</li></ul>|
|iovWeight| O peso relativo define a afinidade do adaptador de rede virtual para a \( função virtual do Sr-IOV de virtualização de e/s de raiz única atribuída \) . <p>Valores permitidos:<ul><li>0 – desabilita o SR-IOV no adaptador de rede virtual.</li><li>1-100 – habilita o SR-IOV no adaptador de rede virtual.</li></ul>|
|iovInterruptModeration|<p>Valores permitidos:<ul><li>padrão – a configuração do fornecedor do adaptador de rede física determina o valor.</li><li>adaptável </li><li>Desligar </li><li>low</li><li>média</li><li>high</li></ul><p>Se você escolher **padrão**, a configuração do fornecedor do adaptador de rede física determinará o valor.  Se você escolher, **adaptável**, o padrão de tráfego de tempo de execução determina a taxa de moderação de interrupção.|
|iovQueuePairsRequested| O número de pares de fila de hardware alocados a uma função virtual SR-IOV. Se for necessário um RSS de dimensionamento no lado do recebimento \( \) e se o adaptador de rede física que se associar ao comutador virtual oferecer suporte a RSS em funções virtuais Sr-IOV, mais de um par de filas será necessário. <p>Valores permitidos: 1 a 4294967295.|
|QosSettings| Defina as seguintes configurações de QoS, todas as quais são opcionais: <ul><li>**outboundReservedValue** -se outboundReservedMode for "Absolute", o valor indicará a largura de banda, em Mbps, garantida para a porta virtual para transmissão (Egresso). Se outboundReservedMode for "Weight", o valor indicará a parte ponderada da largura de banda garantida.</li><li>**outboundMaximumMbps** -indica o máximo permitido de largura de banda do lado do envio, em Mbps, para a porta virtual (Egresso).</li><li>**InboundMaximumMbps** -indica o máximo permitido de largura de banda do lado de recebimento para a porta virtual (entrada) em Mbps.</li></ul> |

---