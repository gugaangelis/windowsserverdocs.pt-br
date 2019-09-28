---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificando requisitos de design de floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41caeca82819eaea3d86d5f1eb4883ab8bbf53cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408797"
---
# <a name="identifying-forest-design-requirements"></a>Identificando requisitos de design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para criar um design de floresta para sua organização, você deve identificar os requisitos de negócios que sua estrutura de diretório precisa acomodar. Isso envolve determinar a quantidade de autonomia que os grupos em sua organização precisam para gerenciar seus recursos de rede e se cada grupo precisa ou não isolar seus recursos na rede de outros grupos.  
  
O Active Directory Domain Services (AD DS) permite que você projete uma infraestrutura de diretório que acomoda vários grupos em uma organização com requisitos de gerenciamento exclusivos e obtenha independência estrutural e operacional entre os grupos conforme necessário.  
  
Os grupos em sua organização podem ter alguns dos seguintes tipos de requisitos:  
  
-   **Requisitos da estrutura organizacional**. Partes de uma organização podem participar de uma infraestrutura compartilhada para economizar custos, mas exigem a capacidade de operar independentemente do restante da organização. Por exemplo, um grupo de pesquisa em uma organização de grande porte pode precisar manter o controle sobre todos os seus dados de pesquisa.  
  
-   **Requisitos operacionais**. Uma parte de uma organização pode posicionar restrições exclusivas sobre a configuração, a disponibilidade ou a segurança do serviço de diretório, ou usar aplicativos que colocam restrições exclusivas no diretório. Por exemplo, unidades de negócios individuais dentro de uma organização podem implantar aplicativos habilitados para diretório que modificam o esquema de diretório que não são implantados por outras unidades de negócios. Como o esquema de diretório é compartilhado entre todos os domínios na floresta, a criação de várias florestas é uma solução para tal cenário. Outros exemplos são encontrados nas seguintes organizações e cenários:  
  
    -   Organizações militares  
  
    -   Cenários de hospedagem  
  
    -   Organizações que mantêm um diretório que está disponível interna e externamente (como aquelas que são publicamente acessíveis por usuários na Internet)  
  
-   **Requisitos legais**. Algumas organizações têm requisitos legais para operar de uma maneira específica, por exemplo, restringindo o acesso a determinadas informações conforme especificado em um contrato de negócios. Algumas organizações têm requisitos de segurança para operar em redes internas isoladas. A falha ao atender a esses requisitos pode resultar na perda do contrato e possivelmente na ação legal.  
  
Parte da identificação de seus requisitos de design de floresta envolve identificar o grau em que os grupos em sua organização podem confiar nos possíveis proprietários de floresta e seus administradores de serviço e identificar os requisitos de autonomia e isolamento para cada um grupo em sua organização.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para serviços e administração de dados para cada grupo na organização que pretende usar AD DS. A equipe também deve observar quaisquer áreas de conectividade limitada que possam afetar a implantação do AD DS.  
  
A equipe de design deve documentar os requisitos de isolamento e autonomia para serviços e administração de dados para cada grupo na organização que pretende usar AD DS. A equipe também deve observar quaisquer áreas de conectividade limitada que possam afetar a implantação do AD DS. Para uma planilha para ajudá-lo a documentar as regiões identificadas, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de [ajudas de trabalho para o kit de implantação do Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558) e abra "requisitos de design de floresta" ( DSSLOGI_2. doc).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Escopo de autoridade do administrador de serviços](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomia vs. isolamento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
