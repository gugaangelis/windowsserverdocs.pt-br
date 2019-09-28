---
title: Suporte à virtualização de MultiPoint Services
description: Descreve como usar os serviços do MultiPoint com o Hyper-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7b94b4a4015e58402a62cf74f9abbb3eb2333f26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395189"
---
# <a name="multipoint-services-virtualization-support"></a>Suporte à virtualização de MultiPoint Services
Os serviços do MultiPoint oferecem suporte à função Hyper-V de duas maneiras:  
  
-   Os serviços do MultiPoint podem ser implantados como um sistema operacional convidado em um servidor que executa o Hyper-V.  
  
-   Os serviços do MultiPoint podem ser usados como um servidor de virtualização.   
  
A execução de serviços do MultiPoint em uma máquina virtual fornece o uso das ferramentas do Hyper-V para gerenciar sistemas operacionais. Essas ferramentas incluem recursos de ponto de verificação e reversão e permitem que você exporte e importe máquinas virtuais. Para instalações maiores, você pode consolidar servidores executando vários computadores virtuais de serviços do MultiPoint em um único servidor físico. Os cenários possíveis incluem:  
  
-   Uma única sala de aula ou laboratório tem mais de 20 estações. Em vez de implantar vários computadores físicos que executam os serviços do MultiPoint, você pode implantar várias máquinas virtuais em um único computador físico.  
  
    > [!NOTE]  
    > Você pode gerenciar vários MultiPoint Servers, seja físico ou virtual, por meio de um único console do MultiPoint Manager.  
  
-   O MultiPoint Server está em execução em uma máquina virtual com outra infraestrutura de servidor no mesmo computador físico. Nesse caso, essa infraestrutura de servidor centraliza o domínio, a segurança e os dados da rede. O MultiPoint Server fornece Serviços de Área de Trabalho Remota e centraliza os desktops.  
  
> [!NOTE]  
> Ao executar os serviços do MultiPoint em uma máquina virtual, há suporte para estações de cliente USB sobre Ethernet e RDP. Não há suporte para estações conectadas a cliente de vídeo direto e USB com zero.  
  
Para obter mais informações sobre a função Hyper-V, consulte [Hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  