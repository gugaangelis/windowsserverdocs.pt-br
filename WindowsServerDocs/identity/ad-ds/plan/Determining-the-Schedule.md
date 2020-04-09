---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinando o cronograma
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d5c1c7c77304317bab2e80223dd745c88bc79efd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822539"
---
# <a name="determining-the-schedule"></a>Determinando o cronograma

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode controlar a disponibilidade do link de site definindo um agendamento para links de site. Quando a replicação entre dois sites atravessa vários links de site, a interseção das agendas de replicação em todos os links relevantes determina a agenda de conexão entre os dois sites.  
  
Para planejar a definição da agenda de links de site, crie duas agendas sobrepostas entre links de site que contêm controladores de domínio que se replicam diretamente entre si. Use o agendamento padrão (100-porcentagem disponível) nesses links, a menos que você queira bloquear o tráfego de replicação durante horários de pico. Ao bloquear a replicação, você dá prioridade a outro tráfego, mas também aumenta a latência de replicação.  
  
Os controladores de domínio armazenam o tempo no UTC (tempo Universal Coordenado). As configurações de hora em agendas de objeto de link de site estão em conformidade com a hora local do site e o computador no qual a agenda está definida. Quando um controlador de domínio entra em contato com um computador que está em um site e fuso horário diferentes, o agendamento no controlador de domínio exibe a configuração de hora de acordo com a hora local do site do computador.  
  


