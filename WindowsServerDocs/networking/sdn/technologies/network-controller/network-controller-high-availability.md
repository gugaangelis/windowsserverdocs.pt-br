---
title: Alta disponibilidade do controlador de rede
description: Você pode usar este tópico para saber mais sobre alta disponibilidade do controlador de rede para o Software Defined Networking (SDN) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813347"
---
# <a name="network-controller-high-availability"></a>Alta disponibilidade do controlador de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre rede alta disponibilidade e escalabilidade configuração do controlador de rede definida pelo Software \(SDN\).

Quando você implantar SDN em seu datacenter, você pode usar controlador de rede para implantar, monitorar e gerenciar vários elementos de rede, incluindo os Gateways de RAS, balanceadores de carga de Software, as políticas de rede virtuais para comunicação de locatário, centralmente, Firewall do Datacenter as políticas de qualidade de serviço \(QoS\) políticas SDN, políticas de rede híbrida e muito mais.

Como o controlador de rede é a base do gerenciamento de SDN, é essencial para implantações de controlador de rede fornecer alta disponibilidade e a capacidade de você a facilmente o dimensionamento para cima ou para baixo de nós de controlador de rede com suas necessidades de datacenter.

Embora você possa implantar o controlador de rede como um cluster único computador, para failover e alta disponibilidade você deve implantar o controlador de rede em um cluster de máquina vários com um mínimo de três máquinas.

>[!NOTE]
>Você pode implantar o controlador de rede nos computadores servidor ou em máquinas virtuais \(VMs\) que estiver executando o Windows Server 2016 Datacenter edition. Se você implantar o controlador de rede em máquinas virtuais, as VMs devem estar executando em hosts do Hyper-V que estejam executando o Datacenter edition. Controlador de rede não está disponível no Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controlador de rede como um aplicativo do Service Fabric

Para alcançar alta disponibilidade e escalabilidade, o controlador de rede depende do Service Fabric. O Service Fabric fornece uma plataforma de sistemas distribuídos para compilar escalonáveis, confiáveis e facilmente gerenciados de aplicativos.

Como uma plataforma, o Service Fabric fornece funcionalidade que é necessária para a criação de um sistema distribuído escalonável. Ele fornece hospedagem de serviços em várias instâncias do sistema operacional, sincronização de informações de estado entre instâncias, eleger um líder, detecção de falhas, balanceamento de carga e muito mais.

>[!NOTE]
>Para obter informações sobre o Service Fabric no Azure, consulte [visão geral do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Quando você implanta um controlador de rede em vários computadores, o controlador de rede é executado como um único aplicativo do Service Fabric em um cluster do Service Fabric. É possível formar um cluster do Service Fabric conectando-se um conjunto de instâncias do sistema operacional.

O aplicativo do controlador de rede é composto de vários serviços com monitoração de estado do Service Fabric. Cada serviço é responsável por uma função de rede, como gerenciamento de rede física, gerenciamento de rede virtual, gerenciamento de firewall ou gerenciamento de gateway. 

Cada serviço do Service Fabric tem uma réplica primária e duas réplicas secundárias. A réplica de serviço primário processa as solicitações, embora as duas réplicas de serviço secundário fornecem alta disponibilidade em circunstâncias em que a réplica primária está desabilitado ou indisponível por algum motivo.

A ilustração a seguir ilustra um cluster de malha de serviço do controlador de rede com cinco máquinas. Quatro serviços são distribuídos por cinco computadores: Balanceamento de carga do serviço, serviço de Gateway, Software de firewall \(SLB\) serviço e a rede virtual \(Vnet\) service.  Cada um dos quatro serviços inclui uma réplica de serviço primário e duas réplicas de serviço secundário.

![Cluster da malha de serviço do controlador de rede](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantagens de usar o Service Fabric

A seguir estão as principais vantagens de usar o Service Fabric para clusters de controlador de rede.

### <a name="high-availability-and-scalability"></a>Alta disponibilidade e escalabilidade

Porque o controlador de rede é o núcleo de uma rede de datacenter, ele deve ser resiliente a falhas tanto ser escalonável para permitir alterações agile em redes de datacenter ao longo do tempo. Os recursos a seguir fornecem essas capacidades: 

- **Failover rápido**. O Service Fabric fornece failover extremamente rápido. Várias réplicas de serviço secundário ativo estão sempre disponíveis. Se uma instância do sistema operacional ficar indisponível devido uma falha de hardware, uma das réplicas secundárias é promovida imediatamente à réplica primária. 
- **Agilidade de escala**. Você pode rapidamente e facilmente dimensionar esses serviços confiáveis de algumas instâncias até milhares de instâncias e, em seguida, volte para algumas instâncias, dependendo das suas necessidades de recursos. 

### <a name="persistent-storage"></a>Armazenamento persistente

O aplicativo do controlador de rede tem grandes requisitos de armazenamento para sua configuração e o estado. O aplicativo também deve ser utilizável entre interrupções planejadas e não planejadas. Para essa finalidade, o Service Fabric fornece uma chave-valor Store \(KVS\) que é um repositório replicado, transacional e persistente.

### <a name="modularity"></a>Modularidade

Controlador de rede foi projetado com uma arquitetura modular, com cada um dos serviços de rede, como o serviço de firewall e o serviço de redes virtuais criadas\-em como os serviços individuais. 

Essa arquitetura de aplicativo oferece os seguintes benefícios.

1. Controlador de rede modularidade permite desenvolvimento independente de cada um dos serviços com suporte, como as necessidades evoluem. Por exemplo, o serviço de balanceamento de carga de Software pode ser atualizado sem afetar qualquer um dos outros serviços ou a operação normal do controlador de rede.
2. Modularidade do controlador de rede permite a adição de novos serviços, à medida que a rede evolui. Novos serviços podem ser adicionados ao controlador de rede sem afetar os serviços existentes.

>[!NOTE]
>No Windows Server 2016, não há suporte para a adição de serviços de terceiros ao controlador de rede.

A modularidade do Service Fabric usa esquemas de modelo de serviço para maximizar a facilidade de desenvolvimento, implantação e manutenção de um aplicativo.

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando o System Center Virtual Machine Manager \(VMM\), consulte [configurar um controlador de rede SDN na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, consulte [implantar um Software definido infraestrutura usando Scripts de rede](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Para obter mais informações sobre o controlador de rede, consulte [controlador de rede](Network-Controller.md).