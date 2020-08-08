---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelos de design de floresta
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b0c6b42812592b5c9b0e763fcdad6b77a8776f0e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941070"
---
# <a name="forest-design-models"></a>Modelos de design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode aplicar um dos três modelos de design de floresta a seguir em seu ambiente de Active Directory:

-   Modelo de floresta organizacional

-   Modelo de floresta de recursos

-   Modelo de floresta de acesso restrito

É provável que você precise usar uma combinação desses modelos para atender às necessidades de todos os grupos diferentes em sua organização.

## <a name="organizational-forest-model"></a>Modelo de floresta organizacional
No modelo de floresta organizacional, as contas de usuário e os recursos estão contidos na floresta e gerenciados de forma independente. A floresta organizacional pode ser usada para fornecer autonomia de serviço, isolamento de serviço ou isolamento de dados, se a floresta estiver configurada para impedir o acesso a qualquer pessoa fora da floresta.

Se os usuários em uma floresta organizacional precisarem acessar recursos em outras florestas (ou vice-versa), as relações de confiança poderão ser estabelecidas entre uma floresta organizacional e as outras florestas. Isso possibilita que os administradores concedam acesso aos recursos na outra floresta. A ilustração a seguir mostra o modelo de floresta organizacional.

![modelos de design de floresta](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)

Cada design de Active Directory inclui pelo menos uma floresta organizacional.

## <a name="resource-forest-model"></a>Modelo de floresta de recursos
No modelo de floresta de recursos, uma floresta separada é usada para gerenciar recursos. As florestas de recursos não contêm contas de usuário diferentes das necessárias para a administração do serviço e as necessárias para fornecer acesso alternativo aos recursos nessa floresta, se as contas de usuário na floresta organizacional ficarem indisponíveis. As relações de confiança de floresta são estabelecidas para que os usuários de outras florestas possam acessar os recursos contidos na floresta de recursos. A ilustração a seguir mostra o modelo de floresta do recurso.

![modelos de design de floresta](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)

As florestas de recursos fornecem isolamento de serviço que é usado para proteger áreas da rede que precisam manter um estado de alta disponibilidade. Por exemplo, se sua empresa incluir uma instalação de fabricação que precisa continuar a operar quando houver problemas no restante da rede, você poderá criar uma floresta de recursos separada para o grupo de produção.

## <a name="restricted-access-forest-model"></a>Modelo de floresta de acesso restrito
No modelo de floresta de acesso restrito, uma floresta separada é criada para conter contas de usuário e dados que devem ser isolados do restante da organização. As florestas de acesso restrito fornecem isolamento de dados em situações em que as consequências de comprometer dados do projeto são graves. A ilustração a seguir mostra um modelo de floresta de acesso restrito.

![modelos de design de floresta](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)

Os usuários de outras florestas não podem receber acesso aos dados restritos porque não existe nenhuma relação de confiança. Nesse modelo, os usuários têm uma conta em uma floresta organizacional para acesso a recursos organizacionais gerais e uma conta de usuário separada na floresta de acesso restrito para acesso aos dados classificados. Esses usuários devem ter duas estações de trabalho separadas, uma conectada à floresta organizacional e a outra conectada à floresta de acesso restrito. Isso protege contra a possibilidade de que um administrador de serviços de uma floresta possa obter acesso a uma estação de trabalho na floresta restrita.

Em casos extremos, a floresta de acesso restrito pode ser mantida em uma rede física separada. As organizações que trabalham em projetos do governo classificados às vezes mantêm florestas de acesso restrito em redes separadas para atender aos requisitos de segurança.



