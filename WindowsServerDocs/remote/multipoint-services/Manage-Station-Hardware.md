---
title: Gerenciar o hardware da estação
description: Fornece uma visão geral de como gerenciar hardware para estações multiponto
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8ffb6fd714293471a0e9aa020390943b201261c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853519"
---
# <a name="manage-station-hardware"></a>Gerenciar o hardware da estação
Um sistema MultiPoint Services consiste em um único computador e pelo menos uma estação. O hardware da estação normalmente consiste em um hub de estação, mouse, teclado e monitor de vídeo. Estações normalmente ficam conectadas fisicamente ao computador.  
  
A ilustração a seguir mostra um exemplo do layout de um sistema MultiPoint Services que contém quatro estações. Cada estação é conectada ao computador dos serviços do MultiPoint usando um hub USB e placas de vídeo com vários monitores. Essa ilustração não representa estações conectadas usando hubs multifuncionais.  
   
![Imagem do layout do sistema baseado em USB dos serviços do MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
Os tópicos nesta seção descrevem como você pode ver o status de hardware conectado ao seu sistema MultiPoint Services e fornecem informações detalhadas sobre os tipos de dispositivos USB e outros dispositivos de hardware periféricos que você pode usar para configurar uma estação do MultiPoint Services. A seguir, há uma breve descrição dos tópicos desta seção que pode ajudá-lo a selecionar o hardware e configurar sua estação do MultiPoint Services.  
  
## <a name="view-hardware-status"></a>Exibir status de hardware  
No MultiPoint Manager, você pode usar a guia **estações** para exibir o status das estações e dispositivos de hardware que estão conectados ao computador dos serviços do MultiPoint. Para obter mais informações sobre como ver o status do seu computador do MultiPoint Services e os dispositivos de hardware conectados a ele, consulte o tópico [Exibir o status de hardware](View-Hardware-Status.md).  
  
## <a name="work-with-usb-devices"></a>Trabalhar com dispositivos USB  
Quando dispositivos USB e outros dispositivos de hardware periféricos são conectados ao sistema MultiPoint Services, eles funcionam de forma diferente quando estão conectados ao computador do MultiPoint Services do que quando estão conectados a uma estação do MultiPoint Services. O tópico [Trabalhar com dispositivos USB](Work-with-USB-Devices.md) descreve como diferentes dispositivos de hardware podem funcionar nessas situações e fornece informações detalhadas sobre como trabalhar com hubs de estação.  
  
## <a name="work-with-video-devices"></a>Trabalhar com dispositivos de vídeo  
O tópico [Trabalhar com dispositivos de vídeo](Work-with-Video-Devices.md) discute como dispositivos de vídeo, como um monitor ou projetor, funcionam quando eles estão conectados a um computador no seu sistema MultiPoint Services ou a uma estação do MultiPoint Services.  
  
## <a name="set-up-a-station"></a>Configurar uma estação  
O tópico [Configurar uma estação](Set-Up-a-Station.md) descreve como conectar os dispositivos de hardware periféricos ao hub de estação do MultiPoint Services para criar uma estação do MultiPoint Services. O MultiPoint Services dá suporte a dois tipos de hubs de estação:  
  
-   Um *hub USB* multiporta com alimentação externa ou barramento que pode dar suporte a um mouse, teclado e outros dispositivos USB periféricos  
  
-   Um *hub multifuncional* que pode incluir um controlador de vídeo integrado e as portas para mouse, teclado e dispositivos periféricos de áudio  
  
Os dois tipos de hubs de estação se conectam ao computador por meio de um cabo USB. Os procedimentos no tópico [Configurar uma estação](Set-Up-a-Station.md) descrevem como conectar dispositivos de hardware para cada tipo de hub de estação.  
  
## <a name="see-also"></a>Consulte também  
[Exibir o status do hardware](View-Hardware-Status.md)  
[Trabalhar com dispositivos USB](Work-with-USB-Devices.md)  
[Trabalhar com dispositivos de vídeo](Work-with-Video-Devices.md)  
[Configurar uma estação](Set-Up-a-Station.md)