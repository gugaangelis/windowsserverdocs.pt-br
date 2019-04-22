---
title: Migrar do Windows Server 2008 Foundation para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22fc0a4-cb82-4e60-afe6-2d03145745e7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 22721833d3ba97c7860c949764d565415bbc7cab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813757"
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>Migrar do Windows Server 2008 Foundation para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar um domínio existente do Windows Server 2008 Foundation para Windows Server® 2012 Essentials em um novo hardware e, em seguida, migrar as configurações e dados. Este guia também descreve como remover seu servidor existente da rede do Windows Server Essentials depois de concluir a migração.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento de produto do Windows Server Essentials recomenda enfaticamente a leitura deste documento antes de começar a migração.  
  
## <a name="additional-resources"></a>Recursos adicionais  
 Para obter links para informações adicionais, ferramentas e recursos da comunidade para orientá-lo durante o processo de migração, consulte [migração do Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termos e definições  
 **Servidor de origem:** O servidor do qual você está migrando as configurações e dados.  
  
 **Servidor de destino:** O novo servidor para o qual as configurações e os dados são migrados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  

1.  [Preparar a migração do servidor de origem para o Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que necessárias para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Adicionar computadores à nova rede do Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta seção abrange o ingresso de computadores cliente para a nova rede do Windows Server Essentials e atualizando as configurações de diretiva de grupo.  
  
4.  [Mover dados e configurações do Windows Server 2008 Foundation para o servidor de destino](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5.  [Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
6.  [Realizar tarefas pós-migração para a migração do Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, convém mapear os computadores permitidos para contas de usuário.  
  
7.  [Executar o analisador de práticas recomendadas do Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados para o Windows Server Essentials, você deve executar o BPA do Windows Server Essentials.  

1.  [Preparar a migração do servidor de origem para o Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que necessárias para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Adicionar computadores à nova rede do Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta seção abrange o ingresso de computadores cliente para a nova rede do Windows Server Essentials e atualizando as configurações de diretiva de grupo.  
  
4.  [Mover dados e configurações do Windows Server 2008 Foundation para o servidor de destino](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5.  [Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
6.  [Realizar tarefas pós-migração para a migração do Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, convém mapear os computadores permitidos para contas de usuário.  
  
7.  [Executar o analisador de práticas recomendadas do Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados para o Windows Server Essentials, você deve executar o BPA do Windows Server Essentials.  

  
 Vários procedimentos de migração exigem a abertura de uma janela de prompt de comando como administrador.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Para abrir uma janela de prompt de comando no servidor de origem como administrador  
  
1.  Clique em **Iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com o botão direito em **cmd**e clique em **Executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir uma janela de prompt de comando no servidor de destino como administrador.  
  
1.  Na tela **Início**, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com o botão direito em **cmd**e clique em **Executar como administrador**.
