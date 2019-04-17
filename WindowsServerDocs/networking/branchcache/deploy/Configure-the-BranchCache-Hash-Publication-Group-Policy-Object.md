---
title: Configurar o objeto de política de grupo de publicação de Hash BranchCache
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar o objeto de política de grupo de publicação de Hash BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar a publicação de hash BranchCache objeto de política de grupo (GPO) para que todos os servidores de arquivos que você adicionou ao seu UO tenham a mesma configuração de política de publicação de hash aplicada a eles.  
  
A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Antes de executar este procedimento, você deve criar uma unidade organizacional do servidores de arquivo BranchCache, mova os servidores de arquivos para a UO e crie a publicação de hash BranchCache GPO. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos de membro do domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar a publicação de hash BranchCache objeto de política de grupo  
  
1.  Execute o Windows PowerShell como administrador, tipo **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft (MMC) é aberta.  
  
2.  No MMC, no **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.  
  
3.  Em **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **Group Policy Management**e clique em **Okey**.  
  
4.  No MMC de gerenciamento de política de grupo, expanda o caminho para a publicação de hash BranchCache GPO que você criou anteriormente. Por exemplo, se sua floresta é denominada example.com, seu domínio é denominado example1.com e o GPO é nomeado **BranchCache Hash publicação**, expanda o caminho a seguir: **Group Policy Management**, **floresta: example.com**, **domínios**, **example1.com**, **objetos de política de grupo**, **BranchCache Hash publicação**.  
  
5.  Clique com botão direito do **BranchCache Hash publicação** GPO e clique em **editar**. O console do Editor de gerenciamento de política de grupo é aberto.  
  
6.  No console do Editor de gerenciamento de política de grupo, expanda o caminho a seguir: **configuração do computador**, **políticas**, **modelos administrativos**, **rede**, **Lanman Server**.  
  
7.  No console do Editor de gerenciamento de política de grupo, clique em **Lanman Server**. No painel de detalhes, clique duas vezes em **Hash de publicação para BranchCache**. O **Hash de publicação para BranchCache** caixa de diálogo é aberta.  
  
8.  No **Hash de publicação para BranchCache** caixa de diálogo, clique em **Enabled**.  
  
9. Em **opções**, clique em **permitir a publicação de hash para todas as pastas compartilhadas**e, em seguida, clique em um destes procedimentos:  
  
    1.  Para habilitar a publicação de hash para todas as pastas compartilhadas para todos os arquivos que você adicionou à UO de servidores, clique em **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
    2.  Para habilitar a publicação de hash somente para pastas compartilhadas para qual BranchCache está habilitado, clique em **permitir a publicação de hash somente para pastas compartilhadas no qual está habilitado BranchCache**.  
  
    3.  Para impedir a publicação de hash para todas as pastas compartilhadas no computador mesmo se BranchCache for habilitado em compartilhamentos de arquivos, clique em **não permitir a publicação de hash em todas as pastas compartilhadas**.  
  
10. Clique em **Okey**.  
  
> [!NOTE]  
> Na maioria dos casos, você deve salvar o console do MMC e atualizar a exibição para exibir as alterações de configuração que você fez.  
  


