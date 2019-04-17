---
title: Crie o objeto de política de grupo de publicação de Hash BranchCache
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Crie o objeto de política de grupo de publicação de Hash BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para criar a publicação de hash BranchCache objeto de política de grupo (GPO).  
  
A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Antes de executar este procedimento, você deve criar a unidade organizacional do BranchCache arquivo servidores e mover os servidores de arquivos para a unidade Organizacional. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos de membro do domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para criar a publicação de hash BranchCache objeto de política de grupo  
  
1.  Abra o Windows PowerShell, tipo **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft (MMC) é aberta.  
  
2.  No MMC, no **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.  
  
3.  Em **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **Group Policy Management**e clique em **Okey**.  
  
4.  No MMC de gerenciamento de política de grupo, expanda o caminho para os servidores de arquivo BranchCache UO que você criou anteriormente. Por exemplo, se sua floresta é denominada example.com, seu domínio é denominado example1.com, e seu UO BranchCache servidores de arquivos, expanda o caminho a seguir: **Group Policy Management**, **floresta: example.com**, **domínios**, **example1.com**, **servidores de arquivos BranchCache**.  
  
5.  Clique com botão direito **servidores de arquivos BranchCache**e clique em **criar um GPO neste domínio e vinculá-lo aqui**. O **novo GPO** caixa de diálogo é aberta. Em **nome**, digite um nome para o novo GPO. Por exemplo, se você quiser nomear o objeto BranchCache Hash de publicação, digite **BranchCache Hash publicação**. Clique em **Okey**.  
  


