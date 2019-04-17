---
title: "Migração de versões anteriores ao Windows Server Essentials ou da experiência do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migração de versões anteriores ao Windows Server Essentials ou da experiência do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar de versões anteriores do Windows Small Business Server e Windows Server Essentials (incluindo o Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 e Windows Small Business Server 2003) para Windows Server Essentials ou Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada.  
  
 **Para ambientes com até 25 usuários e 50 dispositivos**, você pode seguir as etapas neste guia para migrar de versões anteriores do Windows SBS para o Windows Server Essentials.  
  
 **Para ambientes com até 100 usuários e 200 dispositivos**, você pode seguir a mesma diretriz para migrar às edições do Windows Server 2012 R2 Standard ou Datacenter com a função de experiência do Windows Server Essentials instalada.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento do produto Windows Server Essentials recomenda veementemente que você leia esse documento antes de começar a migração.  
  
## <a name="terms-and-definitions"></a>Termos e as definições  
 **Servidor de origem** o servidor existente do qual você está migrando suas configurações e dados.  
  
 **Servidor de destino** o novo servidor ao qual você está migrando suas configurações e dados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  
1.  [Etapa 1: Preparar sua migração do servidor de origem para o Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que seu servidor de origem e a rede estão prontos para migração. Esta seção orienta você através de fazer backup de servidor de origem, avaliar a integridade do sistema de servidor de origem, instalando os service packs e correções mais recentes e verificando a configuração de rede.  
  
2.  [Etapa 2: Instalar Windows Server Essentials como um controlador de domínio réplica novo](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Esta seção descreve como instalar o Windows Server Essentials, ou Windows Server 2012 R2 Standard (com a função de experiência do Windows Server Essentials habilitada) como um controlador de domínio.  
  
3.  [Etapa 3: Ingressar computadores para o novo servidor Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção explica como ingressar em computadores cliente para o novo servidor executando o Windows Server Essentials e atualizar as configurações de política de grupo.  
  
4.  [Etapa 4: Mova as configurações e dados para a migração do servidor de destino para o Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre migração de dados e as configurações do servidor de origem.  
  
5.  [Etapa 5: Habilitar o redirecionamento de pasta sobre a migração do servidor de destino para o Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Se o redirecionamento de pasta está habilitado no servidor de origem, você pode habilitar o redirecionamento de pasta no servidor de destino e, em seguida, excluir a configuração de política de grupo de redirecionamento de pasta antiga.  
  
6.  [Etapa 6: Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de remover o servidor de origem da rede, você deve forçar uma atualização de política de grupo e rebaixar o servidor de origem.  
  
7.  [Etapa 7: Executar tarefas posteriores à migração para a migração do Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados ao Windows Server Essentials, convém mapear computadores permitidos para contas de usuário.  
  
8.  [Etapa 8: Executar o analisador de práticas recomendadas do Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados ao Windows Server Essentials, você deve executar o Windows Server Essentials BPA Best Practices Analyzer ().  
  
 Vários dos procedimentos migração exigem que você abra uma janela de Prompt de comando como administrador. Os procedimentos a seguir explicam como fazer isso.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Abra uma janela de Prompt de comando no servidor de origem como administrador  
  
1.  Clique em **iniciar**.  
  
2.  Na caixa de pesquisa, digite **cmd**.  
  
3.  Na lista de resultados, clique com botão direito **cmd**e clique em **executar como administrador**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Abra uma janela de Prompt de comando no servidor de destino como administrador  
  
1.  Sobre o **iniciar** tela, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com botão direito **cmd**e clique em **executar como administrador**.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Migrar dados do servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar dados do servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

