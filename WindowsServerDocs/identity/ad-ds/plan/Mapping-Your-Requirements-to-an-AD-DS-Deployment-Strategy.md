---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: "Mapeando seus requisitos para uma estratégia de implantação do AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapeando seus requisitos para uma estratégia de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois que você terminar de revisão e identificar o design de serviços de domínio do Active Directory (AD DS) e requisitos de implantação e você determinar qual deles estão relacionados à sua implantação específica, você pode mapear esses requisitos para uma estratégia de implantação específica do AD DS.  
  
Use a tabela a seguir para determinar qual estratégia de implantação do AD DS é mapeado para a combinação adequada de design do AD DS e implantação requisitos para sua organização. ("Sim" significa que um requisito específico é necessário para sua estratégia de implantação; "Não" significa que um requisito específico não é necessário para sua estratégia de implantação.)  
  
Esta tabela se refere apenas ao estratégias de implantação três principais no AD DS, conforme descrito neste guia:  
  
-   [Implantando o AD DS em uma nova organização](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Implantando o AD DS em uma organização do Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Implantando o AD DS em uma organização do Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
No entanto, você pode criar uma estratégia de implantação personalizada do AD DS ou híbridos usando qualquer combinação dos requisitos de design e implantação do AD DS para atender às necessidades da sua organização.  
  
|Requisitos de design e implantação de AD DS|Implantando o AD DS em uma nova organização|Implantando o AD DS em uma organização do Windows Server 2003|Implantando o AD DS em uma organização do Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Projetando a estrutura lógica para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Sim|Sim|Sim|  
|[Projetando a topologia de Site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Sim|Sim|Sim|  
|Planejando a capacidade de controlador de domínio|Sim|Sim|Sim|  
|[Implantando um domínio de raiz da floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sim|Não|Não|  
|[Implantando domínios regionais do Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Sim|Sim|Sim|  
|[Habilitar recursos avançados para o AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sim|Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional do domínio ou floresta para o Windows Server 2008.|Sim, mas todos os controladores de domínio no ambiente devem executar o Windows Server 2008 antes de definir o nível funcional do domínio ou floresta para o Windows Server 2008.|  
|[Atualização de domínios do Active Directory para Windows Server 2008 e Windows Server 2008 R2 AD DS domínios](https://technet.microsoft.com/library/cc731188.aspx)|Não|Sim|Sim|  
|[Reestruturação AD DS domínios entre florestas](https://go.microsoft.com/fwlink/?LinkId=93678)|Sim, se você quiser migrar um domínio piloto em seu ambiente de produção, mesclar com outra organização e consolidar as dois infraestruturas de informática informações ou consolidar domínios de recurso e a conta que foi atualizado no lugar de ambientes Windows 2000 ou Windows Server 2003.|Sim, se você quiser mesclar com outra organização e consolidar as dois infraestruturas IT ou consolidar domínios de recurso e a conta que foi atualizado no lugar de ambientes Windows 2000 ou Windows Server 2003.|Sim, se você quiser mesclar com outra organização e consolidar as dois infraestruturas IT ou consolidar domínios de recurso e a conta que foi atualizado no lugar de ambientes Windows 2000 ou Windows Server 2003.|  
|[Reestruturação AD DS domínios em florestas](https://go.microsoft.com/fwlink/?LinkId=82740))|Não|Sim, se você precisar reduzir o número de domínios, reduzir a quantidade de usuário necessária e a administração de grupo e tráfego de replicação ou simplificar a administração da política de grupo.|Sim, se você precisar reduzir o número de domínios, reduzir a quantidade de usuário necessária e a administração de grupo e tráfego de replicação ou simplificar a administração da política de grupo.|  
  


