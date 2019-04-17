---
title: Migrar do Windows Server 2008 Foundation ao Windows Server Essentials
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>Migrar do Windows Server 2008 Foundation ao Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar um domínio do Windows Server 2008 Foundation existente para o Windows Server® 2012 Essentials em um novo hardware e depois para migrar as configurações e dados. Este guia descreve também como remover seu servidor existente da rede Windows Server Essentials depois de concluir a migração.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento do produto Windows Server Essentials recomenda veementemente que você leia esse documento antes de começar a migração.  
  
## <a name="additional-resources"></a>Recursos adicionais  
 Para obter links para informações adicionais, ferramentas e recursos da comunidade para ajudar a orientar você durante o processo de migração, veja [migração do Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termos e as definições  
 **Servidor de origem:** o servidor existente do qual você está migrando suas configurações e dados.  
  
 **Servidor de destino:** o novo servidor ao qual você está migrando suas configurações e dados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  

1.  [Preparar sua migração do servidor de origem para o Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que seu servidor de origem e a rede estão prontos para migração. Esta seção orienta você através de fazer backup de servidor de origem, avaliar a integridade do sistema de servidor de origem, instalando os service packs e correções mais recentes e verificando a configuração de rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que você deve tomar para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Ingressar computadores para a nova rede do Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta seção aborda ingressando em computadores cliente para a nova rede do Windows Server Essentials e atualizar as configurações de política de grupo.  
  
4.  [Mova as configurações do Windows Server 2008 Foundation e os dados ao servidor de destino](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre migração de dados e as configurações do servidor de origem.  
  
5.  [Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de remover o servidor de origem da rede, você deve forçar uma atualização de política de grupo e rebaixar o servidor de origem.  
  
6.  [Executar tarefas posteriores à migração para migração do Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados ao Windows Server Essentials, convém mapear computadores permitidos para contas de usuário.  
  
7.  [Execute o analisador de práticas recomendadas do Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados ao Windows Server Essentials, você deve executar o Windows Server Essentials BPA.  

1.  [Preparar sua migração do servidor de origem para o Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que seu servidor de origem e a rede estão prontos para migração. Esta seção orienta você através de fazer backup de servidor de origem, avaliar a integridade do sistema de servidor de origem, instalando os service packs e correções mais recentes e verificando a configuração de rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que você deve tomar para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Ingressar computadores para a nova rede do Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Esta seção aborda ingressando em computadores cliente para a nova rede do Windows Server Essentials e atualizar as configurações de política de grupo.  
  
4.  [Mova as configurações do Windows Server 2008 Foundation e os dados ao servidor de destino](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre migração de dados e as configurações do servidor de origem.  
  
5.  [Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de remover o servidor de origem da rede, você deve forçar uma atualização de política de grupo e rebaixar o servidor de origem.  
  
6.  [Executar tarefas posteriores à migração para migração do Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados ao Windows Server Essentials, convém mapear computadores permitidos para contas de usuário.  
  
7.  [Execute o analisador de práticas recomendadas do Windows Server Essentials](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados ao Windows Server Essentials, você deve executar o Windows Server Essentials BPA.  

  
 Vários dos procedimentos migração exigem que você abra uma janela de prompt de comando como administrador.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Abra uma janela de prompt de comando no servidor de origem como administrador  
  
1.  Clique em **iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com botão direito **cmd**e clique em **executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Abra uma janela de prompt de comando no servidor de destino como administrador  
  
1.  Sobre o **iniciar** tela, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com botão direito **cmd**e clique em **executar como administrador**.
