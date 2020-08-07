---
title: Anexar estações adicionais ao servidor do MultiPoint
description: Adicionar mais estações à sua implantação do MultiPoint Services
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: a73c0c9fe077da5baa850bb0c4a8f6d6a018397e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955403"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Anexar estações adicionais aos serviços do MultiPoint
Em seu ambiente de serviços do MultiPoint, os usuários usam estações para se conectarem aos serviços do MultiPoint e realizarem seu trabalho. As estações são os pontos de extremidade do usuário para se conectarem ao computador que executa os serviços do MultiPoint.

Os serviços do MultiPoint dão suporte a três tipos de estação:

-   Estações conectadas diretamente a vídeo

-   Estações conectadas ao cliente USB zero (e USB em estações conectadas a cliente Ethernet zero)

-   Estações conectadas RDP via LAN

As classificações são baseadas no hardware de uma estação e no tipo de conexão que ela usa. Você pode misturar e corresponder tipos de conexão para suas estações. O único requisito é que a estação primária (que você instalou anteriormente) deve ser uma estação conectada por vídeo direto. Para obter mais informações sobre as configurações de estação, consulte [MultiPoint stations](MultiPoint-services-Stations.md).

Para obter instruções que explicam como configurar cada tipo de estação, consulte o seguinte:

-   [Configurar uma estação conectada diretamente ao vídeo](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)

-   [Configurar uma estação conectada ao cliente USB zero](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)

-   [Configurar uma estação conectada usando RDP na LAN](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)

Para obter uma comparação detalhada dos tipos de estação, consulte [comparação de tipo de estação](multipoint-services-stations.md#BKMK_StationTypeComparison).

> [!NOTE]
> -   Os procedimentos para anexar estações não descrevem como configurar hubs intermediários ou hubs downstream. Para obter informações sobre onde instalar esses hubs, consulte [MultiPoint stations](MultiPoint-services-Stations.md).
> -   Em alguns casos, talvez seja necessário criar áreas de trabalho virtuais de estação, que são executadas em máquinas virtuais. Por exemplo, você usa aplicativos que não podem ser instalados no Windows Server ou em aplicativos que não executarão várias instâncias no mesmo computador host. Para obter mais informações, consulte [criar áreas de trabalho virtuais do Windows 10 Enterprise para estações](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).

> [!TIP]
> É útil criar suas estações na ordem de seus locais físicos para que eles sejam identificados em sequência no MultiPoint Server. Se posteriormente você quiser alterar o nome de uma estação, poderá fazer isso no MultiPoint Manager. Para obter mais informações, consulte remapear todas as estações na ajuda e suporte do MultiPoint Server.