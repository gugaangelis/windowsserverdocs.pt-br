---
title: Habilitar a publicação de Hash para servidores de arquivos de membro do domínio
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 318879eae82d37f68acbc18cdb21ae5290f6d02b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar a publicação de Hash para servidores de arquivos de membro do domínio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando você estiver usando os serviços de domínio do Active Directory (AD DS), você pode usar a política de grupo de domínio para habilitar a publicação de hash BranchCache para vários servidores de arquivos. Para fazer isso, você deve criar uma unidade organizacional (UO), adicionar servidores de arquivos para a unidade Organizacional, crie uma publicação de hash BranchCache objeto de política de grupo (GPO) e, em seguida, configure o GPO.  
  
Veja os tópicos a seguir para habilitar a publicação de hash para vários servidores de arquivos.  
  
-   [Criar a unidade organizacional do arquivo BranchCache servidores](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover servidores de arquivos para a unidade organizacional do arquivo BranchCache servidores](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Crie o objeto de política de grupo de publicação de Hash BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar o objeto de política de grupo de publicação de Hash BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


