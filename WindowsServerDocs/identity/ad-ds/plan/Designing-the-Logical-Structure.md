---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Criar a estrutura lógica
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: de56205c163abff1b05d57ea90954fa93606abce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822609"
---
# <a name="designing-the-logical-structure"></a>Criar a estrutura lógica

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O Active Directory Domain Services (AD DS) permite que as organizações criem uma infraestrutura escalonável, segura e gerenciável para gerenciamento de usuários e recursos. Ele também permite que eles ofereçam suporte a aplicativos habilitados para diretório.  
  
Uma estrutura lógica Active Directory bem projetada fornece os seguintes benefícios:  
  
-   Gerenciamento simplificado de redes baseadas no Microsoft Windows que contêm um grande número de objetos  
  
-   Uma estrutura de domínio consolidada e custos de administração reduzidos  
  
-   A capacidade de delegar controle administrativo sobre os recursos, conforme apropriado  
  
-   Impacto reduzido na largura de banda da rede  
  
-   Compartilhamento simplificado de recursos  
  
-   Desempenho de pesquisa ideal  
  
-   Baixo custo total de propriedade  
  
Uma estrutura lógica Active Directory bem projetada facilita a integração eficiente de tais recursos como Política de Grupo; bloqueio de área de trabalho; distribuição de software; e administração de usuário, grupo, estação de trabalho e servidor em seu sistema. Além disso, uma estrutura lógica projetada com cuidado facilita a integração de aplicativos e serviços da Microsoft e que não são da Microsoft, como o Microsoft Exchange Server, a PKI (infraestrutura de chave pública) e um DFS (sistema de arquivos distribuído) baseado em domínio.  
  
Ao criar uma estrutura lógica de Active Directory antes de implantar AD DS, você pode otimizar seu processo de implantação para aproveitar melhor os recursos de Active Directory. Para projetar a estrutura lógica Active Directory, sua equipe de design primeiro identifica os requisitos para sua organização e, com base nessas informações, decide onde posicionar os limites de floresta e domínio. Em seguida, a equipe de design decide como configurar o ambiente DNS (sistema de nomes de domínio) para atender às necessidades da floresta. Por fim, a equipe de design identifica a estrutura da UO (unidade organizacional) necessária para delegar o gerenciamento de recursos em sua organização.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre o modelo lógico de Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Como identificar os participantes do projeto de implantação](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Como criar um design de floresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Como criar um design de domínio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Como criar um design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Como criar um design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Apêndice A: inventário de DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


