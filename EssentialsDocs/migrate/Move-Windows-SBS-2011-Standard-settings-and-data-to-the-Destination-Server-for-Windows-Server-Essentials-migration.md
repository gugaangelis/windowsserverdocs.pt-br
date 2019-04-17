---
title: "Mover dados e configurações do Windows SBS 2011 Standard para a migração do servidor de destino para o Windows Server Essentials"
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
ms.openlocfilehash: b0ee150be2452fdf4c31c6a2a372958640fa5e4a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2011-standard-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover dados e configurações do Windows SBS 2011 Standard para a migração do servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e dados ao servidor de destino, da seguinte maneira:  
  

1.  [Copiar dados ao servidor de destino](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Mover a função de servidor DHCP do servidor de origem para o roteador](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [Configurar a rede](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [Remover herdados Active Directory Group Policy Objects (opcional)](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
6.  [Mapear computadores permitidos para contas de usuário](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copiar dados ao servidor de destino  
 Antes de copiar dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:  
  
-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Criar ou personalizar as pastas no servidor de destino para corresponder a estrutura de pastas que você está migrando do servidor de origem.  
  
-   Analise o tamanho de cada pasta e verifique se o servidor de destino tem espaço de armazenamento suficiente.  
  
-   Verifique as pastas compartilhadas no servidor de origem Read-only para todos os usuários não escrita pode carregar colocar na unidade enquanto você estiver copiando arquivos para o servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar dados do servidor de origem para o servidor de destino  
  
1.  Fazer logon no servidor de destino como administrador do domínio e, em seguida, abra uma janela de comando.  
  
2.  No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Onde:
     - \ < SourceServerName\ > é o nome do servidor de origem
     - \ < SharedSourceFolderName\ > é o nome da pasta compartilhada no servidor de origem
     - \ < DestinationServerName\ > é o nome do servidor de destino,
     - \ < SharedDestinationFolderName\ > é a pasta compartilhada no servidor de destino para o qual os dados serão copiados.  
  
3.  Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)  
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário no Active Directory falhará se algumas propriedades não atender aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar os usuários do Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Para importar uma conta de usuário do Active Directory para o Windows Server Essentials Dashboard  
  
1.  Faça logon no servidor de destino como um administrador de domínio.  
  
2.  Abra o Windows PowerShell como administrador.  
  
3.  Execute o seguinte cmdlet, onde `[AD username]`é o nome da conta de usuário do Active Directory que você deseja importar:  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_MoveDHCP"></a>Mover a função de servidor DHCP do servidor de origem para o roteador  
 Se o servidor de origem está executando a função DHCP, execute as seguintes etapas para mover a função DHCP ao roteador.  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover a função DHCP do servidor de origem para o roteador  
  
1.  Desative o serviço DHCP no servidor de origem, da seguinte maneira:  
  
    1.  No servidor de origem, clique em **iniciar**, clique em **ferramentas administrativas**e clique em **serviços**.  
  
    2.  Na lista de serviços em execução, clique com botão direito **servidor DHCP**e clique em **propriedades**.  
  
    3.  Para **iniciar tipo**, selecione **desabilitada**.  
  
    4.  Interrompa o serviço.  
  
2.  Ative a função DHCP no seu roteador.  
  
    1.  Siga as instruções na documentação do roteador para ativar a função DHCP no roteador.  
  
    2.  Para garantir que os endereços IP emitidos pelo servidor de origem permanecem os mesmos, siga as instruções na documentação do roteador para definir o intervalo DHCP no roteador para ser o mesmo que o intervalo DHCP no servidor de origem.  
  
        > [!IMPORTANT]
        >  Se você não tiver configurado um reservas de DHCP ou de IP estático do roteador para o servidor de destino, e o intervalo DHCP não é o mesmo que o servidor de origem, é possível que o roteador emitirá um novo endereço IP para o servidor de destino. Se isso acontecer, redefina a regras do roteador para encaminhar mensagens para o novo endereço IP do servidor de destino de encaminhamento de porta.  
  
##  <a name="BKMK_Network"></a>Configurar a rede  
 Depois de mover a função DHCP ao roteador, defina as configurações de rede no servidor de destino.  
  
#### <a name="to-configure-the-network"></a>Para configurar a rede  
  
1.  No servidor de destino, abra o painel.  
  
2.  No **Home** página, clique em **instalação**, clique em **configurar o acesso em qualquer lugar**e, em seguida, escolha o **clique para configurar o acesso em qualquer lugar** opção.  
  
3.  Siga as instruções no Assistente para configurar seu roteador e nomes de domínio.  
  
 Se o roteador não der suporte a estrutura UPnP, ou se a estrutura UPnP está desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que eles são direcionados para o endereço IP do servidor de destino:  
  
-   Porta 80: O tráfego HTTP Web  
  
-   Porta 443: O tráfego da Web HTTPS  
  
> [!NOTE]
>  Se você tiver configurado um servidor do Exchange no local em um segundo servidor, você deve garantir porta 25 (SMTP) também é aberta e que ele é redirecionado para o endereço IP do servidor do Exchange no local.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Remover herdados Active Directory Group Policy Objects (opcional)  
 Os objetos de política de grupo (GPOs) são atualizados para o Windows Server Essentials. Eles são um superconjunto dos Windows Small Business Server 2011 GPOs. Para o Windows Server Essentials, um número dos filtros Windows Small Business Server 2011 GPOs e o Windows Management Instrumentation (WMI) deve ser excluído manualmente para evitar conflitos com o Windows Server Essentials GPOs e filtros WMI.  
  
> [!NOTE]
>  Se você tiver modificado os original Windows Small Business Server 2011 GPOs, você deve salvar cópias em um local diferente e, em seguida, exclua-os do Windows Small Business Server 2011.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-small-business-server-2011"></a>Para remover o antigo objetos de política de grupo do Windows Small Business Server 2011  
  
1.  Fazer logon no servidor de origem com uma conta de administrador.  
  
2.  Clique em **iniciar**e clique em **gerenciamento de servidor**.  
  
3.  No painel de navegação, clique em **gerenciamento avançado de**, clique em **Group Policy Management**e clique em **floresta:***< YourDomainName\ >*.  
  
4.  Clique em **domínios**, clique em *< YourDomainName\ >*e clique em **objetos de política de grupo**.  
  
5.  Clique com botão direito **política de auditoria do servidor de negócios pequeno**, clique em **excluir**e clique em **Okey**.  
  
6.  Repita a etapa 5 para excluir os GPOs a seguir que se aplicam à sua rede:  
  
    -   Windows SBS cliente Windows 7 e Windows Vista política  
  
    -   Política do Windows SBS cliente Windows XP  
  
    -   Política do Windows SBS CSE  
  
    -   Política de usuário do Windows SBS  
  
    -   Atualizar a política de computador do cliente de serviços  
  
    -   Atualizar a política de configurações comuns de serviços  
  
    -   Atualizar a política de computador do servidor de serviços  
  
7.  Confirme que todos os GPOs são excluídos.  
  
#### <a name="to-remove-wmi-filters-from-the-source-server"></a>Para remover filtros WMI do servidor de origem  
  
1.  Fazer logon no servidor de origem com uma conta de administrador.  
  
2.  Clique em **iniciar**e clique em **gerenciamento de servidor**.  
  
3.  No painel de navegação, clique em **recursos**, clique em **Group Policy Management**e clique em **floresta:***< YourNetworkDomainName\ >*  
  
4.  Clique em **domínios**, clique em *< YourNetworkDomainName\ >*e clique em **filtros WMI**.  
  
5.  Clique com botão direito **cliente do Windows SBS**, clique em **excluir**e clique em **Sim**.  
  
6.  Clique com botão direito **Windows SBS cliente Windows 7 e Windows Vista**, clique em **excluir**e clique em **Sim**.  
  
7.  Clique com botão direito **Windows SBS cliente Windows XP**, clique em **excluir**e clique em **Sim**.  
  
8.  Confirme que esses três filtros WMI são excluídos.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapear computadores permitidos para contas de usuário  
 No Windows Small Business Server 2011, se um usuário se conecta ao acesso via Web remoto, todos os computadores na rede são exibidos. Isso pode incluir computadores que o usuário não tem permissão para acessar. No Windows Server Essentials, um usuário deve ser explicitamente atribuído a um computador para que ele seja exibido no acesso via Web remoto. Cada conta de usuário que é migrada de Windows Small Business Server 2011 deve ser mapeada para um ou mais computadores.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Mapear as contas de usuário em computadores  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, clique com botão direito uma conta de usuário e, em seguida, clique em **exibir as propriedades da conta**.  
  
4.  Clique no **acesso em qualquer local** guia e, em seguida, clique em **permitir acesso remoto via Web e acesso a aplicativos web services**.  
  
5.  Selecione **pastas compartilhadas**, selecione **computadores**, selecione **links da home page**e clique em **aplicar**.  
  
6.  Clique no **acesso ao computador** guia e, em seguida, clique no nome do computador ao qual você deseja permitir o acesso.  
  
7.  Repita as etapas 3, 4, 5 e 6 para cada conta de usuário.  
  
> [!NOTE]
>  Você não precisa alterar a configuração do computador cliente. Ele é configurado automaticamente.  
  
> [!NOTE]
>  Depois de concluir a migração, se você encontrar um problema quando você cria a primeira nova conta de usuário no servidor de destino, remova a conta de usuário que você adicionou e, em seguida, crie-o novamente.
