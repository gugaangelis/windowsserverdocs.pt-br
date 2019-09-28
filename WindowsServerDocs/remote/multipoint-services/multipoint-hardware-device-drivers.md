---
title: Coletar drivers de hardware e dispositivos necessários à instalação
description: Informações sobre os drivers que você precisa instalar para os serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: cfbb8c8b68768c72b869df539c93f05e7e01d256
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394696"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Coletar drivers de hardware e dispositivos necessários à instalação
Antes de começar a implantar o sistema MultiPoint Services, você precisará de:  
  
-   **Componentes de hardware para o servidor** -instale quaisquer placas de vídeo adicionais ou outros componentes do sistema no momento.  
  
-   **Componentes de hardware para as estações** – para obter informações sobre as estações de planejamento para seu ambiente, consulte [selecionando hardware para o sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **Os drivers mais recentes para suas placas de vídeo** -se o fabricante do seu OEM ou do dispositivo não fornecesse essas informações, você precisará baixá-las no site do fabricante do dispositivo.  
  
-   **Os drivers de cliente USB mais recentes** -se você estiver usando estações de cliente USB zero, deverá instalar os drivers de cliente USB mais recentes.  
  
    > [!IMPORTANT]  
    > Para uma instalação de serviços do MultiPoint, você deve instalar a versão de 64 bits de qualquer driver.  
  
> [!TIP]  
> Se você estiver instalando os serviços do MultiPoint em um computador com uma versão diferente do Windows já instalada, você deverá descobrir a marca e o modelo da placa de vídeo no Device Manager antes de iniciar a instalação do Windows Server e garantir que você possa obter drivers que são disponível para o Windows Server 2016. Abra Device Manager, abra **Gerenciamento do computador** na tela **inicial** . Em seguida, na árvore de console, clique em **Device Manager**.