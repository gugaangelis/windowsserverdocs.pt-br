---
title: Mover configurações e dados do Windows SBS 2003 para o servidor de destino para migração para o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9fd9cdfaea641a0aee615befb5d400fa45160d97
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828550"
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover configurações e dados do Windows SBS 2003 para o servidor de destino para migração para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e os dados para o servidor de destino da seguinte maneira:

1. [Copiar dados para o servidor de destino](#copy-data-to-the-destination-server)

2. [Importar contas de usuário do Active Directory para o painel do Windows Server Essentials (opcional)](#import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard)

3. [Remover os scripts de logon antigo (opcionais)](#remove-old-logon-scripts)

4. [Remover herdados Active Directory Group Policy Objects (opcional)](#remove-legacy-active-directory-group-policy-objects) 

5. [Configurar a rede](#configure-the-network) 

6. [Mapear os computadores permitidos para contas de usuário](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copie os dados para o servidor de destino
Antes de copiar os dados do servidor de origem para o servidor de destino, execute as seguintes tarefas: 

- Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Crie ou personalize as pastas no servidor de destino para corresponderem à estrutura de pasta que você está migrando do servidor de origem. 

- Revise o tamanho de cada pasta e certifique-se de que o servidor de destino tenha espaço de armazenamento suficiente. 

- Torne as pastas compartilhadas no servidor de origem somente leitura para todos os usuários para que nenhuma gravação possa ocorrer na unidade enquanto você estiver copiando arquivos para o servidor de destino. 

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar os dados do servidor de origem para o servidor de destino 

1. Faça logon no servidor de destino como um administrador de domínio. 

2. Clique em **Iniciar**, digite **cmd** na caixa e pesquisa e pressione ENTER. 

3. No prompt de comando, digite o seguinte comando e pressione ENTER: 

    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt` 

Onde:
 - \<SourceServerName\> é o nome do servidor de origem
 - \<Nomedapastacompartilhadadeorigem\> é o nome da pasta compartilhada no servidor de origem
 - \<Nomeservidordestino\> é o nome do servidor de destino,
 - \<Nomedapastacompartilhadadedestino\> é a pasta compartilhada no servidor de destino para o qual os dados serão copiados. 

4. Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem.

## <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard"></a>Importar contas de usuário do Active Directory para o painel do Windows Server Essentials
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário do Active Directory falhará se algumas propriedades não atenderem aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar usuários do Active Directory.

#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar uma conta de usuário do Active Directory para o painel do Windows Server Essentials

1. Faça logon no servidor de destino como um administrador de domínio.

2. Abra o Windows PowerShell como administrador.

3. Execute o seguinte cmdlet, em que `[AD username]` é o nome da conta de usuário do Active Directory que você deseja importar:

    `Import-WssUser SamAccountName [AD username]`

## <a name="remove-old-logon-scripts"></a>Remover os scripts de logon antigo
O Windows SBS 2003 usa scripts de logon para tarefas como instalar o software e personalizar áreas de trabalho. Windows Server Essentials substitui os scripts de logon do Windows SBS 2003 com uma combinação de scripts de logon e objetos de diretiva de grupo.

> [!NOTE]
> Se você tiver modificado os scripts de logon do Windows SBS 2003, deve renomear os scripts para preservar suas personalizações.
>
> Scripts de logon do Windows SBS 2003 aplicam-se apenas a contas de usuário adicionadas usando o **Assistente para Adicionar Novos Usuários**.

#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Para remover os scripts de logon do Windows SBS 2003 

1. Clique em **Iniciar**, aponte para **Ferramentas Administrativas**e clique em **Usuários e Computadores do Active Directory**.

2. Em **Usuários e Computadores do Active Directory**, expanda a rede e, em seguida, clique em **Usuários**.

3. Clique com o botão direito no nome de usuário, clique em **Propriedades**e clique na guia **Perfil** .

4. Exclua o conteúdo da caixa de texto **Script de Logon** e clique em **OK**.

5. Repita as etapas 3 e 4 para cada usuário.

## <a name="remove-legacy-active-directory-group-policy-objects"></a>Remover herdados objetos de diretiva de grupo do Active Directory
Os objetos de diretiva de grupo (GPOs) são atualizados para o Windows Server Essentials. Eles são um subconjunto dos GPOs Windows SBS 2003. Para o Windows Server Essentials, um número de filtros de GPOs do Windows SBS 2003 e Windows Management Instrumentation (WMI) deve ser excluído para evitar conflitos com os filtros WMI e GPOs do Windows Server Essentials. 

> [!NOTE]
> Se você tiver modificado os Objetos de Política de Grupo do Windows SBS 2003 originais, salve uma cópia em um local diferente e, em seguida, exclua-os do Windows SBS 2003.

#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Para remover Objetos de Política de Grupo antigos do Windows SBS 2003 

1. Faça logon no servidor de origem com uma conta de administrador. 

2. Clique em **Iniciar** e, em seguida, em **Gerenciamento de servidor**. 

3. No painel de navegação, clique em **avançadas de gerenciamento**, clique em **gerenciamento de política de grupo**e, em seguida, clique em **floresta: * * * < nome_do_domínio\>* . 

4. Clique em **domínios**, clique em *< nome_do_domínio\>* e, em seguida, clique em **os objetos de diretiva de grupo**. 

5. Clique com o botão direito do mouse em **Política de auditoria do Small Business Server**, clique em **Excluir** e, em seguida, clique em **OK**. 

6. Repita a etapa 5 para excluir os seguintes GPOs que se aplicam à sua rede: 

 - Computador cliente do Small Business Server 

 - Política de senha de domínio do Small Business Server 

É recomendável que você configure a política de senha no Windows Server Essentials para impor senhas fortes. Para configurar a política de senha, use o painel, que grava a configuração de política de domínio padrão. A configuração de política de senha não é gravada no Objeto de Política de Senha do Domínio do Small Business Server, como ocorria no Windows SBS 2003. 

 - Firewall de conexão de Internet do Small Business Server 

 - Política de bloqueio do Small Business Server 

 - Política de assistência remota do Small Business Server 

 - Firewall do Windows Small Business Server 

 - Política de computador cliente do Small Business Server Update Services 

 Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2. 

 - Política de configurações Comuns do Small Business Server Update Services 

 Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2. 
 
 - Política de Computador do Servidor do Small Business Server Update Services 
 
 Esse GPO estará presente se você estiver migrando do Windows SBS 2003 R2.

7. Confirme se todos os GPOs foram excluídos.

#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Para remover os filtros WMI do Windows SBS 2003

1. Faça logon no servidor de origem com uma conta de administrador.

2. Clique em **Iniciar** e, em seguida, em **Gerenciamento de servidor**.

3. No painel de navegação, clique em **avançadas de gerenciamento**, clique em **gerenciamento de política de grupo**e, em seguida, clique em **floresta: * * * < YourNetworkDomainName\>*

4. Clique em **domínios**, clique em *< YourNetworkDomainName\>* e, em seguida, clique em **filtros WMI**.

5. Clique com o botão direito em **PostSP2**, clique em **Excluir** e, em seguida, clique em **Sim**.

6. Clique com o botão direito em **PreSP2**, clique em **Excluir**e clique em **Sim**.

7. Confirme se esses três filtros WMI foram excluídos.

## <a name="configure-the-network"></a>Configurar a rede

#### <a name="to-configure-the-network"></a>Para configurar a rede 

1. No servidor de destino, abra o painel. 

2. Na página **Home** do painel, clique em **INSTALAÇÃO**, clique em **Configurar Acesso em Qualquer Local**e escolha a opção **Clique para configurar o Acesso em Qualquer Local** . 

3. Siga as instruções do assistente **Configurar Acesso em Qualquer Local** para configurar o roteador e o nome de domínio.

 Se o roteador não oferecer suporte para a estrutura UPnP, ou se a estrutura UPnP estiver desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que sejam direcionadas para o endereço IP do servidor de destino:

- Porta 80: Tráfego da Web HTTP

- Porta 443: Tráfego da Web HTTPS

> [!NOTE]
> Se você tiver configurado um servidor do Exchange local em um segundo servidor, deve garantir que a porta 25 (SMTP) também esteja aberta e que seja redirecionada para o endereço IP do servidor do Exchange local.

## <a name="map-permitted-computers-to-user-accounts"></a>Mapeie os computadores permitidos para as contas de usuário
 No Windows SBS 2003, se um usuário se conecta ao acesso via Web remoto, todos os computadores da rede são exibidos. Isso pode incluir computadores que o usuário não tem permissão para acessar. No Windows Server Essentials, um usuário deve ser explicitamente atribuído a um computador para que ele seja exibido no acesso via Web remoto. Cada conta de usuário que for migrada do Windows SBS 2003 deve ser mapeada para um ou mais computadores. 

#### <a name="to-map-user-accounts-to-computers"></a>Para mapear contas de usuário para computadores 

1. No servidor de destino, abra o painel do Windows Server Essentials. 

2. Na barra de navegação, clique em **Usuários**. 

3. Na lista de contas de usuário, clique em uma conta de usuário e clique em **Exibir Propriedades da Conta**. 

4. Clique na guia **Acesso em Qualquer Local** e, em seguida, clique em **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web**. 

5. Clique em **Pastas Compartilhadas**, clique em **Computadores**, clique em **Links da Home Page** e clique em **Aplicar**. 

6. Clique na guia **Acesso ao computador** e clique no nome do computador ao qual você deseja permitir o acesso. 

7. Repita as etapas 3, 4, 5 e 6 para cada conta de usuário. 

> [!NOTE]
> Você não precisará alterar a configuração do computador cliente. Ela é definida automaticamente.
>
> Depois de concluir a migração, se encontrar um problema ao criar a primeira nova conta de usuário no servidor de destino, remova a conta de usuário adicionada e crie a conta novamente.
