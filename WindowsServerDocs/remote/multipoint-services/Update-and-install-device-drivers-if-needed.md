---
title: Atualizar e instalar drivers de dispositivo, se necessário
description: Saiba como verificar e atualizar drivers de dispositivo no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829127"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Atualizar e instalar drivers de dispositivo, se necessário
Se você estiver usando clientes USB zero ou periféricos que requerem drivers, você deve instalar os drivers no momento. É uma boa ideia também Verifique **Gerenciador de dispositivos** para quaisquer alertas do driver e instalar drivers para esses dispositivos.  
  
Em geral, os drivers mais recentes são necessários para os seguintes tipos de dispositivos:  
  
-   Clientes USB zero  
  
-   Clientes de USB over Ethernet zero  
  
-   Controladores de disco  
  
-   Adaptadores de Rede  
  
-   Controladores de som  
  
-   Controladores de host USB

-   Placas gráficas


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Para verificar se há alertas de driver no Gerenciador de dispositivos  
  
1.  Abra a tela inicial.  
  
2.  Tipo de **gerenciamento do computador**e, em seguida, clique em **gerenciamento de computador** nos resultados.  
  
3.  Na árvore de console de gerenciamento do computador, clique em **Gerenciador de dispositivos**.  
  
4.  Nos dispositivos do sistema no lado direito, verifique se há alertas de driver que podem afetar o MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Para instalar drivers de dispositivo no Gerenciador do MultiPoint  
  
1.  Para abrir o Gerenciador do MultiPoint, pesquise "MultiPoint Manager" e, em seguida, clique em **MultiPoint Manager** nos resultados.  
  
2.  No Gerenciador do MultiPoint, clique o **Home** guia e, em seguida, clique em **alterne para modo de console**.  
  
3.  Para instalar um driver de dispositivo, clique duas vezes o arquivo de driver e siga as instruções para instalar o driver.  
  
4.  Repita a etapa anterior para instalar todos os drivers necessários.  
  
    > [!NOTE]  
    > Se a instalação exigir uma reinicialização do computador, você precisará retornar ao modo de console antes de instalar o driver Avançar. MultiPoint server sempre inicia no modo de estação. Para alternar para modo de console, vá para o **página inicial** guia o MultiPoint Manager e clique em **alterne para modo de console**.