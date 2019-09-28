---
title: Configuração do Hyper-V
description: Considerações de configuração do Hyper-V para ajuste de desempenho
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f21115265ca7d2788fc0be078860048602d82c0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370163"
---
# <a name="hyper-v-configuration"></a>Configuração do Hyper-V

## <a name="hardware-selection"></a>Seleção de hardware

As considerações de hardware para servidores que executam o Hyper-V geralmente são semelhantes aos de servidores não virtualizados, mas os servidores que executam o Hyper-V podem apresentar um aumento no uso da CPU, consumir mais memória e precisar de largura de banda de e/s maior devido à consolidação do servidor.

-   **Duo**

    O Hyper-V no Windows Server 2016 apresenta os processadores lógicos como um ou mais processadores virtuais para cada máquina virtual ativa. Agora, o Hyper-V requer processadores que dão suporte a tecnologias de conversão de endereços de segundo nível (SLAT), como EPT (tabelas de páginas estendidas) ou NPT (tabelas de páginas aninhadas).

-   **Armazenar**

    O Hyper-V pode se beneficiar de caches de processador maiores, especialmente para cargas que têm um grande conjunto de trabalho na memória e em configurações de máquina virtual nas quais a taxa de processadores virtuais para processadores lógicos é alta.

-   **Memória**

    O servidor físico requer memória suficiente para as partições raiz e filho. A partição raiz requer memória para executar e/SS com eficiência em nome das máquinas virtuais e operações como um instantâneo de máquina virtual. O Hyper-V garante que memória suficiente esteja disponível para a partição raiz e permite que a memória restante seja atribuída a partições filho. As partições filho devem ser dimensionadas com base nas necessidades da carga esperada para cada máquina virtual.

-   **Armazenamento**

    O hardware de armazenamento deve ter largura de banda de e/s suficiente e capacidade para atender às necessidades atuais e futuras das máquinas virtuais que o servidor físico hospeda. Considere esses requisitos ao selecionar os controladores de armazenamento e os discos e escolher a configuração de RAID. Colocar máquinas virtuais com cargas de trabalho com grande volume de disco em discos físicos diferentes provavelmente melhorará o desempenho geral. Por exemplo, se quatro máquinas virtuais compartilharem um único disco e a utilizarem ativamente, cada máquina virtual poderá produzir apenas 25% da largura de banda do disco.

## <a name="power-plan-considerations"></a>Considerações do plano de energia

Como tecnologia principal, a virtualização é uma poderosa ferramenta útil para aumentar a densidade da carga de trabalho do servidor, reduzindo o número de servidores físicos necessários em seu datacenter, aumentando a eficiência operacional e reduzindo os custos de consumo de energia. O gerenciamento de energia é essencial para o gerenciamento de custos. 

Em um ambiente de datacenter ideal, o consumo de energia é gerenciado por meio da consolidação de trabalho em computadores até que eles estejam mais ocupados e desligando computadores ociosos. Se essa abordagem não for prática, os administradores poderão aproveitar os planos de energia nos hosts físicos para garantir que eles não consumam mais energia do que o necessário. 

As técnicas de gerenciamento de energia do servidor vêm com um custo, especialmente porque as cargas de trabalho de locatário não são confiáveis para ditar a política sobre a infraestrutura física do hoster. O software da camada do host é deixado em inferir como maximizar a taxa de transferência e minimizar o consumo de energia. Em máquinas quase ociosas, isso pode fazer com que a infraestrutura física conclua que o consumo de energia moderado é apropriado, resultando em cargas de trabalho de locatário individuais em execução mais lenta do que de outra forma.

O Windows Server usa a virtualização em uma ampla variedade de cenários. De um servidor IIS levemente carregado a um SQL Server de ocupação moderada, em um host de nuvem com o Hyper-V executando centenas de máquinas virtuais por servidor. Cada um desses cenários pode ter requisitos de hardware, software e desempenho exclusivos. Por padrão, o Windows Server usa e recomenda o plano de energia **equilibrado** que permite a conservação de energia, dimensionando o desempenho do processador com base na utilização da CPU.

