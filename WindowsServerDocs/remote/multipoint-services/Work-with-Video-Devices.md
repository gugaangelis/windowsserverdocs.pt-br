---
title: Trabalhar com dispositivos de vídeo
description: Saiba como monitores e projetores de vídeo funcionam com estações nos serviços do MultiPoint
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d580185a2fc6adfb3bdf7acc160610aaf4d2b5b9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946829"
---
# <a name="work-with-video-devices"></a>Trabalhar com dispositivos de vídeo
Saiba como dispositivos de vídeo, como um monitor ou projetor, funcionam quando eles estão conectados a um computador no seu sistema MultiPoint Services ou a uma *estação* do MultiPoint Services.

## <a name="working-with-video-monitors"></a>Trabalhando com monitores de vídeo
Dependendo do hardware do seu sistema de serviços MultiPoint Services, há duas maneiras de conectar um monitor de vídeo:

-   Para *sistemas baseados em Hub USB*, conecte o cabo do monitor de vídeo a uma porta de vídeo aberta no computador, conforme mostrado na ilustração a seguir:

    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)

-   Para *sistemas baseados em hubs multifuncionais* com suporte a vídeo interno, conecte o cabo do monitor de vídeo à porta de vídeo no Hub multifuncional:

    ![Imagem de hub multifuncional e de conexão de vídeo](./media/WMSMultifunctionHubVideoConnection.gif)

Para obter mais informações, consulte o tópico [Configurar uma estação](Set-Up-a-Station.md).

## <a name="working-with-video-projectors"></a>Trabalhando com projetores de vídeo
Você pode conectar um projetor de vídeo ao seu sistema MultiPoint Services quando desejar projetar uma imagem grande para ser vista por outras pessoas; por exemplo, em laboratório. Para estações baseadas em Hub USB e multifuncionais, você tem duas opções para conectar um projetor a uma estação:

-   Substituir um monitor por um projetor e usar o projetor como o dispositivo de vídeo para essa estação, conforme mostrado na ilustração a seguir:

    ![Imagem de um projetor conectado a um computador](./media/WMSVideoProjectorConnection.gif)

-   Obtenha um dispositivo de divisão de vídeo para conectar um projetor e monitor à porta de vídeo da estação.

    O MultiPoint Services exibirá a mesma imagem em ambos os dispositivos de vídeo. Quando não estiver projetando, você pode desativar o projetor e usar apenas o monitor de vídeo.

Ao usar uma dessas opções, observe o seguinte:

-   Conectar um monitor de vídeo pode exigir que você *associe a estação* novamente para que o MultiPoint Services possa reconhecer corretamente o novo vídeo. Siga as instruções que aparecem no dispositivo de vídeo da estação.

-   Você precisará obter dispositivos adaptadores ou conversores para fazer a conversão entre entradas DVI e VGA.

-   O uso de um cabo de divisão "Y" pode diminuir a qualidade do vídeo em ambos os dispositivos de vídeo.

-   Ao usar um projetor e um monitor por meio de um cabo de divisão "Y", os serviços do MultiPoint ajustam a resolução de tela de ambos os dispositivos para a resolução máxima mais baixa de qualquer dispositivo, geralmente o projetor.

-   Os serviços do MultiPoint não oferecem suporte à expansão de uma exibição de uma única estação em vários monitores.

## <a name="see-also"></a>Consulte Também
[Gerenciar hardware](Manage-Station-Hardware.md) 
 de estação [Configurar uma estação](Set-Up-a-Station.md)