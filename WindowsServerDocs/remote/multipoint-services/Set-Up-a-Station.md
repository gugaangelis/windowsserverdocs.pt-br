---
title: Configurar uma estação
description: Saiba como configurar uma estação de no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2bba32f27ae01052a693d78f152d4487a04bd9bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880677"
---
# <a name="set-up-a-station"></a>Configurar uma estação
Uma *estação* do MultiPoint Services normalmente consiste em um *hub de estação*, mouse, teclado e monitor de vídeo. Este tópico descreve como conectar os dispositivos de hardware ao hub de estação para criar uma estação do MultiPoint Services.  
  
O hub de estação é um dispositivo de hardware que conecta dispositivos periféricos a um computador em um sistema MultiPoint Services. O MultiPoint Services dá suporte a dois tipos de hubs de estação:  
  
-   **Hub USB:** Um genérico multiporta expansão hub USB que está em conformidade com as especificações de barramento serial universal 2.0 ou posterior. Tais hubs normalmente têm duas, quatro ou mais portas USB que permitem que vários dispositivos USB sejam conectados a uma única porta USB no computador. Hubs USB são comumente separada de dispositivos que podem ser ativados ou barramento externamente. Quando usado como um hub de estação com o MultiPoint Services, recomendamos que você use um hub com quatro ou mais portas.  
  
    > [!IMPORTANT]  
    > Se você pretende conectar dispositivos USB que não sejam um teclado e mouse ao hub, recomendamos o uso de um hub alimentado externamente para obter o melhor desempenho.  
  
-   **Hub multifuncional:** Um hub de expansão que se conecta ao computador por meio de uma porta USB e permite a conexão de uma variedade de dispositivos USB não para o hub, incluindo um monitor de vídeo. Hubs multifuncionais produzidos pelos fabricantes de hardware específico e podem exigir a instalação de um driver de dispositivo específico.  
  
Se você deseja adicionar uma estação ao MultiPoint Services, primeiro certifique-se de que você tem portas de conexão suficientes disponíveis para o hardware da estação que deseja usar. Além disso, você deve proteger o número apropriado de *licenças de acesso para cliente (CALs)* para o seu sistema de serviços do MultiPoint.  
  
## <a name="setting-up-station-hardware"></a>Configuração do hardware da estação  
Os procedimentos desta seção descrevem como conectar o hardware da estação do MultiPoint Services aos diferentes tipos de hubs de estação.  
  
### <a name="to-set-up-a-station-with-a-usb-hub"></a>Para configurar uma estação com um hub USB  
  
1.  Antes de conectar uma nova estação, *encerre* todas as *sessões* do usuário, e, em seguida, desligue o computador e outros dispositivos ligados em seu sistema MultiPoint Services.  
  
2.  Conecte o cabo do novo monitor de vídeo à porta de vídeo no computador, conforme mostrado na seguinte ilustração:  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)  
  
3.  Conecte o novo hub USB a uma porta USB aberta no computador:  
  
    ![Imagem de conexão de hub USB do MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
4.  Conecte um teclado e mouse ao hub USB:  
  
    ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
6.  Ligue o computador.  
  
7.  O MultiPoint Services é iniciado. Siga as instruções exibidas no monitor de vídeo da nova estação para associar os dispositivos à nova estação.  
  
### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>Para configurar uma estação com um hub multifuncional  
  
1.  Antes de conectar uma nova estação, encerre todas as sessões do usuário, e, em seguida, desligue o computador e outros dispositivos ligados em seu sistema MultiPoint Services.  
  
2.  Conecte o cabo do novo monitor de vídeo à porta de vídeo DVI ou VGA no hub multifuncional, conforme mostrado na seguinte ilustração:  
  
    ![Imagem de hub multifuncional e de conexão de vídeo](./media/WMSMultifunctionHubVideoConnection.gif)  
  
3.  Conecte o novo hub multifuncional a uma porta USB aberta no computador:  
  
    ![Imagem de uma conexão de hub multifuncional](./media/WMSMultifunctionHubConnection.gif)  
  
4.  Conecte um teclado e mouse aos conectores PS2 ou USB no hub multifuncional:  
  
    ![Imagem de hub multifuncional e de conectores PS2](./media/WMSMultifunctionHubPS2Connection.gif)  
  
5.  Conecte o cabo de alimentação do monitor de vídeo a uma tomada.  
  
6.  Ligue o computador.  
  
7.  O MultiPoint Services é iniciado. Se solicitado, siga as instruções exibidas no monitor de vídeo da nova estação para *associar* os dispositivos à nova estação.  
  
## <a name="see-also"></a>Consulte também  
[Encerrar uma sessão de usuário](End-a-User-Session.md)  
[Reiniciar ou desligar](Restart-or-Shut-Down.md)  
[Gerenciar Hardware da estação](Manage-Station-Hardware.md)  
[Trabalhar com dispositivos USB](Work-with-USB-Devices.md)