---
title: Configuração do Hyper-V
description: Considerações sobre a configuração do Hyper-V para ajuste de desempenho
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e5e55ad492439fcb7150469d9a35b639f5ff9a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830507"
---
# <a name="hyper-v-configuration"></a>Configuração do Hyper-V

## <a name="hardware-selection"></a>Seleção de hardware

As considerações de hardware para servidores que executam o Hyper-V geralmente são semelhantes dos servidores não-virtualizados, mas os servidores que executam o Hyper-V podem usar mais CPU, consome mais memória e precisam maior largura de banda de e/s devido à consolidação de servidores.

-   **Processadores**

    Hyper-V no Windows Server 2016 apresenta os processadores lógicos como um ou mais processadores virtuais para cada máquina virtual ativa. Agora, o Hyper-V exige processadores que dão suporte a tecnologias de conversão de endereço do segundo nível (SLAT) como estendido página EPT (tabelas) ou aninhados página NPT (tabelas).

-   **Cache**

    Hyper-V pode aproveitar os caches de processador maiores, especialmente para cargas que têm um grande conjunto na memória e em configurações de máquina virtual na qual a taxa de processadores virtuais para processadores lógicos é alta de trabalho.

-   **Memória**

    O servidor físico requer memória suficiente para as partições raiz e filho. A partição raiz requer memória para executar com eficiência e/SS em nome de máquinas virtuais e operações, como um instantâneo de máquina virtual. Hyper-V garante que a memória suficiente está disponível na partição raiz e permite que os restantes de memória a ser atribuído a partições filho. Partições filho devem ser dimensionadas com base nas necessidades da carga esperada para cada máquina virtual.

-   **Armazenamento**

    O hardware de armazenamento deve ter largura de banda de e/s suficiente e capacidade para atender às necessidades atuais e futuras de virtual de máquinas que os hosts de servidor físico. Considere os seguintes requisitos ao selecionar controladores de armazenamento e os discos e escolha a configuração de RAID. Colocação de máquinas virtuais com cargas de trabalho altamente intensivo de disco em discos físicos diferentes provavelmente melhorará o desempenho geral. Por exemplo, se quatro máquinas virtuais compartilham um único disco e ativamente usá-lo, cada máquina virtual pode gerar somente 25% da largura de banda de disco.

## <a name="power-plan-considerations"></a>As considerações sobre o plano de energia

Como uma tecnologia de núcleo, a virtualização é uma poderosa ferramenta útil para aumentar a densidade de carga de trabalho de servidor, reduzindo o número de servidores físicos necessários em seu datacenter, aumentando a eficiência operacional e reduzindo os custos de consumo de energia. Gerenciamento de energia é essencial para o gerenciamento de custos. 

Em um ambiente ideal de datacenter, o consumo de energia é gerenciado consolidação de trabalho nos computadores até que sejam mais ocupados e, em seguida, desligando máquinas ociosas. Se essa abordagem não é prática, os administradores podem aproveitar os planos de energia em hosts físicos para garantir que eles não consomem mais energia do que o necessário. 

Técnicas de gerenciamento de energia do servidor vêm com um custo, especialmente como locatário cargas de trabalho não são confiáveis para ditar política sobre a infra-estrutura física do hoster. O software de camada do host é deixado para inferir como maximizar a taxa de transferência, minimizando o consumo de energia. Em máquinas principalmente ocioso, isso pode causar a infraestrutura física concluir esse consumo de energia moderada é apropriado, resultando em cargas de trabalho de locatários individuais executadas mais lentamente do que eles podem, caso contrário.

Windows Server usa a virtualização em uma ampla variedade de cenários. De um servidor IIS pouco carregados para o SQL Server moderadamente ocupado, a um host de nuvem com o Hyper-V executando centenas de máquinas virtuais por servidor. Cada um desses cenários pode ter requisitos exclusivos de hardware, software e desempenho. Por padrão, o Windows Server usa e recomenda a **equilibrado** plano de energia que permite a conservação de energia por meio do dimensionamento de desempenho do processador com base na utilização da CPU.

