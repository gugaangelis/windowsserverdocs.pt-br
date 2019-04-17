---
title: "Executar tarefas posteriores à migração para o Windows Server Essentials migration1"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Executar tarefas posteriores à migração para o Windows Server Essentials migration1

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

As tarefas a seguir o ajudarão a concluir a configuração o servidor de destino com algumas das configurações do mesmo que estavam no servidor de origem. Talvez você tenha desativado algumas dessas configurações no seu servidor de origem durante o processo de migração, portanto, eles não foram migrados para o servidor de destino. Ou eles são as etapas de configuração opcional que você pode querer executar.  
  

-   [Excluir entradas DNS do servidor de origem](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartilhar a linha de negócios e outras pastas de dados de aplicativo](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Corrigir problemas do computador do cliente após a migração](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Conceder o direito de fazer logon como um trabalho em lotes de grupo Administradores interno](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Excluir entradas DNS do servidor de origem](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartilhar a linha de negócios e outras pastas de dados de aplicativo](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Corrigir problemas do computador do cliente após a migração](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Conceder o direito de fazer logon como um trabalho em lotes de grupo Administradores interno](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>Excluir entradas DNS do servidor de origem  
 Depois de desativar o servidor de origem, o servidor do serviço de nome de domínio (DNS) ainda pode conter entradas que apontam para o servidor de origem. Exclua essas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para excluir entradas DNS que apontam para o servidor de origem  
  
1.  No servidor de destino, abra **Gerenciador DNS**.  
  
2.  No Gerenciador DNS, clique com botão direito no nome do servidor, clique em **propriedades**e, em seguida, clique no **encaminhadores** guia.  
  
3.  Determine se existe uma entrada na lista encaminhador que aponta para o servidor de origem. Se houver, clique em **editar**e depois excluir essa entrada no **Editar encaminhadores** janela.  
  
4.  Em **Gerenciador DNS**, expanda o nome do servidor e, em seguida, **zonas de pesquisa direta**.  
  
5.  Para cada zona de pesquisa direta, clique com botão direito a zona, clique em **propriedades**e, em seguida, clique no **servidores de nomes** guia.  
  
6.  Clique em uma entrada do **servidores de nomes** caixa que aponta para o servidor de origem, clique em **remover**e clique em **Okey**.  
  
7.  Repita as etapas 5 e 6 até que todos os ponteiros para o servidor de origem são removidos.  
  
8.  Clique em **Okey** para fechar o **propriedades** janela.  
  
9. No **Gerenciador DNS** do console, expanda **zonas de pesquisa inversa**.  
  
10. Repita as etapas 6 a 9 para remover todas as zonas de pesquisa inversa que apontam para o servidor de origem.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Compartilhar a linha de negócios e outras pastas de dados de aplicativo  
 Você deve definir as permissões de pasta compartilhada e as permissões NTFS para a linha de negócios e outras pastas de dados de aplicativo que você copiou para o servidor de destino. Depois de definir as permissões, as pastas compartilhadas são exibidas no painel Windows Server Essentials no **armazenamento** seção.  
  
 Se você estiver usando um script de logon para mapear unidades às pastas compartilhadas, você deve atualizar o script para mapear para as unidades no servidor de destino.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Corrigir problemas do computador do cliente após a migração  
 Se você migrar para o Windows Server Essentials do Windows Small Business Server 2003 Premium Edition com o Microsoft Internet Security and Acceleration (ISA) Server instalado, computadores cliente na rede ainda tem o cliente de Firewall da Microsoft e Internet Explorer configurado para usar um servidor proxy.  
  
 Isso causa problemas de conectividade no cliente computadores, porque o servidor proxy não existe mais. Se houver um servidor proxy diferentes configurado, os computadores cliente continuam a usar o servidor que executa o Windows SBS 2003 para o servidor proxy. Para corrigir esse problema, será necessário reconfigurar o Internet Explorer para não usar um servidor proxy ou usar o novo servidor proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Reconfigurar o Internet Explorer  
  
1.  No Internet Explorer, clique em **ferramentas**e clique em **opções da Internet**.  
  
2.  Clique no **conexões**, clique em **configurações da LAN**, e siga um destes procedimentos:  
  
    -   Se você não estiver usando um servidor proxy na rede, desmarque todas as caixas de seleção no **configurações da rede Local (LAN)** caixa de diálogo.  
  
    -   Se você quiser usar um servidor proxy novas em sua rede:  
  
        1.  No **configurações da rede Local (LAN)** caixa de diálogo, desmarque as caixas de seleção no **configuração automática** seção.  
  
        2.  No **servidor Proxy** seção, verifique se ambas as caixas de seleção estão marcadas.  
  
        3.  No **endereço**, digite o nome de domínio totalmente qualificado (FQDN) do servidor proxy.  
  
        4.  No **porta**, digite **80**.  
  
3.  Clique em **Okey** duas vezes.  
  
4.  Navegue até um site para garantir que as configurações de conexão estão corretas.  
  
##  <a name="BKMK_AdminGroup"></a>Conceder o direito de fazer logon como um trabalho em lotes de grupo Administradores interno  
 Depois de migrar um domínio existente do Windows Small Business Server 2003 para o Windows Server Essentials, você deve dar o grupo Administradores interno o direito de fazer logon como um trabalho em lotes. Verifique se o grupo Administradores interno ainda tem o direito de fazer logon como um trabalho em lotes para o servidor de destino. Os administradores precisam deste direito para executar um alerta no servidor de destino sem fazer logon.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Para dar integrados administradores grupo o direito de fazer logon como um trabalho em lotes  
  
1.  No servidor de destino, abra o **Group Policy Management** ferramenta administrativa.  
  
2.  No **Group Policy Management** árvore do Console, expanda **floresta:***< ServerName\ >*, expanda domínios e, em seguida, seu servidor.  
  
3.  Expanda **controladores de domínio**, clique com botão direito **política de controladores de domínio padrão**e clique em **editar**.  
  
4.  Em **Editor de gerenciamento de política de grupo**, clique em **política de controladores de domínio padrão***< ServerName\ >***política**e, em seguida, expanda **configuração do computador**.  
  
5.  Expanda **políticas**, expanda **configurações do Windows**e, em seguida, expanda **configurações de segurança**.  
  
6.  No **configurações de segurança** árvore, expanda **políticas locais**e clique em **atribuição de direitos de usuário**.  
  
7.  No painel de resultados, clique com botão direito **faça logon como um trabalho em lotes**e, em seguida, clique em Propriedades.  
  
8.  No **faça logon como um trabalho em lotes propriedades** página, clique em **adicionar usuário ou grupo**.  
  
9. No **adicionar usuário ou grupo** caixa de diálogo, clique em **procurar**.  
  
10. No **selecionar usuários, computadores ou grupos** caixa de diálogo, digite **administradores**.  
  
11. Clique em **verificar nomes** para verificar se o grupo Administradores interno é exibida e, em seguida, clique em **Okey** três vezes para salvar a configuração.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Migrar do Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar dados do servidor para Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar do Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar dados do servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

