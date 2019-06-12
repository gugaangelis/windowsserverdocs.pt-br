---
title: Migrar do Windows Small Business Server 2011 Standard para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8325f87-fd79-471b-bf70-3f052692c383
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e16026b897a24c15e7a0506fe9e64c688bf363b6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432807"
---
# <a name="migrate-windows-small-business-server-2011-standard-to-windows-server-essentials"></a>Migrar do Windows Small Business Server 2011 Standard para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar um domínio existente do Windows Small Business Server 2011 Standard para o Windows Server® 2012 Essentials em um novo hardware e, em seguida, migrar as configurações e dados. Este guia também descreve como remover seu servidor existente da rede do Windows Server Essentials depois de concluir a migração.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento de produto do Windows Server Essentials recomenda enfaticamente a leitura deste documento antes de começar a migração.  
> 
> [!NOTE]
> 
>  Para migrar dados do servidor para a versão mais recente do Windows Server Essentials, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
> 
>  Para migrar dados do servidor para a versão mais recente do Windows Server Essentials, consulte [migrar para o Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Recursos adicionais  
 Para obter links para informações adicionais, ferramentas e recursos da comunidade para orientá-lo durante o processo de migração, consulte [migração do Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termos e definições  
 **Servidor de origem:** O servidor do qual você está migrando as configurações e dados.  
  
 **Servidor de destino:** O novo servidor para o qual as configurações e os dados são migrados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  

1.  [Preparar a migração do servidor de origem para o Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que necessárias para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Fazer computadores ingressarem no novo servidor Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção abrange o ingresso de computadores cliente para a nova rede do Windows Server Essentials e atualizando as configurações de diretiva de grupo.  
  
4.  [Mover dados e configurações do SBS 2011 para o servidor de destino](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5.  [Habilitar o redirecionamento de pasta no servidor de destino do Windows Server Essentials](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Caso o redirecionamento de pastas esteja habilitado no servidor de origem, você poderá habilitá-lo no servidor de destino e excluir a antiga configuração de Política de Grupo de Redirecionamento de Pastas.  
  
6.  [Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
7.  [Realizar tarefas pós-migração para a migração do Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, convém mapear os computadores permitidos para contas de usuário.  
  
8.  [Executar o analisador de práticas recomendadas do Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados para o Windows Server Essentials, você deve executar o BPA do Windows Server Essentials.  

1.  [Preparar a migração do servidor de origem para o Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que necessárias para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Fazer computadores ingressarem no novo servidor Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção abrange o ingresso de computadores cliente para a nova rede do Windows Server Essentials e atualizando as configurações de diretiva de grupo.  
  
4.  [Mover dados e configurações do SBS 2011 para o servidor de destino](../migrate/Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5.  [Habilitar o redirecionamento de pasta no servidor de destino do Windows Server Essentials](../migrate/Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Caso o redirecionamento de pastas esteja habilitado no servidor de origem, você poderá habilitá-lo no servidor de destino e excluir a antiga configuração de Política de Grupo de Redirecionamento de Pastas.  
  
6.  [Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
7.  [Realizar tarefas pós-migração para a migração do Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, convém mapear os computadores permitidos para contas de usuário.  
  
8.  [Executar o analisador de práticas recomendadas do Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados para o Windows Server Essentials, você deve executar o BPA do Windows Server Essentials.  

  
 Vários procedimentos de migração exigem a abertura de uma janela de prompt de comando como administrador.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Para abrir uma janela de prompt de comando no servidor de origem como administrador  
  
1.  Clique em **Iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com o botão direito em **cmd**e clique em **Executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir uma janela de prompt de comando no servidor de destino como administrador.  
  
1.  Na tela **Início**, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com o botão direito em **cmd**e clique em **Executar como administrador**.
