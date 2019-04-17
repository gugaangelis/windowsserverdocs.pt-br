---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Definindo propriedades do Link de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 495ed006ecac5458877191a14060c5fd4b746d96
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="setting-site-link-properties"></a>Definindo propriedades do Link de Site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A duplicação ocorre de acordo com as propriedades dos objetos de conexão. Quando o verificador de consistência de Conhecimento (KCC) cria objetos de conexão, ela deriva o agendamento de replicação de propriedades dos objetos de link de site. Cada objeto de link de site representa a conexão WAN (rede) de longa distância entre dois ou mais sites.  
  
Definindo propriedades de objeto do site link inclui as seguintes etapas:  
  
-   Determinando o custo associado esse caminho de replicação. O KCC usa custo para determinar a rota barata para replicação entre dois locais que duplicar a mesma partição de diretório.  
  
-   Determinar o agendamento que define os horários durante qual a duplicação pode ocorrer.  
  
-   Determinando o intervalo de replicação que define a frequência replicação deve ocorrer durante os horários quando a replicação é permitida, conforme definido no cronograma.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Determinando o custo](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Determinando o agendamento](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Determinando o intervalo](../../ad-ds/plan/Determining-the-Interval.md)  
  


