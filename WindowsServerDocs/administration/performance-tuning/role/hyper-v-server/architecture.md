---
title: Arquitetura do Hyper-V
description: Arquitetura do Hyper-v condsiderations para ajuste de desempenho
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890277"
---
# <a name="hyper-v-architecture"></a>Arquitetura do Hyper-V

Hyper-V apresenta uma arquitetura baseada em hipervisor de tipo 1. O hipervisor virtualiza processadores e memória e fornece mecanismos para a pilha de virtualização na partição raiz para gerenciar partições filho (máquinas virtuais) e expor serviços, como dispositivos de e/s para as máquinas virtuais.

A partição raiz possui e tem acesso direto aos dispositivos físicos e/s. A pilha de virtualização na partição raiz fornece um Gerenciador de memória para máquinas virtuais, APIs de gerenciamento e dispositivos de e/s virtualizados. Ele também implementa dispositivos emulados, como o controlador de disco integrados de dispositivos eletrônicos (IDE) e porta do dispositivo de entrada de PS/2 e ele dá suporte a dispositivos sintéticos de específico do Hyper-V para melhorar o desempenho e redução da sobrecarga.

![arquitetura baseada em hipervisor do Hyper-v](../../media/perftune-guide-hyperv-arch.png)

A arquitetura de e/s de específico do Hyper-V consiste em provedores de serviços de virtualização (VSPs) nas raiz partição e virtualização de clientes de serviço (VSCs) na partição filho. Cada serviço é exposto como um dispositivo por VMBus, que atua como um barramento de e/s e permite a comunicação de alto desempenho entre máquinas virtuais que usam mecanismos, como a memória compartilhada. Gerenciador de Plug and Play do sistema operacional convidado enumera nesses dispositivos, incluindo o VMBus e carrega os drivers de dispositivo apropriado (clientes de serviço virtual). Serviços diferentes e/s também são expostos por meio dessa arquitetura.

Começando com o Windows Server 2008, os esclarecimentos de recursos do sistema operacional para otimizar seu comportamento durante a execução em máquinas virtuais. Os benefícios incluem a redução do custo da virtualização de memória, melhorando a escalabilidade de vários núcleos e diminuir o uso da CPU do sistema operacional convidado do plano de fundo.

As seções a seguir sugerem práticas recomendadas que geram o aumento do desempenho em servidores que executam a função Hyper-V.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
