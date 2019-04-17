---
title: Novidades na virtualização de rede do Hyper-V no Windows Server 2016
description: Este tópico fornece informações sobre os novos recursos de virtualização de rede do Hyper-V no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Novidades na virtualização de rede do Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico descreve a funcionalidade de virtualização de rede do Hyper-V (HNV) que é novo ou alterado no Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Atualizações no HNV  
HNV oferece suporte aprimorado nas seguintes áreas:  
  
|Recursos/funcionalidades|Novos ou melhorados|Descrição|  
|--------------------------|-------------------|---------------|  
|[Comutador programável do Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Novo|Política HNV é programável por meio do controlador de rede Microsoft.|  
|[Suporte para encapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Novo|HNV agora dá suporte a encapsulamento VXLAN.|  
|[Interoperabilidade do software balanceador de carga (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Novo|HNV é totalmente integrado com o balanceador de carga de Software da Microsoft.|  
|[Compatível com cabeçalhos de IEEE Ethernet](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Aprimorado|Compatível com padrões IEEE Ethernet|  
  
### <a name="SDN"></a>Comutador programável do Hyper-V  
HNV é um componente fundamental da solução de Software definido de rede (SDN) atualizado da Microsoft e está totalmente integrada à pilha de SDN.  
  
Novo controlador de rede da Microsoft empurra políticas HNV até um agente de Host em execução em cada host usando vSwitch aberto protocolo de gerenciamento de banco de dados (OVSDB) como a Interface SouthBound (SBI). O agente de Host armazena essa política usando uma personalização do [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) e programas de regras de fluxo complexas em um mecanismo de fluxo eficiente no switch Hyper-V.  
  
O mecanismo de fluxo dentro do switch Hyper-V é o mesmo mecanismo usado no Microsoft Azure&trade;, que comprovadamente em hyper-escala a nuvem pública do Microsoft Azure. Além disso, a pilha SDN inteira para cima por meio do controlador de rede e o provedor de recursos de rede (detalhes em breve) é consistente com o Microsoft Azure, colocando o poder da nuvem pública Microsoft Azure para nossos clientes corporativos e hospedagem serviço provedor.  
  
> [!NOTE]  
> Para saber mais sobre OVSDB, consulte [RFC 7047](http://www.rfc-editor.org/info/rfc7047).  
  
A opção Hyper-V dá suporte a ambas as regras de fluxo com e sem monitoração com base no mecanismo simples de fluxo 'corresponde a ação' dentro da Microsoft.  
 
![Mudar do Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Suporte para encapsulamento VXLAN  
O Virtual eXtensible rede Local (VXLAN - [RFC 7348](http://www.rfc-editor.org/info/rfc7348)) protocolo tem sido adotado no mercado, com suporte de fornecedores como Cisco, Brocade, Dell, HP e outros. HNV agora também dá suporte a esse esquema de encapsulamento usando o modo de distribuição MAC através do controlador de rede Microsoft mapeamentos de programa para locatário sobreposição rede endereços IP (endereço do cliente ou da autoridade de certificação) para os endereços IP base físico rede (endereço do provedor ou PA). NVGRE e VXLAN tarefa descarrega têm suporte para melhorar o desempenho por meio de drivers de terceiros.  
  
### <a name="SLB"></a>Interoperabilidade do software balanceador de carga (SLB)  
Windows Server 2016 inclui um balanceador de carga de software (SLB) com suporte completo para o tráfego de rede virtual e interação perfeita com HNV. O SLB é implementada por meio do mecanismo de fluxo eficiente na opção de v plano dados e controlado pelo controlador de rede para IP Virtual (VIP) / dinâmico mapeamentos IP DIP ().  
  
### <a name="L2"></a>Compatível com cabeçalhos de IEEE Ethernet  
HNV implementa cabeçalhos Ethernet L2 corretos para garantir a interoperabilidade com terceiros dispositivos físicos e virtuais que dependem de protocolos padrão da indústria. Microsoft garante que todos os pacotes transmitidos têm valores compatíveis em todos os campos para garantir que essa interoperabilidade. Além disso, o suporte para Jumbo Frames (MTU > 1780) na física L2 rede será necessária para levar em conta pacote sobrecarga introduzido por protocolos de encapsulamento (NVGRE, VXLAN), garantindo convidado máquinas virtuais conectado a uma rede Virtual HNV manter um MTU 1514.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral de virtualização de rede do Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Detalhes técnicos da virtualização de rede do Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Software definidos rede](../../Software-Defined-Networking--SDN-.md)  
  