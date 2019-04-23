---
title: Coletar drivers de hardware e dispositivos necessários à instalação
description: Informações sobre drivers que você precisa instalar o MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a9d902e2599cdcd69e156d1fabec87a067b1d8ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833417"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Coletar drivers de hardware e dispositivos necessários à instalação
Antes de iniciar a implantação de seu sistema MultiPoint Services, será necessário:  
  
-   **Componentes de hardware para o servidor** -instalar as placas de vídeo adicionais ou outros componentes do sistema no momento.  
  
-   **Componentes de hardware para as estações** - para obter informações sobre planejamento de estações para seu ambiente, consulte [selecionando o Hardware para o sistema do MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **Os drivers mais recentes para as placas de vídeos** -se o fabricante do dispositivo ou OEM não forneceu a eles, você precisará baixá-los do site do fabricante do dispositivo.  
  
-   **Os drivers de cliente mais recentes do USB zero** -se você estiver usando estações de cliente USB zero, você deve instalar os drivers de cliente mais recentes do USB zero.  
  
    > [!IMPORTANT]  
    > Para uma instalação do MultiPoint Services, você deve instalar a versão de 64 bits de todos os drivers.  
  
> [!TIP]  
> Se você estiver instalando o MultiPoint Services em um computador com uma versão diferente do Windows já instalado, você deve encontrar a marca de placa de vídeo e o modelo no Gerenciador de dispositivos antes de iniciar a instalação do Windows Server e certifique-se de que você pode obter os drivers que são disponível para Windows Server 2016. Abrir Gerenciador de dispositivos, abra **gerenciamento do computador** da **iniciar** tela. Em seguida, na árvore de console, clique em **Gerenciador de dispositivos**.