---
title: Migrar do Windows Server Essentials para um novo Hardware
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f695ae90-3160-407b-bebf-9e460f22c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 61106439a63a75143a9cca0989c70370adfedd38
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-essentials-to-new-hardware"></a>Migrar do Windows Server Essentials para um novo Hardware

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia descreve como migrar um domínio existente do Windows Server® 2012 Essentials para Windows Server Essentials em um novo hardware e depois para migrar as configurações e dados. Este guia descreve também como remover seu servidor existente da rede Windows Server Essentials depois de concluir a migração.  
  
> [!NOTE]
>  Para evitar problemas durante a migração, a equipe de desenvolvimento do produto Windows Server Essentials recomenda veementemente que você leia esse documento antes de começar a migração.  
  
> [!NOTE]

>  Para migrar os dados do servidor para a versão mais recente do Windows Server Essentials, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Recursos adicionais  
 Para obter links para informações adicionais, ferramentas e recursos da comunidade para ajudar a orientar você durante o processo de migração, visite o [migração do Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) site.  
  
## <a name="terms-and-definitions"></a>Termos e as definições  
 **Servidor de origem:** o servidor existente do qual você está migrando suas configurações e dados.  
  
 **Servidor de destino:** o novo servidor ao qual você está migrando suas configurações e dados.  
  
## <a name="migration-process-summary"></a>Resumo do processo de migração  
 Este guia de migração inclui as seguintes etapas:  
  

1.  [Preparar sua migração do servidor de origem para o Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Você deve garantir que seu servidor de origem e a rede estão prontos para migração. Esta seção orienta você através de fazer backup de servidor de origem, avaliar a integridade do sistema de servidor de origem, instalando os service packs e correções mais recentes e verificando a configuração de rede.  
  
2.  [Instalar o Windows Server Essentials no modo de migração](Install-Windows-Server-Essentials-in-migration-mode.md).  Esta seção descreve as etapas que você deve tomar para instalar o Windows Server Essentials no servidor de destino no modo de migração.  
  
3.  [Ingressar computadores para o novo servidor Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Esta seção aborda ingressando em computadores cliente para o novo servidor Windows Server Essentials e atualizar as configurações de política de grupo.  
  
4.  [Mova as configurações e dados para a migração do servidor de destino para o Windows Server Essentials](Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Esta seção fornece informações sobre migração de dados e as configurações do servidor de origem.  
  
5.  [Configurar o redirecionamento de pasta no servidor de destino do Windows Server Essentials](Configure-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Se o redirecionamento de pasta está habilitado no servidor de origem, você pode habilitar o redirecionamento de pasta no servidor de destino e, em seguida, excluir a configuração de política de grupo de redirecionamento de pasta antiga.  
  
6.  [Rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Antes de remover o servidor de origem da rede, você deve forçar uma atualização de política de grupo e rebaixar o servidor de origem.  
  
7.  [Executar tarefas posteriores à migração para migração do Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Depois de concluir a migração de todas as configurações e dados ao Windows Server Essentials, convém mapear computadores permitidos para contas de usuário.  
  
8.  [Execute o analisador de práticas recomendadas do Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Depois de concluir a migração das configurações e dados ao Windows Server Essentials, você deve baixar e executar o Windows Server Essentials BPA.  
  
 Vários dos procedimentos migração exigem que você abra uma janela de Prompt de comando como administrador.  
  
#### <a name="to-open-a-command-prompt-window-as-an-administrator"></a>Abra uma janela de Prompt de comando como administrador  
  
1.  Sobre o **iniciar** tela, na caixa de pesquisa, digite **cmd**.  
  
2.  Na lista de resultados, clique com botão direito **cmd**e clique em **executar como administrador**.
