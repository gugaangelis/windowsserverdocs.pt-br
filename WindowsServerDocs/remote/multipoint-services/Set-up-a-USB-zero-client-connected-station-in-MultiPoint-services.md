---
title: Configurar uma estação conectada por cliente USB zero nos serviços do MultiPoint
description: Saiba como criar uma estação de cliente USB zero nos serviços do MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 688b4908cd52dd53ba88f0a35cabccb58c289ba1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855639"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurar uma estação conectada por cliente USB zero nos serviços do MultiPoint
Quando você usa clientes USB com zero para criar estações de serviços de MultiPoint, o monitor de cada estação é conectado à porta de vídeo no cliente USB zero, conforme mostrado na ilustração a seguir. Para obter mais informações sobre esse e outros tipos de estação, consulte [MultiPoint stations](MultiPoint-services-Stations.md).
  
**Sistema de serviços do MultiPoint com uma estação conectada diretamente ao vídeo e duas estações USB com cliente zero**  
  
![Estações conectadas com zero cliente USB](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Antes de configurar estações conectadas a clientes USB com zero, certifique-se de instalar os drivers mais recentes para as placas de vídeo e o cliente USB zero. Drivers desatualizados podem impedir que a configuração de serviços do MultiPoint seja concluída com êxito. Para obter instruções, consulte [atualizar e instalar drivers de dispositivo, se necessário](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Se você estiver usando um cliente USB over-Ethernet zero, siga as instruções do seu fornecedor, em vez desse procedimento, para usar a conexão Ethernet para configurar o dispositivo na rede.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Para configurar uma estação conectada por cliente USB zero  
  
1.  Conecte o cabo do monitor de vídeo à porta de vídeo DVI ou VGA no cliente USB zero, conforme mostrado na ilustração a seguir.  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)  
  
2.  Conecte o cliente USB de zero a uma porta USB aberta no computador.  
  
    ![Imagem da conexão do hub USB dos serviços do MultiPoint](./media/WMSUSBHubConnection.gif)  
  
3.  Conecte um teclado e um mouse ao cliente USB zero.  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Se você estiver usando um cliente USB com alimentação externa, conecte o cabo de alimentação do cliente USB de zero a uma tomada de energia.  
  
5.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
6.  Se você for solicitado a associar dispositivos à estação, siga as instruções no monitor para concluir a instalação. (Em geral, as estações conectadas a clientes USB com zero são associadas às estações automaticamente à medida que são adicionadas ao servidor.)