---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Requisitos de design do AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d81a6bfb3ae29b4a9c76c5e902e8fec8e08c84b0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941539"
---
# <a name="ad-ds-design-requirements"></a>Requisitos de design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="designing-the-active-directory-logical-structure"></a>Projetando a estrutura lógica Active Directory
Antes de implantar o Windows Server 2008 Active Directory Domain Services (AD DS), você deve planejar e projetar a estrutura lógica de AD DS para seu ambiente. A estrutura lógica AD DS determina como os objetos de diretório são organizados e fornece um método eficaz para gerenciar suas contas de rede e recursos compartilhados. Ao projetar sua estrutura de AD DS lógica, você define uma parte significativa da infraestrutura de rede da sua organização.

Para criar a estrutura lógica de AD DS, determine o número de florestas que sua organização requer e, em seguida, crie designs para domínios, infraestrutura de DNS (sistema de nomes de domínio) e UOs (unidades organizacionais). A ilustração a seguir mostra o processo de criação da estrutura lógica.

![Requisitos de design de AD DS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)

Para obter mais informações, consulte [projetando a estrutura lógica para o Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).

## <a name="designing-the-site-topology"></a>Criando a topologia do site
Depois de criar a estrutura lógica para sua infraestrutura de AD DS, você deve criar a topologia de site para sua rede. A topologia do site é uma representação lógica da sua rede física. Ele contém informações sobre o local de AD DS sites, os controladores de domínio AD DS em cada site e os links de site e as pontes de link de site que dão suporte à replicação AD DS entre sites. A ilustração a seguir mostra o processo de design da topologia do site.

![Requisitos de design de AD DS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)

Para obter mais informações, consulte [projetando a topologia do site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md).

## <a name="planning-domain-controller-capacity"></a>Planejando a capacidade do controlador de domínio
Para garantir um desempenho AD DS eficiente, você deve determinar o número apropriado de controladores de domínio para cada site e verificar se eles atendem aos requisitos de hardware do Windows Server 2008. O planejamento de capacidade cuidadoso para seus controladores de domínio garante que você não subestime os requisitos de hardware, o que pode causar um mau desempenho do controlador de domínio e o tempo de resposta do aplicativo. A ilustração a seguir mostra o processo de planejamento de capacidade do controlador de domínio.

![Requisitos de design de AD DS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)

## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Habilitando recursos do Windows Server 2008 Advanced AD DS
Você pode usar o Windows Server 2008 AD DS para apresentar recursos avançados em seu ambiente aumentando o nível funcional de domínio ou floresta. Você pode aumentar o nível funcional para o Windows Server 2008 quando todos os controladores de domínio no domínio ou floresta estiverem executando o Windows Server 2008.

Para obter mais informações, consulte [habilitando recursos avançados para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).



