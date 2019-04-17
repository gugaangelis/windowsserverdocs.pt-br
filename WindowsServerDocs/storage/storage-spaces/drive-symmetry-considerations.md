---
title: Considerações de simetria unidade para espaços de armazenamento diretos
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: f2ef58003da6de049c7c4b578f789a97e0a0f512
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5591842"
---
# Considerações de simetria unidade para espaços de armazenamento diretos 

> Aplica-se a: Windows Server 2019, Windows Server 2016

[Espaços de armazenamento diretos](storage-spaces-direct-overview.md) funciona melhor quando cada servidor tem exatamente as mesmas unidades.

Na realidade, reconhecemos que isso nem sempre é prático: espaços de armazenamento diretos é projetado para ser executado por anos e a escala à medida que crescem as necessidades de sua organização. Hoje em dia, você pode comprar amplo 3 TB unidades de disco rígido; próximo ano, ele pode se tornar impossível encontre aqueles que pequenos. Portanto, há suporte para uma quantidade de mixagem e correspondência.

Este tópico explica as restrições e fornece exemplos de configurações com suporte e sem suporte.

## Restrições

### Tipo

Todos os servidores devem ter os mesmos [tipos de unidades](choosing-drives.md#drive-types).

Por exemplo, se um servidor tiver NVMe, eles devem *todos os* tiver NVMe.

### Número

Todos os servidores devem ter o mesmo número de unidades de cada tipo.

Por exemplo, se um servidor tem seis SSD, eles devem *todos os* tem seis SSD.

   > [!NOTE]
   > É okey para o número de unidades para diferem temporariamente durante falhas ou ao adicionar ou remover unidades.

### Modelo

É recomendável usar unidades do mesmo modelo e a versão do firmware sempre que possível. Se você não pode selecione cuidadosamente unidades que são mais semelhantes possível. Incentivamos mixagem e correspondência unidades do mesmo tipo com características de desempenho ou resistência nitidamente diferentes (a menos que um é o cache e a outra é a capacidade) porque distribui e/s uniformemente e não fazer a distinção com base no modelo de espaços de armazenamento diretos .

   > [!NOTE]
   > É okey para unidades SAS e SATA semelhantes mix-e-correspondência.

### Size

É recomendável usar unidades dos mesmos tamanhos de sempre que possível. Usar unidades de capacidade de tamanhos diferentes pode resultar em alguma capacidade inutilizável e usar unidades de cache de tamanhos diferentes pode melhorar o desempenho do cache. Consulte a próxima seção para obter detalhes.

   > [!WARNING]
   > Diferentes tamanhos de unidades de capacidade em todos os servidores podem resultar em capacidade perdida.

## Entender: desequilíbrio de capacidade

Espaços de armazenamento diretos é robusto para desequilíbrio de capacidade em unidades e em todos os servidores. Mesmo se o desequilíbrio for grave, tudo continuarão a funcionar. No entanto, dependendo de vários fatores, capacidade que não está disponível em todos os servidores pode não ser utilizável.

Para ver por que isso acontece, considere a ilustração simplificada abaixo. Cada caixa colorida representa uma cópia de dados espelhados. Por exemplo, as caixas marcadas A, um ' e um ' são três cópias dos mesmos dados. Aceitar tolerância a falhas de servidor, essas cópias *devem* ser armazenados em servidores diferentes.

### Capacidade perdida

Conforme desenhado, Server 1 (10 TB) e servidor 2 (10 TB) são completo. Servidor 3 tem drives maiores, portanto, sua capacidade total é maior (15 TB). No entanto armazenar mais dados de três vias no servidor 3 exigiria cópias no Server 1 e 2 de servidor também, que já estão cheio. Não pode ser usado a 5 TB de capacidade restantes no servidor 3 – capacidade *"perdidos"* .

![Espelhamento de três vias, três servidores, perdidos capacidade](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### Posicionamento ideal

Por outro lado, com quatro servidores de 10 TB, 10 TB, 10 TB e 15 TB de capacidade e a resiliência por espelhamento de três vias, ele *é* possível colocar válida cópias de maneira que usa toda a capacidade disponível, como desenhado. Sempre que possível, o alocador de espaços de armazenamento diretos encontrará e use o posicionamento ideal, não deixando nenhuma capacidade perdida.

![Espelhamento de três vias, quatro servidores, não há capacidade perdida](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

O número de servidores, a resiliência, a gravidade de desequilíbrio a capacidade e outros fatores afeta se houver capacidade perdida. **A regra geral mais prudente é pressupor que somente capacidade disponível em cada servidor é garantida para ser usado.**

## Entender: desequilíbrio de cache

Espaços de armazenamento diretos é robusto para desequilíbrio de cache em unidades e em todos os servidores. Mesmo se o desequilíbrio for grave, tudo continuarão a funcionar. Além disso, espaços de armazenamento diretos sempre usa todos os cache disponível ao máximo.

No entanto, usar unidades de cache de tamanhos diferentes pode não melhorar o desempenho de cache uniformemente ou previsível: somente de e/s para [associações de unidade](understand-the-cache.md#server-side-architecture) com unidades de cache maiores poderão ver o desempenho aprimorado. Espaços de armazenamento diretos distribui e/s uniformemente entre associações e não fazer a distinção com base na taxa de capacidade de cache.

![Desequilíbrio de cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consulte [Noções básicas sobre o cache](understand-the-cache.md) para saber mais sobre associações de cache.

## Exemplos de configurações

Aqui estão algumas configurações com suporte e sem suporte:

### ![com suporte](media/drive-symmetry-considerations/supported.png) Suporte: modelos diferentes entre servidores

Os dois primeiros servidores usam o modelo de NVMe "X", mas o terceiro servidor usa o modelo de NVMe "Z", que é muito semelhante.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modelo X (cache)    | 2 x NVMe modelo X (cache)    | 2 x NVMe modelo Z (cache)    |
| 10 x SSD modelo Y (capacidade) | 10 x SSD modelo Y (capacidade) | 10 x SSD modelo Y (capacidade) |

Isso é suportado.

### ![com suporte](media/drive-symmetry-considerations/supported.png) Suporte: modelos diferentes dentro do servidor

Cada servidor usa algumas mistura de diferente de modelos HDD "Y" e "Z", que são muito semelhantes. Cada servidor tem 10 HDD total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modelo X (cache)    | 2 x SSD modelo X (cache)    | 2 x SSD modelo X (cache)    |
| 7 dias HDD modelo Y (capacidade) | 5 x HDD modelo Y (capacidade) | 1 x HDD modelo Y (capacidade) |
| 3 x HDD modelo Z (capacidade) | 5 x HDD modelo Z (capacidade) | 9 x HDD modelo Z (capacidade) |

Isso é suportado.

### ![com suporte](media/drive-symmetry-considerations/supported.png) Suporte: tamanhos diferentes em todos os servidores

Os dois primeiros servidores usam HDD de 4 TB, mas o terceiro servidor usa 6 TB HDD muito semelhante.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| 4 x 4 TB (capacidade) HDD | 4 x 4 TB (capacidade) HDD | 4 x 6 TB (capacidade) HDD |

Isso é possível, embora isso resultará em capacidade perdida.

### ![com suporte](media/drive-symmetry-considerations/supported.png) Suporte: tamanhos diferentes dentro do servidor

Cada servidor usa algumas mistura diferente de TB 1.2 e 1,6 TB SSD muito semelhante. Cada servidor tem 4 SSDS total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| 1.2 x 3 TB SSD (cache)   | 1.2 x 2 TB SSD (cache)   | 1.2 x 4 TB SSD (cache)   |
| 1 x 1,6 TB SSD (cache)   | 2 x 1,6 TB SSD (cache)   | -                        |
| 20 x 4 TB (capacidade) HDD | 20 x 4 TB (capacidade) HDD | 20 x 4 TB (capacidade) HDD |

Isso é suportado.

### ![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: diferentes tipos de unidades em todos os servidores

1 servidor tiver NVMe, mas os outros não.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacidade) | 18 x HDD (capacidade) | 18 x HDD (capacidade) |

Isso não é aceito. Os tipos de unidades devem ser o mesmo em cada servidor.

### ![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: um número diferente de cada tipo em todos os servidores

Servidor 3 tem mais que os outros.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacidade) | 10 x HDD (capacidade) | 20 x HDD (capacidade) |

Isso não é aceito. O número de unidades de cada tipo deve ser o mesmo em cada servidor.

### ![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: somente as unidades de disco rígido

Todos os servidores tenham apenas unidades de disco rígido conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-| 
|18 x HDD (capacidade) |18 x HDD (capacidade)|18 x HDD (capacidade)|

Isso não é aceito. Você precisará adicionar pelo menos duas unidades de cache (NvME ou SSD) conectados a cada um dos servidores.

## Resumo

Recapitulando, todos os servidores no cluster devem ter os mesmos tipos de unidades e o mesmo número de cada tipo. Ele tem suporte para tamanhos de unidade conforme necessário, com as considerações acima e modelos de unidade de mistura-e-correspondência.

| Restrição                               |               |
|------------------------------------------|---------------|
| Mesmos tipos de unidades em cada servidor     | **Obrigatório**  |
| Mesmo número de cada tipo em cada servidor | **Obrigatório**  |
| Modelos de unidade mesmo em cada servidor        | Recomendações   |
| Mesmos tamanhos de unidade em cada servidor         | Recomendações   |

## Consulte também

- [Requisitos de hardware dos Espaços de Armazenamento Diretos](storage-spaces-direct-hardware-requirements.md)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
