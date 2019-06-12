---
title: Mover as configurações e dados do Windows SBS 2011 Standard para o servidor de destino para migração para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b24026-2fe3-4bd0-b82f-900e1564be99
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd57e5d00093861686c7a6ed4ef4096fd5cdf0c4
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828570"
---
# <a name="move-windows-sbs-2011-standard-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover as configurações e dados do Windows SBS 2011 Standard para o servidor de destino para migração para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e os dados para o servidor de destino da seguinte maneira: 
 
1. [Copiar dados para o servidor de destino](#copy-data-to-the-destination-server)

2. [Importar contas de usuário do Active Directory para o painel do Windows Server Essentials (opcional)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Mover a função de servidor DHCP do servidor de origem para o roteador](#move-the-dhcp-server-role-from-the-source-server-to-the-router)

4. [Configurar a rede](#configure-the-network)

5. [Remover objetos de diretiva de grupo do Active Directory herdados (opcionais)](#remove-legacy-active-directory-group-policy-objects)

6. [Mapear os computadores permitidos para contas de usuário](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copie os dados para o servidor de destino
 Antes de copiar os dados do servidor de origem para o servidor de destino, execute as seguintes tarefas: 

- Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Crie ou personalize as pastas no servidor de destino para corresponderem à estrutura de pasta que você está migrando do servidor de origem. 

- Revise o tamanho de cada pasta e certifique-se de que o servidor de destino tenha espaço de armazenamento suficiente. 

- Torne as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma gravação possa ocorrer na unidade enquanto você estiver copiando arquivos para o servidor de destino. 

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar os dados do servidor de origem para o servidor de destino 

1. Faça logon no servidor de destino como administrador de domínio e, em seguida, abra uma janela Comando. 

2. No prompt de comando, digite o seguinte comando e pressione ENTER: 

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
 
 Onde:
 - \<SourceServerName\> é o nome do servidor de origem
 - \<Nomedapastacompartilhadadeorigem\> é o nome da pasta compartilhada no servidor de origem
 - \<Nomeservidordestino\> é o nome do servidor de destino,
 - \<Nomedapastacompartilhadadedestino\> é a pasta compartilhada no servidor de destino para o qual os dados serão copiados. 

3. Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem. 

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importar contas de usuário do Active Directory para o painel do Windows Server Essentials
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário do Active Directory falhará se algumas propriedades não atenderem aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar usuários do Active Directory. 
 
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar uma conta de usuário do Active Directory para o painel do Windows Server Essentials
 
1. Faça logon no servidor de destino como um administrador de domínio. 
 
2. Abra o Windows PowerShell como administrador. 
 
3. Execute o seguinte cmdlet, em que `[AD username]` é o nome da conta de usuário do Active Directory que você deseja importar: 
 
 `Import-WssUser SamAccountName [AD username]` 
 
## <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a>Transfira a função de servidor DHCP do servidor de origem para o roteador
 Se o servidor de origem estiver executando a função de DHCP, execute as seguintes etapas para mover a função DHCP para o roteador. 
 
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover a função DHCP do servidor de origem para o roteador 
 
1. Desative o serviço DHCP no servidor de origem da seguinte maneira: 

    1. No servidor de origem, clique em **Iniciar**, clique em **Ferramentas Administrativas** e clique em **Serviços**.

    2. Na lista de serviços em execução, clique com o botão direito em **Servidor DHCP** e clique em **Propriedades**.

    3. Para **Tipo de Início**, selecione **Desabilitado**.

    4. Interrompa o serviço.

2. Ative a função DHCP no roteador.

    1. Siga as instruções na documentação do roteador para ativar a função DHCP no roteador.

    2. Para garantir que os endereços IP emitidos pelo servidor de origem permaneçam os mesmos, siga as instruções na documentação do seu roteador para configurar o intervalo DHCP no roteador para que seja igual ao intervalo DHCP no servidor de origem.

 > [!IMPORTANT]
 > Se você não tiver configurado reservas de DHCP ou IP estático no roteador para o servidor de destino e o intervalo DHCP não for igual ao do servidor de origem, é possível que o roteador emita um novo endereço IP para o servidor de destino. Se isso acontecer, redefina as regras do roteador para encaminhar para o novo endereço IP do servidor de destino. 
 
## <a name="configure-the-network"></a>Configurar a rede
 Depois que você mover a função DHCP no roteador, defina as configurações de rede no servidor de destino. 
 
#### <a name="to-configure-the-network"></a>Para configurar a rede 
 
1. No servidor de destino, abra o painel. 
 
2. Na página **Home** do painel, clique em **INSTALAÇÃO**, clique em **Configurar Acesso em Qualquer Local** e escolha a opção **Clique para configurar o Acesso em Qualquer Local**. 
 
3. Siga as instruções no assistente para configurar seu roteador e nomes de domínio. 
 
 Se o roteador não oferecer suporte para a estrutura UPnP, ou se a estrutura UPnP estiver desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que sejam direcionadas para o endereço IP do servidor de destino: 
 
- Porta 80: Tráfego da Web HTTP 
 
- Porta 443: Tráfego da Web HTTPS 
 
> [!NOTE]
> Se você tiver configurado um servidor do Exchange local em um segundo servidor, deve garantir que a porta 25 (SMTP) também esteja aberta e que seja redirecionada para o endereço IP do servidor do Exchange local. 
 
## <a name="remove-legacy-active-directory-group-policy-objects"></a>Remover herdados objetos de diretiva de grupo do Active Directory
 Os objetos de diretiva de grupo (GPOs) são atualizados para o Windows Server Essentials. Eles são um subconjunto dos GPOs do Windows Small Business Server 2011. Para o Windows Server Essentials, um número de filtros de GPOs do Windows Small Business Server 2011 e do Windows Management Instrumentation (WMI) deve ser excluído para evitar conflitos com os filtros WMI e GPOs do Windows Server Essentials. 
 
> [!NOTE]
> Se você tiver modificado os Objetos de Política de Grupo do Windows SBS 2011 originais, deverá salvar uma cópia em um local diferente e, em seguida, excluí-los do Windows SBS 2011. 
 
#### <a name="to-remove-old-group-policy-objects-from-windows-small-business-server-2011"></a>Para remover objetos de Política de Grupo antigos do Windows Small Business Server 2011 
 
1. Faça logon no servidor de origem com uma conta de administrador. 
 
2. Clique em **Iniciar** e, em seguida, em **Gerenciamento de servidor**. 
 
3. No painel de navegação, clique em **avançadas de gerenciamento**, clique em **gerenciamento de política de grupo**e, em seguida, clique em **floresta: * * * < nome_do_domínio\>* . 
 
4. Clique em **domínios**, clique em *< nome_do_domínio\>* e, em seguida, clique em **os objetos de diretiva de grupo**. 
 
5. Clique com o botão direito do mouse em **Política de auditoria do Small Business Server**, clique em **Excluir** e, em seguida, clique em **OK**. 
 
6. Repita a etapa 5 para excluir os seguintes GPOs que se aplicam à sua rede: 
 
 - Windows SBS Client Windows 7 e Windows Vista política 
 
 - Política do Windows SBS cliente Windows XP 
 
 - Política do Windows SBS CSE 
 
 - Política de usuário do Windows SBS 
 
 - Atualizar política de computador cliente de serviços 
 
 - Atualizar a política de configurações comuns de serviços 
 
 - Atualizar política de computador do servidor de serviços 
 
7. Confirme se todos os GPOs foram excluídos. 
 
#### <a name="to-remove-wmi-filters-from-the-source-server"></a>Para remover os filtros WMI do servidor de origem 
 
1. Faça logon no servidor de origem com uma conta de administrador. 
 
2. Clique em **Iniciar** e, em seguida, em **Gerenciamento de servidor**. 
 
3. No painel de navegação, clique em **recursos**, clique em **gerenciamento de política de grupo**e, em seguida, clique em **floresta: * * * < YourNetworkDomainName\>* 
 
4. Clique em **domínios**, clique em *< YourNetworkDomainName\>* e, em seguida, clique em **filtros WMI**. 
 
5. Clique com botão direito **Windows SBS Client**, clique em **Excluir**e clique em **Sim**. 
 
6. Clique com botão direito **Windows SBS Client Windows 7 e Windows Vista**, clique em **excluir**e, em seguida, clique em **Sim**. 
 
7. Clique com botão direito **Windows SBS Client Windows XP**, clique em **excluir**e, em seguida, clique em **Sim**. 
 
8. Confirme se esses três filtros WMI foram excluídos. 
 
## <a name="map-permitted-computers-to-user-accounts"></a>Mapeie os computadores permitidos para as contas de usuário
 No Windows Small Business Server 2011, se um usuário se conecta ao Acesso Remoto via Web, todos os computadores da rede são exibidos. Isso pode incluir computadores que o usuário não tem permissão para acessar. No Windows Server Essentials, um usuário deve ser explicitamente atribuído a um computador para que ele seja exibido no acesso via Web remoto. Cada conta de usuário que for migrada do Windows Small Business Server 2011 deve ser mapeada para um ou mais computadores. 
 
#### <a name="to-map-user-accounts-to-computers"></a>Para mapear contas de usuário para computadores 
 
1. Abra o Painel do Windows Server Essentials. 
 
2. Na barra de navegação, clique em **Usuários**. 
 
3. Na lista de contas de usuário, clique em uma conta de usuário e clique em **Exibir Propriedades da Conta**. 
 
4. Clique na guia **Acesso em Qualquer Local** e, em seguida, clique em **Permitir Acesso via Web Remoto e acesso a aplicativos de serviços Web**. 
 
5. Selecione **Pastas Compartilhadas**, selecione **Computadores**, selecione **Links da Home Page**e, em seguida, clique em **Aplicar**. 
 
6. Clique na guia **Acesso ao Computador** e, em seguida, clique no nome do computador ao qual deseja permitir o acesso. 
 
7. Repita as etapas 3, 4, 5 e 6 para cada conta de usuário. 
 
> [!NOTE]
> Você não precisará alterar a configuração do computador cliente. Ela é definida automaticamente. 
>
> Depois de concluir a migração, se encontrar um problema ao criar a primeira nova conta de usuário no servidor de destino, remova a conta de usuário adicionada e crie a conta novamente.
