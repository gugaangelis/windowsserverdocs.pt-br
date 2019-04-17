---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinando o agendamento
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>Determinando o agendamento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode controlar a disponibilidade do link de site, definindo uma agenda para links de site. Quando a replicação entre dois locais percorre vários links de site, a intersecção dos cronogramas replicação em todos os links relevantes determina o agendamento de conexão entre os dois locais.  
  
Para planejar para definir o agendamento do link de site, crie duas agendas sobrepostas entre os links de site que contém controladores de domínio que replicam diretamente entre si. Use a programação padrão (100% disponível) nesses links, a menos que você deseja bloquear o tráfego de replicação durante horários de pico. Bloqueando a duplicação, dar prioridade a outro tráfego, mas você também aumenta a latência de replicação.  
  
Controladores de domínio armazenam tempo no tempo Universal Coordenado (UTC). As configurações de tempo no agendamento de objeto do link de sites estão em conformidade com a hora local do site e do computador no qual o agendamento for definido. Quando um controlador de domínio entra em contato com um computador que esteja em um site diferente e o fuso horário, a programação no controlador de domínio exibe a configuração de tempo de acordo com a hora local para o site do computador.  
  


