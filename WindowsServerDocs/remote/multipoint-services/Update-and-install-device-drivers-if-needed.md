---
title: Atualizar e instalar drivers de dispositivo, se necessário
description: Saiba como verificar e atualizar drivers de dispositivo nos serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 766e2175a16cd20a68730870c8980ed9c9204a3c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394880"
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