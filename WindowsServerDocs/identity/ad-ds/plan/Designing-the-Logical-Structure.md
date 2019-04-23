---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Criar a estrutura lógica
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832327"
---
# <a name="designing-the-logical-structure"></a>Criar a estrutura lógica

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) permite que as organizações criem uma infraestrutura escalável, segura e gerenciável para gerenciamento de usuários e recursos. Ele também permite dar suporte a aplicativos habilitados por diretório.  
  
Uma estrutura lógica do Active Directory bem projetada fornece os seguintes benefícios:  
  
-   Gerenciamento simplificado de redes da Microsoft com base em Windows que contêm um grande número de objetos  
  
-   Uma estrutura de domínio consolidado e os custos de administração reduzidos  
  
-   A capacidade de delegar controle administrativo sobre recursos, conforme apropriado  
  
-   Impacto reduzido na largura de banda de rede  
  
-   Compartilhamento de recursos simplificados  
  
-   Desempenho da pesquisa ideal  
  
-   Baixo custo total de propriedade  
  
Uma estrutura lógica do Active Directory bem projetada facilita a integração eficiente de recursos, como a diretiva de grupo; bloqueio de área de trabalho; distribuição de software; e a administração de usuário, grupo, estação de trabalho e servidor em seu sistema. Além disso, uma estrutura lógica cuidadosamente desenvolvida facilita a integração do Microsoft e aplicativos não-Microsoft e serviços, como o Microsoft Exchange Server, infraestrutura de chave pública (PKI) e um baseado em domínio distribuído (DFS) do sistema de arquivos.  
  
Quando você cria uma estrutura lógica do Active Directory antes de implantar o AD DS, você pode otimizar seu processo de implantação para tirar melhor proveito dos recursos do Active Directory. Para criar a estrutura lógica do Active Directory, sua equipe de design primeiro identifica os requisitos de sua organização e, com base nessas informações, decide onde colocar os limites de floresta e domínio. Em seguida, a equipe de design decide como configurar o ambiente de sistema de nome de domínio (DNS) para atender às necessidades da floresta. Por fim, a equipe de design identifica a estrutura de unidade organizacional (UO) que é necessário para delegar o gerenciamento de recursos em sua organização.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre o modelo lógico do Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificar os participantes do projeto de implantação](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Criar um Design de floresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Criar um Design de domínio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Criar um Design de infraestrutura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Criar um Design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Apêndice a: Inventário DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


