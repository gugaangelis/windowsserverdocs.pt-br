---
title: Suporte à virtualização de MultiPoint Services
description: Descreve como usar os serviços do MultiPoint com o Hyper-V
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: d23034b6a70d36df259fc26829b36ebebeade5ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857619"
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