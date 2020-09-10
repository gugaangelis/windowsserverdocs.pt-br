---
title: Mover as configurações e dados do Windows SBS 2011 Essentials para o servidor de destino para migração para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5ab8e54fe94fa2f733e28dd461b7d35988589864
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625693"
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover as configurações e dados do Windows SBS 2011 Essentials para o servidor de destino para migração para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e os dados para o servidor de destino da seguinte maneira:


1.  [Copie os dados para o servidor de destino](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_CopyData)

2.  [Importar Active Directory contas de usuário para o painel do Windows Server Essentials (opcional)](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_ImportADaccounts)

3.  [Configurar a rede](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_Network)

4.  [Mapeie os computadores permitidos para as contas de usuário](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_MapPermittedComputers)

##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a> Copiar dados para o servidor de destino
 Antes de copiar os dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:

-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Crie ou personalize as pastas no servidor de destino para corresponderem à estrutura de pasta que você está migrando do servidor de origem.

-   Revise o tamanho de cada pasta e certifique-se de que o servidor de destino tenha espaço de armazenamento suficiente.

-   Torne as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma gravação possa ocorrer na unidade enquanto você estiver copiando arquivos para o servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar os dados do servidor de origem para o servidor de destino

1.  Faça logon no servidor de destino como administrador de domínio e, em seguida, abra uma janela Comando.

2.  No prompt de comando, digite o seguinte comando e pressione ENTER:

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

     Sendo que:
     - \<SourceServerName\> é o nome do servidor de origem
     - \<SharedSourceFolderName\> é o nome da pasta compartilhada no servidor de origem
     - \<DestinationServerName\> é o nome do servidor de destino,
     - \<SharedDestinationFolderName\> é a pasta compartilhada no servidor de destino para o qual os dados serão copiados.

3.  Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem.

##  <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard-optional"></a><a name="BKMK_ImportADaccounts"></a> Importar Active Directory contas de usuário para o painel do Windows Server Essentials (opcional)
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário do Active Directory falhará se nem todas as propriedades atenderem aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar usuários do Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar uma conta de usuário Active Directory para o painel do Windows Server Essentials

1.  Faça logon no servidor de destino como um administrador de domínio.

2.  Abra o Windows PowerShell como administrador.

3.  Execute o seguinte cmdlet, em que `[AD username]` é o nome da conta de usuário do Active Directory que você deseja importar:

     `Import-WssUser  SamAccountName [AD username]`

##  <a name="configure-the-network"></a><a name="BKMK_Network"></a> Configurar a rede

#### <a name="to-configure-the-network"></a>Para configurar a rede

1. No servidor de destino, abra o painel.

2. Na página **Home** do painel, clique em **INSTALAÇÃO**, clique em **Configurar Acesso em Qualquer Local** e escolha a opção **Clique para configurar o Acesso em Qualquer Local**.

3. Siga as instruções no assistente para configurar seu roteador e nomes de domínio.

   Se o roteador não oferecer suporte para a estrutura UPnP, ou se a estrutura UPnP estiver desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que sejam direcionadas para o endereço IP do servidor de destino:

-   Porta 80: tráfego HTTP da Web

-   Porta 443: tráfego HTTPS da Web

##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a> Mapear computadores permitidos para contas de usuário
 Cada conta de usuário é migrada do Windows Small Business Server 2011 Essentials deve ser mapeada para um ou mais computadores.

#### <a name="to-map-user-accounts-to-computers"></a>Para mapear contas de usuário para computadores

1.  Abra o painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na lista de contas de usuário, clique em uma conta de usuário e clique em **Exibir Propriedades da Conta**.

4.  Clique na guia **Acesso em Qualquer Local** e, em seguida, clique em **Permitir Acesso via Web Remoto e acesso a aplicativos de serviços Web**.

5.  Selecione **Pastas Compartilhadas**, selecione **Computadores**, selecione **Links da Home Page** e, em seguida, clique em **Aplicar**.

6.  Clique na guia **Acesso ao Computador** e, em seguida, clique no nome do computador ao qual deseja permitir o acesso.

7.  Repita as etapas 3, 4, 5 e 6 para cada conta de usuário.

> [!NOTE]
>  Você não precisará alterar a configuração do computador cliente. Ela é definida automaticamente.

> [!NOTE]
>  Depois de concluir a migração, se encontrar um problema ao criar a primeira nova conta de usuário no servidor de destino, remova a conta de usuário adicionada e crie a conta novamente.
