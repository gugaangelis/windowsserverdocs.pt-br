---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Determinando o intervalo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d5eb824b3b15c8b0734b2df23b79649778b28e9d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-interval"></a>Determinando o intervalo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve definir a propriedade de intervalo de replicação de link do site para indicar a frequência você deseja que a replicação para ocorrer durante os horários quando o agendamento permite a replicação. Por exemplo, se o agendamento permite a replicação entre 02:00 horas e horas 04:00 e o intervalo de replicação é definido por 30 minutos, a replicação pode ocorrer até quatro vezes durante o horário agendado. O intervalo de replicação padrão é 180 minutos ou 3 horas. O intervalo mínimo é 15 minutos.  
  
Considere os seguintes critérios para determinar quantas vezes replicação ocorre dentro da janela do agendamento:  
  
-   Um pequeno intervalo reduz a latência, mas aumenta a quantidade de tráfego de rede (WAN) de longa distância.  
  
-   Para manter as partições de diretório atualizadas, baixa latência é preferencial.  
  
Com uma estratégia de replicação de armazenar e encaminhar, é difícil determinar apenas quanto tempo uma atualização de diretório pode demorar para ser replicada para todos os controladores de domínio. Para fornecer uma estimativa conservadora de latência máxima, execute estas tarefas:  
  
-   Crie uma tabela de todos os sites em sua rede, conforme mostrado no exemplo a seguir:  
  
    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |Nova York||||0.25||  
    |Washington, D.C.|||||0.25|  
  
    Estima-se que uma latência pior dentro de um site é de 15 minutos.  
  
-   O agendamento de replicação, determine a latência máxima de replicação que é possível em qualquer link de site que conecta os dois sites de hub.  
  
    Por exemplo, se ocorre a replicação entre Seattle e Nova York cada três horas, o atraso máximo para replicação entre esses locais é três horas. Se o atraso de replicação entre Nova York e Seattle é o maior atraso agendado entre todos os sites de hub, a latência máxima entre todos os hubs é três horas.  
  
-   Para cada site de hub, crie uma tabela das latências máximas entre o site de hub e qualquer um de seus sites de satélite.  
  
    Por exemplo, se a replicação ocorre entre Nova York e Washington, D.C., cada quatro horas e isso é o maior atraso de replicação entre Nova York e qualquer um de seus sites de satélite, a latência máxima entre Nova York e suas satélites é quatro horas.  
  
-   Combine esses latências máximas para determinar a latência máxima para a rede inteira.  
  
    Por exemplo, se a latência máxima entre Seattle e seu site de satélite em Los Angeles é um dia, a latência máxima de replicação para esse conjunto de links (Washington, D.C.-Nova York-Seattle-Los Angeles) é 31 horas, ou seja, 4 (Washington, D.C.-Nova York) + 3 (Nova York-Seattle) + 24 (são Paulo-Los Angeles), conforme mostrado na tabela a seguir.  
  
    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4.00|4.00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |Nova York||||0.25|4.00|  
    |Washington, D.C.|||||0.25|  
  


