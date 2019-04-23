---
title: Mover servidores de arquivos para a unidade organizacional de servidores de arquivos BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885897"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover servidores de arquivos para a unidade organizacional de servidores de arquivos BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para adicionar servidores de arquivos BranchCache a uma UO (unidade organizacional) no AD DS (Serviços de Domínio Active Directory).  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Você deve criar uma UO de servidores de arquivos BranchCache no console de Usuários e Computadores do Active Directory antes de adicionar contas de computador à UO com este procedimento. Para obter mais informações, consulte [Criar unidade organizacional de servidores de arquivos BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover servidores de arquivos para a unidade organizacional de servidores de arquivos BranchCache  
  
1.  Em um computador onde AD DS está instalado, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory Users and Computers**. O console de Usuários e Computadores do Active Directory é aberto.  
  
2.  No console de Usuários e Computadores do Active Directory, localize a conta de computador de um servidor de arquivos BranchCache, clique para selecionar a conta e arraste e solte a conta de computador na UO de servidores de arquivos BranchCache que você criou. Por exemplo, se você criou uma UO denominada **servidores de arquivos BranchCache**, arraste e solte a conta de computador sobre o **servidores de arquivos BranchCache** UO.  
  
3.  Repita a etapa anterior para cada servidor de arquivos BranchCache no domínio que você deseja mover para a UO.  
  


