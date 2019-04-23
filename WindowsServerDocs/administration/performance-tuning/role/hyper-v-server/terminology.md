---
title: Terminologia do Hyper-V
description: Terminologia do Hyper-v úteis no ajuste de desempenho do Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841137"
---
# <a name="hyper-v-terminology"></a>Terminologia do Hyper-V
Esta seção resume a terminologia chave específica à tecnologia de máquina virtual que é usada em todo este tópico de ajuste de desempenho:

| Termo        | Definição           |
| ------------- |:------------|
|*partição filho* | Qualquer máquina virtual que é criada pela partição raiz.|
|*virtualização de dispositivo* | Um mecanismo que permite que um hardware recursos ser abstraídos e compartilhado entre vários consumidores.|
|*dispositivo emulado*|Um dispositivo virtualizado que simula um dispositivo de hardware físico real, para que os convidados podem usar os drivers típicos para esse dispositivo de hardware.|
|*enlightenment*|Uma otimização para um sistema operacional de convidado para que ele fique ciente ambientes de máquina virtual e ajustar seu comportamento para máquinas virtuais.|
|*guest*|Software que está em execução em uma partição. Ele pode ser um sistema de operacional completo ou um kernel pequeno, com finalidade especial. O hipervisor é independente de convidado.|
|*hypervisor*|Uma camada de software que fica acima do hardware e abaixo de um ou mais sistemas operacionais. Seu trabalho principal é fornecer ambientes de execução isolados chamados partições. Cada partição tem seu próprio conjunto de recursos de hardware virtualizado (unidade de processamento central ou CPU, memória e dispositivos). O hipervisor controla e arbitra o acesso ao hardware subjacente.|
|*processador lógico*| Uma unidade de processamento que manipula um thread de execução (fluxo de instruções). Pode haver um ou mais processadores lógicos por núcleo de processador e um ou mais núcleos por soquete de processador.|
| *acesso ao disco de passagem*|Uma representação de um disco físico inteiro como um disco virtual no convidado. Os comandos e os dados são passados para o disco físico (por meio da pilha de armazenamento nativo da partição raiz) sem processamento intermediária pela pilha virtual.|
|*partição raiz*|A partição raiz que é criada pela primeira vez e detém todos os recursos que o hipervisor não oferece, inclusive a maioria dos dispositivos e memória do sistema. A partição raiz hospeda a pilha de virtualização e cria e gerencia as partições filho.|
|*Dispositivo específico do Hyper-V*|Um dispositivo virtualizado com nenhum analógica do hardware físico, portanto, convidados talvez seja necessário um driver (cliente de serviço de virtualização) que o dispositivo específico do Hyper-V. O driver pode usar o barramento da máquina virtual (VMBus) para se comunicar com o software de dispositivo virtualizado na partição raiz.|
|*máquina virtual*|Um computador virtual que foi criado por emulação de software e tem as mesmas características de um computador real.|
| *comutador de rede virtual*|(também conhecido como um comutador virtual) Uma versão virtual de um comutador de rede física. Uma rede virtual pode ser configurada para fornecer acesso aos recursos de rede local ou externa para uma ou mais máquinas virtuais.|
|*processador virtual*|Uma abstração de virtual de um processador que está agendado para ser executado em um processador lógico. Uma máquina virtual pode ter um ou mais processadores virtuais.|
|*cliente do serviço de virtualização (VSC)*|Um módulo de software que é carregado de um convidado para consumir um recurso ou serviço. Para dispositivos de e/s, o cliente do serviço de virtualização pode ser um driver de dispositivo que carrega o kernel do sistema operacional.|
| *provedor de serviços de virtualização (VSP)*|  Um provedor exposto pela pilha de virtualização na partição raiz, que fornece recursos ou serviços, como e/s para uma partição filho.|
| *pilha de virtualização*|Uma coleção de componentes de software na partição raiz que trabalham juntos para dar suporte a máquinas virtuais. A pilha de virtualização funciona com e fica acima do hipervisor. Ele também fornece recursos de gerenciamento.|
|*VMBus*|Mecanismo de comunicação baseada em canal usado para enumeração de dispositivo e a comunicação entre partições em sistemas com várias partições virtualizados Active Directory. O VMBus é instalado com serviços de integração do Hyper-V.|

## <a name="see-also"></a>Consulte também

-   [Arquitetura do Hyper-V](architecture.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
