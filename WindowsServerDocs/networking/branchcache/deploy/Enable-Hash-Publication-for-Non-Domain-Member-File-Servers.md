---
title: Habilitar a publicação de Hash não membro do domínio para servidores de arquivos
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Habilitar a publicação de Hash não membro do domínio para servidores de arquivos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar a publicação de hash para BranchCache usando política de grupo do computador local em um servidor de arquivo que está executando o Windows Server 2016 com o **BranchCache para arquivos de rede** serviço de função da função de servidor Serviços de arquivo instalado.  
  
Este procedimento destina-se para uso em um servidor de arquivos não membro do domínio. Se você executar este procedimento em um servidor de arquivos de membro do domínio e você também configurar BranchCache usando política de grupo de domínio, configurações de política de grupo de domínio substituem as configurações de política de grupo locais.  
  
A associação ao grupo **administradores**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Se você tiver um ou mais servidores de arquivos de membro do domínio, você pode adicioná-los a uma unidade organizacional (UO) nos serviços de domínio do Active Directory e, em seguida, usar a política de grupo para configurar a publicação de hash para todos os servidores de arquivos de uma só vez, em vez de configurar individualmente cada servidor de arquivos. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos de membro do domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Para habilitar a publicação de hash para um servidor de arquivos  
  
1.  Abra o Windows PowerShell, tipo **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft (MMC) é aberta.  
  
2.  No MMC, no **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.  
  
3.  Em **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **Editor de objeto de política de grupo**. O Assistente de política de grupo é aberto com o objeto de computador Local selecionado. Clique em **concluir**e clique em **Okey**.  
  
4.  No MMC Editor de política de Grupo Local, expanda o caminho a seguir: **política de computador Local**, **configuração do computador**, **modelos administrativos**, **rede**, **Lanman Server**. Clique em **Lanman Server**.  
  
5.  No painel de detalhes, clique duas vezes em **Hash de publicação para BranchCache**. O **Hash de publicação para BranchCache** caixa de diálogo é aberta.  
  
6.  No **Hash de publicação para BranchCache** caixa de diálogo, clique em **Enabled**.  
  
7.  Em **opções**, clique em **permitir a publicação de hash para todas as pastas compartilhadas**e, em seguida, clique em um destes procedimentos:  
  
    1.  Para habilitar a publicação de hash para todas as pastas compartilhadas neste computador, clique em **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
    2.  Para habilitar a publicação de hash somente para pastas compartilhadas para qual BranchCache está habilitado, clique em **permitir a publicação de hash somente para pastas compartilhadas no qual está habilitado BranchCache**.  
  
    3.  Para impedir a publicação de hash para todas as pastas compartilhadas no computador mesmo se BranchCache for habilitado em compartilhamentos de arquivos, clique em **não permitir a publicação de hash em todas as pastas compartilhadas**.  
  
8.  Clique em **Okey**.  
  


