---
title: Atualizar e instalar drivers de dispositivo, se necessário
description: Saiba como verificar e atualizar drivers de dispositivo nos serviços do MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6d20aa80edeafa4311262a380cfd7aad65ae0315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820599"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Atualizar e instalar drivers de dispositivo, se necessário
Se você estiver usando clientes USB com zero ou periféricos que exigem drivers, instale os drivers no momento. É uma boa ideia também verificar **Device Manager** em busca de alertas de driver e instalar drivers para esses dispositivos.  
  
Em geral, os drivers mais atuais são necessários para os seguintes tipos de dispositivos:  
  
-   Clientes USB zero  
  
-   Clientes USB over-Ethernet nulos  
  
-   Controladores de disco  
  
-   Adaptadores de Rede  
  
-   Controladores de som  
  
-   Controladores de host USB

-   Placas gráficas


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Para verificar alertas de driver no Device Manager  
  
1.  Abra a tela iniciar.  
  
2.  Digite **Gerenciamento do computador**e clique em **Gerenciamento do computador** nos resultados.  
  
3.  Na árvore de console de gerenciamento do computador, clique em **Device Manager**.  
  
4.  Nos dispositivos do sistema à direita, verifique se há alertas de driver que podem afetar o MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Para instalar drivers de dispositivo no MultiPoint Manager  
  
1.  Para abrir o Gerenciador do MultiPoint, pesquise "Gerenciador do MultiPoint" e clique em **Gerenciador do MultiPoint** nos resultados.  
  
2.  No Gerenciador do MultiPoint, clique na guia **início** e, em seguida, clique em **alternar para o modo de console**.  
  
3.  Para instalar um driver de dispositivo, clique duas vezes no arquivo de driver e siga as instruções para instalar o driver.  
  
4.  Repita a etapa anterior para instalar todos os drivers necessários.  
  
    > [!NOTE]  
    > Se uma instalação exigir uma reinicialização do computador, será necessário voltar para o modo de console antes de instalar o próximo driver. O MultiPoint Server sempre inicia no modo de estação. Para alternar para o modo de console, vá para a guia **início** no Gerenciador do MultiPoint e clique em **alternar para o modo de console**.