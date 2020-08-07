---
title: Variáveis que afetam o desempenho do sistema MultiPoint Services
description: Informações de desempenho para serviços do MultiPoint
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f7e540f95c55e743cc588aed76c04a3320161a97
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951491"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variáveis que afetam o desempenho do sistema MultiPoint Services
Há muitas variáveis que podem afetar o desempenho geral do seu sistema de serviços do MultiPoint. Talvez você queira considerar isso ao criar seu sistema.

## <a name="usage"></a>Uso

-   **Aplicativos** do O tipo e o número de aplicativos em execução ao mesmo tempo, especialmente \- os aplicativos gráficos pesados ou com uso intensivo de memória afetarão o desempenho geral do seu sistema. Para obter mais informações, consulte [aplicativos e conteúdo da Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).

-   **Uso da Internet** Considere se os usuários exibirão conteúdo multimídia ou páginas da Web que usam vídeos de movimento completo. Esse tipo de conteúdo pode sobrecarregar o sistema se muitos usuários estiverem exibindo simultaneamente.

    > [!NOTE]
    > O recurso de projeção nos serviços do MultiPoint, que permite que os professores projetem suas telas em seus monitores de alunos, não foi projetado para projetar vídeo de movimento completo. O recurso de projeção é projetado para fins de demonstração, como mostrar um procedimento.

-   **Dispositivos de alta velocidade** Se muitos usuários estiverem usando simultaneamente um dispositivo de alta velocidade, como uma câmara da Web ou um DVD Player, isso afetará o desempenho geral do sistema.

## <a name="configuration"></a>Configuração

-   **CPU, GPU e RAM** Consulte [otimizar o desempenho do sistema dos serviços do MultiPoint](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) neste guia para obter recomendações de CPU, GPU e RAM.
-   **Largura de banda da rede** Para estações conectadas RDP via LAN, a largura de banda de rede e a capacidade do cliente (por exemplo, um cliente fino, PC desktop ou laptop) são importantes, especialmente se o vídeo estiver em execução na sessão do usuário. Se você estiver usando clientes USB over-Ethernet zero, a largura de banda de rede também deverá ser uma consideração. Os dados de vídeo de todos os dispositivos são enviados pela mesma conexão Ethernet, portanto, talvez você queira considerar a configuração de uma rede Gigabit Ethernet separada ao usar esses dispositivos.
-   **RemoteFX** Para estações conectadas RDP via LAN, você poderá usar o RemoteFX para melhorar muito a entrega de conteúdo multimídia de alta definição.
-   **Resolução de vídeo** Se você tiver muita utilização de vídeo de tela inteira, convém considerar a redução da resolução do monitor para maximizar o desempenho.
-   **Número de clientes USB zero** O número total de clientes USB zero em um único hub raiz no servidor afetará diretamente o desempenho do vídeo. Para obter mais informações, consulte [layout para estações conectadas a clientes USB com zero](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). O número de estações de cliente USB via Ethernet com suporte pode ser um pouco menor do que o número de clientes USB zero.
-   **Largura de banda USB** Considere a largura de banda USB quando estiver projetando seu sistema.  Isso é especialmente importante para clientes USB com zero, que enviam dados de vídeo pela conexão USB. Para otimizar a largura de banda, minimize o número de dispositivos que estão conectados a uma única porta USB no servidor. Isso se aplica a estações encadeadas e hubs intermediários. Para obter mais informações, consulte [hubs de estação](MultiPoint-services-Site-Planning.md#station-hubs) e [hubs intermediários](MultiPoint-services-Site-Planning.md#intermediate-hubs).

-   **Tipo USB** Usar USB 3,0 em vez de USB 2,0 aumenta a largura de banda disponível entre o servidor e o hub intermediário se você estiver conectando mais de três clientes USB zero ao Hub ou se estiver usando dispositivos USB de alta largura de banda.

-   **Estações** O número total de estações afeta o desempenho. Se você tiver gráficos pesados, processamento ou necessidades de vídeo, talvez queira limitar o número total de estações. Para obter mais informações, consulte [otimizar o desempenho do sistema dos serviços do MultiPoint](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).