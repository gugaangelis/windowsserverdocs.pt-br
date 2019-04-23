---
title: Variáveis que afetam o desempenho do sistema MultiPoint Services
description: Informações de desempenho para o MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867577"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variáveis que afetam o desempenho do sistema MultiPoint Services
Há muitas variáveis que podem afetar o desempenho geral do seu sistema MultiPoint Services. Você poderá considerar esses ao projetar seu sistema.  
  
## <a name="usage"></a>Uso  
  
-   **Aplicativos** o tipo e o número de aplicativos em execução ao mesmo tempo, especialmente gráfico\-pesado ou aplicativos com uso intenso afetará o desempenho geral do seu sistema de memória. Para obter mais informações, consulte [aplicativos e conteúdo de Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Uso da Internet** considerar se seus usuários irão ver conteúdo multimídia ou páginas da web que use vídeos de movimento completo. Esse tipo de conteúdo pode sobrecarregar o sistema se muitos usuários estão exibindo simultaneamente.  
  
    > [!NOTE]  
    > O recurso de projeção no MultiPoint Services, que permite aos professores projetar suas telas em seus monitores de aluno, não foi projetado para vídeo full-motion do projeto. O recurso de projeção foi projetado para fins de demonstração, como mostrar um procedimento.  
  
-   **Dispositivos de alta velocidade** se muitos usuários simultaneamente estiver usando um dispositivo de alta velocidade, como uma webcam ou um DVD player, isso afeta o desempenho geral do sistema.  
  
## <a name="configuration"></a>Configuração  
  
-   **CPU, GPU e RAM** ver [otimizar o desempenho do sistema MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) neste guia para obter recomendações de RAM, CPU e GPU.  
-   **Largura de banda de rede** para RDP através de LAN estações conectadas, a largura de banda de rede e a capacidade do cliente (por exemplo, um cliente fino, desktop PC ou laptop) é importante, especialmente se o vídeo está em execução na sessão do usuário. Se você estiver usando clientes USB over Ethernet zero, a largura de banda de rede também deve ser uma consideração. Dados de vídeo para todos os dispositivos são enviados pela mesma conexão de Ethernet, portanto, você talvez queira considerar como configurar uma rede Gigabit Ethernet separada ao usar esses dispositivos.  
-   **RemoteFX** para estações de RDP através de LAN conectada, você poderá usar o RemoteFX para melhorar consideravelmente a entrega de conteúdo multimídia de alta definição.  
-   **Resolução de vídeo** se você tiver o uso intenso de vídeo em tela inteira, você talvez queira considerar a reduzir a resolução de monitor para maximizar o desempenho.  
-   **Número de clientes USB zero** o número total de clientes de zero USB em um hub de raiz única no servidor irá afetar diretamente o desempenho do vídeo. Para obter mais informações, consulte [Layout para USB Zero cliente conectado estações](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). O número de estações de cliente de USB over Ethernet zero com suporte pode ser um pouco menor que o número de clientes USB zero.  
-   **Largura de banda USB** considerar a largura de banda USB durante a criação de seu sistema.  Isso é especialmente importante para clientes USB zero, que enviam dados de vídeo sobre a conexão USB. Para otimizar a largura de banda, minimize o número de dispositivos que estão conectados a uma única porta USB no servidor. Isso se aplica a estações com corrente margarida encadeada e hubs de intermediários. Para obter mais informações, consulte [hubs de estação](MultiPoint-services-Site-Planning.md#station-hubs) e [intermediária hubs](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Tipo USB** usando USB 3.0 em vez de USB 2.0 aumenta a largura de banda disponível entre o servidor e o hub intermediário, se você estiver se conectando a mais de três clientes USB zero para o hub ou se você estiver usando dispositivos USB de alta largura de banda.  
  
-   **Estações** o número total de estações afeta o desempenho. Se você tiver necessidades de vídeos, processamento ou gráficos pesados, você talvez queira limitar o número total de estações. Para obter mais informações, consulte [desempenho do sistema do MultiPoint Services otimizar](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).