---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificando requisitos de design de floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835327"
---
# <a name="identifying-forest-design-requirements"></a>Identificando requisitos de design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para criar um design de floresta para sua organização, você deve identificar os requisitos de negócios que precisa acomodar sua estrutura de diretório. Isso inclui determinar quanta autonomia os grupos em sua organização necessitam para gerenciar seus recursos de rede e se deseja ou não precisa de cada grupo para isolar seus recursos na rede a partir de outros grupos.  
  
Os serviços de domínio do Active Directory (AD DS) permite que você criar uma infra-estrutura de diretório que acomoda a vários grupos dentro de uma organização que têm requisitos exclusivos de gerenciamento e para atingir a independência estrutural e operacional entre grupos conforme o necessário.  
  
Grupos em sua organização podem ter alguns dos seguintes tipos de requisitos:  
  
-   **Requisitos de estrutura organizacional**. Partes de uma organização podem participar de uma infraestrutura compartilhada para economizar custos, mas exigem a capacidade de operar de forma independente do restante da organização. Por exemplo, um grupo de pesquisa em uma grande organização, talvez seja necessário manter controle sobre todos os seus próprios dados de pesquisa.  
  
-   **Os requisitos operacionais**. Uma parte de uma organização pode impor restrições exclusivas sobre a configuração do serviço de diretório, a disponibilidade ou segurança, ou usar aplicativos que colocar as restrições exclusivas no diretório. Por exemplo, unidades de negócios individuais dentro de uma organização podem implantar aplicativos habilitados por diretório que modificam o esquema de diretório que não são implantados por outras unidades de negócios. Como o esquema de diretório é compartilhado entre todos os domínios na floresta, a criação de várias florestas é uma solução para esse cenário. Outros exemplos são encontrados nas seguintes organizações e cenários:  
  
    -   Organizações militares  
  
    -   Cenários de hospedagem  
  
    -   Organizações que mantêm um diretório que está disponível internamente e externamente (como aqueles que são acessíveis publicamente por usuários na Internet)  
  
-   **Requisitos legais**. Algumas organizações têm requisitos legais para operar de forma específica, por exemplo, restringindo o acesso a determinadas informações conforme especificado em um contrato de negócios. Algumas organizações têm requisitos de segurança para operar em redes isoladas de internos. Falha para atender a esses requisitos pode resultar na perda do contrato e ações legais possivelmente.  
  
Identificando o grau ao qual os grupos em sua organização podem confiar os possíveis proprietários de floresta e seus administradores de serviço e identificando os requisitos de autonomia e isolamento para cada envolve a parte de identificação de seus requisitos de design de floresta grupo em sua organização.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para administração de serviços e dados para cada grupo na organização que pretende usar o AD DS. A equipe também deve observar todas as áreas de conectividade limitada que podem afetar a implantação do AD DS.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para administração de serviços e dados para cada grupo na organização que pretende usar o AD DS. A equipe também deve observar todas as áreas de conectividade limitada que podem afetar a implantação do AD DS. Para uma planilha ajudar a documentar as regiões que você identificou, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip da [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e abra "floresta Requisitos de design"(DSSLOGI_2.doc).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Escopo do administrador de serviços de autoridade](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomia vs. isolamento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
