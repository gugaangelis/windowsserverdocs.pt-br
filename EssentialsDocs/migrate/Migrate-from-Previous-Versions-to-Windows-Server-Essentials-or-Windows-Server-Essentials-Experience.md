---
title: Migrar de versões anteriores para o Windows Server Essentials ou Experiência do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5c73714dff2d89201ac93704105038c604f12e06
ms.sourcegitcommit: 2f072c0c02e3e0deae331ca64b375d63b89d0522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404579"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migrar de versões anteriores para o Windows Server Essentials ou Experiência do Windows Server Essentials

>Aplica-se a: Windows Server 2012 R2 Essentials

Este guia descreve como migrar de versões anteriores do Windows Small Business Server e do Windows Server Essentials (incluindo o Windows Server Essentials, o Windows Small Business Server 2011 Standard, o Windows Small Business Server 2011 Essentials, o Windows Small Business Server 2008 e o Windows Small Business Server 2003) para o Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada.  
  
 **Para ambientes com até 25 usuários e 50 dispositivos**, você pode seguir as etapas neste guia para migrar de versões anteriores do Windows SBS para o Windows Server Essentials.  
  
 **Para ambientes com até 100 usuários e 200 dispositivos**, você pode seguir as mesmas diretrizes para migrar para as edições Standard ou Datacenter do Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento do produto Windows Server Essentials recomenda enfaticamente a leitura deste documento antes de começar a migração.  
  
## <a name="terms-and-definitions"></a>Termos e definições  
 **Servidor de origem:** o servidor exustente do qual você está migrando as configurações e dados.  
  
 **Servidor de destino:** o novo servidor para o qual as configurações e os dados são migrados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  
1. [Etapa 1: preparar o servidor de origem para a migração do Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2. [Etapa 2: Instale o Windows Server Essentials como um novo controlador de domínio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Esta seção descreve como instalar o Windows Server Essentials ou o Windows Server 2012 R2 Standard (com a função de experiência do Windows Server Essentials habilitada) como um controlador de domínio.  
  
3. [Etapa 3: unir computadores ao novo servidor do Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção explica como unir computadores cliente ao novo servidor executando o Windows Server Essentials e atualizando as configurações de Política de Grupo.  
  
4. [Etapa 4: mover as configurações e os dados para o servidor de destino para a migração do Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5. [Etapa 5: habilitar o redirecionamento de pasta no servidor de destino para a migração do Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Caso o redirecionamento de pastas esteja habilitado no servidor de origem, você poderá habilitá-lo no servidor de destino e excluir a antiga configuração de Política de Grupo de Redirecionamento de Pastas.  
  
6. [Etapa 6: rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
7. [Etapa 7: executar tarefas de pós-implantação para a migração do Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, talvez você queira mapear os computadores permitidos para as contas de usuário.  
  
8. [Etapa 8: executar o analisador de práticas recomendadas do Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração de configurações e dados para o Windows Server Essentials, você deve executar o BPA (Windows Server Essentials Analisador de Práticas Recomendadas).  
  
   Vários procedimentos de migração exigem a abertura de uma janela de prompt de comando como administrador. Os procedimentos a seguir explicam como fazer isso.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Para abrir uma janela de prompt de comando no servidor de origem como administrador  
  
1.  Clique em **Iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com o botão direito em **cmd** e clique em **Executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir uma janela de prompt de comando no servidor de destino como administrador.  
  
1.  Na tela **Início**, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com o botão direito em **cmd** e clique em **Executar como administrador**.  
  
## <a name="see-also"></a>Confira também  
  
-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

