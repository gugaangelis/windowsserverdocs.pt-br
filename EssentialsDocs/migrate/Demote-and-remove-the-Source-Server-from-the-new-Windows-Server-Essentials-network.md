---
title: Rebaixar e remova o servidor de origem do novo network1 do Windows Server Essentials
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
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Rebaixar e remova o servidor de origem do novo network1 do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Depois de concluir a instalação do Windows Server Essentials e concluir as tarefas no Assistente de migração, você deve executar as seguintes tarefas:  
  

1.  [Desinstalar o servidor do Exchange 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconecte impressoras que estão conectadas diretamente para o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rebaixar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover a função de servidor DHCP do servidor de origem para o roteador](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Remover e reutilizar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Desinstalar o servidor do Exchange 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Desconecte impressoras que estão conectadas diretamente para o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rebaixar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Mover a função de servidor DHCP do servidor de origem para o roteador](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Remover e reutilizar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>Desinstalar o servidor do Exchange 2003  
  
> [!IMPORTANT]
>  Se você adicionar contas de usuário após mover caixas de correio para o servidor de destino e antes de desinstalar o Exchange Server 2003 do servidor de origem, as caixas de correio são adicionadas no servidor de origem. Isso ocorre por design. Você deverá mover as caixas de correio para o servidor de destino para todas as contas de usuário que são adicionadas durante esse tempo. Repita as instruções em caixas de correio do Exchange Server mover e as configurações de migração do Windows Server Essentials antes de desinstalar o Exchange Server 2003.  
  
 Você deve desinstalar o Exchange Server 2003 do servidor de origem antes de você rebaixá-lo. Isso remove todas as referências nos serviços de domínio Active Directory (AD DS) para Exchange Server no servidor de origem. Você deve ter sua mídia do Windows Small Business Server 2003 para remover o Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Para desinstalar o servidor do Exchange 2003 do servidor de origem  
  
1.  Faça logon no servidor de origem como administrador  
  
2.  Clique em **iniciar**, clique em **painel de controle**e clique em **adicionar ou remover programas**.  
  
3.  Na lista de programas, selecione **Windows Small Business Server 2003**e clique em **alterar ou remover**.  
  
4.  No Assistente de instalação, clique em **próxima** até que o **seleção de componentes** página será exibida.  
  
5.  Na página seleção de componentes, expanda **Exchange Server**e, em seguida, escolha **remover**.  
  
    > [!NOTE]

    >  Exchange Server verificará para certificar-se de que não há nenhuma caixas de correio ou pastas públicas no servidor. Se nenhum dado permanece, uma mensagem de erro aparece quando você clica **remover**. Para evitar esse problema, certifique-se de que você tenha concluído todos os procedimentos no tópico [mover SBS 2003 configurações e dados ao servidor de destino](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

    >  Exchange Server verificará para certificar-se de que não há nenhuma caixas de correio ou pastas públicas no servidor. Se nenhum dado permanece, uma mensagem de erro aparece quando você clica **remover**. Para evitar esse problema, certifique-se de que você tenha concluído todos os procedimentos no tópico [mover SBS 2003 configurações e dados ao servidor de destino](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6.  Clique em **próxima**.  
  
7.  Quando solicitado, insira Windows Small Business Server 2003 CD #3 e siga as instruções na tela.  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>Desconecte impressoras que estão conectadas diretamente para o servidor de origem  
 Antes de você rebaixar o servidor de origem, Desconecte fisicamente todas as impressoras que estão conectadas diretamente para o servidor de origem e são compartilhadas por meio do servidor de origem. Certifique-se de que nenhum objetos do Active Directory permaneçam para as impressoras que foram conectadas diretamente ao servidor de origem. As impressoras podem ser diretamente conectadas ao servidor de destino e compartilhadas do Windows Server Essentials.  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>Rebaixar o servidor de origem  
 Antes de você rebaixar o servidor de origem da função do controlador de domínio do AD DS para a função de um servidor membro do domínio, certifique-se de que as configurações de política de grupo são aplicadas a todos os computadores cliente, conforme descrito no procedimento a seguir.  
  
> [!IMPORTANT]
>  O servidor de origem e o servidor de destino devem estar conectado à rede enquanto as alterações de política de grupo são atualizadas nos computadores cliente.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Para forçar uma atualização de política de grupo em um computador cliente  
  
1.  Faça logon no computador cliente como administrador.  
  
2.  Abra uma janela de Prompt de comando como administrador.  
  
3.  No prompt de comando, digite **gpupdate /force**, e pressione ENTER.  
  
4.  O processo de exigir que você faça logoff e logon novamente para concluir. Clique em **Sim** para confirmar.  
  
##### <a name="to-demote-the-source-server"></a>Para rebaixar o servidor de origem  
  
1.  No servidor de origem, clique em **iniciar**, clique em **executar**, tipo **dcpromo**e clique em **Okey**.  
  
2.  Clique em **próxima** duas vezes.  
  
    > [!NOTE]
    >  Não selecione **esse servidor é o último controlador de domínio no domínio**.  
  
3.  Digite uma senha para a nova conta de administrador no servidor e, em seguida, clique em **próxima**.  
  
4.  No **resumo** caixa de diálogo, você será informado que AD DS será removido do computador e que o servidor se torne um membro do domínio. Clique em **próxima**.  
  
5.  Clique em **concluir **. O servidor de origem for reiniciado.  
  
6.  Depois que o servidor de origem for reiniciado, adicione o servidor de origem como um membro de um grupo de trabalho antes de desconectar da rede.  
  
 Depois de adicionar o servidor de origem como um membro de um grupo de trabalho e desconecte-o da rede, você deve removê-lo do AD DS no servidor de destino.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Para remover o servidor de origem do Active Directory  
  
1.  No servidor de destino, abra **usuários e Active Directory computadores**.  
  
2.  No **usuários e Active Directory computadores** painel de navegação, expanda o nome do domínio e, em seguida, **computadores**.  
  
3.  O nome do servidor de origem se ele ainda existe na lista de servidores, clique com o botão direito **excluir**e clique em **Sim**.  
  
4.  Verifique se o servidor de origem não está listado e, em seguida, fechar **usuários e Active Directory computadores**.  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>Mover a função de servidor DHCP do servidor de origem para o roteador  
  
> [!NOTE]

>  Se você já executou essa tarefa antes de iniciar o processo de migração, continue com a seção [remover e reutilizar o servidor de origem](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

>  Se você já executou essa tarefa antes de iniciar o processo de migração, continue com a seção [remover e reutilizar o servidor de origem](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Se o servidor de origem está executando a função DHCP, execute as seguintes etapas para mover a função DHCP ao roteador.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Para mover a função DHCP do servidor de origem para o roteador  
  
1.  Desative o serviço DHCP no servidor de origem, da seguinte maneira:  
  
    1.  No servidor de origem, clique em **iniciar**, clique em **ferramentas administrativas**e clique em **serviços**.  
  
    2.  Na lista de serviços em execução, clique com botão direito do **Windows Server**e clique em **propriedades**.  
  
    3.  Para **iniciar tipo**, selecione **desabilitada**.  
  
    4.  Interrompa o serviço.  
  
2.  Ative a função DHCP no seu roteador  
  
    1.  Siga as instruções na documentação do roteador para ativar a função DHCP no roteador.  
  
    2.  Para garantir que os endereços IP emitidos pelo servidor de origem permanecem os mesmos, siga as instruções na documentação do roteador para definir o intervalo DHCP no roteador para ser o mesmo que o intervalo DHCP no servidor de origem.  
  
    > [!IMPORTANT]
    >  Se você não tiver configurado um reservas de DHCP ou de IP estático do roteador para o servidor de destino, e o intervalo DHCP não é o mesmo que o servidor de origem, é possível que o roteador emitirá um novo endereço IP para o servidor de destino. Se isso acontecer, redefina a regras do roteador para encaminhar mensagens para o novo endereço IP do servidor de destino de encaminhamento de porta.  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>Remover e reutilizar o servidor de origem  
 Desativar o servidor de origem e desconecte-o da rede. Recomendamos que você não reformatar o servidor de origem pelo menos uma semana para garantir que todos os dados necessários migrados para o servidor de destino. Depois de você ter confirmado que todos os dados tem migrados, você poderá reinstalar esse servidor na rede como um servidor secundário para outras tarefas, se necessário.  
  
> [!NOTE]
>  Depois que você rebaixar e remove o servidor de origem, reinicie o servidor de destino.  
  
 Depois que você rebaixar o servidor de origem, ele não está em um estado íntegro. Se você quiser reutilizar o servidor de origem, a maneira mais simples é reformatá-la, instalar um sistema operacional de servidor e, em seguida, configurou para uso como um servidor adicional.
