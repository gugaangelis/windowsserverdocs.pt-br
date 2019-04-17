---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: "Criando a estrutura lógica"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>Criando a estrutura lógica

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) permite que as empresas criem uma infraestrutura escalável, segura e gerenciável de usuário e gerenciamento de recursos. Também permite que eles dão suporte a aplicativos habilitados para diretórios.  
  
Uma estrutura lógica do Active Directory bem projetada oferece os seguintes benefícios:  
  
-   Gerenciamento simplificado de redes Microsoft baseado no Windows que contêm um grande número de objetos  
  
-   Uma estrutura de domínio consolidado e custos reduzidos de administração  
  
-   A capacidade de delegar controle administrativo sobre recursos, conforme apropriado  
  
-   Menor impacto sobre a largura de banda de rede  
  
-   Compartilhamento de recursos simplificado  
  
-   Desempenho da pesquisa ideal  
  
-   Baixo custo total de propriedade  
  
Uma estrutura lógica do Active Directory bem projetada facilita a integração eficiente de recursos como a política de grupo; bloqueio da área de trabalho; distribuição de software; e a administração de usuário, grupo, estação de trabalho e servidor em seu sistema. Além disso, uma estrutura lógica cuidadosamente projetada facilita a integração de aplicativos não Microsoft e serviços Microsoft, como o Microsoft Exchange Server, infraestrutura de chave pública (PKI), e um baseado em domínio distribuído (DFS) do sistema de arquivos.  
  
Quando você cria uma estrutura lógica do Active Directory antes de implantar o AD DS, você pode otimizar seu processo de implantação para aproveitar melhor os recursos do Active Directory. Para criar a estrutura lógica do Active Directory, sua equipe de design primeiro identifica os requisitos para sua organização e, com base nessas informações, decide onde colocar os limites de floresta e domínio. Em seguida, a equipe de design decide como configurar o ambiente do sistema de nome de domínio (DNS) para atender às necessidades da floresta. Por fim, a equipe de design identifica a estrutura de unidade organizacional (UO) que é necessária para delegar o gerenciamento de recursos em sua organização.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre o modelo lógico do Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificando os participantes do projeto de implantação](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Criando um projeto de floresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Criando um projeto de domínio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Criando um projeto de infraestrutura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Criando um Design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Apêndice a: inventário DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


