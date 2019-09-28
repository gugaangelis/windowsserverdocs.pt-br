---
title: Considerações de energia de hardware do servidor
description: Considerações de energia de hardware do servidor
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: a9d4653824d497ea0c42337260aa788bab354ba3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355019"
---
# <a name="server-hardware-power-considerations"></a>Considerações de energia de hardware do servidor

É importante reconhecer a importância crescente da eficiência de energia em ambientes corporativos e data center. O uso de alto desempenho e baixa energia costuma ser metas conflitantes, mas selecionando cuidadosamente os componentes do servidor, você pode obter o equilíbrio correto entre eles. As seções a seguir listam diretrizes para características de energia e recursos de componentes de hardware do servidor.

## <a name="processor-recommendations"></a>Recomendações de processador

A frequência, a tensão operacional, o tamanho do cache e a tecnologia do processo afetam o consumo de energia dos processadores. Os processadores têm uma classificação TDP (ponto de design térmico) que fornece uma indicação básica do consumo de energia em relação a outros modelos.

Em geral, opte pelo menor processador TDP que atenderá às suas metas de desempenho. Além disso, as gerações mais recentes de processadores geralmente são mais eficientes em termos de energia e podem expor mais Estados de energia para os algoritmos de gerenciamento de energia do Windows, o que permite um melhor gerenciamento de energia em todos os níveis de desempenho. Ou eles podem usar algumas das novas técnicas de gerenciamento de energia "cooperativa" que a Microsoft desenvolveu em parceria com fabricantes de hardware.

Para obter mais informações sobre técnicas de gerenciamento de energia cooperativas, consulte a seção chamada controle de desempenho de processador colaborativo na [especificação de configuração avançada e de interface de energia](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Recomendações de memória
As contas de memória para uma fração crescente da potência total do sistema. Muitos fatores afetam o consumo de energia de um DIMM de memória, como tecnologia de memória, ECC (código de correção de erro), frequência de barramento, capacidade, densidade e número de classificações. Portanto, é melhor comparar as avaliações de energia esperadas antes de comprar grandes quantidades de memória.

A memória de baixa energia agora está disponível, mas você deve considerar as compensações de desempenho e custo. Se o servidor estiver paginando, você também deverá considerar o custo de energia dos discos de paginação.


## <a name="disks-recommendations"></a>Recomendações de discos
RPM maior significa maior consumo de energia. As unidades SSD são mais eficientes em termos de eficiência do que as unidades de rotação. Além disso, as unidades de 2,5 polegadas geralmente exigem menos energia do que as unidades de 3,5 polegadas.

## <a name="network-and-storage-adapter-recommendations"></a>Recomendações de adaptador de rede e de armazenamento
Alguns adaptadores diminuem o consumo de energia durante períodos ociosos. Essa é uma consideração importante para adaptadores de rede de 10 GB e links de armazenamento de alta largura de banda (4-8 GB). Esses dispositivos podem consumir quantidades significativas de energia.


## <a name="power-supply-recommendations"></a>Recomendações de fonte de energia
Melhorar a eficiência da fonte de energia é uma ótima maneira de reduzir o consumo de energia sem afetar o desempenho. As fontes de alimentação de alta eficiência podem economizar muitas quilowatts por ano, por servidor.


## <a name="fan-recommendations"></a>Recomendações de ventilador
Ventiladores, como fontes de alimentação, são uma área em que você pode reduzir o consumo de energia sem afetar o desempenho do sistema. Os fãs de velocidade variável podem reduzir o RPM à medida que a carga do sistema diminui, eliminando o consumo de energia desnecessário de outra forma.


## <a name="usb-devices-recommendations"></a>Recomendações de dispositivos USB
Por padrão, o Windows Server 2016 habilita a suspensão seletiva para dispositivos USB. No entanto, um driver de dispositivo mal escrito ainda pode interromper a eficiência de energia do sistema por uma margem considerável. Para evitar possíveis problemas, desconecte dispositivos USB, desabilite-os no BIOS ou escolha servidores que não exigem dispositivos USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Recomendações do Power Strip gerenciadas remotamente
As faixas de energia não são parte integrante do hardware do servidor, mas podem fazer uma grande diferença na data center. As medições mostram que os servidores de volume que estão conectados, mas que foram desostensivamentedos, ainda podem exigir até 30 watts de energia.

Para evitar o desperdício de eletricidade, você pode implantar uma faixa de energia gerenciada remotamente para cada rack de servidores para desconectar programaticamente a energia de servidores específicos.

## <a name="processor-terminology"></a>Terminologia do processador
A terminologia do processador usada em todo este tópico reflete a hierarquia de componentes disponíveis na figura a seguir. Os termos usados de maior para menor granularidade de componentes são os seguintes:

-   Soquete do processador
-   Nó NUMA
-   Core
-   Processador lógico

![terminologia do processador](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de hardware do servidor](index.md)
- [Power and Performance Tuning](power/power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](power/processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](power/recommended-balanced-plan-parameters.md)
