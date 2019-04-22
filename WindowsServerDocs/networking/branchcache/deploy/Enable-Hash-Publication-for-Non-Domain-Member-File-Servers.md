---
title: Habilitar publicação de hash para servidores de arquivos do membro que não sejam de domínio
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00be97abbd583e4c5e762775ea563ba0720d5142
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814947"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Habilitar publicação de hash para servidores de arquivos do membro que não sejam de domínio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar a publicação de hash para BranchCache usando a diretiva de grupo do computador local em um servidor de arquivos que está executando o Windows Server 2016 com o **BranchCache para arquivos de rede** serviço de função dos serviços de arquivo função de servidor instalada.  
  
Este procedimento se destina ao uso em um servidor de arquivos que não é membro do domínio. Se você executar este procedimento em um servidor de arquivos membro do domínio e também configurar o BranchCache usando a Diretiva de Grupo do domínio, as configurações da Diretiva de Grupo do domínio substituirão as configurações da Diretiva de Grupo local.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Se você tiver um ou mais servidores de arquivos membro do domínio, poderá adicioná-los a uma UO (unidade organizacional) nos Serviços de Domínio Active Directory e então usar a Diretiva de Grupo para configurar publicação de hash para todos os servidores de arquivos de uma vez, em vez de configurar cada servidor de arquivos individualmente. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos do membro de domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Para habilitar publicação de hash para um servidor de arquivos  
  
1.  Abra o Windows PowerShell, digite **mmc**e pressione ENTER. O Console de Gerenciamento Microsoft (MMC) é aberto.  
  
2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.  
  
3.  Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Editor de Objeto de Diretiva de Grupo**. O Assistente de Diretiva de Grupo é aberto com o objeto Computador Local selecionado. Clique em **Concluir**e em **OK**.  
  
4.  No MMC do Editor de Diretiva de Grupo Local, expanda o seguinte caminho: **Política do computador local**, **configuração do computador**, **modelos administrativos**, **rede**, **servidor Lanman**. Clique em **Servidor Lanman**.  
  
5.  No painel de detalhes, clique duas vezes em **Publicação de Hash para BranchCache**. A caixa de diálogo **Publicação de Hash para BranchCache** é aberta.  
  
6.  Na caixa de diálogo **Publicação de Hash para BranchCache**, clique em **Habilitado**.  
  
7.  Na **opções**, clique em **permitir a publicação de hash para todas as pastas compartilhadas**e, em seguida, clique em um dos seguintes:  
  
    1.  Para habilitar a publicação de hash para todas as pastas compartilhadas neste computador, clique em **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
    2.  Para habilitar a publicação de hash somente para as pastas compartilhadas nas quais o BranchCache está habilitado, clique em **Permitir a publicação de hash somente em pastas compartilhadas nas quais o BranchCache está habilitado**.  
  
    3.  Para não permitir a publicação de hash para todas as pastas compartilhadas no computador, mesmo que o BranchCache esteja habilitado nos compartilhamentos de arquivos, clique em **Impedir a publicação de hash em todas as pastas compartilhadas**.  
  
8.  Clique em **OK**.  
  


