---
title: Configurar o computador físico e a estação principal
description: Saiba como configurar seu sistema primeiro, a estação principal, no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812007"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurar o computador físico e a estação principal
Antes de instalar o MultiPoint Services, você precisa configurar a estação principal para o seu sistema MultiPoint Services. Se você for usar uma rede local (LAN), conecte o computador à rede local.  
  
Um *estação* é um ponto de extremidade pelo qual o MultiPoint Services é acessado. O *estação principal* é a primeira estação para iniciar quando o MultiPoint Services é iniciado. Os administradores podem usar configurações e os menus de inicialização de acesso. A estação principal fornece acesso à configuração do sistema e solução de problemas de informações que está disponíveis apenas durante a inicialização e antes do MultiPoint Services sistema está em execução. Após a inicialização, você pode usar a estação principal, como você faria com qualquer outra estação.  
  
A estação principal deve ser uma estação de vídeo diretamente conectados. O procedimento a seguir descreve como conectar-se o hardware necessário para o computador do MultiPoint Services.  
  
Para obter mais informações sobre as estações, consulte [estações do MultiPoint](multipoint-services-stations.md). Para obter ajuda com fazer seleções de hardware, consulte [selecionando o Hardware para o sistema do MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Para obter informações sobre como conectar outras estações tipos ao MultiPoint Services, consulte [anexar estações adicionais para o computador do MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Para criar uma estação de vídeo conectado, você deve usar um teclado latino (por exemplo, um teclado inglês ou espanhol).  
  
## <a name="to-set-up-your-primary-station"></a>Para configurar sua estação principal  
  
1.  Certifique-se de que o computador executando o MultiPoint Services é desligado e desconectado.  
  
2.  Conecte o cabo de alimentação do monitor a uma tomada elétrica e conecte o cabo do monitor para a porta de vídeo no computador, conforme mostrado abaixo.  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)  
  
3.  Se a estação de usar um teclado USB e mouse, conclua as seguintes etapas:  
  
    1.  Conecte-se um hub USB externo para uma porta USB aberta no computador, conforme mostrado abaixo.  
  
        ![Imagem de conexão de hub USB do MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
    2.  Conecte-se o mouse e teclado USB ao hub USB.  
  
        ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Se o computador do MultiPoint Services tem portas PS/2, você pode, se necessário, use um mouse conectado diretamente ao computador e o teclado PS/2. No entanto, essa configuração tem limitações significativas. Os usuários não podem usar dispositivos de áudio, webcam e flash drives em estações de PS/2.  
  
    3.  Se você estiver usando um hub alimentado externamente, conecte o cabo de alimentação do hub a uma tomada elétrica.  
  
        > [!IMPORTANT]  
        > É altamente recomendável o uso de um hub alimentado. Comportamento imprevisível do sistema pode resultar de condições corrigiremos atual.  
        >   
        > Os usuários não devem anexar mouses e teclados diretamente às portas USB do computador. Isso provavelmente causará a associação incorreta de vários teclados e mouses a mesma estação ou a nenhuma estação em todos os.  
  
        > [!NOTE]  
        > O dispositivo de áudio de host na placa-mãe do sistema está disponível apenas enquanto o MultiPoint Services está no modo de console. Para garantir que o áudio ininterrupto para uma estação que usa um hub USB externo, você deve usar um dispositivo de áudio USB conectado ao hub.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Para conectar o computador à rede local  
  
-   Se você tiver uma rede local, conecte-se o computador à sua rede com um cabo de rede.