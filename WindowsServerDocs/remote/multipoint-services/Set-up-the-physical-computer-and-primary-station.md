---
title: Configurar o computador físico e a estação principal
description: Saiba como configurar seu primeiro sistema, a estação principal, nos serviços do MultiPoint
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 97141a300156c74ef5a2a3b1c4d738b4e33834f1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991471"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurar o computador físico e a estação principal
Antes de instalar os serviços do MultiPoint, você precisa configurar a estação primária para o seu sistema de serviços do MultiPoint. Se você usar uma rede local (LAN), conecte o computador à LAN.

Uma *estação* é um ponto de extremidade pelo qual os serviços do MultiPoint são acessados. A *estação principal* é a primeira estação a ser iniciada quando os serviços do MultiPoint são iniciados. Os administradores podem usá-lo para acessar os menus e as configurações de inicialização. A estação principal fornece acesso à configuração do sistema e às informações de solução de problemas disponíveis somente durante a inicialização e antes da execução do sistema de serviços do MultiPoint. Após a inicialização, você pode usar a estação primária como faria com qualquer outra estação.

A estação principal deve ser uma estação conectada diretamente ao vídeo. O procedimento a seguir descreve como conectar o hardware necessário ao computador dos serviços do MultiPoint.

Para obter mais informações sobre estações, consulte [MultiPoint stations](multipoint-services-stations.md). Para obter ajuda sobre como fazer seleções de hardware, consulte [selecionando hardware para o sistema de serviços do MultiPoint](./select-hardware-mps.md). Para obter informações sobre como conectar outros tipos de estações a serviços do MultiPoint, consulte [anexar estações adicionais ao computador dos serviços do MultiPoint](./multipoint-attach-additional-stations.md).

> [!NOTE]
> Para criar uma estação conectada por vídeo, você deve usar um teclado latino (como um teclado inglês-ou espanhol).

## <a name="to-set-up-your-primary-station"></a>Para configurar sua estação primária

1.  Verifique se o computador que está executando os serviços do MultiPoint está desligado e desconectado.

2.  Conecte o cabo de alimentação do monitor a uma tomada de energia e conecte o cabo do monitor à porta de exibição do vídeo no computador, conforme mostrado abaixo.

    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)

3.  Se a estação usar um teclado e mouse USB, conclua as seguintes etapas:

    1.  Conecte um hub USB externo a uma porta USB aberta no computador, conforme mostrado abaixo.

        ![Imagem da conexão do hub USB dos serviços do MultiPoint](./media/WMSUSBHubConnection.gif)

    2.  Conecte o teclado USB e o mouse ao hub USB.

        ![Imagem de conexões de dispositivo de entrada de hub USB](./media/WMSUSBDeviceConnection.gif)

        > [!NOTE]
        > Se o computador dos serviços do MultiPoint tiver portas PS/2, você poderá, se necessário, usar um teclado e mouse PS/2 conectado diretamente ao computador. No entanto, essa configuração tem limitações significativas. Os usuários não podem usar dispositivos de áudio, Web câmeras e flash drives em estações PS/2.

    3.  Se você estiver usando um hub com energia externa, conecte o cabo de alimentação do hub a uma tomada de energia.

        > [!IMPORTANT]
        > É altamente recomendável o uso de um hub equipado. Comportamentos incorretos do sistema podem resultar de condições sob medida atuais.
        >
        > Os usuários não devem anexar mouses e teclados diretamente às portas USB do computador. Isso provavelmente causará a associação incorreta de vários teclados e mouses à mesma estação, ou não a nenhuma estação.

        > [!NOTE]
        > O dispositivo de áudio do host na placa-mãe do sistema só está disponível quando os serviços do MultiPoint estão no modo de console. Para garantir o áudio ininterrupto de uma estação que usa um hub USB externo, você deve usar um dispositivo de áudio USB conectado ao Hub.

## <a name="to-connect-the-computer-to-the-lan"></a>Para conectar o computador à LAN

-   Se você tiver uma LAN, conecte o computador à sua rede com um cabo de rede.