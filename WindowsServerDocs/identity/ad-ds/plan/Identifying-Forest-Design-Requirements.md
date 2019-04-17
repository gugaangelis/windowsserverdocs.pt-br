---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificando os requisitos de Design de floresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>Identificando os requisitos de Design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para criar um design de floresta para sua organização, você deve identificar os requisitos de negócios que sua estrutura de diretório precisa conter. Isso envolve a determinar quanto autonomia os grupos na sua organização necessitam para gerenciar seus recursos de rede e se deseja ou não cada grupo precisa isolar seus recursos na rede de outros grupos.  
  
Os serviços de domínio do Active Directory (AD DS) permite que você criar uma infraestrutura de diretório adequa vários grupos dentro de uma organização que possuem requisitos de gerenciamento exclusivo e a alcançar independência estrutural e operacional entre grupos conforme necessário.  
  
Grupos em sua organização podem ter alguns dos seguintes tipos de requisitos:  
  
-   **Requisitos da estrutura organizacional**. Partes de uma organização podem participar de uma infraestrutura compartilhada para poupar custos, mas exigem a capacidade de operar de forma independente do restante da organização. Por exemplo, um grupo de pesquisa em uma grande organização talvez seja necessário controlar todos os seus próprios dados de pesquisa.  
  
-   **Requisitos operacionais**. Uma parte de uma organização pode impor restrições exclusivas sobre a configuração de serviço de diretório, a disponibilidade ou a segurança ou usar aplicativos que fazem restrições exclusivas no diretório. Por exemplo, unidades de negócios individuais dentro de uma organização podem implantar aplicativos habilitados para diretórios que modificam o esquema de diretório que não são implantados por outras unidades de negócios. Como o esquema de diretório é compartilhado entre todos os domínios na floresta, criação de várias florestas é uma solução para esse cenário. Outros exemplos são encontrados nas seguintes organizações e os cenários:  
  
    -   Organizações militares  
  
    -   Cenários de hospedagem  
  
    -   Organizações que mantêm um diretório que está disponível interna e externa (como aqueles que estão publicamente acessível por usuários na Internet)  
  
-   **Requisitos legais**. Algumas organizações têm requisitos legais para operar de forma específica, por exemplo, restringindo o acesso a determinadas informações conforme especificado em um contrato de negócios. Algumas organizações têm requisitos de segurança para operar em redes isoladas internos. Falha ao atender a esses requisitos pode resultar em perda do contrato e possivelmente judiciais.  
  
Parte de identificação de seus requisitos de design de floresta envolve identificando o grau ao qual grupos em sua organização possam confiar os possíveis proprietários de floresta e os administradores de serviço e identificar os requisitos de autonomia e isolamento para cada grupo em sua organização.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para administração de serviço e os dados para cada grupo na organização que pretende usar o AD DS. A equipe também deve observar as áreas de conectividade limitada que podem afetar a implantação do AD DS.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para administração de serviço e os dados para cada grupo na organização que pretende usar o AD DS. A equipe também deve observar as áreas de conectividade limitada que podem afetar a implantação do AD DS. Para uma planilha para ajudá-lo a documentar as regiões que você identificou, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Escopo de serviço de administrador da autoridade](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Isolamento de autonomia vs.](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


