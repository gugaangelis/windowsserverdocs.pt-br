---
title: Alta disponibilidade do controlador de rede
description: Você pode usar este tópico para saber mais sobre alta disponibilidade de controlador de rede para Software definido de rede (SDN) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>Alta disponibilidade do controlador de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre o controlador de rede alta disponibilidade e configuração de escalabilidade para \(SDN\) Software de rede definidos.

Quando você implanta SDN em seu datacenter, você pode usar o controlador de rede para implantar centralmente, monitorar e gerenciar vários elementos de rede, incluindo Gateways RAS, balanceadores de carga de Software, políticas de redes virtuais para comunicação de locatário, políticas de Firewall de Datacenter, qualidade de serviço \(QoS\) SDN políticas, políticas de rede híbrido, e muito mais.

Como o controlador de rede é a base de gerenciamento de SDN, é fundamental para implantações de controlador de rede fornecer alta disponibilidade e a capacidade para você ser facilmente dimensionada para cima ou para baixo de nós de controlador de rede com suas necessidades de datacenter.

Embora seja possível implantar o controlador de rede como um cluster única máquina, para alta disponibilidade e failover implante controlador de rede em um cluster de máquina vários com um mínimo de três computadores.

>[!NOTE]
>Você pode implantar o controlador de rede em qualquer um dos computadores de servidor ou em \(VMs\) máquinas virtuais que executam o Windows Server 2016 Datacenter edition. Se você implantar o controlador de rede em VMs, VMs devem estar executando em hosts Hyper-V que estejam executando o Datacenter edition. Controlador de rede não está disponível no Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controlador de rede como um aplicativo de malha do serviço

Para obter alta disponibilidade e escalabilidade, o controlador de rede depende Service Fabric. Service Fabric fornece uma plataforma de sistemas distribuídos para compilar escalável e confiável e fácil de gerenciar aplicativos.

Como uma plataforma, Service Fabric fornece funcionalidade que é necessária para a criação de um sistema distribuído escalonável. Ele fornece a hospedagem de serviços em várias instâncias de sistema operacional, a sincronização de informações de estado entre as instâncias, decidindo um líder, a detecção da falha, balanceamento de carga e muito mais.

>[!NOTE]
>Para obter informações sobre Service Fabric no Azure, consulte [visão geral do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Ao implantar o controlador de rede em vários computadores, o controlador de rede é executado como um único aplicativo Service Fabric em um cluster Service Fabric. É possível formar um cluster Service Fabric conectando-se um conjunto de instâncias de sistema operacional.

O aplicativo controlador de rede é composto de vários serviços Service Fabric com monitoração de estado. Cada serviço é responsável por uma função de rede, como gerenciamento de rede física, gerenciamento de rede virtual, gerenciamento de firewall ou gerenciamento de gateway. 

Cada serviço Service Fabric tem uma réplica principal e dois secundários. A réplica principais serviço processa as solicitações, enquanto as duas réplicas serviço secundário fornecem alta disponibilidade em circunstâncias em que a réplica principal é desabilitada ou não está disponível por algum motivo.

A ilustração a seguir mostra um cluster rede controlador Service Fabric com cinco computadores. Quatro serviços são distribuídos em cinco máquinas: serviço do Firewall, o serviço de Gateway, serviço de balanceamento de carga de Software \(SLB\) e serviço de \(Vnet\) de rede virtual.  Cada um dos quatro serviços inclui uma réplica principais serviço e dois serviço secundário.

![Cluster de controlador Service Fabric de rede](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantagens de usar Service Fabric

A seguir estão as principais vantagens da usando Service Fabric para clusters de controlador de rede.

### <a name="high-availability-and-scalability"></a>Alta disponibilidade e escalabilidade

Como controlador de rede é o núcleo de uma rede de datacenter, ele deve tanto resilientes a falhas e escalonável para permitir que o agile mudanças no datacenter redes ao longo do tempo. Os recursos a seguir fornecem desses capacidades: 

- **Failover rápido**. Service Fabric fornece failover extremamente rápida. Várias réplicas quente serviço secundários sempre estão disponíveis. Se uma instância do sistema operacional não estiver disponível devido a falha de hardware, uma das réplicas secundárias é promovida imediatamente a réplica principal. 
- **Agilidade de escala**. Você pode rapidez e facilidade dimensionar esses serviços confiáveis de algumas instâncias até milhares de instâncias e, em seguida, de volta para baixo para alguns casos, dependendo de suas necessidades de recurso. 

### <a name="persistent-storage"></a>Armazenamento persistente

O aplicativo controlador de rede tem requisitos de armazenamento grandes para sua configuração e o estado. O aplicativo também deve ser utilizável em planejado e interrupções. Para essa finalidade, Service Fabric fornece um \(KVS\) armazenamento de chave-valor que é um armazenamento persistente e replicado transacional.

### <a name="modularity"></a>Modularidade

Controlador de rede foi projetado com uma arquitetura modular, com cada um dos serviços de rede, como o serviço de redes virtuais e o serviço de firewall, built\-in como serviços individuais. 

Essa arquitetura de aplicativo fornece os seguintes benefícios.

1. Controlador de rede modularidade permite o desenvolvimento independente de cada um dos serviços com suporte, como necessidades evoluem. Por exemplo, o serviço de balanceamento de carga de Software pode ser atualizado sem afetar qualquer um dos outros serviços ou a operação normal do controlador de rede.
2. Modularidade de controlador de rede permite a adição de novos serviços, como a rede evolui. Novos serviços podem ser adicionados ao controlador de rede sem afetar os serviços existentes.

>[!NOTE]
>No Windows Server 2016, a adição de serviços de terceiros para o controlador de rede não é permitida.

Modularidade Service Fabric usa esquemas de modelo de serviço para maximizar a facilidade de desenvolvimento, implantação e manutenção de um aplicativo.

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando o System Center Virtual Machine Manager \(VMM\), veja [configurar um controlador de rede SDN na malha VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, veja [implantar um Software definidos rede infraestrutura usando Scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, veja [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Para obter mais informações sobre o controlador de rede, consulte [rede controlador](Network-Controller.md).