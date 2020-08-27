---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Examinando conceitos de design da UO
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: adb770769d9d79ca5f5d9a9a753ecb90914e2309
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938546"
---
# <a name="reviewing-ou-design-concepts"></a>Examinando conceitos de design da UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura da UO (unidade organizacional) para um domínio inclui o seguinte:

-   Um diagrama da hierarquia de UO

-   Uma lista de UOs

-   Para cada UO:

    -   A finalidade da UO

    -   Uma lista de usuários ou grupos que têm controle sobre a UO ou os objetos na UO

    -   O tipo de controle que os usuários e grupos têm sobre os objetos na UO

A hierarquia de UO não precisa refletir a hierarquia departamental da organização ou do grupo. As UOs são criadas para uma finalidade específica, como a delegação de administração, a aplicação de Política de Grupo ou para limitar a visibilidade dos objetos.

Você pode criar sua estrutura de UO para delegar administração a indivíduos ou grupos dentro de sua organização que exigem a autonomia para gerenciar seus próprios recursos e dados. As UOs representam limites administrativos e permitem que você controle o escopo da autoridade de administradores de dados.

Por exemplo, você pode criar uma UO chamada ResourceOU e usá-la para armazenar todas as contas de computador que pertencem aos servidores de arquivos e de impressão gerenciados por um grupo. Em seguida, você pode configurar a segurança na UO para que somente os administradores de dados no grupo tenham acesso à UO. Isso impede que os administradores de dados em outros grupos violem as contas do servidor de arquivos e de impressão.

Você pode refinar ainda mais sua estrutura de UO criando subárvores de UOs para fins específicos, como o aplicativo de Política de Grupo ou para limitar a visibilidade de objetos protegidos para que apenas determinados usuários possam vê-los. Por exemplo, se você precisar aplicar Política de Grupo a um grupo selecionado de usuários ou recursos, poderá adicionar esses usuários ou recursos a uma UO e, em seguida, aplicar Política de Grupo a essa UO. Você também pode usar a hierarquia de UO para habilitar a delegação adicional do controle administrativo.

Embora não haja nenhum limite técnico para o número de níveis em sua estrutura de UO, para capacidade de gerenciamento, recomendamos que você limite sua estrutura de UO a uma profundidade de no máximo 10 níveis. Não há nenhum limite técnico para o número de UOs em cada nível. Observe que os aplicativos habilitados para Active Directory Domain Services (AD DS) podem ter restrições sobre o número de caracteres usados no nome distinto (ou seja, o caminho completo do protocolo LDAP para o objeto no diretório) ou a profundidade da OU na hierarquia.

A estrutura da UO no AD DS não se destina a ser visível para os usuários finais. A estrutura da UO é uma ferramenta administrativa para administradores de serviços e para administradores de dados e é fácil de alterar. Continue a examinar e atualizar seu design de estrutura de UO para refletir as alterações em sua estrutura administrativa e para dar suporte à administração baseada em políticas.



