---
title: 'Etapa 4: Mover configurações e dados para o servidor de destino para migração para o Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 888ecc5c5ab8fd609264f0f184686a144e2f8ce8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625478"
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Etapa 4: Mover configurações e dados para o servidor de destino para migração para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

Esta seção fornece informações sobre a migração de dados e configurações do servidor de origem. Mova as configurações e os dados para o servidor de destino da seguinte maneira:

-   [Copie os dados para o servidor de destino](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)

-   [Configurar a rede](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)

-   [Mapeie os computadores permitidos para as contas de usuário](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)

##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a> Copiar dados para o servidor de destino
 Antes de copiar os dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:

-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Crie ou personalize as pastas no servidor de destino para corresponderem à estrutura de pasta que você está migrando do servidor de origem.

-   Revise o tamanho de cada pasta e certifique-se de que o servidor de destino tenha espaço de armazenamento suficiente.

-   Tornar as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma gravação possa ocorrer na unidade enquanto você estiver copiando arquivos para o servidor de destino.

-   A pasta **Backup do computador cliente** não pode ser migrada para o servidor de destino. Antes da migração de servidor, certifique-se da integridade de todos os computadores cliente. Após a migração de servidor, é recomendável que você configure e inicie os backups de computador cliente, para garantir o backup dos dados para todos os computadores cliente importantes.

-   A pasta **backups do histórico de arquivos** não pode ser migrada diretamente para o servidor de destino devido à estrutura de pastas e às alterações de metadados de backup no Windows Server Essentials. No entanto, é possível migrar a pasta **Backups do histórico de arquivos** para um usuário específico em um computador específico. Para fazer isso, você deve localizar a pasta **Dados** na pasta **Backups do histórico de arquivos** para esse usuário e computador, depois copiar essa pasta **Dados** para a pasta **Backups do histórico de arquivos** no servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar os dados do servidor de origem para o servidor de destino

1. Autentique-se no servidor de destino como um administrador de domínio e, em seguida, abra uma janela de Prompt de comando ou um prompt de comando do Windows PowerShell.

2. Se você usar a janela de Prompt de Comando, digite o seguinte comando e pressione ENTER:

   `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

    Sendo que:

   - \<SourceServerName\> é o nome do servidor de origem

   - \<SharedSourceFolderName\> é o nome da pasta compartilhada no servidor de origem

   - \<PathOfTheDestination\> é o caminho absoluto onde você deseja mover a pasta

   - \<SharedDestinationFolderName\> é a pasta no servidor de destino para o qual os dados serão copiados.

     Por exemplo, `robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.

3. Se você usar o Windows PowerShell, digite o comando a seguir e pressione ENTER.

    `Add-Wssfolder  Path \ -Name  -KeepPermission`

4. Repita esse processo para cada pasta compartilhada que você está migrando do servidor de origem.

##  <a name="configure-the-network"></a><a name="BKMK_Network"></a> Configurar a rede

#### <a name="to-configure-the-network"></a>Para configurar a rede

1. No servidor de destino, abra o painel.

2. Na página **Início** do painel, clique em **Instalação**, clique em **Configurar Acesso em Qualquer Local** e escolha a opção **Clique para configurar o Acesso em Qualquer Local**.

3. O assistente de configuração do Acesso em Qualquer Local é exibido. Siga as instruções no assistente para configurar seu roteador e nomes de domínio.

   Se o roteador não oferecer suporte para a estrutura UPnP, ou se a estrutura UPnP estiver desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que sejam direcionadas para o endereço IP do servidor de destino:

-   Porta 80: tráfego HTTP da Web

-   Porta 443: tráfego HTTPS da Web

> [!NOTE]
>  Se você deseja configurar um nome de domínio público no servidor de destino, você deve liberar o nome de domínio do seu servidor de origem para evitar competição da atualização dinâmica de DNS.

##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a> Mapear computadores permitidos para contas de usuário
 Cada conta de usuário migrada de versões anteriores do Windows Small Business Server ou Windows Server Essentials deve ser mapeada para um ou mais computadores.

#### <a name="to-map-user-accounts-to-computers"></a>Para mapear contas de usuário para computadores

1.  Abra o painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na lista de contas de usuário, clique em uma conta de usuário e clique em **Exibir Propriedades da Conta**.

4.  Clique na guia **Acesso em Qualquer Local** e, em seguida, clique em **Permitir Acesso via Web Remoto e acesso a aplicativos de serviços Web**.

5.  Clique em **Pastas Compartilhadas**, clique em **Computadores**, clique em **Links da Página Inicial** e clique em **Aplicar**.

6.  Clique na guia **Acesso ao Computador** e, em seguida, clique no nome do computador ao qual deseja permitir o acesso.

7.  Repita as etapas 3, 4, 5 e 6 para cada conta de usuário.

> [!NOTE]
>  Você não precisará alterar a configuração do computador cliente. Ela é definida automaticamente.

> [!NOTE]
>  Depois de concluir a migração, se encontrar um problema ao criar a primeira nova conta de usuário no servidor de destino, remova a conta de usuário adicionada e crie a conta novamente.

## <a name="next-steps"></a>Próximas etapas
 Você moveu suas configurações e dados para o servidor de destino. Agora, vá para a [etapa 5: habilitar o redirecionamento de pasta no servidor de destino para a migração do Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).


Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

