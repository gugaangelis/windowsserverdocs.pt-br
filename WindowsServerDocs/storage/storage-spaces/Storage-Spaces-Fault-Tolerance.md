---
title: Falha de tolerância e eficiência de armazenamento em Espaços de Armazenamento Diretos
ms.prod: windows-server
ms.author: cosmosdarwin
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: Uma discussão sobre opções de resiliência em Espaços de Armazenamento Diretos incluindo espelhamento e paridade.
ms.localizationpriority: medium
ms.openlocfilehash: 517b5484bc02e377f40df84422a1910014c9b830
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955388"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>Falha de tolerância e eficiência de armazenamento em Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2016

Este tópico apresenta as opções de resiliência disponíveis nos [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md) e descreve os requisitos de escala, a eficiência de armazenamento e as vantagens e desvantagens em geral de cada um. Ele também apresenta algumas instruções de uso para começar e faz referência a alguns excelentes documentos, blogs e conteúdo adicional, nos quais você pode obter mais informações.

Se você já estiver familiarizado com os Espaços de Armazenamento, passe para a seção [Resumo](#summary).

## <a name="overview"></a>Visão geral

Basicamente, os Espaços de Armazenamento dizem respeito ao fornecimento de tolerância, geralmente chamada 'resiliência', para seus dados. Sua implementação é semelhante ao RAID, mas é distribuído em vários servidores e implementado no software.

Assim como ocorre com o RAID, há algumas maneiras diferentes pelas quais os Espaços de Armazenamento podem fazer isso, que tornam as compensações diferentes entre tolerância padrão, eficiência de armazenamento e a complexidade de computação. Elas estão amplamente agrupadas em duas categorias: 'espelhamento' e 'paridade', a última às vezes é chamada de 'codificação de eliminação'.

## <a name="mirroring"></a>Espelhamento

O espelhamento fornece tolerância a falhas por manter várias cópias de todos os dados. Isso é mais parecido com RAID-1. A maneira pela qual esses dados são distribuídos e colocados não é comum, (consulte [este blog](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959) para saber mais), mas é absolutamente verdade dizer que todos os dados armazenados que usam o espelhamento são gravados em sua totalidade várias vezes. Cada cópia é gravada em um hardware físico diferente (unidades diferentes em servidores diferentes) que supostamente falhariam de forma independente.

No Windows Server 2016, os Espaços de Armazenamento oferecem dois tipos de espelhamento: 'duas vias' e 'três vias'.

### <a name="two-way-mirror"></a>Espelho de duas vias

O espelhamento bidirecional grava duas cópias de tudo. A eficiência de armazenamento é de 50% – para gravar 1 TB de dados, é necessário ter pelo menos 2 TB de capacidade de armazenamento físico. Da mesma forma, você precisa de pelo menos dois [domínios de falha de hardware](../../failover-clustering/fault-domains.md) – com espaços de armazenamento diretos, isso significa dois servidores.

![espelhamento de duas vias](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > Se você tiver mais de dois servidores, recomendamos usar espelhamento de três vias.

### <a name="three-way-mirror"></a>Espelho de três vias

O espelhamento de três vias grava três cópias de tudo. A eficiência de armazenamento é de 33,3% – para gravar 1 TB de dados, é necessário ter pelo menos 3 TB de capacidade de armazenamento físico. Da mesma forma, você precisa de pelo menos três domínios de falha de hardware – com Espaços de Armazenamento Diretos, e isso significa três servidores.

O espelhamento de três vias pode tolerar com segurança pelo menos [dois problemas de hardware (unidade ou servidor) de cada vez](#examples). Por exemplo, se você estiver reiniciando um servidor quando, de repente, outra unidade ou servidor falhar, todos os dados permanecem seguros e continuamente acessíveis.

![espelhamento de três vias](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>Parity

A codificação de paridade, geralmente chamada de 'codificação de eliminação', oferece a tolerância a falhas usando a aritmética bit a bit, que pode se tornar [notavelmente complicada](https://www.microsoft.com/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf). A maneira como isso funciona é menos óbvia que o espelhamento, e há muitos recursos online excelentes (por exemplo, este [Guia para Iniciantes na Codificação de Eliminação](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/)) de terceiros que pode ajudá-lo a ter uma ideia. Basta dizer que fornece melhor eficiência de armazenamento sem comprometer a tolerância a falhas.

No Windows Server 2016, os Espaços de Armazenamento oferecem dois tipos de paridade, a paridade 'única' e a paridade 'dupla', sendo que a última emprega uma técnica avançada chamada 'códigos de reconstrução local' em escalas maiores.

> [!IMPORTANT]
> É recomendado usar o espelhamento para a maioria das cargas de trabalho de detecção de desempenho. Para saber mais sobre como equilibras o desempenho e a capacidade de acordo com sua carga de trabalho, consulte [Planejar volumes](plan-volumes.md#choosing-the-resiliency-type).

### <a name="single-parity"></a>Paridade única
A paridade única mantém apenas um símbolo de paridade bit a bit, que fornece tolerância a falhas contra apenas uma falha de cada vez. Isso é mais parecido com o RAID-5. Para usar a paridade única, você precisa de pelo menos três domínios de falha de hardware – com Espaços de Armazenamento Diretos, e isso significa três servidores. Como o espelhamento triplo fornece mais tolerância a falhas na mesma escala, não incentivamos o uso da paridade única. Porém, ela está lá se você insistir em usá-la, e é totalmente compatível.

   >[!WARNING]
   > Não incentivamos o uso da paridade única porque ela só pode tolerar com segurança uma falha de hardware por vez. Se você estiver reiniciando um servidor quando repentinamente outra unidade ou servidor falha, você terá um tempo de inatividade. Se você tiver apenas três servidores, recomendamos usar o espelhamento de três vias. Se você tem quatro ou mais, consulte a próxima seção.

### <a name="dual-parity"></a>Paridade dupla

A paridade dupla implementa códigos de correção Reed-Solomon para manter os dois símbolos de paridade bit a bit, oferecendo assim a mesma tolerância a falhas que o espelhamento triplo (ou seja, até duas falhas de uma só vez), mas com mais eficiência de armazenamento. Isso é mais parecido com o RAID-6. Para usar a paridade dupla, você precisa de pelo menos quatro domínios de falha de hardware – com Espaços de Armazenamento Diretos, e isso significa quatro servidores. Nessa escala, a eficiência de armazenamento é de 50% – para armazenar 2 TB de dados, você precisa de 4 TB de capacidade de armazenamento físico.

![paridade dupla](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

A eficiência de armazenamento de paridade dupla aumenta à medida que você tem mais domínios de falha de hardware, de 50% até 80%. Por exemplo, com sete (com Espaços de Armazenamento Diretos, isso significa sete servidores), a eficiência pula para 66,7% – para armazenar 4 TB de dados, você precisa apenas de 6 TB de capacidade de armazenamento físico.

![dual-parity-wide](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

Consulte a seção [Resumo](#summary) para a eficiência de códigos de reconstrução local e de paridade dupla em cada escala.

### <a name="local-reconstruction-codes"></a>Códigos de reconstrução local

Os Espaços de Armazenamento no Windows Server 2016 apresentam uma técnica avançada desenvolvida pela Microsoft Research chamada de 'códigos de reconstrução local' ou LRC. Em grande escala, a paridade dupla usa o LRC para dividir sua codificação/decodificação em alguns grupos menores para reduzir a sobrecarga necessária para fazer gravações ou recuperar-se de falhas.

Com unidades de disco rígido (HDD), o tamanho do grupo é de quatro símbolos; com unidades de estado sólido (SSD), o tamanho do grupo é de seis símbolos. Por exemplo, veja a aparência do layout com unidades de disco rígido e 12 domínios de falha de hardware (ou seja, 12 servidores) – há dois grupos de quatro símbolos de dados. Ele atinge eficiência de armazenamento de 72,7%.

![local-reconstruction-codes](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

É recomendável que este passo a passo detalhado, mas eminentemente legível, de [como os códigos de reconstrução local lidam com vários cenários de falha e por que eles são atraentes](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB), por nosso próprio [Joergensen Claus](https://twitter.com/clausjor).

## <a name="mirror-accelerated-parity"></a>Paridade acelerada por espelho

A partir do Windows Server 2016, um volume de Espaços de Armazenamento Diretos pode ser parte espelho e parte paridade. As gravações são feitas primeiro na parte espelhada e, depois, são gradualmente movidas para a parte de paridade. Na verdade, isso [usa o espelhamento para acelerar a codificação de eliminação](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).

Para misturar o espelhamento de três vias e a paridade dupla, você precisa de pelo menos quatro domínios de falha, o que significa quatro servidores.

A eficiência de armazenamento de paridade acelerada por espelho está entre o que você obteria usando apenas espelhamento ou apenas paridade e depende das proporções que você escolher. Por exemplo, a demonstração aos 37 minutos desta apresentação mostra [várias combinações atingindo 46%, 54% e 65% de eficiência](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) com 12 servidores.

> [!IMPORTANT]
> É recomendado usar o espelhamento para a maioria das cargas de trabalho de detecção de desempenho. Para saber mais sobre como equilibras o desempenho e a capacidade de acordo com sua carga de trabalho, consulte [Planejar volumes](plan-volumes.md#choosing-the-resiliency-type).

## <a name="summary"></a><a name="summary"></a>Resumo

Esta seção resume os tipos de resiliência disponíveis em Espaços de Armazenamento Diretos, os requisitos de escala mínimos para usar cada tipo, quantas falhas cada tipo pode tolerar e a eficiência de armazenamento correspondente.

### <a name="resiliency-types"></a>Tipos de resiliência

|    Resiliência          |    Tolerância a falhas       |    Eficiência de armazenamento      |
|------------------------|----------------------------|----------------------------|
|    Espelho de duas vias      |    1                       |    50,0%                   |
|    Espelho de três vias    |    2                       |    33,3%                   |
|    Paridade dupla         |    2                       |    50,0% - 80,0%           |
|    Mixed               |    2                       |    33,3% - 80,0%           |

### <a name="minimum-scale-requirements"></a>Requisitos mínimos de escala

|    Resiliência          |    Mínimo necessário de domínios de falha   |
|------------------------|-------------------------------------|
|    Espelho de duas vias      |    2                                |
|    Espelho de três vias    |    3                                |
|    Paridade dupla         |    4                                |
|    Mixed               |    4                                |

   >[!TIP]
   > A menos que você esteja usando [tolerância a falhas em chassis ou rack](../../failover-clustering/fault-domains.md), o número de domínios com falha se refere ao número de servidores. O número de unidades em cada servidor não afeta quais tipos de resiliência, você pode usar, desde que atenda aos requisitos mínimos de Espaços de Armazenamento Diretos.

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>Eficiência de paridade dupla para implantações híbridas

Esta tabela mostra a eficiência de armazenamento de paridade dual e códigos de reconstrução locais em cada escala para implantações híbridas que contenham unidades de disco rígido (HDD) e unidades de estado sólido (SSD).

|    Domínios de falha      |    Layout           |    Eficiência   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50,0%        |
|    5                  |    RS 2+2           |    50,0%        |
|    6                  |    RS 2+2           |    50,0%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 4+2           |    66,7%        |
|    10                 |    RS 4+2           |    66,7%        |
|    11                 |    RS 4+2           |    66,7%        |
|    12                 |    LRC (8, 2, 1)    |    72,7        |
|    13                 |    LRC (8, 2, 1)    |    72,7        |
|    14                 |    LRC (8, 2, 1)    |    72,7        |
|    15                 |    LRC (8, 2, 1)    |    72,7        |
|    16                 |    LRC (8, 2, 1)    |    72,7        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>Eficiência de paridade dupla para implantações tudo flash

Esta tabela mostra a eficiência de armazenamento de paridade dual e códigos de reconstrução locais em cada escala para implantações tudo flash que contenham apenas unidades de estado sólido (SSD). O layout de paridade pode usar tamanhos de grupo maiores e conseguir mais eficiência de armazenamento em uma configuração tudo flash.

|    Domínios de falha      |    Layout           |    Eficiência   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50,0%        |
|    5                  |    RS 2+2           |    50,0%        |
|    6                  |    RS 2+2           |    50,0%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 6+2           |    75,0%        |
|    10                 |    RS 6+2           |    75,0%        |
|    11                 |    RS 6+2           |    75,0%        |
|    12                 |    RS 6+2           |    75,0%        |
|    13                 |    RS 6+2           |    75,0%        |
|    14                 |    RS 6+2           |    75,0%        |
|    15                 |    RS 6+2           |    75,0%        |
|    16                 |    LRC (12, 2, 1)   |    80,0%        |

## <a name="examples"></a><a name="examples"></a>Exemplos

A menos que você tenha apenas dois servidores, recomendamos usar espelhamento triplo e/ou paridade dupla, porque eles oferecem uma tolerância a falhas melhor. Mais especificamente, eles garantem que todos os dados continuem seguros e acessíveis continuamente, mesmo quando dois domínios com falha – com Espaços de Armazenamento Diretos, isso significa dois servidores – são afetados por falhas simultâneas.

### <a name="examples-where-everything-stays-online"></a>Exemplos de onde tudo fica online

Estes seis exemplos mostram o que o espelhamento triplo e/ou a paridade dupla **pode** tolerar.

- **1.**    Uma unidade perdida (inclui unidades de cache)
- **2.**    Um servidor perdido

![fault-tolerance-examples-1-and-2](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    Um servidor e uma unidade perdidos
- **4.**    Duas unidades perdidas em servidores diferentes

![fault-tolerance-examples-3-and-4](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    Mais de duas unidades perdidas, desde que, no máximo, dois servidores sejam afetados
- **6.**    Dois servidores perdidos

![fault-tolerance-examples-5-and-6](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...em todos os casos, todos os volumes permanecerão online. (Verifique se que o cluster mantém quórum.)

### <a name="examples-where-everything-goes-offline"></a>Exemplos de onde tudo fica offline

Durante a vida útil, Espaços de Armazenamento podem tolerar qualquer número de falhas, uma vez que restauram a resiliência completa depois de cada uma, dando tempo suficiente. No entanto, no máximo, dois domínios de falha podem ser afetados com segurança por falhas em um dado momento. Estes são, portanto, exemplos do que o espelhamento triplo e/ou a paridade dupla **não pode** tolerar.

- **7.** Unidades perdidas em três ou mais servidores de uma só vez
- **8.** Três ou mais servidores perdidos simultaneamente

![exemplos de tolerância de falha 7 e 8](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>Uso

Consulte [Criando volumes em Espaços de Armazenamento Diretos](create-volumes.md).

## <a name="additional-references"></a>Referências adicionais

Cada link abaixo está embutido em algum lugar no corpo deste tópico.

- [Espaços de Armazenamento Diretos no Windows Server 2016](storage-spaces-direct-overview.md)
- [Reconhecimento de domínio de falha no Windows Server 2016](../../failover-clustering/fault-domains.md)
- [Codificação de eliminação no Azure pela Microsoft Research](https://www.microsoft.com/research/publication/erasure-coding-in-windows-azure-storage/)
- [Códigos de reconstrução local e acelerando volumes de paridade](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)
- [Volumes na API de Gerenciamento de Armazenamento](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)
- [Demonstração de eficiência de armazenamento no Microsoft Ignite 2016](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [PRÉVIA da calculadora de capacidade para Espaços de Armazenamento Diretos](https://aka.ms/s2dcalc)
