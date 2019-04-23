---
title: Gerenciar o hardware da estação
description: Fornece uma visão geral de como gerenciar o hardware com estações MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 3a1cdfd12c9bd6a21fec9cbfffae04573ef5ea98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841627"
---
# <a name="manage-station-hardware"></a>Gerenciar o hardware da estação
Um sistema MultiPoint Services consiste em um único computador e pelo menos uma estação. Hardware da estação normalmente consiste em um hub de estação, mouse, teclado e monitor de vídeo. Estações normalmente ficam conectadas fisicamente ao computador.  
  
A ilustração a seguir mostra um exemplo do layout de um sistema MultiPoint Services que contém quatro estações. Cada estação é conectada ao computador do MultiPoint Services usando um hub USB e placas de vídeo de vários monitores. Esta ilustração não representa estações conectadas por meio de hubs multifuncionais.  
   
![Imagem do layout do sistema com base em USB do MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
Os tópicos nesta seção descrevem como você pode ver o status de hardware conectado ao seu sistema MultiPoint Services e fornecem informações detalhadas sobre os tipos de dispositivos USB e outros dispositivos de hardware periféricos que você pode usar para configurar uma estação do MultiPoint Services. A seguir, há uma breve descrição dos tópicos desta seção que pode ajudá-lo a selecionar o hardware e configurar sua estação do MultiPoint Services.  
  
## <a name="view-hardware-status"></a>Exibir status de hardware  
No Gerenciador do MultiPoint, você pode usar o **estações** guia para exibir o status das estações e dispositivos de hardware que estão conectados ao computador do MultiPoint Services. Para obter mais informações sobre como ver o status do seu computador do MultiPoint Services e os dispositivos de hardware conectados a ele, consulte o tópico [Exibir o status de hardware](View-Hardware-Status.md).  
  
## <a name="work-with-usb-devices"></a>Trabalhar com dispositivos USB  
Quando dispositivos USB e outros dispositivos de hardware periféricos são conectados ao sistema MultiPoint Services, eles funcionam de forma diferente quando estão conectados ao computador do MultiPoint Services do que quando estão conectados a uma estação do MultiPoint Services. O [trabalhar com dispositivos USB](Work-with-USB-Devices.md) tópico descreve o hardware como diferentes dispositivos podem funcionar nessas situações e fornece informações detalhadas sobre como trabalhar com hubs de estação.  
  
## <a name="work-with-video-devices"></a>Trabalhar com dispositivos de vídeo  
O tópico [Trabalhar com dispositivos de vídeo](Work-with-Video-Devices.md) discute como dispositivos de vídeo, como um monitor ou projetor, funcionam quando eles estão conectados a um computador no seu sistema MultiPoint Services ou a uma estação do MultiPoint Services.  
  
## <a name="set-up-a-station"></a>Configurar uma estação  
O tópico [Configurar uma estação](Set-Up-a-Station.md) descreve como conectar os dispositivos de hardware periféricos ao hub de estação do MultiPoint Services para criar uma estação do MultiPoint Services. O MultiPoint Services dá suporte a dois tipos de hubs de estação:  
  
-   Uma porta com vários externamente ou ativada por barramento *hub USB* que pode dar suporte a um mouse, teclado e outros dispositivos USB periféricos  
  
-   Um *hub multifuncional* que pode incluir um controlador de vídeo integrado e as portas para mouse, teclado e dispositivos periféricos de áudio  
  
Os dois tipos de hubs de estação se conectam ao computador por meio de um cabo USB. Os procedimentos no tópico [Configurar uma estação](Set-Up-a-Station.md) descrevem como conectar dispositivos de hardware para cada tipo de hub de estação.  
  
## <a name="see-also"></a>Consulte também  
[Exibir o Status de Hardware](View-Hardware-Status.md)  
[Trabalhar com dispositivos USB](Work-with-USB-Devices.md)  
[Trabalhar com dispositivos de vídeo](Work-with-Video-Devices.md)  
[Configurar uma estação](Set-Up-a-Station.md)