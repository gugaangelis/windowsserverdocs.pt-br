---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Escolher unidades para Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: d16a9ddea96760b41c6cd2e4239b9bb51c8b3b38
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501636"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>Escolher unidades para Espaços de Armazenamento Diretos

>Aplica-se a: Windows 2019, Windows Server 2016

Este tópico fornece diretrizes sobre como escolher unidades para [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md) para atender às suas necessidades de desempenho e capacidade.

## <a name="drive-types"></a>Tipos de unidade

Os Espaços de Armazenamento Diretos atualmente funcionam com três tipos de unidades:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (Non-Volatile Memory Express) refere-se a unidades de estado sólido que ficam diretamente no barramento PCIe. Os fatores forma comuns são U.2 de 2,5", PCIe Add-In-Card (AIC) e M.2. O NVMe oferece taxa de transferência IOPS e E/S mais alta com menor latência do que qualquer outro tipo de unidade com suporte atualmente.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> refere-se a unidades de estado sólido que se conectam via SATA ou SAS convencional.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> refere-se a unidades de disco rígido magnéticas rotacionais que oferecem ampla capacidade de armazenamento.
        </td>
    </tr>
</table>

## <a name="built-in-cache"></a>Cache interno

Os Espaços de Armazenamento Diretos têm um cache no servidor interno. Trata-se de um cache de leitura e gravação grande, persistente e em tempo real. Em implantações com vários tipos de unidades, ele é configurado automaticamente para usar todas as unidades do tipo "mais rápido". As unidades restantes são usadas para capacidade.

Para obter mais informações, consulte [Noções básicas sobre o cache em Espaços de Armazenamento Diretos](understand-the-cache.md).

## <a name="option-1--maximizing-performance"></a>Opção 1 – maximizar o desempenho

Para alcançar a latência previsível e uniforme abaixo de milissegundos em leituras e gravações aleatórias para qualquer tipo de dados ou para alcançar uma taxa de transferência extremamente alta em IOPS (chegamos a [mais de seis milhões](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)!) ou em E/S (chegamos a [mais de 1 Tb/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)!), você deve usar "tudo flash".

Atualmente, há três maneiras de fazer isso:

