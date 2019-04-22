---
title: Realizar tarefas pós-migração para migration1 do Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821017"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Realizar tarefas pós-migração para migration1 do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

As tarefas a seguir ajudaram a concluir a configuração do servidor de destino com algumas das mesmas configurações que estavam no servidor de origem. Você pode ter desabilitado algumas dessas configurações no servidor de origem durante o processo de migração para que elas não fossem migradas para o servidor de destino. Ou são etapas de configuração opcionais que você pode executar.  
  

-   [Excluir entradas DNS do servidor de origem](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartilhar a linha de negócios e outras pastas de dados de aplicativo](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Corrigir problemas do computador do cliente após a migração](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Dar ao grupo Administradores interno o direito de fazer logon como um trabalho em lotes](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Excluir entradas DNS do servidor de origem](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Compartilhar a linha de negócios e outras pastas de dados de aplicativo](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Corrigir problemas do computador do cliente após a migração](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Dar ao grupo Administradores interno o direito de fazer logon como um trabalho em lotes](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a> Excluir entradas DNS do servidor de origem  
 Depois de desativar o servidor de origem, o servidor do serviço de nomes de domínio (DNS) ainda pode conter entradas que apontem para o servidor de origem. Exclua essas entradas DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Para excluir as entradas DNS que apontam para o Servidor de Origem  
  
1.  No servidor de destino, abra o **Gerenciador de DNS**.  
  
2.  No Gerenciador DNS, clique com o botão direito no nome do servidor, clique em **Propriedades**e clique na guia **Encaminhadores** .  
  
3.  Determine se há uma entrada na lista de encaminhadores que aponte para o servidor de origem. Se houver, clique em **Editar** e exclua essa entrada na janela **Editar Encaminhadores**.  
  
4.  No **Gerenciador DNS**, expanda o nome do servidor e expanda **Zonas de Pesquisa Direta**.  
  
5.  Para cada zona de pesquisa direta, clique com o botão direito na zona, clique em **Propriedades** e clique na guia **Servidores de Nome**.  
  
6.  Clique em uma entrada na caixa **Servidores de nomes** que aponte para o servidor de origem, clique em **Remover** e clique em **OK**.  
  
7.  Repita as etapas 5 e 6 até que todos os ponteiros para o servidor de origem sejam removidos.  
  
8.  Clique em **OK** para fechar a janela **Propriedades**.  
  
9. No console **Gerenciador DNS**, expanda **Zonas de Pesquisa Inversa**.  
  
10. Repita as etapas de 6 a 9 para remover todas as Zonas de pesquisa inversa que apontam para o Servidor de origem.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> Compartilhar a linha de negócios e outras pastas de dados de aplicativo  
 Você deve definir as permissões da pasta compartilhada e as permissões NTFS para as pastas de dados de linha de negócios e de outros aplicativos que você copiou para o Servidor de Destino. Depois de definir as permissões, as pastas compartilhadas são exibidas no painel do Windows Server Essentials na **armazenamento** seção.  
  
 Se você estiver usando um script de logon para mapear unidades para as pastas compartilhadas, deverá atualizar o script para mapear para as unidades no Servidor de Destino.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> Corrigir problemas do computador do cliente após a migração  
 Se você migrar para o Windows Server Essentials do Windows Small Business Server 2003 Premium Edition com o Microsoft Internet Security e Acceleration (ISA) Server instalado, computadores cliente na rede ainda terá o Microsoft Firewall Client e a Internet Explorer configurado para usar um servidor proxy.  
  
 Isso causa problemas de conectividade nos computadores cliente, pois o servidor proxy não existe mais. Se houver um servidor proxy diferente configurado, os computadores cliente continuarão a usar o servidor executando o Windows SBS 2003 para o servidor proxy. Para corrigir esse problema, reconfigure o Internet Explorer para não usar um servidor proxy ou para usar o novo servidor proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Para reconfigurar o Internet Explorer  
  
1.  No Internet Explorer, clique em **Ferramentas**e clique em **Opções da Internet**.  
  
2.  Clique na guia **Conexões**, clique em **Configurações da LAN** e siga um destes procedimentos:  
  
    -   Se você não estiver usando um servidor proxy na rede, desmarque todas as caixas de seleção na caixa de diálogo **Configurações de Rede Local (LAN)**.  
  
    -   Se você deseja usar um novo servidor proxy na rede:  
  
        1.  Na caixa de diálogo **Configurações da Rede Local (LAN)**, desmarque as caixas de seleção na seção **Configuração Automática**.  
  
        2.  Na seção **Servidor Proxy**, verifique se ambas as caixas de seleção estão marcadas.  
  
        3.  Na caixa **Endereço**, digite o FQDN (nome de domínio totalmente qualificado) do servidor proxy.  
  
        4.  Na caixa **Porta**, digite **80**.  
  
3.  Clique em **OK** duas vezes.  
  
4.  Navegue até um site para garantir que as configurações de conexão estejam corretas.  
  
##  <a name="BKMK_AdminGroup"></a> Dar ao grupo Administradores interno o direito de fazer logon como um trabalho em lotes  
 Depois de migrar um domínio existente do Windows Small Business Server 2003 para o Windows Server Essentials, você deve fornecer o grupo Administradores interno o direito de fazer logon como um trabalho em lotes. Verifique se o grupo Administradores interno ainda tem o direito de logon como um trabalho em lotes para o servidor de destino. Os administradores precisam desse direito para executar um alerta no servidor de destino sem fazer logon.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Para dar ao grupo de Administradores interno o direito de fazer logon como um trabalho em lotes  
  
1.  No servidor de destino, abra a ferramenta administrativa **Gerenciamento de Política de Grupo**.  
  
2.  No **gerenciamento de política de grupo** árvore de Console, expanda **floresta:** *< ServerName\>*, expanda domínios e, em seguida, expanda seu servidor.  
  
3.  Expanda **Controladores de Domínio**, clique com botão direito na **Política de Controladores de Domínio Padrão** e clique em **Editar**.  
  
4.  Na **Editor de gerenciamento de diretiva de grupo**, clique em **política de controladores de domínio padrão ***< ServerName\>*** política**e, em seguida, expanda  **Configuração do computador**.  
  
5.  Expanda **Políticas**, expanda **Configurações do Windows** e então expanda **Configurações de Segurança**.  
  
6.  Na árvore **Configurações de Segurança**, expanda **Políticas Locais** e clique em **Atribuição de Direitos de Usuário**.  
  
7.  No painel de resultados, clique com botão direito em **Fazer logon como um trabalho em lotes** e clique em Propriedades.  
  
8.  Na página **Propriedades de Fazer logon como um trabalho em lotes**, clique em **Adicionar Usuário ou Grupo**.  
  
9. Na caixa de diálogo **Adicionar Usuário ou Grupo**, clique em **Procurar**.  
  
10. Na caixa de diálogo **Selecionar Usuários, Computadores ou Grupos**, digite **Administrators**.  
  
11. Clique em **Verificar Nomes** para verificar se o grupo de administradores interno é exibido e, em seguida, clique em **OK** três vezes para salvar a configuração.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Migrar do Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrar do Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

