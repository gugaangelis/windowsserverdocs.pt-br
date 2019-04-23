---
title: Suporte à virtualização de MultiPoint Services
description: Descreve como usar o MultiPoint Services com o Hyper-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872517"
---
# <a name="multipoint-services-virtualization-support"></a>Suporte à virtualização de MultiPoint Services
MultiPoint Services suporta a função Hyper-V de duas maneiras:  
  
-   MultiPoint Services pode ser implantado como um sistema operacional de convidado em um servidor que executa o Hyper-V.  
  
-   MultiPoint Services pode ser usado como um servidor de virtualização.   
  
Executando o MultiPoint Services em uma máquina virtual fornece o uso das ferramentas do Hyper-V para gerenciar sistemas operacionais. Essas ferramentas incluem recursos de ponto de verificação e a reversão, e eles permitem que você exportar e importar máquinas virtuais. Para instalações maiores, você pode consolidar servidores executando várias máquinas virtuais do MultiPoint Services em um único servidor físico. Os cenários possíveis incluem:  
  
-   Uma única sala de aula ou laboratório tem mais de 20 estações. Em vez de implantar vários computadores físicos executando o MultiPoint Services, você pode implantar várias máquinas virtuais em um único computador físico.  
  
    > [!NOTE]  
    > Você pode gerenciar vários servidores MultiPoint, seja ele físico ou virtual, por meio de um único console do Gerenciador do MultiPoint.  
  
-   O MultiPoint server está em execução em uma máquina virtual com outra infra-estrutura de servidor no mesmo computador físico. Nesse caso, essa infraestrutura de servidor centraliza o domínio, segurança e dados para a rede. O MultiPoint server fornece serviços de área de trabalho remota e centraliza as áreas de trabalho.  
  
> [!NOTE]  
> Durante a execução do MultiPoint Services em uma máquina virtual, USB over Ethernet e estações de cliente RDP são suportadas. Não há suporte para vídeo direto e estações de cliente conectado USB zero.  
  
Para obter mais informações sobre a função Hyper-V, consulte [Hyper-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  