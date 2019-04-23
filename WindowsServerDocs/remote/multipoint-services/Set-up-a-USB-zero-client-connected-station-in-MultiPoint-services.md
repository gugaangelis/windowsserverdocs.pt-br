---
title: Configurar um USB zero estação cliente conectado no MultiPoint Services
description: Saiba como criar um USB zero estação do cliente no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878927"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurar um USB zero estação cliente conectado no MultiPoint Services
Quando você usa clientes USB zero para criar estações do MultiPoint Services, o monitor para cada estação está conectado à porta de vídeo de zero cliente USB, conforme mostrado na ilustração a seguir. Para obter mais informações sobre esse e outros tipos de estação, consulte [estações do MultiPoint](MultiPoint-services-Stations.md).
  
**Sistema multiPoint Services com uma estação de vídeo diretamente conectados e duas estações USB zero cliente conectado**  
  
![Estações conectadas com zero cliente USB](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Antes de configurar o USB zero estações conectadas ao cliente, certifique-se de instalar os drivers mais recentes de placas de vídeo e o zero cliente USB. Drivers desatualizados podem impedir que a configuração do MultiPoint Services seja concluída com êxito. Para obter instruções, consulte [Update e instale drivers de dispositivo, se necessário](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Se você estiver usando um cliente de USB over Ethernet zero, siga as instruções do fornecedor, esse procedimento, em vez de usar a conexão de Ethernet para configurar o dispositivo na rede.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Para configurar um USB zero estação cliente conectado  
  
1.  Conecte o cabo do monitor de vídeo à porta de vídeo DVI ou VGA no USB zero cliente, conforme mostrado na ilustração a seguir.  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)  
  
2.  Conecte-se de zero cliente USB a uma porta USB aberta no computador.  
  
    ![Imagem de conexão de hub USB do MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
3.  Conecte um teclado e mouse para o zero cliente USB.  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Se você estiver usando um alimentado externamente zero cliente USB, conecte o cabo do zero cliente USB a uma tomada elétrica.  
  
5.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
6.  Se você for solicitado a associar os dispositivos com a estação, siga as instruções no monitor para concluir a instalação. (Em geral, USB zero estações cliente conectado são associadas com estações automaticamente conforme você adicioná-los para o servidor.)