---
title: Alta disponibilidade do controlador de rede
description: Você pode usar este tópico para saber mais sobre a alta disponibilidade do controlador de rede para SDN (rede definida pelo software) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ce3a0dd33ff105fa7cc36305048b8a311577aa21
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317061"
---
# <a name="network-controller-high-availability"></a>Alta disponibilidade do controlador de rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a configuração de alta disponibilidade e escalabilidade do controlador de rede para redes definidas pelo software \(SDN\).

Ao implantar o SDN em seu datacenter, você pode usar o controlador de rede para implantar, monitorar e gerenciar centralmente vários elementos de rede, incluindo gateways RAS, balanceadores de carga de software, políticas de rede virtual para comunicação de locatário, políticas de firewall de datacenter, qualidade de serviço \(\) de QoS para políticas de SDN, políticas de rede híbrida e muito mais.

Como o controlador de rede é a base do gerenciamento de SDN, é essencial que as implantações de controlador de rede forneçam alta disponibilidade e a capacidade de você dimensionar ou reduzir facilmente os nós do controlador de rede com suas necessidades de datacenter.

Embora seja possível implantar o controlador de rede como um cluster de computador único, para alta disponibilidade e failover, você deve implantar o controlador de rede em um cluster de vários computadores com um mínimo de três computadores.

>[!NOTE]
>Você pode implantar o controlador de rede em computadores de servidor ou em máquinas virtuais \(VMs\) que estejam executando o Windows Server 2016 Datacenter Edition. Se você implantar o controlador de rede em VMs, as VMs deverão estar em execução em hosts Hyper-V que também estejam executando o Datacenter Edition. O controlador de rede não está disponível no Windows Server 2016 Standard Edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Controlador de rede como um aplicativo Service Fabric

Para obter alta disponibilidade e escalabilidade, o controlador de rede conta com Service Fabric. O Service Fabric fornece uma plataforma de sistemas distribuídos para criar aplicativos escalonáveis, confiáveis e facilmente gerenciados.

Como plataforma, a Service Fabric fornece funcionalidade necessária para a criação de um sistema distribuído escalonável. Ele fornece Hospedagem de serviço em várias instâncias de sistema operacional, sincronizando informações de estado entre instâncias, optando por um líder, detecção de falhas, balanceamento de carga e muito mais.

>[!NOTE]
>Para obter informações sobre Service Fabric no Azure, consulte [visão geral da Service Fabric do Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Quando você implanta o controlador de rede em vários computadores, o controlador de rede é executado como um único aplicativo de Service Fabric em um Cluster Service Fabric. Você pode formar um Cluster Service Fabric conectando um conjunto de instâncias do sistema operacional.

O aplicativo do controlador de rede é composto por vários serviços de Service Fabric com estado. Cada serviço é responsável por uma função de rede, como gerenciamento de rede física, gerenciamento de rede virtual, gerenciamento de firewall ou gerenciamento de gateway. 

Cada serviço de Service Fabric tem uma réplica primária e duas réplicas secundárias. A réplica de serviço primário processa solicitações, enquanto as duas réplicas de serviço secundárias fornecem alta disponibilidade em circunstâncias em que a réplica primária está desabilitada ou indisponível por algum motivo.

A ilustração a seguir descreve um cluster de Service Fabric de controlador de rede com cinco computadores. Quatro serviços são distribuídos entre as cinco máquinas: serviço de firewall, serviço de gateway, balanceamento de carga de software \(serviço de\) SLB e rede virtual \(serviço de\) vnet.  Cada um dos quatro serviços inclui uma réplica de serviço primário e duas réplicas de serviço secundárias.

![Cluster de Service Fabric do controlador de rede](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Vantagens de usar o Service Fabric

A seguir estão as principais vantagens para usar Service Fabric para clusters de controlador de rede.

### <a name="high-availability-and-scalability"></a>Alta disponibilidade e escalabilidade

Como o controlador de rede é o núcleo de uma rede de datacenter, ele deve ser resiliente a falhas e ser escalonável o suficiente para permitir alterações ágeis em redes de data center ao longo do tempo. Os recursos a seguir fornecem essas capacidades: 

- **Failover rápido**. Service Fabric fornece um failover extremamente rápido. Várias réplicas de serviço de Hot-Secondary estão sempre disponíveis. Se uma instância do sistema operacional ficar indisponível devido a uma falha de hardware, uma das réplicas secundárias será imediatamente promovida para a réplica primária. 
- **Agilidade da escala**. Você pode dimensionar facilmente e rapidamente esses Reliable Services de algumas instâncias até milhares de instâncias e, em seguida, fazer backup em algumas instâncias, dependendo de suas necessidades de recursos. 

### <a name="persistent-storage"></a>Armazenamento persistente

O aplicativo do controlador de rede tem grandes requisitos de armazenamento para sua configuração e estado. O aplicativo também deve ser utilizável entre interrupções planejadas e não planejadas. Para essa finalidade, Service Fabric fornece um repositório de chave-valor \(KVS\) que é um repositório replicado, transacional e persistente.

### <a name="modularity"></a>Modularidade

O controlador de rede é projetado com uma arquitetura modular, com cada um dos serviços de rede, como o serviço de redes virtuais e o serviço de firewall, criado\-no como serviços individuais. 

Essa arquitetura de aplicativo oferece os seguintes benefícios.

1. A modularidade do controlador de rede permite o desenvolvimento independente de cada um dos serviços com suporte, conforme as necessidades evoluem. Por exemplo, o serviço de balanceamento de carga de software pode ser atualizado sem afetar nenhum dos outros serviços ou a operação normal do controlador de rede.
2. A modularidade do controlador de rede permite a adição de novos serviços, à medida que a rede evolui. Novos serviços podem ser adicionados ao controlador de rede sem afetar os serviços existentes.

>[!NOTE]
>No Windows Server 2016, não há suporte para a adição de serviços de terceiros ao controlador de rede.

A modularidade de Service Fabric usa esquemas de modelo de serviço para maximizar a facilidade de desenvolvimento, implantação e manutenção de um aplicativo.

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando System Center Virtual Machine Manager \(\)do VMM, consulte [configurar um controlador de rede Sdn na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, consulte [implantar uma infraestrutura de rede definida pelo software usando scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Para obter mais informações sobre o controlador de rede, consulte [controlador de rede](Network-Controller.md).