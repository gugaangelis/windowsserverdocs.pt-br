---
title: Anexar estações adicionais ao seu servidor MultiPoint
description: Adicionar mais estações para sua implantação de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889257"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Anexar estações adicionais ao MultiPoint Services
Em seu ambiente do MultiPoint Services, os usuários usar estações para se conectar aos serviços do MultiPoint e fazer seu trabalho. As estações são os pontos de extremidade do usuário para se conectar ao computador executando o Multipoint Services.  
  
MultiPoint services dá suporte a três tipos de estação:  
  
-   Estações de vídeo diretamente conectados  
  
-   USB zero cliente conectado estações (e USB em estações de cliente conectado Ethernet zero)  
  
-   Estações conectadas à RDP através de LAN  
  
As classificações são baseadas em hardware de uma estação e o tipo de conexão que ele usa. Você pode misturar e combinar tipos de conexão para suas estações. O único requisito é que a estação principal (o que você instalou anteriormente) deve ser uma estação de vídeo diretamente conectados. Para obter mais informações sobre configurações de estação, consulte [estações do MultiPoint](MultiPoint-services-Stations.md).  
  
Para obter instruções que explicam como configurar cada tipo de estação, consulte o seguinte:  
  
-   [Configurar uma estação de vídeo diretamente conectados](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurar um USB zero estação cliente conectado](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configurar uma estação de RDP através de LAN conectada](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Para obter uma comparação detalhada dos tipos de estação, consulte [comparação de tipo de estação](multipoint-services-stations.md#BKMK_StationTypeComparison).  
  
> [!NOTE]  
> -   Os procedimentos para anexar estações não descrevem como configurar hubs intermediários ou hubs downstream. Para obter informações sobre onde instalar esses hubs, consulte [estações do MultiPoint](MultiPoint-services-Stations.md).  
> -   Em alguns casos, talvez você precise criar áreas de trabalho virtuais que são executados em máquinas virtuais de estação. Por exemplo, você usar aplicativos que não podem ser instalados no Windows Server ou aplicativos que não executará várias instâncias no mesmo computador host. Para obter mais informações, consulte [áreas de trabalho virtuais para estações de criar o Windows 10 Enterprise](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> É útil criar suas estações na ordem de seus respectivos locais físicos, para que eles são identificados em sequência no MultiPoint Server. Se posteriormente você desejar alterar o nome de uma estação, você pode fazer isso no Gerenciador do MultiPoint. Para obter mais informações, consulte remapear todas as estações na Ajuda do MultiPoint Server e suporte.