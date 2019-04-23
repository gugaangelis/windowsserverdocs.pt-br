---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Definindo propriedades do link de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870357"
---
# <a name="setting-site-link-properties"></a>Definindo propriedades do link de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Replicação entre sites ocorre de acordo com as propriedades dos objetos de conexão. Quando o Knowledge Consistency Checker (KCC) cria objetos de conexão, ele deriva o agendamento da replicação das propriedades dos objetos de link de site. Cada objeto de link de site representa a conexão de rede de (longa distância WAN) de longa distância entre dois ou mais sites.  
  
Configurando propriedades do objeto de link de site inclui as seguintes etapas:  
  
-   Determinar o custo associado esse caminho de replicação. O KCC usa o custo para determinar a rota menos dispendiosa para replicação entre dois sites que são replicados na mesma partição de diretório.  
  
-   Determinar o agendamento que define os horários durante os quais a replicação entre sites pode ocorrer.  
  
-   Determinando o intervalo de replicação que define a frequência com que a replicação deve ocorrer durante os horários quando a replicação é permitida, conforme definido na agenda.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Determinar o custo](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Determinando o agendamento](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Determinando o intervalo](../../ad-ds/plan/Determining-the-Interval.md)  
  


