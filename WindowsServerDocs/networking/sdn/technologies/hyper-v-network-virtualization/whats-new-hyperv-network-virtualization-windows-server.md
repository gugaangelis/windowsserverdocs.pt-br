---
title: Quais são as novidades na virtualização de rede Hyper-V no Windows Server 2016
description: Este tópico fornece informações sobre os novos recursos de virtualização de rede do Hyper-V no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: c87ccfba7b9ccc77646f58ade2853766524e67b1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284037"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Quais são as novidades na virtualização de rede Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a funcionalidade de virtualização de rede do Hyper-V (HNV) que é nova ou alterada no Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Atualizações da HNV  
A HNV dá suporte aprimorado nas seguintes áreas:  
  
|Recurso/funcionalidade|Novo ou melhorado|Descrição|  
|--------------------------|-------------------|---------------|  
|[Programável comutador do Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Novo|Política da HNV é programável por meio do controlador de rede da Microsoft.|  
|[Suporte a encapsulamento VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Novo|A HNV agora dá suporte a encapsulamento VXLAN.|  
|[Interoperabilidade de Balanceador de carga (SLB) de software](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Novo|A HNV é totalmente integrada com o balanceador de carga de Software da Microsoft.|  
|[Compatível com cabeçalhos de Ethernet IEEE](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Melhorado|Em conformidade com padrões do IEEE Ethernet|  
  
### <a name="SDN"></a>Programável comutador do Hyper-V  
A HNV é um componente fundamental da solução de Software Defined Networking (SDN) atualizado da Microsoft e é totalmente integrada à pilha de SDN.  
  
Novo controlador de rede da Microsoft envia por push políticas HNV para baixo até um agente de Host em execução em cada host usando abrir vSwitch protocolo de gerenciamento de banco de dados (OVSDB) como a Interface SouthBound (SBI). O agente de Host armazena essa política usando uma personalização do [esquema VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) e programas de regras de fluxo complexos em um mecanismo de fluxo de alto desempenho no comutador do Hyper-V.  
  
O mecanismo de fluxo dentro de comutador do Hyper-V é o mesmo mecanismo usado no Microsoft Azure&trade;, que é comprovadamente em larga escala na nuvem pública do Microsoft Azure. Além disso, toda a pilha SDN backup por meio do controlador de rede e o provedor de recursos de rede (detalhes em breve) é consistente com o Microsoft Azure, portanto, levando a potência da nuvem pública do Microsoft Azure para nossa empresa e o serviço de hospedagem clientes do provedor.  
  
> [!NOTE]  
> Para obter mais informações sobre OVSDB, consulte [7047 RFC](https://www.rfc-editor.org/info/rfc7047).  
  
O comutador do Hyper-V dá suporte a ambas as regras de fluxo de com e sem estado com base no mecanismo de fluxo simples 'corresponde a ação' dentro da Microsoft.  
 
![Opção de Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Suporte a encapsulamento VXLAN  
O Virtual extensível rede Local (VXLAN - [7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocolo foi amplamente adotado no mercado, com suporte dos fornecedores como Cisco, Brocade, Dell, HP e outros. A HNV agora também dá suporte a esse esquema de encapsulamento usando o modo de distribuição do MAC por meio do controlador de rede da Microsoft para mapeamentos de programa para locatário sobreposição endereços IP de rede (endereço do cliente ou da autoridade de certificação) para os endereços IP de rede física de base (provedor Endereço ou PA). NVGRE e a VXLAN descarrega as tarefas têm suporte para melhorar o desempenho por meio de drivers de terceiros.  
  
### <a name="SLB"></a>Interoperabilidade de Balanceador de carga (SLB) de software  
Windows Server 2016 inclui um balanceador de carga de software (SLB) com suporte completo para o tráfego de rede virtual e a interação direta com a HNV. O SLB é implementado por meio do mecanismo de fluxo de alto desempenho em que o plano de dados v-Switch e controlado pelo controlador de rede para o IP Virtual (VIP) / dinâmico mapeamentos do IP (DIP).  
  
### <a name="L2"></a>Compatível com cabeçalhos de Ethernet IEEE  
A HNV implementa cabeçalhos de L2 Ethernet corretos para garantir a interoperabilidade com terceiros dispositivos virtuais e físicos que dependem de protocolos padrão do setor. Microsoft garante que o todos os pacotes transmitidos tenham valores em conformidade com todos os campos para garantir que essa interoperabilidade. Além disso, o suporte para quadros Jumbo (MTU > 1780) da rede física de L2 será necessários para a conta para pacote de sobrecarga introduzido por protocolos de encapsulamento (NVGRE, VXLAN), garantindo a manutenção de máquinas virtuais conectadas a uma rede Virtual da HNV de convidado um MTU 1514.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão Geral da Virtualização de Rede Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Detalhes técnicos da Virtualização de Rede Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Rede definida pelo software](../../Software-Defined-Networking--SDN-.md)  
  