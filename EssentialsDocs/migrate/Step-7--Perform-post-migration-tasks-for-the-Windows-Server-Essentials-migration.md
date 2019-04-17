---
title: "Etapa 7: Executar tarefas posteriores à migração para a migração do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Etapa 7: Executar tarefas posteriores à migração para a migração do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

As tarefas a seguir o ajudarão a concluir a configuração o servidor de destino com algumas das configurações do mesmo que estavam no servidor de origem. Talvez você tenha desativado algumas dessas configurações no seu servidor de origem durante o processo de migração, portanto, eles não foram migrados para o servidor de destino.  
  
1.  [Excluir entradas DNS do servidor de origem](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Compartilhar a linha de negócios e outras pastas de dados de aplicativo](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>Excluir entradas DNS do servidor de origem  
 Depois de desativar o servidor de origem, o servidor do serviço de nome de domínio (DNS) ainda pode conter entradas que apontam para o servidor de origem. Exclua essas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para excluir entradas DNS que apontam para o servidor de origem  
  
1.  No servidor de destino, abra **Gerenciador DNS**.  
  
2.  No Gerenciador DNS, clique com botão direito no nome do servidor, clique em **propriedades**e, em seguida, clique no **encaminhadores** guia.  
  
3.  Determine se existe uma entrada na lista encaminhador que aponta para o servidor de origem. Se houver, clique em **editar**e depois excluir essa entrada no **Editar encaminhadores** janela.  
  
4.  Em **Gerenciador DNS**, expanda o nome do servidor e, em seguida, **zonas de pesquisa direta**.  
  
5.  Para cada zona de pesquisa direta, clique com botão direito a zona, clique em **propriedades**e, em seguida, clique no **servidores de nomes** guia.  
  
6.  Selecione uma entrada no **servidores de nomes** caixa de texto que aponta para o servidor de origem, clique em **remover**e clique em **Okey**.  
  
7.  Repita as etapas 5 e 6 até que todos os ponteiros para o servidor de origem são removidos.  
  
8.  Clique em **Okey** para fechar o **propriedades** janela.  
  
9. Em **Gerenciador DNS**, expanda **zonas de pesquisa inversa**.  
  
10. Repita as etapas 6 a 9 para remover todas as zonas de pesquisa inversa que apontam para o servidor de origem.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartilhar a linha de negócios e outras pastas de dados de aplicativo  
 Você deve definir as permissões de pasta compartilhada e as permissões NTFS para a linha de negócios e outras pastas de dados de aplicativo que você copiou para o servidor de destino. Depois de definir as permissões, as pastas compartilhadas são exibidas no painel sobre o **armazenamento** guia.  
  
 Se você estiver usando um script de logon para mapear unidades às pastas compartilhadas, você deve atualizar o script para mapear para as unidades no servidor de destino.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você executou as tarefas após a migração para migração do Windows Server Essentials. Acesse agora [etapa 8 - executar o analisador de práticas recomendadas do Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

