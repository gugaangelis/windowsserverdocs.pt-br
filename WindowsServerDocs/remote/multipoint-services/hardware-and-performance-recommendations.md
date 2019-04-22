---
title: Requisitos de hardware e recomendações de desempenho
description: Fornece os requisitos de hardware e de desempenho e recomendações para o MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823387"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisitos de hardware e recomendações de desempenho
Este tópico descreve o hardware que é necessário para executar um sistema MultiPoint Services e dar suporte a cenários de aplicativo do usuário. O cenário do usuário afeta diretamente a CPU, RAM e requisitos de largura de banda de rede.  

## <a name="optimize-multipoint-services-system-performance"></a>Otimizar o desempenho do sistema MultiPoint Services  
O desempenho do seu sistema MultiPoint Services será afetado diretamente pelo recurso de CPU, GPU e a quantidade de RAM disponível no computador que está executando o MultiPoint Services.  
  
### <a name="applications-and-internet-content"></a>Aplicativos e conteúdo da Internet  
Como MultiPoint Services é uma solução de computação de recurso compartilhado, o tipo e o número de aplicativos que estão em execução em estações podem afetar o desempenho do seu sistema MultiPoint Services. É importante considerar os tipos de programas que são usados regularmente ao planejar seu sistema. Por exemplo, um aplicativo intensivo de gráficos requer um computador mais potente do que um aplicativo como um processador de texto. Sobrecarregar o computador com aplicativos com uso intensivo de gráficos provavelmente causará problemas de latência em todo o sistema.  
  
O tipo de conteúdo que é acessado por aplicativos também afeta o desempenho do sistema. Se várias estações estão usando navegadores da web para acessar conteúdo multimídia, como vídeos, menos estações podem ser conectadas antes de afetar negativamente o desempenho do sistema. Por outro lado, se várias estações estão usando navegadores da web para acessar o conteúdo web estático, mais estações podem ser conectadas sem um impacto significativo no desempenho.  
  
### <a name="hardware-recommendations"></a>Recomendações de hardware  
Para atingir um bom desempenho com seu sistema MultiPoint Services sob cargas diversas, use as diretrizes na tabela a seguir quando estiver planejando e testando o seu sistema. Estes são os requisitos básicos forMultiPoint serviços. O dimensionamento da configuração real depende da configuração do seu sistema, a carga de trabalho que você está executando e a capacidade de hardware. Você deve sempre valide testando seus aplicativos e hardware.  
  
> [!NOTE]  
> C = 2 de 2 núcleos, 4, C = 4 núcleos, C = 6 de 6 núcleos, MT = multithreading. Velocidade do processador deve ser pelo menos 2.0 GHz (gigahertz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Mínimo recomendado de hardware para a execução de estações do MultiPoint Server padrão  
  
|Cenário de aplicativo|Até 5 estações|Estações de 6 a 8|Estações de 9 a 12|13-16 estações|17-20 estações|Estações de 21 a 24|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Produtividade**<br /><br />Office, aplicativos web de navegação, a linha de negócios|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C<br /><br />RAM: 8 GB|CPU: C + MT 4 ou 6C<br /><br />RAM: 10 GB| CPU: 6C+MT<br /><br />RAM: 12 GB|
|**Misto**<br /><br />Office, navegação na web, aplicativos de linha de negócios e uso ocasional de vídeo por alguns usuários|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: C + MT 4 ou 6C<br /><br />RAM: 8 GB|CPU: 6C+MT<br /><br />RAM: 10 GB| CPU: 6C+MT<br /><br />RAM: 12 GB| 
|**Com uso intensivo de vídeo**<br /><br />Office, navegação na web, aplicativos de linha de negócios e uso frequente de vídeo por todos os usuários **Observação:** Vídeo de teste foi executado usando o vídeo H.264 360p na resolução nativa.|CPU: 4C+MT<br /><br />RAM: 2 GB|CPU: 6C+MT<br /><br />RAM: 4 GB|CPU: 8C+MT<br /><br />RAM: 6 GB|CPU: 12C+MT<br /><br />RAM: 8 GB|CPU: 16C+MT<br /><br />RAM: 10 GB<br /><br />-Thin Client: RemoteFX<br />-Vídeo USB não recomendado| CPU: 20C+MT<br /><br />RAM: 12 GB<br /><br />-Thin Client: RemoteFX<br />-Vídeo USB não recomendado|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Mínimo recomendado de hardware para a execução completas desktops virtuais do Windows 10  
Executando uma instância de sistema operacional completo virtual para cada estação é que muito mais intensivo de recursos de computação que a execução de sessões de área de trabalho MultiPoint do padrão, portanto os requisitos de hardware do host por estação são maiores:  
  
1.  CPU: 1 núcleo ou thread por estação  
  
2.  Unidade de estado sólido (SSD)  
  
    1.  Capacidade de > = 20GB por estação + 40GB para o WMS de host do sistema operacional  
  
    2.  IOPS de leitura/gravação aleatória > = 3 mil por estação  
  
3.  RAM > = 2GB por estação + 2GB para o WMS de host do sistema operacional  
  
Configuração do BIOS CPU foi configurada para habilitar a virtualização – conversão de endereço de segundo nível (SLAT)  
  
Para obter mais informações sobre como escolher o melhor dos hardwares do MultiPoint Services para suas necessidades, entre em contato com o fornecedor do hardware.  