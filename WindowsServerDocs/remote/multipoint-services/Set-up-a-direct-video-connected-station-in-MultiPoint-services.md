---
title: Configurar uma estação conectada diretamente ao vídeo nos serviços do MultiPoint
description: Saiba como criar uma estação conectada diretamente ao vídeo nos serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: eda8d5eee0635370873adec5b1fde2d65fc9fd9c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871598"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configurar uma estação conectada diretamente ao vídeo nos serviços do MultiPoint
Em uma estação conectada direta por vídeo, o monitor é conectado diretamente a uma porta de vídeo no computador do MultiPoint Server. Um teclado e um mouse são conectados a um hub USB e estão associados ao monitor.  
  
A ilustração a seguir mostra um ambiente do MultiPoint Server que tem um único computador MultiPoint Server e quatro estações conectadas diretamente ao vídeo. Para obter mais informações, consulte [estações do MultiPoint Server](MultiPoint-services-Stations.md).  
  
**Sistema de serviços do MultiPoint com quatro conexões de vídeo diretas**  
  
![Imagem do layout do sistema baseado em USB dos serviços do MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Para configurar uma estação conectada diretamente ao vídeo, você deve usar um teclado latino (como um teclado inglês ou espanhol).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Para configurar uma estação conectada direta por vídeo  
  
1.  Conecte o cabo do monitor à porta de exibição do vídeo no computador, conforme mostrado abaixo.  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif) 
  
2.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
3.  Conecte um hub USB a uma porta USB aberta no computador, conforme mostrado abaixo.  
  
    ![Imagem da conexão do hub USB dos serviços do MultiPoint](./media/WMSUSBHubConnection.gif)  
  
4.  Conecte um teclado e um mouse ao Hub de estação USB.  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Conecte quaisquer periféricos adicionais, como fones de ouvido, ao hub USB.  
  
6.  Se você estiver usando um hub com energia externa, conecte o cabo de alimentação do hub a uma tomada de energia.  
  
    > [!IMPORTANT]  
    > É altamente recomendável o uso de um hub equipado. Comportamentos incorretos do sistema podem resultar de condições sob medida atuais.  
    >   
    > Os usuários não devem anexar mouses e teclados diretamente às portas USB do computador. Isso provavelmente causará a associação incorreta de vários teclados e mouses à mesma estação, ou não a nenhuma estação.  
  
7.  Siga as instruções que aparecem no monitor para criar a estação.  
  
Se você adicionar mais de uma estação conectada ao vídeo direta ao seu ambiente de serviços do MultiPoint, a estação principal poderá ser alterada. Você pode descobrir facilmente qual estação conectada ao vídeo direta é sua estação principal.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Para descobrir qual estação conectada ao vídeo direta é a estação principal  
  
1.  Ative todos os monitores que estão conectados diretamente aos adaptadores de vídeo do computador (placas de vídeo).  
  
2.  Inicie (ou reinicie) o computador dos serviços do MultiPoint e veja qual Monitor exibe as telas de inicialização. Essa estação é a estação principal.  
  
    > [!NOTE]  
    > Em alguns casos, as informações de inicialização do BIOS são exibidas simultaneamente em vários monitores. Nesse caso, qualquer um dos monitores pode ser considerado a "estação principal" com a finalidade de acessar o BIOS.