![All-Flash-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Todos NVMe.** Usar tudo NVMe oferece um desempenho incomparável, incluindo a baixa latência mais previsível. Se todas as unidades forem do mesmo modelo, não haverá cache. Você também pode combinar modelos NVMe de resistência superior e resistência inferior e configurar o primeiro para armazenar em cache as gravações do último ([requer configuração](understand-the-cache.md#manual)).

2. **NVMe + SSD.** Ao usar o NVMe com SSDs, o NVMe armazenará automaticamente as gravações em cache nos SSDs. Isso permite que as gravações sejam agrupadas em cache e desescalonadas somente conforme necessário, para reduzir o desgaste nos SSDs. Isso fornece características de gravação estilo NVMe, enquanto as leituras são fornecidas diretamente dos SSDs igualmente rápidos.

3. **Todos SSD.** Como na opção Tudo NVMe, não existe cache quando todas as unidades são do mesmo modelo. Combinando modelos de resistência superior e resistência inferior, você pode configurar o primeiro para armazenar em cache as gravações do último ([requer configuração](understand-the-cache.md#manual)).

   >[!NOTE]
   > Uma vantagem de usar tudo NVMe ou tudo SSD sem cache é que você obtém capacidade de armazenamento utilizável de cada unidade. Não há capacidade "gasta" no armazenamento em cache, o que pode ser atraente em menor escala.

## <a name="option-2--balancing-performance-and-capacity"></a>Opção 2 – balancear desempenho e capacidade

Para ambientes com uma variedade de aplicativos e cargas de trabalho, alguns com requisitos de desempenho rigorosos e outros que requerem considerável capacidade de armazenamento, você deve optar por "híbrido" com cache NVMe ou SSDs para HDDs maiores.

![Hybrid-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**. As unidades NVMe aceleram as leituras e gravações armazenando ambas em cache. O armazenamento de leituras em cache permite que o HDDs foquem nas gravações. O armazenamento de gravações em cache absorve picos e permite que as gravações sejam agrupadas e desescalonadas somente conforme necessário, de maneira artificialmente serializada que maximiza a taxa de transferência de IOPS e E/S do HDD. Isso proporciona características de gravação estilo NVMe e, para dados lidos recentemente ou com frequência, características de leitura estilo NVMe também.

2. **SSD + HDD**. Semelhante ao especificado acima, o SSD acelera leituras e gravações armazenando ambas em cache. Isso proporciona características de gravação estilo SSD, bem como características de leitura estilo SSD para dados lidos recentemente ou com frequência.

    Há uma opção adicional e um tanto exótica: usar unidades dos *três* tipos.

3. **NVMe + SSD + HDD.** Com unidades dos três tipos, as unidades NVMe armazenarão os dados em cache para os SSDs e HDDs. O atrativo é que você pode criar volumes em SSDs e em HDDs, lado a lado no mesmo cluster, todos acelerados pelo NVMe. Os primeiros são exatamente como na implantação "tudo flash", e os últimos são exatamente como nas implantações "híbridas" descritas acima. Isso é conceitualmente como ter dois pools, com gerenciamento de capacidade amplamente independente, ciclos de falha e reparo e assim por diante.

   >[!IMPORTANT]
   > Recomendamos usar a camada SSD para colocar as cargas de trabalho mais dependentes de desempenho em all-flash.

## <a name="option-3--maximizing-capacity"></a>Opção 3 – maximizar a capacidade

Para cargas de trabalho que exijam grande capacidade e gravem com pouca frequência, como destinos de arquivamento, de backup, data warehouses ou armazenamento "frio", você deverá combinar alguns SSDs para armazenamento em cache com HDDs muito maiores para capacidade.

![Opções de implantação para maximizar a capacidade](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD + HDD**. O SSDs armazenam leituras e gravações em cache para absorver picos e oferecer um desempenho de gravação estilo SSD, com desescalonamento otimizado posterior para os HDDs.

>[!IMPORTANT]
>Configuração com HDDs apenas não é suportada. Não é aconselhável SSDs alta resistência de cache para durabilidade baixa SSDs.

## <a name="sizing-considerations"></a>Considerações sobre o dimensionamento

### <a name="cache"></a>Cache

Cada servidor deve ter pelo menos duas unidades de cache (o mínimo necessário para redundância). É recomendável que o número de unidades de capacidade seja um múltiplo do número de unidades de cache. Por exemplo, se você tiver 4 unidades de cache, terá um desempenho mais consistente com 8 unidades de capacidade (proporção 1:2) do que com 7 ou 9.

O cache deve ser dimensionado para acomodar o conjunto de trabalho de seus aplicativos e cargas de trabalho, ou seja, todos os dados ativamente, eles estão lendo e gravando em um determinado momento. Não há qualquer requisito de dimensionamento de cache além desse. Para implantações com HDDs, um ponto de partida razoável é 10% da capacidade – por exemplo, se cada servidor tem 4 x 4 TB HDD = 16 TB de capacidade, em seguida, 2 x 800 GB SSD = 1,6 TB do cache por servidor. Para todos os flash implantações, especialmente com muito [alta resistência](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/) SSDs, talvez seja razoável para iniciar mais perto de 5% da capacidade – por exemplo, se cada servidor tem 24 x 1,2 TB SSD = 28,8 TB de capacidade, em seguida, 2 x NVMe de 750 GB = 1,5 TB de cache por servidor. Você pode adicionar ou remover unidades de cache posteriormente para ajustar.

### <a name="general"></a>Geral

É recomendável limitar a capacidade de armazenamento total por servidor a aproximadamente 100 TB. Quanto maior a capacidade de armazenamento por servidor, mais tempo é necessário para ressincronizar os dados após a inatividade ou reinicialização, como na aplicação de atualizações de software.

O tamanho máximo atual por pool de armazenamento é 4 petabytes (PB) (4.000 TB) para Windows Server 2019 ou 1 petabyte para Windows Server 2016.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Entender o cache em espaços de armazenamento diretos](understand-the-cache.md)
- [Requisitos de hardware de espaços diretos de armazenamento](storage-spaces-direct-hardware-requirements.md)
- [Planejando volumes em espaços de armazenamento diretos](plan-volumes.md)
- [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md)
