---
title: Migrar do Windows Small Business Server 2011 Essentials para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 32fc90d8-31c5-4c7e-9fe3-483cf3c35f78
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 190d49fe3a33c5391f199cb013d661cc519b1ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852499"
---
# <a name="migrate-windows-small-business-server-2011-essentials-to-windows-server-essentials"></a>Migrar do Windows Small Business Server 2011 Essentials para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar um domínio existente do Windows Small Business Server 2011 Essentials para o Windows Server&reg; 2012 Essentials e, em seguida, migrar as configurações e os dados. Este guia também descreve como remover o servidor existente da rede do Windows Server Essentials depois de concluir a migração.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento de produtos do Windows Server Essentials recomenda enfaticamente que você leia este documento antes de começar a migração.  
> 
> [!NOTE]
> 
>  Para migrar os dados do servidor para a versão mais recente do Windows Server Essentials, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
> 
>  Para migrar os dados do servidor para a versão mais recente do Windows Server Essentials, consulte [migrar para o Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Recursos adicionais  
 Para obter links para informações adicionais, ferramentas e recursos da Comunidade para ajudá-lo a orientá-lo durante o processo de migração, consulte [migração do Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termos e definições  
 **Servidor de origem:** O servidor existente do qual você está migrando suas configurações e dados.  
  
 **Servidor de destino:** O novo servidor para o qual você está migrando suas configurações e dados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  

1.  [Prepare o servidor de origem para a migração do Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que o servidor de origem e a rede estejam prontos para a migração. Esta seção o guia pelos processos de backup do servidor de origem, avaliação da integridade do sistema do servidor de origem, instalação dos service packs e das correções mais recentes e verificação da configuração da rede.  
  
2.  [Instale o Windows Server Essentials no modo de migração](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que devem ser seguidas para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Junte computadores ao novo servidor do Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção aborda a adição de computadores cliente ao novo servidor do Windows Server Essentials e a atualização de Política de Grupo configurações.  
  
4.  [Mova os dados e as configurações do SBS 2011 Essentials para o servidor de destino](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md).  Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem.  
  
5.  [Habilite o redirecionamento de pasta no servidor de destino do Windows Server Essentials](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Caso o redirecionamento de pastas esteja habilitado no servidor de origem, você poderá habilitá-lo no servidor de destino e excluir a antiga configuração de Política de Grupo de Redirecionamento de Pastas.  
  
6.  [Rebaixe e remova o servidor de origem da nova rede do Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes da remoção do servidor de origem da rede, você deve forçar uma atualização da Política de Grupo e rebaixar o servidor de origem.  
  
7.  [Executar tarefas de pós-implantação para a migração do Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados para o Windows Server Essentials, talvez você queira mapear os computadores permitidos para as contas de usuário.  
  
8.  [Execute o analisador de práticas recomendadas do Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração de configurações e dados para o Windows Server Essentials, você deve baixar e executar o BPA do Windows Server Essentials.  

 Vários procedimentos de migração exigem a abertura de uma janela de prompt de comando como administrador.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Para abrir uma janela de prompt de comando no servidor de origem como administrador  
  
1.  Clique em **Iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com o botão direito em **cmd** e clique em **Executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Para abrir uma janela de prompt de comando no servidor de destino como administrador.  
  
1.  Na tela **Início**, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com o botão direito em **cmd** e clique em **Executar como administrador**.
