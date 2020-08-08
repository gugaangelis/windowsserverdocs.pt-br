---
title: O que há de novo na virtualização de rede Hyper-V no Windows Server 2016
description: Este tópico fornece informações sobre os novos recursos na virtualização de rede Hyper-V no Windows Server 2016
manager: grcusanz
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: anpaul
author: AnirbanPaul
ms.date: 03/19/2018
ms.openlocfilehash: aa53b13526172e37a46fbb3108278ad7aa859b64
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952481"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>O que há de novo na virtualização de rede Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve a funcionalidade de HNV (virtualização de rede) do Hyper-V que é nova ou alterada no Windows Server 2016.

## <a name="updates-in-hnv"></a><a name="BKMK_IPAM2012R2"></a>Atualizações no HNV
O HNV oferece suporte aprimorado nas seguintes áreas:

|Recurso/funcionalidade|Novo ou melhorado|Descrição|
|--------------------------|-------------------|---------------|
|[Comutador Hyper-V programável](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Novo|A política de HNV é programável por meio do controlador de rede da Microsoft.|
|[Suporte ao encapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Novo|HNV agora dá suporte ao encapsulamento VXLAN.|
|[Interoperabilidade do SLB (software Load Balancer)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Novo|O HNV é totalmente integrado ao Load Balancer de software da Microsoft.|
|[Cabeçalhos de Ethernet IEEE em conformidade](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Melhoria de|Em conformidade com os padrões IEEE Ethernet|

### <a name="programmable-hyper-v-switch"></a><a name="SDN"></a>Comutador Hyper-V programável
O HNV é um bloco de construção fundamental da solução de SDN (rede definida pelo software) atualizada pela Microsoft e é totalmente integrado à pilha SDN.

O novo controlador de rede da Microsoft envia políticas de HNV para um agente de host em execução em cada host usando o protocolo OVSDB (Open vSwitch Database Management) como SBI (interface SouthBound). O agente de host armazena essa política usando uma personalização das regras de fluxo complexo de programas e [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) em um mecanismo de fluxo de alto desempenho no comutador Hyper-V.

O mecanismo de fluxo dentro da opção Hyper-V é o mesmo mecanismo usado no Microsoft Azure &trade; , que foi comprovado em hiperescala na nuvem pública Microsoft Azure. Além disso, toda a pilha do SDN através do controlador de rede e do provedor de recursos de rede (detalhes em breve) é consistente com Microsoft Azure, o que traz o poder da nuvem pública Microsoft Azure para nossos clientes do provedor de serviços corporativos e de hospedagem.

> [!NOTE]
> Para obter mais informações sobre OVSDB, consulte [RFC 7047](https://www.rfc-editor.org/info/rfc7047).

A opção Hyper-V dá suporte a regras de fluxo com e sem estado com base na ' ação de correspondência ' simples no mecanismo de fluxo da Microsoft.

![Comutador Hyper-V do Windows Server 2016](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)

### <a name="vxlan-encapsulation-support"></a><a name="VXLAN"></a>Suporte ao encapsulamento VXLAN
O protocolo VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)(rede local extensível virtual) foi amplamente adotado no mercado, com suporte de fornecedores como Cisco, Brocade, Dell, HP e outros. O HNV também dá suporte a esse esquema de encapsulamento usando o modo de distribuição MAC por meio do controlador de rede da Microsoft para programar mapeamentos para endereços IP de rede de sobreposição de locatário (endereço de cliente ou CA) para os endereços IP de rede física underlay (endereço do provedor ou PA). Os descarregamentos de tarefas NVGRE e VXLAN têm suporte para melhorar o desempenho por meio de drivers de terceiros.

### <a name="software-load-balancer-slb-interoperability"></a><a name="SLB"></a>Interoperabilidade do SLB (software Load Balancer)
O Windows Server 2016 inclui um SLB (balanceador de carga de software) com suporte completo para tráfego de rede virtual e interação direta com o HNV. O SLB é implementado por meio do mecanismo de fluxo de desempenho na opção v-switch do plano de dados e controlado pelo controlador de rede para mapeamentos de IP virtual (VIP)/IP dinâmico (DIP).

### <a name="compliant-ieee-ethernet-headers"></a><a name="L2"></a>Cabeçalhos de Ethernet IEEE em conformidade
O HNV implementa cabeçalhos Ethernet L2 corretos para garantir a interoperabilidade com dispositivos virtuais e físicos de terceiros que dependem de protocolos padrão do setor. A Microsoft garante que todos os pacotes transmitidos tenham valores em conformidade em todos os campos para garantir essa interoperabilidade. Além disso, o suporte para quadros Jumbo (MTU > 1780) na rede de L2 física será necessário para considerar a sobrecarga de pacotes introduzida pelos protocolos de encapsulamento (NVGRE, VXLAN), ao mesmo tempo em que garante que as máquinas virtuais convidadas anexadas a uma rede virtual HNV mantenham um MTU de 1514.

## <a name="additional-references"></a>Referências adicionais

-   [Visão Geral da Virtualização de Rede Hyper-V](hyperv-network-virtualization-overview-windows-server.md)

-   [Detalhes técnicos da Virtualização de Rede Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)

-   [Rede definida pelo software](../../Software-Defined-Networking--SDN-.md)
