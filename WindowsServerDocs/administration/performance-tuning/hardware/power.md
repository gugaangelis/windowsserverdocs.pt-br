---
title: Considerações de energia de Hardware do servidor
description: Considerações de energia de Hardware do servidor
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5fe91888188796c96d5da80e8f9bd3ed627b9d43
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564738"
---
# <a name="server-hardware-power-considerations"></a>Considerações de energia de Hardware do servidor

É importante reconhecer a importância de cada vez maior de eficiência energética em ambientes de centro de empresa e dados. Alto desempenho e uso de baixa energia geralmente são objetivos conflitantes, mas cuidadosamente selecionando os componentes de servidor, você pode obter o equilíbrio correto entre eles. As seções a seguir lista as diretrizes para características de energia e recursos dos componentes de hardware do servidor.

## <a name="processor-recommendations"></a>Recomendações de processador

Frequência, tensão, tamanho do cache e tecnologia de processo operacionais afeta o consumo de energia de processadores. Processadores têm um design térmico aponte classificação (TDP) que fornece uma indicação básica do consumo de energia em relação a outros modelos.

Em geral, otimizado para o processador TDP mais baixo que atenda às suas metas de desempenho. Além disso, mais recentes gerações de processadores são geralmente mais eficiente de energia, e eles podem expor mais estados de energia para os algoritmos de gerenciamento de energia do Windows, que permite um melhor gerenciamento de energia em todos os níveis de desempenho. Ou eles podem usar algumas das novas técnicas de gerenciamento de energia "cooperativa" que a Microsoft desenvolveu em parceria com fabricantes de hardware.

Para obter mais informações sobre as técnicas de gerenciamento cooperativo de energia, consulte a seção denominada colaborativa controle de desempenho do processador na [especificação de Interface de energia e configuração avançada](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Recomendações de memória
Contas de memória para uma fração de cada vez maior da potência total do sistema. Muitos fatores afetam o consumo de energia de uma memória DIMM, como a tecnologia de memória, o código de correção de erros (ECC), frequência de barramento, capacidade, densidade e número de classificações. Portanto, é melhor comparar as classificações de energia esperada antes de adquirir grandes quantidades de memória.

Memória de baixa energia agora está disponível, mas você deve considerar as vantagens e desvantagens de desempenho e custo. Se seu servidor será de paginação, você também deve incluir o custo de energia dos discos de paginação.


## <a name="disks-recommendations"></a>Recomendações de discos
RPM mais alta significa que o consumo de energia maior. Unidades SSD são mais energia eficiente que unidades rotacional. Além disso, unidades de 2,5 polegadas geralmente exigem menos energia do que as unidades de 3,5 polegadas.

## <a name="network-and-storage-adapter-recommendations"></a>Recomendações de adaptador de rede e de armazenamento
Alguns adaptadores de diminuir o consumo de energia durante períodos ociosos. Essa é uma consideração importante para adaptadores de rede de 10 Gb e links de armazenamento de (4 a 8 Gb) de alta largura de banda. Esses dispositivos podem consumir uma quantidade significativa de energia.


## <a name="power-supply-recommendations"></a>Recomendações de fornecimento de energia
Melhorando a eficiência de fonte de energia é uma ótima maneira de reduzir o consumo de energia sem afetar o desempenho. Fontes de alimentação de alta eficiência podem economizar muitas kilowatt-hours por ano, por servidor.


## <a name="fan-recommendations"></a>Recomendações de ventilador
Ventiladores, como fontes de alimentação, são uma área onde você pode reduzir o consumo de energia sem afetar o desempenho do sistema. Os fãs de velocidade variável podem reduzir o RPM como o carga do sistema diminui, eliminando o consumo de energia desnecessário caso contrário.


## <a name="usb-devices-recommendations"></a>Dispositivos USB recomendações
Habilita o Windows Server 2016 selective suspend para dispositivos USB por padrão. No entanto, um driver de dispositivo mal-escrito ainda pode interromper a eficiência energética do sistema por uma margem considerável. Para evitar possíveis problemas, desconecte os dispositivos USB, desabilitá-los no BIOS ou escolher os servidores que não exigem dispositivos USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Recomendações de faixa Power gerenciados remotamente
Fios elétricos não são parte integrante do hardware de servidor, mas eles podem fazer uma grande diferença no data center. Medidas mostram que os servidores de volume que estão conectados, mas tiveram sido ostensivamente desligados, ainda podem exigir até 30 watts de potência.

Para evitar desperdício de energia, você pode implantar uma extensão de energia gerenciado remotamente para cada rack de servidores programaticamente desconectar a energia de servidores específicos.

## <a name="processor-terminology"></a>Terminologia de processador
A terminologia de processador usada ao longo deste tópico reflete a hierarquia de componentes disponíveis na figura a seguir. Termos do maior para menor granularidade dos componentes são os seguintes:

-   Soquete de processador
-   Nó NUMA
-   Core
-   processador lógico

![Terminologia de processador](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Consulte também
- [Considerações de desempenho de Hardware do servidor](index.md)
- [Power and Performance Tuning](power/power-performance-tuning.md) (Energia e ajuste de desempenho)
- [Processor Power Management Tuning](power/processor-power-management-tuning.md) (Ajuste de gerenciamento de energia do processador)
- [Parâmetros de plano balanceado recomendados](power/recommended-balanced-plan-parameters.md)
