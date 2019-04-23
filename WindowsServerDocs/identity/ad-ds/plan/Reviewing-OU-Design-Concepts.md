---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Examinando conceitos de design da UO
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832167"
---
# <a name="reviewing-ou-design-concepts"></a>Examinando conceitos de design da UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura da unidade organizacional (UO) para um domínio inclui o seguinte:  
  
-   Um diagrama da hierarquia de UO  
  
-   Uma lista de unidades organizacionais  
  
-   Para cada UO:  
  
    -   A finalidade para a unidade Organizacional  
  
    -   Uma lista de usuários ou grupos que têm controle sobre a UO ou objetos na UO  
  
    -   O tipo de controle que os usuários e grupos têm sobre os objetos na UO  
  
A hierarquia de UO não precisa refletir a hierarquia departamental da organização ou grupo. Unidades organizacionais são criadas para uma finalidade específica, como a delegação de administração, o aplicativo de diretiva de grupo, ou para limitar a visibilidade dos objetos.  
  
Você pode projetar a estrutura de UO para delegar a administração a indivíduos ou grupos em sua organização que exigem a autonomia para gerenciar seus próprios recursos e dados. Unidades organizacionais representam limites administrativos e permitem controlar o escopo de autoridade de administradores de dados.  
  
Por exemplo, você pode criar uma UO chamada ResourceOU e usá-lo para armazenar todas as contas de computador que pertencem ao arquivo e gerenciados por um grupo de servidores de impressão. Em seguida, você pode configurar a segurança na UO de para que somente os administradores de dados do grupo têm acesso à UO. Isso impede que os administradores de dados em outros grupos viole o arquivo e as contas do servidor de impressão.  
  
Você pode refinar ainda mais sua estrutura de UO, criando subárvores de UOs para fins específicos, como o aplicativo de diretiva de grupo ou para limitar a visibilidade dos objetos protegidos para que somente determinados usuários podem vê-los. Por exemplo, se você precisar aplicar diretiva de grupo para um grupo seleto de usuários ou recursos, você pode adicionar esses usuários ou os recursos a uma UO e, em seguida, aplicar a diretiva de grupo a essa UO. Você também pode usar a hierarquia de OU para habilitar a delegação de controle administrativo.  
  
Embora não haja nenhum limite técnico para o número de níveis em sua estrutura de UO, para capacidade de gerenciamento é recomendável que você limite a sua estrutura de UO em uma profundidade de não mais do que 10 níveis. Não há nenhum limite técnico para o número de unidades organizacionais em cada nível. Observe que serviços de domínio Active Directory (AD DS)-aplicativos habilitados podem ter restrições sobre o número de caracteres usado no nome distinto (ou seja, o caminho Lightweight Directory Access Protocol (LDAP) completo para o objeto no diretório) ou no Profundidade da UO dentro da hierarquia.  
  
Não se destina a estrutura de UO no AD DS seja visível para os usuários finais. A estrutura de UO é uma ferramenta administrativa para os administradores de serviço e para administradores de dados, e é fácil alterar. Continue a analisar e atualizar o design de estrutura de UO para refletir as alterações em sua estrutura administrativa e para dar suporte à administração baseada em política.  
  


