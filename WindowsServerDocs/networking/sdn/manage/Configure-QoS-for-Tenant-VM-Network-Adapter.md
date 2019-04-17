---
title: Configurar a qualidade de serviço (QoS) para um adaptador de rede do locatário VM
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurar a qualidade de serviço (QoS) para um adaptador de rede do locatário VM

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando você configura QoS para um adaptador de rede do locatário VM, você tem uma escolha entre a ponte do Centro de dados \ (DCB\) ou Software de rede definidos \(SDN\) QoS.

1.  **DCB**. Você pode configurar DCB usando os cmdlets do Windows PowerShell NetQoS. Por exemplo, veja a seção "Habilitar dados central ligando" no tópico [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS SDN**. Você pode habilitar SDN QoS usando o controlador de rede, que pode ser definido para limitar a largura de banda em uma interface virtual para impedir que uma VM de alto tráfego impedindo que outros usuários.  Você também pode configurar SDN QoS para reservar uma quantidade específica de largura de banda de uma VM garantir que a VM é acessível, independentemente da quantidade de tráfego de rede.  

Todas as configurações de Qos SDN são aplicadas por meio das configurações de porta das propriedades de Interface de rede de acordo com a tabela a seguir.

|Nome do elemento|Descrição|
|------------|-----------| 
|macSpoofing|Especifica se VMs podem alterar o endereço \(MAC\) de controle de acesso da mídia do origem em pacotes de saída para um endereço MAC que não seja atribuído à VM. Permitido valores são "enabled" \ (permitindo que a VM usar um address\ MAC diferente) e "desabilitado" \ (permitindo que a VM usar somente o endereço MAC atribuído ao it\).|
|arpGuard|Especifica se ARP guard está habilitado.  Guard ARP permite que apenas os endereços que são especificados na ArpFilter para passar por meio da porta.  Valores permitidos são "enabled" ou "desabilitados".
|dhcpGuard|Especifica se é receber mensagens de DHCP de uma VM que diz ser um servidor DHCP. Valores permitidos são "enabled", que descarta mensagens DHCP porque o servidor DHCP virtualizado é considerado não confiável, ou "desabilitado", que permite que a mensagem a ser recebida porque o servidor DHCP virtualizado é considerado confiável.
|stormLimit|Especifica o número de pacotes de transmissão, multicast e unicast desconhecido por segundo que uma VM tem permissão para enviar por meio do adaptador de rede virtual especificado. Pacotes de transmissão, multicast e desconhecido unicast além do limite durante um intervalo segundo são descartados. Um valor de zero \(0\) significa que não há nenhum limite.
|portFlowLimit|Especifica o número máximo de fluxos que podem ser executadas para a porta.  Um valor em branco ou zero \(0\) significa que não há nenhum limite.
|vmqWeight|Especifica se a fila de máquina virtual (VMQ) está habilitada no adaptador de rede virtual. A importância relativa descreve a afinidade do adaptador de rede virtual para usar VMQ. O intervalo de valor é de 0 a 100. Especifique 0 para desabilitar VMQ no adaptador de rede virtual.
|iovWeight|Especifica se a virtualização de e/s única raiz \(SR-IOV\) é ativado neste adaptador de rede virtual. A importância relativa define a afinidade do adaptador de rede virtual para a função de virtual atribuída SR-IOV. O intervalo do valor é de 0 a 100. Especifique 0 para desabilitar SR-IOV no adaptador de rede virtual. 
|iovInterruptModeration|Especifica o valor de moderação de interrupção para uma única raiz e/s virtualização \(SR-IOV\) função virtual atribuído a um adaptador de rede virtual. Valores permitidos são "default", "adaptável", "off", "baixa", "médios" e "alta".   Se o padrão é escolhido, o valor é determinado pela configuração do fornecedor de adaptador de rede física.  Se for escolhido adaptável, a taxa de moderação de interrupção é baseada no padrão de tráfego do tempo de execução. 
|iovQueuePairsRequested|Especifica o número de pares de fila de hardware para ser alocado para uma função virtual SR-IOV. Se \(RSS\) dimensionamento lado receber for necessária, e se o adaptador de rede físico que é vinculado ao switch virtual dá suporte a RSS no SR-IOV virtual funções, em seguida, mais de par de uma fila é necessária. Valores permitidos variam de 1 a 4294967295. 
|QosSettings|Você pode configurar as seguintes configurações de Qos, todos os que são opcionais:  <br/><br />**outboundReservedValue:**<br/>Se for "absoluto" outboundReservedMode o valor indica a largura de banda, em Mbps, garantida de que a porta virtual para transmissão (saída). Se outboundReservedMode for "acento" o valor indica a parte ponderada da largura de banda garantida. <br/><br />**outboundMaximumMbps:**  <br/>Indica que o número máximo permitido largura de banda do lado do envio, em Mbps, para a porta virtual (saída). <br/><br/>**InboundMaximumMbps:**  <br/>Indica que o máximo permitido de largura de banda lateral receber para a porta virtual (ingresso) em Mbps. |