Com o plano de energia **equilibrado** , os mais altos Estados de energia (e as latências de resposta mais baixas em cargas de trabalho de locatário) são aplicados somente quando o host físico está relativamente ocupado. Se você valor de uma resposta determinística e de baixa latência para todas as cargas de trabalho de locatário, considere alternar do plano de energia **equilibrado** padrão para o plano de energia de **alto desempenho** . O plano de energia de **alto desempenho** executará os processadores em plena velocidade o tempo todo, desabilitando efetivamente a alternância baseada em demanda juntamente com outras técnicas de gerenciamento de energia e otimizando o desempenho em relação à economia de energia.

Para os clientes que estão satisfeitos com a economia de custos com a redução do número de servidores físicos e desejam garantir que eles atinjam o desempenho máximo para suas cargas de trabalho virtualizadas, você deve considerar o uso do plano de energia de **alto desempenho** .

Para obter recomendações adicionais e informações sobre como aproveitar os planos de energia para otimizar sua infraestrutura, leia [parâmetros de plano de energia balanceados recomendados para tempos de resposta rápidos](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Opção de instalação do Server Core

O Windows Server 2016 recurso a opção de instalação Server Core. O Server Core oferece um ambiente mínimo para hospedar um conjunto selecionado de funções de servidor, incluindo o Hyper-V. Ele apresenta uma superfície de disco menor para o sistema operacional do host, além de um ataque menor e uma área de manutenção. Portanto, é altamente recomendável que os servidores de virtualização do Hyper-V usem a opção de instalação Server Core.

Uma instalação do Server Core oferece uma janela de console somente quando o usuário está conectado, mas o Hyper-V expõe recursos de gerenciamento remoto, incluindo o [Windows PowerShell](https://technet.microsoft.com/library/hh848559.aspx) para que os administradores possam gerenciá-lo remotamente.

## <a name="dedicated-server-role"></a>Função de servidor dedicada

A partição raiz deve ser dedicada ao Hyper-V. A execução de funções de servidor adicionais em um servidor que executa o Hyper-V pode afetar negativamente o desempenho do servidor de virtualização, especialmente se consumirem CPU significativa, memória ou largura de banda de e/s. Minimizar as funções de servidor na partição raiz tem benefícios adicionais, como a redução da superfície de ataque.

Os administradores do sistema devem considerar cuidadosamente qual software está instalado na partição raiz porque alguns softwares podem afetar negativamente o desempenho geral do servidor que executa o Hyper-V.

## <a name="guest-operating-systems"></a>Sistemas operacionais convidados

O Hyper-V dá suporte e foi ajustado para vários sistemas operacionais convidados diferentes. O número de processadores virtuais com suporte por convidado depende do sistema operacional convidado. Para obter uma lista dos sistemas operacionais convidados com suporte, consulte [visão geral do Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Estatísticas de CPU

O Hyper-V publica contadores de desempenho para ajudar a caracterizar o comportamento do servidor de virtualização e relatar o uso de recursos. O conjunto padrão de ferramentas para exibir contadores de desempenho no Windows inclui o monitor de desempenho e logman. exe, que pode exibir e registrar os contadores de desempenho do Hyper-V. Os nomes dos objetos de contador relevantes são prefixados com o **Hyper-V**.

Você deve sempre medir o uso da CPU do sistema físico usando os contadores de desempenho do processador lógico do hipervisor do Hyper-V. Os contadores de utilização da CPU que o Gerenciador de tarefas e o monitor de desempenho relatam nas partições raiz e filho não refletem o uso real da CPU física. Use os seguintes contadores de desempenho para monitorar o desempenho:

- **Processador lógico do hipervisor Hyper-V (\*) \\% tempo de execução total** O tempo total não ocioso dos processadores lógicos

- **Processador lógico do hipervisor Hyper-V (\*) \\% tempo de execução do convidado** O tempo gasto na execução de ciclos em um convidado ou dentro do host

- **Processador lógico do hipervisor Hyper-V (\*) \\% tempo de execução do hipervisor** O tempo gasto na execução dentro do hipervisor

- **Processador virtual da raiz do hipervisor do Hyper\*-\\V ()\\** * mede o uso da CPU da partição raiz

- **Processador virtual do hipervisor do Hyper-\*V\\()\\** * mede o uso da CPU de partições de convidado


## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