Com o **equilibrado** plano de energia, os estados de energia mais altos (e latências mais baixas de resposta em cargas de trabalho de locatário) são aplicadas somente quando o host físico é relativamente ocupado. Se você valorizar resposta determinística e baixa latência para todas as cargas de trabalho de locatário, você deve considerar a alternância do padrão **equilibrado** plano de energia para o **alto desempenho** plano de energia. O **de alto desempenho** plano de energia executarão os processadores em plena velocidade o tempo todo, desabilitando efetivamente a comutação com base em demanda, juntamente com outras técnicas de gerenciamento de energia e otimizar o desempenho ao longo de economia de energia.

Para clientes, o que estiver satisfeito com as economias de custos, reduzindo o número de servidores físicos e quiser garantir que eles atingem o máximo desempenho para suas cargas de trabalho virtualizadas, você deve considerar usar o **de alto desempenho** plano de energia.

Para obter recomendações adicionais e insight sobre aproveitando os planos de energia para otimizar sua infra-estrutura, leia [recomendado com balanceamento de energia planejar parâmetros para os tempos de resposta rápida](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Opção de instalação do Server Core

A opção de instalação Server Core do recurso do Windows Server 2016. Server Core oferece um ambiente mínimo para a hospedagem de um conjunto selecionado de funções de servidor, inclusive o Hyper-V. Ele apresenta uma menor superfície de disco para o host do sistema operacional e um ataque de menor e manutenção de superfície. Portanto, é altamente recomendável que os servidores de virtualização do Hyper-V usam a opção de instalação Server Core.

Uma instalação Server Core oferece uma janela de console somente quando o usuário fizer logon, mas o Hyper-V expõe recursos de gerenciamento remoto, incluindo [Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx) para que os administradores podem gerenciar remotamente.

## <a name="dedicated-server-role"></a>Função de servidor dedicado

A partição raiz deve ser dedicada ao Hyper-V. Executando funções de servidor adicionais em um servidor que executa o Hyper-V pode afetar negativamente o desempenho do servidor de virtualização, especialmente se eles consomem largura de banda da CPU, memória ou e/s significativa. Minimizar as funções de servidor na partição raiz tem benefícios adicionais, como a redução da superfície de ataque.

Os administradores do sistema devem considerar cuidadosamente qual software está instalado na partição raiz porque algum software pode afetar negativamente o desempenho geral do servidor que executa o Hyper-V.

## <a name="guest-operating-systems"></a>Sistemas operacionais convidados

Hyper-V oferece suporte e foi ajustada para um número de sistemas operacionais convidados diferentes. O número de processadores virtuais por convidado com suporte depende do sistema operacional convidado. Para obter uma lista dos sistemas operacionais convidados com suporte, consulte [visão geral do Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Estatísticas de CPU

Hyper-V publica contadores de desempenho para ajudar a caracterizar o comportamento do servidor de virtualização e o uso de recursos de relatório. O conjunto padrão de ferramentas para exibir os contadores de desempenho no Windows inclui o Monitor de desempenho e Logman.exe, que pode exibir e registrar os contadores de desempenho do Hyper-V. Os nomes dos objetos de contador relevantes são prefixados com **Hyper-V**.

Você sempre deve medir o uso da CPU do sistema físico usando os contadores de desempenho do processador lógico do hipervisor do Hyper-V. A utilização da CPU contadores de Monitor de desempenho e Gerenciador de tarefas do relatório na raiz e partições filho não refletem o uso de CPU físico real. Use os seguintes contadores de desempenho para monitorar o desempenho:

-   **Processador lógico Hypervisor do Hyper-V (\*)\\% tempo de execução Total** o tempo não ocioso total dos processadores lógicos

-   **Processador lógico Hypervisor do Hyper-V (\*)\\% tempo de execução de convidado** o tempo gasto ciclos em execução dentro de um convidado ou dentro do host

-   **Processador lógico Hypervisor do Hyper-V (\*)\\% tempo de execução do hipervisor** o tempo gasto na execução do hipervisor

-   **Processador Virtual do Hyper-V hipervisor raiz (\*)\\ \***  mede o uso da CPU da partição raiz

-   **Processador Virtual do hipervisor do Hyper-V (\*)\\ \***  mede o uso da CPU de partições de convidado


## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura do Hyper-V](architecture.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
