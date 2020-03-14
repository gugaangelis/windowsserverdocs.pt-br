---
title: Requisitos de hardware e recomendações de desempenho
description: Fornece requisitos de hardware e desempenho e recomendações para os serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 284131028b308ee86389f25102d934390ba2f16d
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322078"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisitos de hardware e recomendações de desempenho
Este tópico descreve o hardware necessário para executar um sistema de serviços do MultiPoint e dar suporte a cenários de aplicativos de usuário. O cenário do usuário afeta diretamente os requisitos de CPU, RAM e largura de banda de rede.  

## <a name="optimize-multipoint-services-system-performance"></a>Otimizar o desempenho do sistema de serviços do MultiPoint  
O desempenho do seu sistema de serviços do MultiPoint será diretamente afetado pela capacidade da CPU, da GPU e da quantidade de RAM disponível no computador que está executando os serviços do MultiPoint.  
  
### <a name="applications-and-internet-content"></a>Aplicativos e conteúdo da Internet  
Como os serviços do MultiPoint são uma solução de computação de recursos compartilhados, o tipo e o número de aplicativos em execução nas estações podem afetar o desempenho do seu sistema de serviços do MultiPoint. É importante considerar os tipos de programas que são usados regularmente quando você está planejando o sistema. Por exemplo, um aplicativo com uso intensivo de gráficos requer um computador mais potente do que um aplicativo, como um processador de texto. A sobrecarga do computador com aplicativos com uso intensivo de gráficos provavelmente causará problemas de atraso em todo o sistema.  
  
O tipo de conteúdo que é acessado por aplicativos também afeta o desempenho do sistema. Se várias estações estiverem usando navegadores da Web para acessar conteúdo multimídia, como vídeo de movimento completo, menos estações poderão ser conectadas antes de afetar negativamente o desempenho do sistema. Por outro lado, se as várias estações estiverem usando navegadores da Web para acessar conteúdo estático da Web, mais estações poderão ser conectadas sem um efeito significativo sobre o desempenho.  
  
### <a name="hardware-recommendations"></a>Recomendações de hardware  
Para obter um bom desempenho com seu sistema de serviços do MultiPoint sob várias cargas, use as diretrizes na tabela a seguir quando estiver planejando e testando seu sistema. Esses são os requisitos básicos dos serviços forMultiPoints. O dimensionamento da configuração real depende da configuração do sistema, da carga de trabalho que você está executando e do recurso de hardware. Você sempre deve validar testando seus aplicativos e hardware.  
  
> [!NOTE]  
> 2C = 2 núcleos, 4C = 4 núcleos, 6C = 6 núcleos, MT = multithreading. A velocidade do processador deve ser de pelo menos 2,0 gigahertz (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Hardware mínimo recomendado para executar estações do MultiPoint Server padrão  
  
|Cenário do aplicativo|Até 5 estações|6-8 estações|9-12 estações|13-16 estações|17-20 estações|21-24 estações|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Dos**<br /><br />Office, navegação na Web, aplicativos de linha de negócios|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C<br /><br />RAM: 8 GB|CPU: 4C + MT ou 6C<br /><br />RAM: 10 GB| CPU: 6C + MT<br /><br />RAM: 12 GB|
|**Norte**<br /><br />Office, navegação na Web, aplicativos de linha de negócios e uso ocasional de vídeos por alguns usuários|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C + MT ou 6C<br /><br />RAM: 8 GB|CPU: 6C + MT<br /><br />RAM: 10 GB| CPU: 6C + MT<br /><br />RAM: 12 GB| 
|**Uso intensivo de vídeo**<br /><br />Office, navegação na Web, aplicativos de linha de negócios e uso frequente de vídeos por todos os usuários **Observação:** o teste de vídeo foi executado usando o vídeo 360p H. 264 na resolução nativa.|CPU: 4C + MT<br /><br />RAM: 2 GB|CPU: 6C + MT<br /><br />RAM: 4 GB|CPU: 8C + MT<br /><br />RAM: 6 GB|CPU: 12C + MT<br /><br />RAM: 8 GB|CPU: 16C + MT<br /><br />RAM: 10 GB<br /><br />-Cliente fino: RemoteFX<br />-Vídeo USB não recomendado| CPU: 20C + MT<br /><br />RAM: 12 GB<br /><br />-Cliente fino: RemoteFX<br />-Vídeo USB não recomendado|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Hardware mínimo recomendado para executar áreas de trabalho virtuais do Windows 10 completas  
A execução de uma instância completa do sistema operacional virtual para cada estação é mais computacional com uso intensivo de recursos do que a execução das sessões padrão do MultiPoint desktop, de modo que os requisitos de hardware do host por estação são maiores:  
  
1.  CPU: 1 núcleo ou thread por estação  
  
2.  Unidade de estado sólido (SSD)  
  
    1.  Capacidade > = 20 GB por estação + 40 GB para o sistema operacional do host do WMS  
  
    2.  IOPS de leitura/gravação aleatória > = 3K por estação  
  
3.  RAM > = 2GB por estação + 2GB para o sistema operacional do host do WMS  
  
A configuração de CPU do BIOS foi configurada para habilitar a virtualização – SLAT (conversão de endereços de segundo nível)  
  
Para obter mais informações sobre como escolher o melhor hardware de serviços do MultiPoint para suas necessidades, entre em contato com seu fornecedor de hardware.  