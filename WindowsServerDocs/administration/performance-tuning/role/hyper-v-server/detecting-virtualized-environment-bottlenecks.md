---
title: Detectar gargalos em um ambiente virtualizado
description: Como detectar e resolver possíveis gargalos de desempenho do Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867527"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Detectar gargalos em um ambiente virtualizado

Esta seção deve lhe dar algumas dicas sobre o que monitorar usando o Monitor de desempenho e como identificar onde o problema pode ser quando o host ou algumas das máquinas virtuais não executam conforme você deve ter esperado.

## <a name="processor-bottlenecks"></a>Gargalos do processador

Aqui estão alguns cenários comuns que podem causar gargalos do processador:

-   Um ou mais processadores lógicos são carregados

-   Um ou mais processadores virtuais são carregados

Você pode usar os seguintes contadores de desempenho do host:

-   Utilização de processador lógico - \\processador lógico Hypervisor do Hyper-V (\*)\\Total de % tempo de execução

-   Utilização do processador virtual - \\processador Virtual do hipervisor Hyper-V (\*)\\Total de % tempo de execução

-   Raiz de utilização do processador Virtual - \\processador Virtual da raiz de hipervisor Hyper-V (\*)\\Total de % tempo de execução

Se o **processador lógico Hypervisor do Hyper-V (\_Total)\\% tempo de execução Total** contador é mais de 90%, o host está sobrecarregado. Você deve adicionar mais poder de processamento ou mover algumas máquinas virtuais em um host diferente.

Se o **Hyper-V hipervisor Virtual Processor(VM Name:VP x)\\% tempo de execução Total** contador é mais de 90% de todos os processadores virtuais, você deve fazer o seguinte:

-   Verifique se o host não está sobrecarregado

-   Descubra se a carga de trabalho pode aproveitar mais processadores virtuais

-   Atribuir mais processadores virtuais para a máquina virtual

Se **Hyper-V hipervisor Virtual Processor(VM Name:VP x)\\% tempo de execução Total** contador é mais de 90% para alguns, mas não todos, dos processadores virtuais, você deve fazer o seguinte:

-   Se sua carga de trabalho é receber de rede intensivo, você deve considerar usar o vRSS.

-   Se as máquinas virtuais não estão executando o Windows Server 2012 R2, você deve adicionar mais adaptadores de rede.

-   Se sua carga de trabalho faz uso intensivo de armazenamento, você deve habilitar o NUMA virtual e adicionar mais discos virtuais.

Se o **processador Virtual do Hyper-V hipervisor raiz (VP de raiz x)\\% tempo de execução Total** contador é de mais de 90% para alguns, mas nem todos os processadores virtuais e o **processador (x)\\% tempo de interrupção e Processador (x)\\% tempo de DPC** contador aproximadamente contribui para o valor para o **raiz de Processor(Root VP x) Virtual\\% tempo de execução Total** do contador, certifique-se habilitar a VMQ em os adaptadores de rede.

## <a name="memory-bottlenecks"></a>Afunilamentos de memória

Aqui estão alguns cenários comuns que podem causar afunilamentos de memória:

-   O host não está respondendo.

-   As máquinas virtuais não pode ser iniciadas.

-   Máquinas virtuais ficar sem memória.

Você pode usar os seguintes contadores de desempenho do host:

-   Memória\\Mbytes disponíveis

-   Balanceador de memória dinâmica do Hyper-V (\*)\\memória disponível

Você pode usar os seguintes contadores de desempenho da máquina virtual:

-   Memória\\Mbytes disponíveis

Se o **memória\\Mbytes disponíveis** e **balanceador de memória dinâmica do Hyper-V (\*)\\memória disponível** contadores são baixos no host, você deve parar não essenciais de serviços e migração de uma ou mais máquinas virtuais para outro host.

Se o **memória\\Mbytes disponíveis** contador é baixa na máquina virtual, você deve atribuir mais memória à máquina virtual. Se você estiver usando a memória dinâmica, você deve aumentar a configuração máxima de memória.

## <a name="network-bottlenecks"></a>Gargalos de rede

Aqui estão alguns cenários comuns que podem causar gargalos de rede:

-   O host é de rede associado.

-   A máquina virtual é rede associada.

Você pode usar os seguintes contadores de desempenho do host:

-   Interface de rede (*nome do adaptador de rede*)\\Bytes/s

Você pode usar os seguintes contadores de desempenho da máquina virtual:

-   O adaptador de rede Virtual do Hyper-V (*nome da máquina virtual&lt;GUID&gt;*)\\Bytes/s

Se o **NIC física Bytes/s** contador for maior que ou igual a 90% da capacidade, você deve adicionar adaptadores de rede adicionais, migrar as máquinas virtuais para outro host e configurar o QoS de rede.

Se o **Bytes/s do adaptador de rede Virtual do Hyper-V** contador for maior que ou igual a 250 MBps, você deve adicionar adaptadores de rede emparelhados adicionais na máquina virtual, habilite o vRSS e usar SR-IOV.

Se suas cargas de trabalho não podem atender sua latência de rede, habilite o SR-IOV apresentar os recursos do adaptador de rede física para a máquina virtual.

## <a name="storage-bottlenecks"></a>Gargalos de armazenamento

Aqui estão alguns cenários comuns que podem causar gargalos de armazenamento:

-   O host e a máquina virtual de operações são lentos ou o tempo limite.

-   A máquina virtual está lenta.

Você pode usar os seguintes contadores de desempenho do host:

-   Disco físico (*letra de disco*)\\média de disco s/leitura

-   Disco físico (*letra de disco*)\\média de disco s/gravação

-   Disco físico (*letra de disco*)\\da fila de leitura de média de disco

-   Disco físico (*letra de disco*)\\comprimento da fila de gravação de média de disco

Se as latências são consistentemente superiores a 50 ms, você deve fazer o seguinte:

-   Distribuir as máquinas virtuais entre armazenamento adicional

-   Considere a compra de um armazenamento mais rápido

-   Considere a possibilidade de espaços de armazenamento em camadas, que foi introduzida no Windows Server 2012 R2

-   Considere o uso de QoS de armazenamento, que foi introduzida no Windows Server 2012 R2

-   Use VHDX

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura do Hyper-V](architecture.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
