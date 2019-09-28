---
title: Arquitetura do Hyper-V
description: Condsiderations de arquitetura do Hyper-v para ajuste de desempenho
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0feb2977791dd181907c381e4898924ff51c2bc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383493"
---
# <a name="hyper-v-architecture"></a>Arquitetura do Hyper-V

O Hyper-V apresenta uma arquitetura baseada em hipervisor tipo 1. O hipervisor virtualiza processadores e memória e fornece mecanismos para a pilha de virtualização na partição raiz para gerenciar partições filho (máquinas virtuais) e expor serviços como dispositivos de e/s para as máquinas virtuais.

A partição raiz possui e tem acesso direto aos dispositivos de e/s físicos. A pilha de virtualização na partição raiz fornece um Gerenciador de memória para máquinas virtuais, APIs de gerenciamento e dispositivos de e/s virtualizados. Ele também implementa dispositivos emulados, como o controlador de disco IDE (Integrated Device Electronics) e a porta do dispositivo de entrada PS/2, e oferece suporte a dispositivos sintéticos específicos do Hyper-V para aumentar o desempenho e reduzir a sobrecarga.

![arquitetura baseada em hipervisor do Hyper-v](../../media/perftune-guide-hyperv-arch.png)

A arquitetura de e/s específica do Hyper-V consiste em provedores de serviço de virtualização (VSPs) na partição raiz e nos clientes de serviço de virtualização (VSCs) na partição filho. Cada serviço é exposto como um dispositivo por meio de VMBus, que atua como um barramento de e/s e permite a comunicação de alto desempenho entre máquinas virtuais que usam mecanismos como memória compartilhada. O Gerenciador de Plug and Play do sistema operacional convidado enumera esses dispositivos, incluindo o VMBus, e carrega os drivers de dispositivo apropriados (clientes de serviço virtual). Outros serviços além de e/s também são expostos por meio dessa arquitetura.

A partir do Windows Server 2008, o sistema operacional apresenta esclarecimentos para otimizar seu comportamento quando ele está em execução em máquinas virtuais. Os benefícios incluem a redução do custo da virtualização de memória, a melhoria da escalabilidade de vários núcleos e a redução do uso da CPU em segundo plano do sistema operacional convidado.

As seções a seguir sugerem práticas recomendadas que produzem maior desempenho em servidores que executam a função do Hyper-V.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
