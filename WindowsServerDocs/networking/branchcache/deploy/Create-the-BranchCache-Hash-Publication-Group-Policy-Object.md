---
title: Criar o Objeto de Política de Grupo de publicação de hash BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865427"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Criar o Objeto de Política de Grupo de publicação de hash BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para criar a publicação de hash do BranchCache objeto de diretiva de grupo (GPO).  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Antes de executar este procedimento, crie a unidade organizacional de servidores de arquivos BranchCache e mova os servidores de arquivos para a UO. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos do membro de domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para criar a publicação de hash do BranchCache objeto de diretiva de grupo  
  
1.  Abra o Windows PowerShell, digite **mmc**e pressione ENTER. O Console de Gerenciamento Microsoft (MMC) é aberto.  
  
2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.  
  
3.  Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Gerenciamento de Diretiva de Grupo** e clique em **OK**.  
  
4.  No MMC do Gerenciamento de Diretiva de Grupo, expanda o caminho para a UO dos servidores de arquivos BranchCache criada anteriormente. Por exemplo, se a sua floresta for denominada exemplo.com, seu domínio for denominado exemplo1.com e sua OU for denominada Servidores de arquivos BranchCache, expanda o seguinte caminho: **Gerenciamento de diretiva de grupo**, **floresta: exemplo.com**, **domínios**, **exemplo1.com**, **servidores de arquivos BranchCache**.  
  
5.  Clique com botão direito **servidores de arquivos BranchCache**e, em seguida, clique em **criar um GPO neste domínio e vinculá-lo aqui**. A caixa de diálogo **Novo GPO** é aberta. Na **nome**, digite um nome para o novo GPO. Por exemplo, se desejar denominar o objeto como Publicação de Hash do BranchCache, digite **Publicação de Hash do BranchCache**. Clique em **OK**.  
  


