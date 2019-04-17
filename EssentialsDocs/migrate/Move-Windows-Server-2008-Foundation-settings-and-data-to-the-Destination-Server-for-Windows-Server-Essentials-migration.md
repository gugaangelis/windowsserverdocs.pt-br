---
title: "Mover dados e configurações do Windows Server 2008 Foundation para a migração do servidor de destino para o Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ff7d040-ebd1-421c-80db-765deacedd4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 80e70ba954d981985caa5329379661d978456c7f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-server-2008-foundation-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover dados e configurações do Windows Server 2008 Foundation para a migração do servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Mova as configurações e dados ao servidor de destino, da seguinte maneira: 

1.  [Copiar dados no servidor de destino (opcional)](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Mover a função de servidor DHCP do servidor de origem para o roteador](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [Configurar a rede](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [Mapear computadores permitidos para contas de usuário](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copiar dados no servidor de destino (opcional)  
 Antes de copiar dados do servidor de origem para o servidor de destino, execute as seguintes tarefas:  
  
-   Examine a lista de pastas compartilhadas no servidor de origem, incluindo as permissões para cada pasta. Criar ou personalizar as pastas no servidor de destino para corresponder a estrutura de pastas que você está migrando do servidor de origem.  
  
-   Analise o tamanho de cada pasta e verifique se o servidor de destino tem espaço de armazenamento suficiente.  
  
-   Verifique as pastas compartilhadas no servidor de origem Read-only para todos os usuários não escrita pode carregar colocar na unidade enquanto você estiver copiando arquivos para o servidor de destino.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar dados do servidor de origem para o servidor de destino  
  
1.  Entrar servidor de destino como administrador do domínio e, em seguida, abra uma janela de comando.  
  
2.  No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Onde:
     - \ < SourceServerName\ > é o nome do servidor de origem
     - \ < SharedSourceFolderName\ > é o nome da pasta compartilhada no servidor de origem
     - \ < DestinationServerName\ > é o nome do servidor de destino,
     - \ < SharedDestinationFolderName\ > é a pasta compartilhada no servidor de destino para o qual os dados serão copiados.  
  
3.  Repita a etapa anterior para cada pasta compartilhada que você está migrando do servidor de origem.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importar contas de usuário do Active Directory para Windows Server Essentials Dashboard (opcional)  
 Por padrão, todas as contas de usuário criadas no servidor de origem são migradas automaticamente para o painel no Windows Server Essentials. No entanto, a migração automática de uma conta de usuário no Active Directory falhará se todas as propriedades não atender aos requisitos de migração. Você pode usar o seguinte cmdlet do Windows PowerShell para importar os usuários do Active Directory.  
  
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
  
2.  No painel **Home** página, clique em **instalação**, clique em **configurar o acesso em qualquer lugar**e, em seguida, escolha o **clique para configurar o acesso em qualquer lugar** opção.  
  
3.  Siga as instruções no Assistente para configurar seu roteador e nomes de domínio.  
  
 Se o roteador não der suporte a estrutura UPnP, ou se a estrutura UPnP está desabilitada, um ícone de aviso amarelo pode aparecer ao lado do nome do roteador. Certifique-se de que as seguintes portas estejam abertas e que eles são direcionados para o endereço IP do servidor de destino:  
  
-   Porta 80: O tráfego HTTP Web  
  
-   Porta 443: O tráfego da Web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapear computadores permitidos para contas de usuário  
 No Windows Server Essentials, um usuário deve ser explicitamente atribuído a um computador para que ele seja exibido no acesso via Web remoto. Cada conta de usuário migrada do Windows Server 2008 Foundation deve ser mapeada para um ou mais computadores.  
  
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
