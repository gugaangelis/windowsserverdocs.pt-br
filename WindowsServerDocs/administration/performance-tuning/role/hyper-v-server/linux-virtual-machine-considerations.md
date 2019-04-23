---
title: Considerações sobre a máquina Virtual Linux
description: Máquina virtual Linux e BSD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869677"
---
# <a name="linux-virtual-machine-considerations"></a>Considerações sobre a máquina Virtual Linux

BSD máquinas virtuais Linux e apresentam considerações adicionais em comparação às máquinas de virtuais do Windows no Hyper-V.

A primeira questão é se os serviços de integração estão presentes ou se a VM estiver executando apenas em hardware emulada com nenhum iluminismo. Uma tabela de versões do Linux e BSD com serviços de integração interna ou para download está disponível no [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). Essas páginas têm a grades de recursos disponíveis do Hyper-V disponíveis para versões de distribuição do Linux e observações sobre esses recursos, onde aplicável.

Mesmo quando o convidado está executando serviços de integração, ele pode ser configurado com hardware herdado que não apresenta o melhor desempenho. Por exemplo, configurar e usar um adaptador de ethernet virtual para o convidado em vez de usar um adaptador de rede herdado. Com o Windows Server 2016, sistema de rede avançado, como SR-IOV também estão disponíveis.

## <a name="linux-network-performance"></a>Desempenho de rede do Linux

Linux por padrão permite a aceleração de hardware e reduz a carga por padrão. Se o vRSS é ativado nas propriedades de uma NIC no host e convidado Linux tem a capacidade de usar o vRSS o recurso será habilitado. No Powershell, esse mesmo parâmetro pode ser alterado com o `EnableNetAdapterRSS` comando.

Da mesma forma, o recurso VMMQ (RSS do comutador Virtual) pode ser habilitado na NIC física usada pelo convidado **propriedades** > **configurar...**   >  **Avançado** guia > definir **RSS do comutador Virtual** para **habilitado** ou habilitar VMMQ no Powershell usando o seguinte:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

No convidado ajuste adicional de TCP pode ser executado, aumentando os limites. Para obter o melhor desempenho distribuir a carga de trabalho em várias CPUs e profunda das cargas de trabalho gera a melhor taxa de transferência, como cargas de trabalho virtualizadas terão latência mais alta do que "bare metal" aqueles.

Alguns parâmetros de ajuste de exemplo que tenham sido úteis em benchmarks de rede incluem:

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

Uma ferramenta útil para a rede microbenchmarks é ntttcp, que está disponível no Linux e Windows. A versão do Linux é o código-fonte aberto e disponível no [ntttcp-for-linux em github.com](https://github.com/Microsoft/ntttcp-for-linux). A versão do Windows pode ser encontrada na [Centro de download](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Ao ajustar cargas de trabalho é melhor usar fluxos tantos conforme necessário para obter a melhor taxa de transferência. Usando o ntttcp ao tráfego de modelo, o `-P` parâmetro define o número de conexões paralelas usadas.

## <a name="linux-storage-performance"></a>Desempenho de armazenamento do Linux

Algumas práticas recomendadas, como a seguir, são listadas na [práticas recomendadas para Linux em execução no Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). O kernel do Linux tem diferentes agendadores de e/s para reordenar as solicitações com algoritmos diferentes. NOOP é uma fila de primeiro a entrar primeiro a sair que passa a decisão de agenda a ser feita pelo hipervisor. É recomendável usar NOOP como o Agendador ao executar a máquina virtual do Linux no Hyper-V. Para alterar o Agendador para um dispositivo específico, na configuração do carregador de inicialização (/ etc/grub.conf, por exemplo), adicione `elevator=noop` aos parâmetros de kernel e reinicie.

Semelhante ao sistema de rede, desempenho de convidado do Linux com o armazenamento beneficia o máximo de várias filas com profundidade suficiente para manter o host ocupado. Desempenho do armazenamento Microbenchmarking é provavelmente melhor com a ferramenta de parâmetro de comparação de fio com o mecanismo libaio.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura do Hyper-V](architecture.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)
