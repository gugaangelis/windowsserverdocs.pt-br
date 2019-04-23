---
title: Considerações de simetria de unidade para espaços de armazenamento diretos
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866877"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considerações de simetria de unidade para espaços de armazenamento diretos 

> Aplica-se a: Windows Server 2019, Windows Server 2016

[Espaços de armazenamento diretos](storage-spaces-direct-overview.md) funciona melhor quando cada servidor tem exatamente as mesmas unidades.

Na realidade, reconhecemos que isso nem sempre é prático: Espaços de armazenamento diretos foi projetado para ser executado por anos e a escala conforme as necessidades da sua organização. Hoje, você pode comprar espaçoso 3 TB discos rígidos; próximo ano, ele pode se tornar impossível encontrar aqueles que pequeno. Portanto, há suporte para alguma quantidade de mistura e combinação.

Este tópico explica as restrições e fornece exemplos de configurações compatíveis e sem suportados.

## <a name="constraints"></a>Restrições

### <a name="type"></a>Tipo

Todos os servidores devem ter o mesmo [tipos de unidades](choosing-drives.md#drive-types).

Por exemplo, se um servidor tiver NVMe, eles devem *todos os* têm NVMe.

### <a name="number"></a>Número

Todos os servidores devem ter o mesmo número de unidades de cada tipo.

Por exemplo, se um servidor tiver seis SSD, eles devem *todos os* tem seis SSD.

   > [!NOTE]
   > É okey para o número de unidades diferentes temporariamente durante falhas ou ao adicionar ou remover unidades.

### <a name="model"></a>Modelo

É recomendável usar unidades do mesmo modelo e a versão de firmware, sempre que possível. Se não for possível, selecione cuidadosamente as unidades que são mais semelhantes possível. Não incentivamos mistura e combinação de unidades do mesmo tipo com características de desempenho ou a durabilidade nitidamente diferentes (a menos que um é o cache e a outra é a capacidade) porque Distribui uniformemente o e/s e não Discriminar com base no modelo de espaços de armazenamento diretos .

   > [!NOTE]
   > É okey para unidades SATA e SAS semelhantes misturar e corresponder.

### <a name="size"></a>Tamanho

É recomendável usar unidades dos mesmos tamanhos sempre que possível. Usar unidades de capacidade de tamanhos diferentes pode resultar em alguma capacidade inutilizável e usar unidades de cache de tamanhos diferentes pode melhorar o desempenho de cache. Consulte a próxima seção para obter detalhes.

   > [!WARNING]
   > Diferentes tamanhos de unidades de capacidade em servidores podem resultar em capacidade subutilizada.

## <a name="understand-capacity-imbalance"></a>Entender: desequilíbrio de capacidade

Espaços de armazenamento diretos é robusto para desequilíbrio de capacidade entre unidades e entre servidores. Mesmo se o desequilíbrio for grave, tudo o que continuarão a funcionar. No entanto, dependendo de vários fatores, capacidade que não está disponível em todos os servidores pode não ser utilizável.

Para ver por que isso acontece, considere a ilustração simplificada a seguir. Cada caixa colorida representa uma cópia de dados espelhados. Por exemplo, as caixas marcadas A, um ' e um ' são três cópias dos mesmos dados. A tolerância a falhas do servidor, essas cópias de honrar *deve* ser armazenados em servidores diferentes.

### <a name="stranded-capacity"></a>Capacidade subutilizada

Conforme desenhada, servidor 1 (10 TB) e o servidor 2 (10 TB) estão cheios. Servidor 3 tem unidades maiores, portanto, sua capacidade total é maior (15 TB). No entanto armazenar mais dados de espelho triplo em Server 3 exigiria cópias no servidor 1 e 2 do servidor também, que já estão cheio. A capacidade restante de 5 TB em 3 de servidor não pode ser usada – trata-se *"perdidos"* capacidade.

![Espelho de três vias, três servidores, presos capacidade](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Posicionamento ideal

Por outro lado, com quatro servidores de 10 TB, 10 TB, 10 TB e 15 TB de capacidade e resiliência de espelho triplo, ele *é* possível colocar válida cópias de uma maneira que usa toda a capacidade disponível, como desenhado. Sempre que possível, o alocador de espaços de armazenamento diretos encontrará e usar o posicionamento ideal, não deixando nenhuma capacidade do subutilizada.

![Espelho de três vias, quatro servidores, não há capacidade do subutilizada](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

O número de servidores, a resiliência, a severidade do desequilíbrio de capacidade e outros fatores afeta se há capacidade subutilizada. **A regra geral mais aconselhável é supor que a capacidade única disponível em todos os servidores é garantida para ser usado.**

## <a name="understand-cache-imbalance"></a>Compreender: desequilíbrio de cache

Espaços de armazenamento diretos é robusto para desequilíbrio de cache entre unidades e entre servidores. Mesmo se o desequilíbrio for grave, tudo o que continuarão a funcionar. Além disso, espaços de armazenamento diretos sempre usa todo o cache disponível ao máximo.

No entanto, usar unidades de cache de tamanhos diferentes pode não melhorar o desempenho do cache uniformemente ou de maneira previsível: somente e/s para [unidade associações](understand-the-cache.md#server-side-architecture) com cache maior unidades podem ver desempenho aprimorado. Espaços de armazenamento diretos distribui e/s de uniformemente entre as associações e não Discriminar com base na taxa de capacidade de cache.

![Desequilíbrio de cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Ver [Noções básicas sobre o cache](understand-the-cache.md) para saber mais sobre as associações de cache.

## <a name="example-configurations"></a>Exemplos de configurações

Aqui estão algumas configurações compatíveis e sem suportadas:

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![compatível](media/drive-symmetry-considerations/supported.png) Com suporte: diferentes modelos entre servidores

Os primeiros dois servidores usam o modelo de NVMe "X", mas o terceiro servidor usa o modelo de NVMe "Z", que é muito semelhante.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modelo X (cache)    | 2 x NVMe modelo X (cache)    | 2 x NVMe modelo Z (cache)    |
| 10 vezes o modelo SSD Y (capacidade) | 10 vezes o modelo SSD Y (capacidade) | 10 vezes o modelo SSD Y (capacidade) |

Isso é suportado.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![compatível](media/drive-symmetry-considerations/supported.png) Com suporte: modelos diferentes dentro do servidor

Cada servidor usa alguma combinação diferente de modelos HDD "Y" e "Z", que são muito semelhantes. Cada servidor tem 10 HDD total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modelo X (cache)    | 2 x SSD modelo X (cache)    | 2 x SSD modelo X (cache)    |
| 7 vezes o modelo HDD Y (capacidade) | 5 x HDD modelo Y (capacidade) | 1 x HDD modelo Y (capacidade) |
| 3 vezes o modelo HDD Z (capacidade) | 5 x HDD modelo Z (capacidade) | 9 x HDD modelo Z (capacidade) |

Isso é suportado.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![compatível](media/drive-symmetry-considerations/supported.png) Com suporte: tamanhos diferentes entre servidores

Os primeiros dois servidores usam HDD de 4 TB, mas o terceiro servidor usa 6 TB HDD muito semelhante.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| 4 x 4 TB HDD (capacidade) | 4 x 4 TB HDD (capacidade) | 4 x 6 TB HDD (capacidade) |

Isso é suportado, embora isso resultará em capacidade subutilizada.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![compatível](media/drive-symmetry-considerations/supported.png) Com suporte: tamanhos diferentes dentro do servidor

Todos os servidores usa alguns combinação diferente de 1,2 TB e 1,6 TB SSD muito semelhante. Cada servidor tem 4 SSD total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1,2 TB SSD (cache)   | 2 x 1,2 TB SSD (cache)   | 4 x 1,2 TB SSD (cache)   |
| 1 x 1,6 TB SSD (cache)   | 2 x 1,6 TB SSD (cache)   | -                        |
| 20 x 4 TB HDD (capacidade) | 20 x 4 TB HDD (capacidade) | 20 x 4 TB HDD (capacidade) |

Isso é suportado.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: tipos diferentes de unidades entre servidores

O servidor 1 tem NVMe, mas outros não.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacidade) | 18 x HDD (capacidade) | 18 x HDD (capacidade) |

Não há suporte para isso. Os tipos de unidades devem ser o mesmo em todos os servidores.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: um número diferente de cada tipo entre servidores

Servidor 3 tem mais unidades que os outros.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacidade) | 10 x HDD (capacidade) | 20 x HDD (capacidade) |

Não há suporte para isso. O número de unidades de cada tipo deve ser o mesmo em todos os servidores.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Não tem suporte: somente as unidades de disco rígido

Todos os servidores têm somente as unidades de disco rígido conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-| 
|18 x HDD (capacidade) |18 x HDD (capacidade)|18 x HDD (capacidade)|

Não há suporte para isso. Você precisará adicionar um mínimo de duas unidades de cache (NvME ou SSD) anexados a cada um dos servidores.

## <a name="summary"></a>Resumo

Para recapitular, todos os servidores no cluster devem ter os mesmos tipos de unidades e o mesmo número de cada tipo. Há suporte para modelos de combinação e correspondência de unidade e tamanhos de unidade conforme necessário, com as considerações acima.

| Restrição                               |               |
|------------------------------------------|---------------|
| Mesmos tipos de unidades em cada servidor     | **Necessário**  |
| Mesmo número de cada tipo em todos os servidores | **Necessário**  |
| Mesmos modelos de unidade em todos os servidores        | Recomendações   |
| Mesmos tamanhos de unidade em todos os servidores         | Recomendações   |

## <a name="see-also"></a>Consulte também

- [Requisitos de hardware de espaços diretos de armazenamento](storage-spaces-direct-hardware-requirements.md)
- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
