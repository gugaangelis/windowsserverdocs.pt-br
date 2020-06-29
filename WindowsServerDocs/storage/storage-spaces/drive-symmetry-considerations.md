---
title: Considerações sobre simetria de unidade para Espaços de Armazenamento Diretos
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5e7a4469a3f72737801a5110e322533df9764e20
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473583"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considerações sobre simetria de unidade para Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

[Espaços de armazenamento diretos](storage-spaces-direct-overview.md) funciona melhor quando cada servidor tem exatamente as mesmas unidades.

Na realidade, reconhecemos que isso nem sempre é prático: Espaços de Armazenamento Diretos é projetado para ser executado por anos e ser dimensionado conforme as necessidades de sua organização crescem. Hoje, você pode comprar discos rígidos de 3 TB espaçoso; no próximo ano, pode se tornar impossível encontrar aqueles pequenos. Portanto, há suporte para alguma quantidade de combinação e correspondência.

Este tópico explica as restrições e fornece exemplos de configurações com e sem suporte.

## <a name="constraints"></a>Restrições

### <a name="type"></a>Digite

Todos os servidores devem ter os mesmos [tipos de unidades](choosing-drives.md#drive-types).

Por exemplo, se um servidor tiver o NVMe, *todos* eles terão o nvme.

### <a name="number"></a>Número

Todos os servidores devem ter o mesmo número de unidades de cada tipo.

Por exemplo, se um servidor tiver seis SSD, *todos* eles deverão ter seis SSD.

   > [!NOTE]
   > Não há problema para que o número de unidades seja diferente temporariamente durante falhas ou ao adicionar ou remover unidades.

### <a name="model"></a>Modelo

É recomendável usar unidades do mesmo modelo e versão de firmware sempre que possível. Se você não puder, selecione cuidadosamente as unidades que são as mais semelhantes possíveis. Não recomendamos a combinação de unidades do mesmo tipo com desempenho e características de Endurance de maneira nítida (a menos que um seja o cache e a outra seja a capacidade), pois Espaços de Armazenamento Diretos distribui e/s uniformemente e não discriminar várias com base no modelo.

   > [!NOTE]
   > Não há problema em misturar e comparar unidades SATA e SAS semelhantes.

### <a name="size"></a>Tamanho

É recomendável usar unidades dos mesmos tamanhos sempre que possível. O uso de unidades de capacidade de tamanhos diferentes pode resultar em alguma capacidade inutilizável e o uso de unidades de cache de tamanhos diferentes pode não melhorar o desempenho do cache. Confira a próxima seção para saber mais detalhes.

   > [!WARNING]
   > Diferentes tamanhos de unidades de capacidade entre servidores podem resultar em capacidade subutilizada.

## <a name="understand-capacity-imbalance"></a>Entender: desequilíbrio de capacidade

Espaços de Armazenamento Diretos é robusto para desequilíbrio de capacidade entre unidades e entre servidores. Mesmo que o desequilíbrio seja grave, tudo continuará funcionando. No entanto, dependendo de vários fatores, a capacidade que não está disponível em todos os servidores pode não ser utilizável.

Para ver por que isso acontece, considere a ilustração simplificada abaixo. Cada caixa colorida representa uma cópia dos dados espelhados. Por exemplo, as caixas marcadas a, A ' e ' ' são três cópias dos mesmos dados. Para honrar a tolerância a falhas do servidor, essas cópias *devem* ser armazenadas em servidores diferentes.

### <a name="stranded-capacity"></a>Capacidade subutilizada

Como desenhado, o servidor 1 (10 TB) e o servidor 2 (10 TB) estão cheios. O servidor 3 tem unidades maiores, portanto, sua capacidade total é maior (15 TB). No entanto, para armazenar mais dados de espelho de três vias no servidor 3, também seria necessário copiar o servidor 1 e o servidor 2, que já estão cheios. A capacidade restante de 5 TB no servidor 3 não pode ser usada – a capacidade é *"retido"* .

![Espelho de três vias, três servidores, capacidade subutilizada](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Posicionamento ideal

Por outro lado, com quatro servidores de capacidade de 10 TB, 10 TB, 10 TB e 15 TB e resiliência de espelho de três vias, *é* possível posicionar de forma válida as cópias de uma maneira que usa toda a capacidade disponível, como desenhada. Sempre que isso for possível, o alocador de Espaços de Armazenamento Diretos encontrará e usará o posicionamento ideal, não deixando nenhuma capacidade subutilizada.

![Espelho de três vias, quatro servidores, sem capacidade comprometida](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

O número de servidores, a resiliência, a gravidade do desequilíbrio de capacidade e outros fatores afetam se há capacidade subafetada. **A regra geral mais prudente é assumir que apenas a capacidade disponível em cada servidor é garantida para ser utilizável.**

## <a name="understand-cache-imbalance"></a>Entender: desequilíbrio de cache

Espaços de Armazenamento Diretos é robusto para o desequilíbrio de cache entre unidades e entre servidores. Mesmo que o desequilíbrio seja grave, tudo continuará funcionando. Além disso, Espaços de Armazenamento Diretos sempre usa todo o cache disponível para o mais completo.

No entanto, o uso de unidades de cache de tamanhos diferentes pode não melhorar o desempenho de cache de forma uniforme ou previsível: somente a e/s para [direcionar associações](understand-the-cache.md#server-side-architecture) com unidades de cache maiores pode ver um desempenho aprimorado. O Espaços de Armazenamento Diretos distribui a e/s uniformemente entre associações e não discriminar várias com base na proporção de cache para capacidade.

![Desequilíbrio de cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consulte [noções básicas sobre o cache](understand-the-cache.md) para saber mais sobre associações de cache.

## <a name="example-configurations"></a>Configurações de exemplo

Aqui estão algumas configurações com e sem suporte:

### <a name="supported-supported-different-models-between-servers"></a>![com suporte](media/drive-symmetry-considerations/supported.png) Com suporte: modelos diferentes entre servidores

Os dois primeiros servidores usam o modelo NVMe "X", mas o terceiro servidor usa o modelo de NVMe "Z", que é muito semelhante.

| Servidor 1                    | Servidor 2                    | Servidor 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x o modelo de NVMe X (cache)    | 2 x o modelo de NVMe X (cache)    | 2 x modelo de NVMe Z (cache)    |
| 10 x modelo SSD Y (capacidade) | 10 x modelo SSD Y (capacidade) | 10 x modelo SSD Y (capacidade) |

Com suporte.

### <a name="supported-supported-different-models-within-server"></a>![com suporte](media/drive-symmetry-considerations/supported.png) Com suporte: modelos diferentes no servidor

Cada servidor usa algumas combinações diferentes de modelos de HDD "Y" e "Z", que são muito semelhantes. Cada servidor tem 10 HDD total.

| Servidor 1                   | Servidor 2                   | Servidor 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x modelo de SSD X (cache)    | 2 x modelo de SSD X (cache)    | 2 x modelo de SSD X (cache)    |
| 7 x modelo de HDD Y (capacidade) | 5 x modelo de HDD Y (capacidade) | 1 x modelo de HDD Y (capacidade) |
| 3 x modelo de HDD Z (capacidade) | 5 x modelo de HDD Z (capacidade) | 9 x modelo de HDD Z (capacidade) |

Com suporte.

### <a name="supported-supported-different-sizes-across-servers"></a>![com suporte](media/drive-symmetry-considerations/supported.png) Com suporte: tamanhos diferentes entre servidores

Os dois primeiros servidores usam HDD de 4 TB, mas o terceiro servidor usa HDD de 6 TB muito semelhante.

| Servidor 1                | Servidor 2                | Servidor 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) | 2 x 800 GB NVMe (cache) |
| HDD de 4 x 4 TB (capacidade) | HDD de 4 x 4 TB (capacidade) | HDD de 4 x 6 TB (capacidade) |

Há suporte para isso, embora isso resulte em capacidade com falha.

### <a name="supported-supported-different-sizes-within-server"></a>![com suporte](media/drive-symmetry-considerations/supported.png) Com suporte: tamanhos diferentes no servidor

Cada servidor usa uma combinação diferente de 1,2 TB e um SSD de 1,6 TB muito semelhante. Cada servidor tem 4 SSD total.

| Servidor 1                 | Servidor 2                 | Servidor 3                 |
|--------------------------|--------------------------|--------------------------|
| SSD de 3 x 1,2 TB (cache)   | 2 x 1,2 TB SSD (cache)   | 4 x 1,2 TB SSD (cache)   |
| SSD de 1 x 1,6 TB (cache)   | 2 x 1,6 TB SSD (cache)   | -                        |
| HDD de 20 x 4 TB (capacidade) | HDD de 20 x 4 TB (capacidade) | HDD de 20 x 4 TB (capacidade) |

Com suporte.

### <a name="unsupported-not-supported-different-types-of-drives-across-servers"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Sem suporte: tipos diferentes de unidades entre servidores

O servidor 1 tem o NVMe, mas os outros não.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| HDD de 18 x (capacidade) | HDD de 18 x (capacidade) | HDD de 18 x (capacidade) |

Não há suporte para isso. Os tipos de unidades devem ser os mesmos em todos os servidores.

### <a name="unsupported-not-supported-different-number-of-each-type-across-servers"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Sem suporte: número diferente de cada tipo entre servidores

O servidor 3 tem mais unidades do que as outras.

| Servidor 1            | Servidor 2            | Servidor 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| HDD de 10 x (capacidade) | HDD de 10 x (capacidade) | HDD de 20 x (capacidade) |

Não há suporte para isso. O número de unidades de cada tipo deve ser o mesmo em todos os servidores.

### <a name="unsupported-not-supported-only-hdd-drives"></a>![não compatível](media/drive-symmetry-considerations/unsupported.png) Sem suporte: somente unidades HDD

Todos os servidores têm apenas unidades de HDD conectadas.

|Servidor 1|Servidor 2|Servidor 3|
|-|-|-|
|HDD de 18 x (capacidade) |HDD de 18 x (capacidade)|HDD de 18 x (capacidade)|

Não há suporte para isso. Você precisa adicionar no mínimo duas unidades de cache (NvME ou SSD) anexadas a cada um dos servidores.

## <a name="summary"></a>Resumo

Para recapitular, todos os servidores no cluster devem ter os mesmos tipos de unidades e o mesmo número de cada tipo. Há suporte para misturar e combinar modelos de unidade e tamanhos de unidade, conforme necessário, com as considerações acima.

| Constraint                               |               |
|------------------------------------------|---------------|
| Mesmos tipos de unidades em cada servidor     | **Necessário**  |
| Mesmo número de cada tipo em cada servidor | **Necessário**  |
| Mesmos modelos de unidade em cada servidor        | Recomendadas   |
| Mesmos tamanhos de unidade em cada servidor         | Recomendadas   |

## <a name="additional-references"></a>Referências adicionais

- [Requisitos de hardware Espaços de Armazenamento Diretos](storage-spaces-direct-hardware-requirements.md)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
