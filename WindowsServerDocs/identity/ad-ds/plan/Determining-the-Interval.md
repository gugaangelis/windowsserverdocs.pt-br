---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Determinando o intervalo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57b282102f10a595ce3842ac3a4eb1d289b86d22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822157"
---
# <a name="determining-the-interval"></a>Determinando o intervalo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve definir a propriedade de intervalo de replicação de link do site para indicar a frequência com que você deseja que a replicação ocorra durante as horas que a agenda permitirá a replicação. Por exemplo, se o agendamento permite a replicação entre 02:00 horas e 04:00 horas e o intervalo de replicação será definido para 30 minutos, a replicação pode ocorrer até quatro vezes durante o horário agendado. O intervalo de replicação padrão é 180 minutos ou 3 horas. O intervalo mínimo é de 15 minutos.  
  
Considere os seguintes critérios para determinar a frequência com que a replicação ocorre dentro da janela de agendamento:  
  
-   Um pequeno intervalo diminui a latência, mas aumenta a quantidade de tráfego de rede de (longa distância WAN).  
  
-   Para manter as partições de diretório de domínio atualizado, baixa latência é preferencial.  
  
Com uma estratégia de replicação de armazenamento e encaminhamento, é difícil determinar apenas quanto a atualização do diretório pode levar a serem replicados para cada controlador de domínio. Para fornecer uma estimativa conservadora de latência máxima, execute estas tarefas:  
  
-   Crie uma tabela de todos os sites em sua rede, conforme mostrado no exemplo a seguir:  
  
    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |Nova York||||0.25||  
    |Washington, D.C.|||||0.25|  
  
    Uma latência de pior caso dentro de um site é estimada para 15 minutos.  
  
-   O agendamento da replicação, determine a latência máxima de replicação que é possível em qualquer link de site que conecta dois sites de hub.  
  
    Por exemplo, se a replicação ocorre entre Seattle e Nova York, a cada três horas, o atraso máximo para a replicação entre esses sites é de três horas. Se o atraso de replicação entre a Nova York e Seattle é o atraso mais longo agendado entre todos os sites de hub, a latência máxima entre todos os hubs é três horas.  
  
-   Para cada site de hub, crie uma tabela das latências máximo entre o site de hub e qualquer um dos seus sites de satélite.  
  
    Por exemplo, se a replicação ocorre entre a Nova York e Washington, D.C., a cada quatro horas, e este é o atraso de replicação mais longo entre a Nova York e qualquer um de seus sites de satélite, a latência máxima entre a Nova York e seus satélites é de quatro horas.  
  
-   Combine essas latências máximas para determinar a latência máxima de toda a rede.  
  
    Por exemplo, se a latência máxima entre Seattle e seu site de satélite em Los Angeles é um dia, a latência máxima de replicação para esse conjunto de links (Washington, D.C.-Nova York-Seattle-Los Angeles) é 31 horas, ou seja, 4 (Washington, D.C.-Nova York) + (novo 3 York-Seattle) + 24 (Seattle-Los Angeles), conforme mostrado na tabela a seguir.  
  
    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4,00|4,00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |Nova York||||0.25|4,00|  
    |Washington, D.C.|||||0.25|  
  


