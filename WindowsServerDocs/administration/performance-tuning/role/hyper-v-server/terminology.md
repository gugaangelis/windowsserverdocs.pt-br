---
title: Terminologia do Hyper-V
description: Terminologia do Hyper-v útil no ajuste de desempenho do Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 88aaebaac9161849fefe8116a1115eb628bcbf9e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851759"
---
# <a name="hyper-v-terminology"></a>Terminologia do Hyper-V
Esta seção resume a principal terminologia específica para a tecnologia de máquina virtual que é usada em todo este tópico de ajuste de desempenho:

| Termo        | Definição           |
| ------------- |:------------|
|*partição filho* | Qualquer máquina virtual criada pela partição raiz.|
|*virtualização de dispositivos* | Um mecanismo que permite que um recurso de hardware seja abstrato e compartilhado entre vários consumidores.|
|*dispositivo emulado*|Um dispositivo virtualizado que imita um dispositivo de hardware físico real para que os convidados possam usar os drivers típicos para esse dispositivo de hardware.|
|*Iluminismo*|Uma otimização para um sistema operacional convidado para que ele reconheça os ambientes de máquinas virtuais e ajuste seu comportamento para máquinas virtuais.|
|*convite*|Software que está sendo executado em uma partição. Pode ser um sistema operacional completo ou um kernel pequeno e de finalidade especial. O hipervisor é independente de convidado.|
|*visor*|Uma camada de software que fica acima do hardware e abaixo de um ou mais sistemas operacionais. Seu trabalho principal é fornecer ambientes de execução isolados chamados partições. Cada partição tem seu próprio conjunto de recursos de hardware virtualizados (unidade de processamento central ou CPU, memória e dispositivos). O hipervisor controla e arbitra o acesso ao hardware subjacente.|
|*processador lógico*| Uma unidade de processamento que manipula um thread de execução (fluxo de instrução). Pode haver um ou mais processadores lógicos por núcleo do processador e um ou mais núcleos por soquete do processador.|
| *acesso ao disco de passagem*|Uma representação de um disco físico inteiro como um disco virtual dentro do convidado. Os dados e comandos são passados para o disco físico (por meio da pilha de armazenamento nativo da partição raiz) sem processamento intermediário pela pilha virtual.|
|*partição raiz*|A partição raiz que é criada primeiro e proprietária de todos os recursos que o hipervisor não tem, incluindo a maioria dos dispositivos e a memória do sistema. A partição raiz hospeda a pilha de virtualização e cria e gerencia as partições filho.|
|*Dispositivo específico do Hyper-V*|Um dispositivo virtualizado sem um hardware físico analógico, para que os convidados possam precisar de um driver (cliente de serviço de virtualização) para o dispositivo específico do Hyper-V. O driver pode usar o VMBus (barramento de máquina virtual) para se comunicar com o software de dispositivo virtualizado na partição raiz.|
|*máquina virtual*|Um computador virtual que foi criado pela emulação de software e tem as mesmas características de um computador real.|
| *comutador de rede virtual*|(também conhecido como um comutador virtual) Uma versão virtual de um comutador de rede física. Uma rede virtual pode ser configurada para fornecer acesso a recursos de rede locais ou externos para uma ou mais máquinas virtuais.|
|*processador virtual*|Uma abstração virtual de um processador que está agendado para ser executado em um processador lógico. Uma máquina virtual pode ter um ou mais processadores virtuais.|
|*cliente do serviço de virtualização (VSC)*|Um módulo de software que um convidado carrega para consumir um recurso ou serviço. Para dispositivos de e/s, o cliente do serviço de virtualização pode ser um driver de dispositivo carregado pelo kernel do sistema operacional.|
| *provedor de serviços de virtualização (VSP)*|  Um provedor exposto pela pilha de virtualização na partição raiz que fornece recursos ou serviços como e/s para uma partição filho.|
| *pilha de virtualização*|Uma coleção de componentes de software na partição raiz que funcionam em conjunto para dar suporte a máquinas virtuais. A pilha de virtualização funciona com e fica acima do hipervisor. Ele também fornece recursos de gerenciamento.|
|*VMBus*|Mecanismo de comunicação baseado em canal usado para comunicação entre partições e enumeração de dispositivos em sistemas com várias partições virtualizadas ativas. O VMBus é instalado com serviços de integração do Hyper-V.|

## <a name="see-also"></a>Consulte também

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
