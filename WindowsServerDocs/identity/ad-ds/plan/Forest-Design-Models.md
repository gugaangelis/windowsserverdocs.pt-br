---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelos de Design de floresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>Modelos de Design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode aplicar um dos seguintes modelos de design de três floresta no ambiente do Active Directory:  
  
-   Modelo de floresta organizacional  
  
-   Modelo de recurso da floresta  
  
-   Modelo de floresta de acesso restrito  
  
É provável que você precisará usar uma combinação desses modelos para atender às necessidades de todos os grupos diferentes em sua organização.  
  
## <a name="organizational-forest-model"></a>Modelo de floresta organizacional  
No modelo de floresta organizacional, recursos e contas de usuário estão contidos na floresta e gerenciados de forma independente. Floresta organizacional pode ser usada para fornecer autonomia de serviço, o isolamento de serviço ou o isolamento de dados, se a floresta está configurada para impedir o acesso a qualquer pessoa fora da floresta.  
  
Se os usuários em uma floresta organizacional precisam acessar recursos em outras florestas (ou vice-versa), relações de confiança podem ser estabelecidas entre uma floresta organizacional e as outras florestas. Isso possibilita que os administradores deve conceder acesso a recursos em outra floresta. A ilustração a seguir mostra o modelo de floresta organizacional.  
  
![modelos de design de floresta](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Cada design do Active Directory inclui pelo menos uma floresta organizacional.  
  
## <a name="resource-forest-model"></a>Modelo de recurso da floresta  
No modelo de floresta de recurso, uma floresta separada é usada para gerenciar recursos. Florestas de recurso não contêm contas de usuário diferentes daquelas necessários para a administração de serviço e aqueles obrigados a fornecer acesso alternativo para os recursos na floresta, se as contas de usuário na floresta organizacional indisponíveis. Relações de confiança de floresta são estabelecidas para que os usuários de outras florestas podem acessar os recursos contidos na floresta de recursos. A ilustração a seguir mostra o modelo de floresta de recursos.  
  
![modelos de design de floresta](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Florestas de recurso fornecem isolamento de serviço que é usado para proteger áreas da rede que precisam manter um estado de alta disponibilidade. Por exemplo, se sua empresa inclui uma instalação de fabricação que precisa continuam a operar quando há problemas no restante da rede, você pode criar uma floresta de recursos separado para o grupo de fabricação.  
  
## <a name="restricted-access-forest-model"></a>Modelo de floresta de acesso restrito  
No modelo de floresta de acesso restrito, uma floresta separada é criada para conter contas de usuário e dados que devem ser isolados o resto da organização. Acesso restrito florestas fornecem isolamento de dados em situações onde as consequências de comprometimento de dados do projeto são graves. A ilustração a seguir mostra um modelo de floresta de acesso restrito.  
  
![modelos de design de floresta](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Os usuários de outras florestas não podem ser concedidos acesso aos dados restritos porque não existe nenhuma relação de confiança. Nesse modelo, os usuários tenham uma conta em uma floresta organizacional para acesso aos recursos organizacionais gerais e uma conta de usuário separada na floresta acesso restrito para acessar os dados confidenciais. Esses usuários devem ter duas separadas estações de trabalho, conectado à floresta organizacional e o outro conectado à floresta acesso restrito. Isso protege contra a possibilidade de que um administrador de serviço de uma floresta pode obter acesso a uma estação de trabalho na floresta restrita.  
  
Em casos extremos, floresta acesso restrito pode ser mantida em uma rede física separada. As organizações que funcionam em projetos do governo classificados, às vezes, mantém o acesso restrito florestas em redes separadas para atender aos requisitos de segurança.  
  


