---
title: "Etapa 4: Mova as configurações e dados para a migração do servidor de destino para o Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Etapa 4: Mova as configurações e dados para a migração do servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta seção fornece informações sobre migração de dados e as configurações do servidor de origem. Mova as configurações e dados ao servidor de destino, da seguinte maneira:  
  
-   [Copiar dados ao servidor de destino](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurar a rede](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Mapear computadores permitidos para contas de usuário](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copiar dados ao servidor de destino  
 Antes de copiar dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:  
  
-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Criar ou personalizar as pastas no servidor de destino para corresponder a estrutura de pastas que você está migrando do servidor de origem.  
  
-   Analise o tamanho de cada pasta e verifique se o servidor de destino tem espaço de armazenamento suficiente.  
  
-   Verifique as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma escrita pode ser realizados no disco rígido enquanto você estiver copiando arquivos para o servidor de destino.  
  
-   O **Backup do computador cliente** pasta não pode ser migrada para o servidor de destino. Antes da migração do servidor, certifique-se de que todos os computadores cliente são íntegros. Após a migração do servidor, é recomendável que você configure e inicie backups de computador cliente para garantir que o backup dos dados para todos os computadores cliente importantes.  
  
-   O **Backups de histórico de arquivos** pasta não pode ser migrada diretamente para o servidor de destino devido a pasta backup e estrutura metadados alterações no Windows Server Essentials. No entanto, é possível migrar o **Backups de histórico de arquivos** pasta para um determinado usuário em um computador específico. Para fazer isso, você deve localizar o **dados** pasta no **Backups de histórico de arquivos** pasta para esse usuário e computador, em seguida, copie esse **dados** pasta para a **Backups de histórico de arquivos** pasta no servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar dados do servidor de origem para o servidor de destino  
  
1.  Entrar no servidor de destino como administrador do domínio e, em seguida, abra uma janela de Prompt de comando ou em um prompt de comando do Windows PowerShell.  
  
2.  Se você usar a janela do Prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Onde:  
  
    -   \ < SourceServerName\ > é o nome do servidor de origem  
  
    -   \ < SharedSourceFolderName\ > é o nome da pasta compartilhada no servidor de origem  
  
    -   \ < PathOfTheDestination\ > é o caminho absoluto onde você deseja mover a pasta  
  
    -   \ < SharedDestinationFolderName\ > é a pasta no servidor de destino para o qual os dados serão copiados  
  
     Por exemplo, `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Se você usar o Windows PowerShell, digite o seguinte comando e pressione ENTER.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Repita esse processo para cada pasta compartilhada que você está migrando do servidor de origem.  
  
##  <a name="BKMK_Network"></a>Configurar a rede  
  
#### <a name="to-configure-the-network"></a>Para configurar a rede  
  
1.  No servidor de destino, abra o painel.  
  
2.  No painel **Home** página, clique em **instalação**, clique em **configurar o acesso em qualquer lugar**e, em seguida, escolha o **clique para configurar o acesso em qualquer lugar** opção.  
  
3.  Configurar Assistente de acesso em qualquer lugar onde é exibida. Siga as instruções no Assistente para configurar seu roteador e nomes de domínio.  
  
 Se o roteador não der suporte a estrutura UPnP, ou se a estrutura UPnP está desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que eles são direcionados para o endereço IP do servidor de destino:  
  
-   Porta 80: O tráfego HTTP Web  
  
-   Porta 443: O tráfego da Web HTTPS  
  
> [!NOTE]
>  Se você quiser definir um nome de domínio público no servidor de destino, você deve liberar o nome de domínio do seu servidor de origem para evitar concorrência de atualização dinâmica de DNS.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapear computadores permitidos para contas de usuário  
 Cada conta de usuário que é migrada de versões anteriores do Windows Small Business Server ou Windows Server Essentials deve ser mapeada para um ou mais computadores.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Mapear as contas de usuário em computadores  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, clique com botão direito uma conta de usuário e, em seguida, clique em **exibir as propriedades da conta**.  
  
4.  Clique no **acesso em qualquer local** guia e, em seguida, clique em **permitir acesso remoto via Web e acesso a aplicativos web services**.  
  
5.  Clique em **pastas compartilhadas**, clique em**computadores**, clique em **links da home page**e clique em **aplicar**.  
  
6.  Clique no **acesso ao computador** guia e, em seguida, clique no nome do computador ao qual você deseja permitir o acesso.  
  
7.  Repita as etapas 3, 4, 5 e 6 para cada conta de usuário.  
  
> [!NOTE]
>  Você não precisa alterar a configuração do computador cliente. Ele é configurado automaticamente.  
  
> [!NOTE]
>  Depois de concluir a migração, se você encontrar um problema quando você cria a primeira nova conta de usuário no servidor de destino, remova a conta de usuário que você adicionou e, em seguida, crie-o novamente.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você moveu suas configurações e dados ao servidor de destino. Acesse agora [etapa 5: habilitar o redirecionamento de pasta sobre a migração do servidor de destino para o Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

