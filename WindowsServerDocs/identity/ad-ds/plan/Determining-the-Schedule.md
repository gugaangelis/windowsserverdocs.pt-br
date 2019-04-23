---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinando o cronograma
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838997"
---
# <a name="determining-the-schedule"></a>Determinando o cronograma

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode controlar a disponibilidade de link de site, definindo uma agenda para links de site. Quando a replicação entre dois sites que atravessa os vários links de site, a interseção dos agendamentos de replicação em todos os links relevantes determina o agendamento de conexão entre os dois sites.  
  
Para planejar para configurar o agendamento do link de site, crie dois agendamentos sobrepostos entre links de site que contêm controladores de domínio que são replicados diretamente entre si. Use o agendamento padrão (100 por cento disponíveis) nesses links, a menos que você deseja bloquear o tráfego de replicação durante o horário de pico. Bloqueando a duplicação, dar prioridade a outro tráfego, mas também é aumentar a latência de replicação.  
  
Controladores de domínio armazenam a hora em tempo Universal Coordenado (UTC). Configurações de hora no agendamento do objeto de link de site estão em conformidade com a hora local do site e do computador em que a agenda está definida. Quando um controlador de domínio entra em contato com um computador que está em um site diferente e o fuso horário, o agendamento no controlador de domínio exibe a configuração de tempo acordo com a hora local para o site do computador.  
  


