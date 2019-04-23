---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Requisitos de design do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 79c694112f39adf5d37cd28f6bd7a770dedf3976
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857517"
---
# <a name="ad-ds-design-requirements"></a>Requisitos de design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Criando a estrutura lógica do Active Directory  
Antes de implantar o Windows Server 2008 Active Directory serviços de domínio (AD DS), você deve planejar e projetar a estrutura lógica do AD DS para o seu ambiente. A estrutura lógica do AD DS determina como os objetos de diretório são organizados e fornece um método eficaz para gerenciar suas contas de rede e recursos compartilhados. Ao projetar a estrutura lógica do AD DS, você define uma parte significativa da infra-estrutura de rede da sua organização.  
  
Para criar a estrutura lógica do AD DS, determinar o número de florestas necessárias para sua organização e, em seguida, criar designs para domínios, infraestrutura de sistema de nome de domínio (DNS) e unidades organizacionais (OUs). A ilustração a seguir mostra o processo para criar a estrutura lógica.  
  
![Requisitos de design do AD DS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Para obter mais informações, consulte [Projetando a lógica estrutura para o Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Criando a topologia de site  
Depois de criar a estrutura lógica para sua infraestrutura do AD DS, você deve projetar a topologia de site para sua rede. A topologia de site é uma representação lógica de sua rede física. Ele contém informações sobre o local dos sites do AD DS, os controladores de domínio do AD DS em cada site e os links de site e pontes de link de site que dão suporte à replicação do AD DS entre sites. A ilustração a seguir mostra o processo de design da topologia de site.  
  
![Requisitos de design do AD DS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Para obter mais informações, consulte [projetar o Site de topologia para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planejando a capacidade do controlador de domínio  
Para garantir um desempenho eficiente de AD DS, você deve determinar o número apropriado de controladores de domínio para cada site e verificar se eles atendem aos requisitos de hardware do Windows Server 2008. Cuidadoso planejamento de capacidade para os controladores de domínio garante que não sejam subestimados requisitos de hardware, que podem causar tempo de resposta ruim domínio controlador desempenho e aplicativo. A ilustração a seguir mostra o processo de planejamento de capacidade do controlador de domínio.  
  
![Requisitos de design do AD DS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Habilitar o Windows Server 2008 avançado de recursos do AD DS  
Você pode usar o AD DS do Windows Server 2008 para introduzir recursos avançados no seu ambiente, gerando o nível funcional do domínio ou floresta. Você pode aumentar o nível funcional para o Windows Server 2008 quando todos os controladores de domínio no domínio ou floresta executam o Windows Server 2008.  
  
Para obter mais informações, consulte [habilitando recursos avançados para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


