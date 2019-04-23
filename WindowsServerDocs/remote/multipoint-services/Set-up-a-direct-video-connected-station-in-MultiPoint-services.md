---
title: Configurar uma estação de vídeo diretamente conectados no MultiPoint Services
description: Saiba como criar uma estação de vídeo diretamente conectados no MultiPoint Services
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
ms.openlocfilehash: 58197164c91ab6b69b0ef331c025287f593f94c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850737"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configurar uma estação de vídeo diretamente conectados no MultiPoint Services
Em uma estação de vídeo conectado direta, o monitor está conectado diretamente a uma porta de vídeo no computador do MultiPoint Server. Um teclado e mouse são conectados a um hub USB e estão associadas com o monitor.  
  
A ilustração a seguir mostra um ambiente do MultiPoint Server que tem um único computador do MultiPoint Server e quatro estações vídeo diretamente conectados. Para obter mais informações, consulte [estações do MultiPoint Server](MultiPoint-services-Stations.md).  
  
**Sistema multiPoint Services com quatro conexões diretas de vídeos**  
  
![Imagem do layout do sistema com base em USB do MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Para configurar uma estação de vídeo diretamente conectados, você deve usar um teclado latino (por exemplo, um teclado de idioma inglês ou espanhol).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Para configurar uma estação direta de vídeo conectado  
  
1.  Conecte o cabo do monitor para a porta de vídeo no computador, conforme mostrado abaixo.  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif) 
  
2.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
3.  Conecte-se um hub USB a uma porta USB aberta no computador, conforme mostrado abaixo.  
  
    ![Imagem de conexão de hub USB do MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
4.  Conecte um teclado e mouse ao hub de estação USB.  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Conecte-se todos os periféricos adicionais, como fones de ouvido, para o hub USB.  
  
6.  Se você estiver usando um hub alimentado externamente, conecte o cabo de alimentação do hub a uma tomada elétrica.  
  
    > [!IMPORTANT]  
    > É altamente recomendável o uso de um hub alimentado. Comportamento imprevisível do sistema pode resultar de condições corrigiremos atual.  
    >   
    > Os usuários não devem anexar mouses e teclados diretamente às portas USB do computador. Isso provavelmente causará a associação incorreta de vários teclados e mouses a mesma estação ou a nenhuma estação em todos os.  
  
7.  Siga as instruções exibidas no monitor para criar a estação.  
  
Se você adicionar mais de uma estação direta de vídeo conectado ao seu ambiente do MultiPoint Services, a estação principal pode mudar. Você pode facilmente descobrir qual direcionar o vídeo estação conectada é sua principal estação.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Para descobrir qual direcionar estação vídeo conectado é a estação principal  
  
1.  Ative todos os monitores conectados diretamente a adaptadores de vídeo do computador (placas de vídeo).  
  
2.  Iniciar (ou reiniciar) do computador do MultiPoint Services e veja qual monitor exibirá as telas de inicialização. Essa estação é a estação principal.  
  
    > [!NOTE]  
    > Em alguns casos, informações de inicialização do BIOS são exibidas em vários monitores simultaneamente. Nesse caso, qualquer um dos monitores pode ser considerado "estação principal" com a finalidade de acessar o BIOS.