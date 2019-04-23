---
title: Trabalhar com dispositivos de vídeo
description: Saiba como o vídeo monitores e projetores trabalhar com estações no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d828ea911aaff27a1df79d0380dfe92987c3d2aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844197"
---
# <a name="work-with-video-devices"></a>Trabalhar com dispositivos de vídeo
Saiba como dispositivos de vídeo, como um monitor ou projetor, funcionam quando eles estão conectados a um computador no seu sistema MultiPoint Services ou a uma *estação* do MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Trabalhando com monitores de vídeo  
Dependendo do hardware do seu sistema de serviços MultiPoint Services, há duas maneiras de conectar um monitor de vídeo:  
  
-   Para *os sistemas baseados em hub USB*, conecte o cabo de monitor de vídeo à porta de vídeo no computador, conforme mostrado na ilustração a seguir:  
  
    ![Imagem de conexão de vídeo para sistema com base em hub USB](./media/WMSVideoConnection.gif)  
  
-   Para *multifuncionais sistemas baseados em hub* com suporte interno de vídeo, conecte o cabo do monitor de vídeo à porta de vídeo no hub multifuncional:  
  
    ![Imagem de hub multifuncional e de conexão de vídeo](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Para obter mais informações, consulte o tópico [Configurar uma estação](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Trabalhando com projetores de vídeo  
Você pode conectar um projetor de vídeo ao seu sistema MultiPoint Services quando desejar projetar uma imagem grande para ser vista por outras pessoas; por exemplo, em laboratório. Para ambas as USB com base em hub multifuncional com base em hub estações e, você tem duas opções para conectar um projetor a uma estação:  
  
-   Substituir um monitor por um projetor e usar o projetor como o dispositivo de vídeo para essa estação, conforme mostrado na ilustração a seguir:  
  
    ![Imagem de um projetor conectado ao computador](./media/WMSVideoProjectorConnection.gif)  
  
-   Obter um dispositivo divisor de vídeo para conectar um projetor e o monitor à porta de vídeo da estação.  
  
    O MultiPoint Services exibirá a mesma imagem em ambos os dispositivos de vídeo. Quando não estiver projetando, você pode desativar o projetor e usar apenas o monitor de vídeo.  
  
Ao usar uma dessas opções, observe o seguinte:  
  
-   Conectar um monitor de vídeo pode exigir que você *associe a estação* novamente até que o MultiPoint Services possa reconhecer corretamente o novo vídeo. Siga as instruções que aparecem no dispositivo de vídeo da estação.  
  
-   Você precisará obter dispositivos adaptadores ou conversores para fazer a conversão entre entradas DVI e VGA.  
  
-   A utilização de um cabo divisor “Y” pode diminuir a qualidade do vídeo em ambos os dispositivos de vídeo.  
  
-   Ao usar um projetor e um monitor por meio de um cabo divisor "Y", o MultiPoint Services ajusta a resolução da tela de ambos os dispositivos até a menor resolução máxima de qualquer dispositivo — geralmente o projetor.  
  
-   O MultiPoint Services não dá suporte à exibição de uma única estação em vários monitores.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar Hardware da estação](Manage-Station-Hardware.md)  
[Configurar uma estação](Set-Up-a-Station.md) 