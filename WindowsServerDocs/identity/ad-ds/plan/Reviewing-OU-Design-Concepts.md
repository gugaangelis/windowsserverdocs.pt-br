---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisando os conceitos de Design de UO
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>Revisando os conceitos de Design de UO

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A estrutura de unidade organizacional (UO) para um domínio inclui o seguinte:  
  
-   Um diagrama da hierarquia de UO  
  
-   Uma lista de unidades organizacionais  
  
-   Para cada unidade Organizacional:  
  
    -   A finalidade da unidade Organizacional  
  
    -   Uma lista de usuários ou grupos que têm controle sobre a UO ou os objetos na UO  
  
    -   O tipo de controle que os usuários e grupos têm sobre os objetos na UO  
  
A hierarquia de UO não precisa refletir a hierarquia da organização ou grupo departamental. UOs são criados para uma finalidade específica, como a delegação de administração, o aplicativo de política de grupo, ou para limitar a visibilidade dos objetos.  
  
Você pode projetar sua estrutura de UO para delegar a administração para indivíduos ou grupos que exigem a autonomia para gerenciar seus próprios recursos e dados em sua organização. UOs representam limites administrativos e permitem controlar o escopo de autoridade de administradores de dados.  
  
Por exemplo, você pode criar uma unidade Organizacional chamada ResourceOU e usá-lo para armazenar todas as contas de computador que pertencem ao arquivo e gerenciados por um grupo de servidores de impressão. Em seguida, você pode configurar segurança na unidade Organizacional para que apenas os administradores de dados no grupo tem acesso à unidade Organizacional. Isso impede que os administradores de dados em outros grupos falsifique as contas de servidor de impressão e o arquivo.  
  
Você pode refinar ainda mais sua estrutura de UO criando subárvores de UOs para fins específicos, como o aplicativo de política de grupo ou para limitar a visibilidade dos objetos protegidos para que apenas determinados usuários possam vê-los. Por exemplo, se você precisar aplicar a política de grupo para um grupo selecionado de usuários ou recursos, você pode adicionar esses usuários ou recursos a uma UO e, em seguida, aplicar a política de grupo para essa UO. Você também pode usar a hierarquia de UO para permitir que ainda mais delegação de controle administrativo.  
  
Embora não haja nenhum limite técnica para o número de níveis na sua estrutura de UO, para o gerenciamento do é recomendável que você limite sua estrutura de UO a uma profundidade de não mais do que 10 níveis. Não há nenhum limite técnica para o número de unidades organizacionais em cada nível. Observe que serviços de domínio Active do Directory (AD DS)-aplicativos habilitados podem ter restrições sobre o número de caracteres usados no nome diferenciado (ou seja, o caminho Lightweight Directory Access Protocol (LDAP) completo para o objeto no diretório) ou na profundidade UO dentro da hierarquia.  
  
A estrutura de UO no AD DS não se destina a serem visível para os usuários finais. A estrutura de UO é uma ferramenta administrativa para administradores de serviços e para os administradores de dados, e é fácil alterar. Continue revisar e atualizar seu design de estrutura de UO para refletir as alterações em sua estrutura administrativa e dar suporte a administração baseada em política.  
  


