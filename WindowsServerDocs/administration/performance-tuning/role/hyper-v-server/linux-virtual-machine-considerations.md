---
title: Considerações sobre a máquina virtual do Linux
description: Máquina virtual Linux e BSD
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: be9d2f0d758f6f59282c770ef1a48d71a947ba00
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078283"
---
# <a name="linux-virtual-machine-considerations"></a>Considerações sobre a máquina virtual do Linux

As máquinas virtuais Linux e BSD têm considerações adicionais em comparação com as máquinas virtuais do Windows no Hyper-V.

A primeira consideração é se Integration Services estão presentes ou se a VM está sendo executada meramente em hardware emulado sem esclarecimento. Uma tabela de versões do Linux e BSD que têm Integration Services internos ou baixáveis está disponível em [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](../../../../virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md). Essas páginas têm grades de recursos disponíveis do Hyper-V disponíveis para as versões de distribuição do Linux e observações sobre esses recursos, quando aplicável.

Mesmo quando o convidado está em execução Integration Services, ele pode ser configurado com hardware herdado que não apresenta o melhor desempenho. Por exemplo, configure e use um adaptador Ethernet virtual para o convidado em vez de usar um adaptador de rede herdado. Com o Windows Server 2016, a rede avançada, como SR-IOV, também está disponível.

## <a name="linux-network-performance"></a>Desempenho de rede do Linux

Por padrão, o Linux habilita aceleração e descarregamentos de hardware por padrão. Se o vRSS estiver habilitado nas propriedades de uma NIC no host e o convidado do Linux tiver a capacidade de usar o vRSS, a funcionalidade será habilitada. No PowerShell, esse mesmo parâmetro pode ser alterado com o `EnableNetAdapterRSS` comando.

Da mesma forma, o recurso VMMQ (RSS switch virtual) pode ser habilitado na NIC física usada pelas **Propriedades**de convidado  >  **Configure...**  >  Guia **avançado** > definir **RSS do comutador virtual** como **habilitado** ou habilitar VMMQ no PowerShell usando o seguinte:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

No, o ajuste TCP adicional pode ser realizado aumentando os limites. Para obter a melhor carga de trabalho de propagação de desempenho em várias CPUs e ter cargas de trabalho profundas produz a melhor taxa de transferência, já que as cargas de trabalho virtualizadas terão uma latência maior do que as "bare-metal".

Alguns parâmetros de ajuste de exemplo que foram úteis em benchmarks de rede incluem:

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

Uma ferramenta útil para netbenchmarks de rede é o ntttcp, que está disponível no Linux e no Windows. A versão do Linux é de software livre e está disponível [em ntttcp-para-Linux no github.com](https://github.com/Microsoft/ntttcp-for-linux). A versão do Windows pode ser encontrada no [centro de download](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Ao ajustar cargas de trabalho, é melhor usar tantos fluxos quantos forem necessários para obter a melhor taxa de transferência. Usando ntttcp para modelar o tráfego, o `-P` parâmetro define o número de conexões paralelas usadas.

## <a name="linux-storage-performance"></a>Desempenho do armazenamento do Linux

Algumas práticas recomendadas, como as seguintes, estão listadas em [práticas recomendadas para executar o Linux no Hyper-V](../../../../virtualization/hyper-v/best-practices-for-running-linux-on-hyper-v.md). O kernel do Linux tem diferentes agendadores de e/s para reordenar solicitações com algoritmos diferentes. NOOP é uma fila de primeiro a entrar, que passa a decisão de agendamento para ser feita pelo hipervisor. É recomendável usar NOOP como o Agendador ao executar a máquina virtual do Linux no Hyper-V. Para alterar o Agendador de um dispositivo específico, na configuração do carregador de inicialização (/etc/grub.conf, por exemplo), adicione `elevator=noop` aos parâmetros do kernel e reinicie o.

Semelhante à rede, o desempenho de convidado do Linux com o armazenamento beneficia o máximo de várias filas com profundidade suficiente para manter o host ocupado. O desempenho de armazenamento de MicroBenchMark é provavelmente melhor com a ferramenta de benchmark fio com o mecanismo libaio.

## <a name="additional-references"></a>Referências adicionais

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)