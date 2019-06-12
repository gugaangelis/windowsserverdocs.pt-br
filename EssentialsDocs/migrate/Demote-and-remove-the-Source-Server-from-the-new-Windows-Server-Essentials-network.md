---
title: Rebaixar e remover o servidor de origem do network1 novos do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e5bcdd58f4d88f7a555151d755bf427ecc9b5108
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433005"
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Rebaixar e remover o servidor de origem do network1 novos do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Depois de concluir a instalação do Windows Server Essentials e você concluir as tarefas no Assistente de migração, você deve executar as seguintes tarefas:  
  

1.  [Desinstalar o Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconectar impressoras diretamente conectadas ao servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rebaixar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover a função de servidor DHCP do servidor de origem para o roteador](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Remover e realocar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Desinstalar o Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconectar impressoras diretamente conectadas ao servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rebaixar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover a função de servidor DHCP do servidor de origem para o roteador](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Remover e realocar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a> Uninstall Exchange Server 2003  
  
> [!IMPORTANT]
>  Se você adicionar contas de usuário depois de mover as caixas de correio ao servidor de destino e antes de desinstalar o Exchange Server 2003 do servidor de origem, as caixas de correio são adicionadas no servidor de origem. Isso ocorre por design. Você deve mover as caixas de correio para o servidor de destino para todas as contas de usuário adicionadas durante esse período. Repita as instruções em caixas de correio de mover o Exchange Server e as configurações de migração do Windows Server Essentials antes de desinstalar o Exchange Server 2003.  
  
 Você deve desinstalar o Exchange Server 2003 do servidor de origem antes de rebaixá-lo. Isso remove todas as referências nos serviços de domínio Active Directory (AD DS) para o Exchange Server no servidor de origem. Você deve ter sua mídia do Windows Small Business Server 2003 para remover o Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Para desinstalar o Exchange Server 2003 do servidor de origem  
  
1. Faça logon no servidor de origem como administrador  
  
2. Clique em **Iniciar**, clique em **Painel de Controle** e clique em **Adicionar ou Remover Programas**.  
  
3. Na lista de programas, selecione **Windows Small Business Server 2003**e, em seguida, clique em **Alterar/remover**.  
  
4. No Assistente de instalação, clique em **Avançar** até a página **Seleção do Componente** ser exibida.  
  
5. Na página de seleção de componente, expanda **Exchange Server** e escolha **Remover**.  
  
   > [!NOTE]
   > 
   >  O Exchange Server verifica se não há caixas de correio ou pastas públicas no servidor. Se restar algum dado, uma mensagem de erro aparece ao clicar **Remover**. Para evitar esse problema, certifique-se de que você concluiu todos os procedimentos no tópico [mover SBS 2003 configurações e dados para o servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
   > 
   >  O Exchange Server verifica se não há caixas de correio ou pastas públicas no servidor. Se restar algum dado, uma mensagem de erro aparece ao clicar **Remover**. Para evitar esse problema, certifique-se de que você concluiu todos os procedimentos no tópico [mover SBS 2003 configurações e dados para o servidor de destino](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6. Clique em **Avançar**.  
  
7. Quando solicitado, insira o Windows Small Business Server 2003 CD #3 e siga as instruções na tela.  
  
###  <a name="BKMK_PhysicallyDisconnect"></a> Desconectar impressoras diretamente conectadas ao servidor de origem  
 Antes de rebaixar o servidor de origem, desconecte fisicamente todas as impressoras diretamente conectadas ao servidor de origem e compartilhadas por meio do servidor de origem. Garanta que nenhum objeto do Active Directory permaneça para as impressoras estiverem diretamente conectadas ao servidor de origem. As impressoras podem ser diretamente conectadas ao servidor de destino e compartilhadas do Windows Server Essentials.  
  
###  <a name="BKMK_DemoteTheSourceServer"></a> Rebaixar o servidor de origem  
 Antes de rebaixar o servidor de origem da função de controlador de domínio AD DS para a função de um servidor membro de domínio, certifique-se de que as configurações de política de grupo tenham sido aplicadas a todos os computadores cliente, conforme descrito no procedimento a seguir.  
  
> [!IMPORTANT]
>  O servidor de origem e o servidor de destino devem estar conectados à rede enquanto as alterações de política de grupo são atualizadas nos computadores cliente.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forçar uma atualização de Política de Grupo em um computador cliente  
  
1.  Faça logon no computador cliente como administrador.  
  
2.  Abra uma janela de Prompt de Comando como administrador.  
  
3.  No prompt de comando, digite **gpupdate /force** e pressione ENTER.  
  
4.  O processo pode exigir fazer logoff e logon novamente para ser concluído. Clique em **Sim** para confirmar.  
  
##### <a name="to-demote-the-source-server"></a>Para rebaixar o servidor de origem  
  
1. No servidor de origem, clique em **Iniciar**, clique em **Executar**, digite **dcpromo** e clique em **OK**.  
  
2. Clique em **Avançar** duas vezes.  
  
   > [!NOTE]
   >  Não selecione **Este servidor é o último controlador de domínio no domínio**.  
  
3. Digite uma senha para a nova conta de administrador no servidor e, em seguida, clique em **Avançar**.  
  
4. No **resumo** caixa de diálogo, você será informado de que o AD DS será removido do computador e que o servidor se tornará um membro do domínio. Clique em **Avançar**.  
  
5. Clique em **concluir**. O servidor de origem reinicia.  
  
6. Depois de reiniciar o servidor de origem, adicione-o como membro de um grupo de trabalho antes de desconectá-lo da rede.  
  
   Depois de adicionar o servidor de origem como um membro de um grupo de trabalho e desconectá-lo da rede, remova-o do AD DS no servidor de destino.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Para remover o servidor de origem do Active Directory  
  
1.  No servidor de destino, abra **Usuários e Computadores do Active Directory**.  
  
2.  No painel de navegação **Usuários e Computadores do Active Directory** , expanda o nome do domínio e expanda **Computadores**.  
  
3.  Clique com o botão direito no nome do servidor de origem se ele ainda existir na lista de servidores, clique em **Excluir** e clique em **Sim**.  
  
4.  Verifique se o servidor de origem não está listado e, em seguida, feche **Usuários e Computadores do Active Directory**.  
  
###  <a name="BKMK_MoveTheDHCPRole"></a> Mover a função de servidor DHCP do servidor de origem para o roteador  
  
> [!NOTE]
> 
>  Se você tiver executado essa tarefa antes de iniciar o processo de migração, continue com a seção [Remover e realocar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  
> 
>  Se você tiver executado essa tarefa antes de iniciar o processo de migração, continue com a seção [Remover e realocar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Se o servidor de origem estiver executando a função de DHCP, execute as seguintes etapas para mover a função DHCP para o roteador.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover a função DHCP do servidor de origem para o roteador  
  
1.  Desative o serviço DHCP no servidor de origem da seguinte maneira:  
  
    1.  No servidor de origem, clique em **Iniciar**, clique em **Ferramentas Administrativas**e clique em **Serviços**.  
  
    2.  Na lista de serviços em execução, com o botão direito do **Windows Server**e clique em **Propriedades**.  
  
    3.  Para **Tipo de Início**, selecione **Desabilitado**.  
  
    4.  Interrompa o serviço.  
  
2.  Ative a função DHCP no roteador  
  
    1.  Siga as instruções na documentação do roteador para ativar a função DHCP no roteador.  
  
    2.  Para garantir que os endereços IP emitidos pelo servidor de origem permaneçam os mesmos, siga as instruções na documentação do seu roteador para configurar o intervalo DHCP no roteador para que seja igual ao intervalo DHCP no servidor de origem.  
  
    > [!IMPORTANT]
    >  Se você não tiver configurado reservas de DHCP ou IP estático no roteador para o servidor de destino e o intervalo DHCP não for igual ao do servidor de origem, é possível que o roteador emita um novo endereço IP para o servidor de destino. Se isso acontecer, redefina as regras do roteador para encaminhar para o novo endereço IP do servidor de destino.  
  
###  <a name="BKMK_RemoveTheSourceServer"></a> Remover e realocar o servidor de origem  
 Desative o servidor de origem e desconecte-o da rede. É recomendável não reformatar o servidor de origem por pelo menos uma semana para garantir que todos os dados necessários sejam migrados para o servidor de destino. Depois de ter verificado que todos os dados foram migrados, você pode reinstalar este servidor na rede como um servidor secundário para outras tarefas, se necessário.  
  
> [!NOTE]
>  Depois de rebaixar e remover o servidor de origem, reinicie o servidor de destino.  
  
 Depois de rebaixar o servidor de origem, ele não está em estado íntegro. Se você desejar realocar o servidor de origem, a maneira mais simples é reformatá-lo, instalar um sistema operacional de servidor e, em seguida, configurá-lo para uso como um servidor adicional.
