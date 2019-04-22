---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelos de Design de floresta
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aca59a31f75628b311a92bd842d93ee91408dc4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819787"
---
# <a name="forest-design-models"></a>Modelos de Design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode aplicar um dos seguintes modelos de design de três floresta em seu ambiente do Active Directory:  
  
-   Modelo de floresta organizacional  
  
-   Modelo de floresta de recursos  
  
-   Modelo de floresta de acesso restrito  
  
É provável que você precisará usar uma combinação desses modelos para atender às necessidades de todos os grupos diferentes em sua organização.  
  
## <a name="organizational-forest-model"></a>Modelo de floresta organizacional  
No modelo de floresta organizacional, contas de usuário e recursos estão contidos na floresta e gerenciados de forma independente. A floresta organizacional pode ser usada para fornecer a autonomia do serviço, o isolamento de serviço ou o isolamento de dados, se a floresta está configurada para impedir o acesso a ninguém de fora da floresta.  
  
Se os usuários em uma floresta organizacional precisam acessar recursos em outras florestas (ou vice-versa), as relações de confiança podem ser estabelecidas entre uma floresta organizacional e as outras florestas. Isso possibilita que os administradores podem conceder acesso a recursos em outra floresta. A ilustração a seguir mostra o modelo de floresta organizacional.  
  
![modelos de design de floresta](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Cada design do Active Directory inclui pelo menos uma floresta organizacional.  
  
## <a name="resource-forest-model"></a>Modelo de floresta de recursos  
No modelo de floresta de recursos, uma floresta separada é usada para gerenciar recursos. Florestas de recursos não contêm as contas de usuário que não sejam aqueles necessários para a administração do serviço e aqueles necessários para fornecer acesso alternativo para os recursos na floresta, se as contas de usuário na floresta organizacional ficam indisponíveis. Relações de confiança de floresta são estabelecidas para que os usuários de outras florestas podem acessar os recursos contidos na floresta de recursos. A ilustração a seguir mostra o modelo de floresta de recursos.  
  
![modelos de design de floresta](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Florestas de recursos fornecem isolamento de serviço que é usado para proteger as áreas da rede que precisa para manter um estado de alta disponibilidade. Por exemplo, se sua empresa inclui uma instalação de manufatura que precisa para continuar a operar quando há problemas no restante da rede, você pode criar uma floresta de recursos separado para o grupo de fabricação.  
  
## <a name="restricted-access-forest-model"></a>Modelo de floresta de acesso restrito  
No modelo de floresta de acesso restrito, uma floresta separada é criada para conter contas de usuário e dados que devem ser isolados do restante da organização. Florestas de acesso restrito fornecem isolamento de dados em situações em que as consequências de comprometer os dados do projeto são graves. A ilustração a seguir mostra um modelo de floresta de acesso restrito.  
  
![modelos de design de floresta](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Usuários de outras florestas não podem ser concedidos o acesso aos dados restritos porque nenhuma relação de confiança existe. Nesse modelo, os usuários têm uma conta em uma floresta organizacional para acessar recursos organizacionais gerais e uma conta de usuário separada da floresta de acesso restrito para o acesso aos dados confidenciais. Esses usuários devem ter duas estações de trabalho separadas, um conectado à floresta organizacional e o outro conectado à floresta de acesso restrito. Isso protege contra a possibilidade de que um administrador de serviços de uma floresta pode obter acesso a uma estação de trabalho na floresta restrita.  
  
Em casos extremos, a floresta de acesso restrito pode ser mantida em uma rede física separada. As organizações que funcionam em projetos do governo classificadas, às vezes, mantêm as florestas de acesso restrito em redes separadas para atender aos requisitos de segurança.  
  


