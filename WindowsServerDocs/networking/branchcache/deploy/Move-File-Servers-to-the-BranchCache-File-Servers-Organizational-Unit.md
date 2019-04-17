---
title: Mover servidores de arquivos para a unidade organizacional do arquivo BranchCache servidores
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover servidores de arquivos para a unidade organizacional do arquivo BranchCache servidores

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para adicionar BranchCache servidores de arquivos para uma unidade organizacional (UO) nos serviços de domínio do Active Directory (AD DS).  
  
A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Você deve criar uma UO de servidores de arquivos BranchCache no console de usuários e Active Directory computadores antes de adicionar contas de computador à UO de com este procedimento. Para obter mais informações, consulte [criar a unidade organizacional de servidores de arquivo BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover os servidores de arquivos para o BranchCache unidade organizacional servidores de arquivos  
  
1.  Em um computador onde AD DS é instalado, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. O console de usuários e Active Directory computadores é aberto.  
  
2.  No console do Active Directory usuários e computadores, localize a conta de computador para um servidor de arquivos BranchCache, clique em selecionar a conta e, em seguida, arrastar e soltar a conta de computador nos servidores de arquivos BranchCache UO que você criou anteriormente. Por exemplo, se você criou anteriormente uma UO denominada **servidores de arquivos BranchCache**, arraste e solte a conta de computador a **BranchCache servidores de arquivos** UO.  
  
3.  Repita a etapa anterior para cada servidor de arquivos BranchCache no domínio que você deseja mover para a UO.  
  


