---
title: Gerenciar dispositivos no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>Gerenciar dispositivos no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 As seções a seguir abordam os recursos de gerenciamento de dispositivo de um servidor e explicam como configurar e usar dispositivos em sua rede:  
  
-   [Gerenciar dispositivos usando o painel](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Atribuir contas de usuário permissão para fazer logon em computadores de rede específicos](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Remover um computador do servidor](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Definir configurações de política de grupo para segurança e redirecionamento de pasta](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Conectar a um computador de rede usando uma sessão de área de trabalho remota](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Exibir propriedades de computador](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>Gerenciar dispositivos usando o painel  
 Windows Server Essentials torna possível realizar tarefas administrativas comuns usando o painel do Windows Server Essentials. O **dispositivos** página do painel fornece os seguintes:  
  
-   Uma lista de computadores em rede, que exibe:  
  
    -   O nome do computador  
  
    -   O status do computador, qualquer **on-line** ou **off-line**  
  
    -   A descrição do computador  
  
    -   O status do backup do computador  
  
    -   O status de atualização do computador  
  
    -   O status de segurança do computador  
  
    -   O status de alertas do computador  
  
    -   Informações de política de grupo para o computador  
  
-   Um painel de detalhes com informações adicionais sobre um computador selecionado  
  
-   Um painel de tarefas que inclui um conjunto de tarefas administrativas do dispositivo, como exibir alertas e propriedades do computador, configurando o backup do computador e restaurar arquivos e pastas de um backup  
  
#### <a name="to-view-the-status-of-network-computers"></a>Para exibir o status dos computadores da rede  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **dispositivos**.  
  
3.  Exiba o status de todos os computadores na rede no painel de lista.  
  
 A tabela a seguir descreve os vários computador e as tarefas de backup que estão disponíveis no painel Windows Server Essentials. Algumas das tarefas são específicas do computador, e eles só ficam visíveis quando você seleciona um computador na lista.  
  
### <a name="computer-tasks-in-the-dashboard"></a>No painel de tarefas do computador  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|Exibir as propriedades de computador|Exibe informações gerais para um computador selecionado e permite que você exiba detalhes para os backups do computador.|  
|Configurar o backup para este computador|Executa a configurar o Assistente de Backup.|  
|Personalizar o backup do computador|Abre as propriedades de backup, do qual você pode fazer alterações para as configurações de backup do computador selecionado.|  
|Inicie um backup do computador|Inicia um backup de um computador selecionado.|  
|Parar de backup do computador|Interrompe o backup de um computador selecionado.|  
|Restaurar arquivos ou pastas para o computador|Executa a restaurar arquivos e pastas assistente, que permite que você restaure unidades, pastas ou arquivos específicos.|  
|Exibir alertas para o computador|Exibe críticas e outras alertas informativos e permite que você execute uma ação corretiva sempre que possível.|  
|Área de trabalho remota no computador|Abre a Conexão de área de trabalho remota no computador selecionado.|  
|Remover o computador|Executa a remover um Assistente de computador, que desliga o computador a partir do Windows Server Essentials Dashboard.|  
|Personalizar o backup do computador e as configurações do histórico de arquivos|Abre a página de configurações de backup, do qual você pode fazer alterações para o agendamento de backup e configurações do histórico de arquivos para o cliente de computadores.|  
|Como eu conecto computadores ao servidor?|Abre um tópico da Ajuda que descreve as etapas para executar para ingressar um computador à rede.|  
|Política de grupo de implementar|Aplica as configurações de política para computadores Windows 8 e Windows 7 que ingressam no domínio.|  
  
##  <a name="BKMK_2"></a>Atribuir contas de usuário permissão para fazer logon em computadores de rede específicos  
 Você pode atribuir permissões a contas de usuário para que os usuários podem fazer logon somente determinados computadores de rede quando o acesso à rede do Windows Server Essentials de um local remoto.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Para alterar o acesso ao computador para uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja alterar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **exibir as propriedades da conta**. O **propriedades** página para a conta de usuário é exibido.  
  
5.  Sobre o **acesso ao computador**, selecione o computador que esse usuário pode acessar remotamente e, em seguida, clique em **Okey**.  
  
##  <a name="BKMK_3"></a>Remover um computador do servidor  
 Quando você remove um computador de um servidor que está executando o Windows Server Essentials usando o Dashboard, ele não é gerenciado pelo servidor. Como resultado, o servidor vai parar de criar backups de computador ou monitorar sua integridade após a remoção da rede.  
  
> [!NOTE]
>  Remoção de um computador do servidor não desconecte o computador da rede. O computador ainda pode acessar recursos da rede da mesma forma que ele poderia antes de estar conectado ao servidor. Para impedir que o computador acessando recursos do servidor e desconecte-o do servidor, você deve remover o computador do domínio. Além disso, a remoção do computador do servidor não automaticamente desinstalar o software Connector ou barra inicial do computador que está sendo removido. Você deve remover manualmente o software do conector do computador. Para obter mais informações, consulte a seção desinstalar o software do conector em [conectado](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Para remover um computador da rede usando o Dashboard  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique no **dispositivos** guia.  
  
3.  Na lista de computadores, clique com botão direito do computador que você deseja remover da rede e, em seguida, clique em **remover o computador**.  
  
##  <a name="BKMK_5"></a>Definir configurações de política de grupo para segurança e redirecionamento de pasta  
 Você pode configurar a política de grupo e implantá-la nos computadores da rede do Windows Server Essentials usando o painel do Windows Server Essentials. Política de grupo no Windows Server Essentials inclui as configurações de segurança e redirecionamento de pasta que impactam Windows Update, o Windows Defender e o firewall de rede.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Para configurar a política de grupo no Windows Server Essentials  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **dispositivos**.  
  
3.  Para o Windows Server Essentials: No elemento global **usuários tarefas** painel, clique em **política de grupo de implementar**.  
  
     Para o Windows Server Essentials: No elemento global **dispositivos tarefas** painel, clique em **política de grupo de implementar**.  
  
4.  Abre o Assistente de política de grupo de implementar.  
  
5.  Sobre o **permitir política de grupo de redirecionamento de pasta** página do assistente, você pode escolher as pastas do usuário que você quer redirecionar.  
  
6.  Sobre o **ativar configurações de política de segurança** página do assistente, você pode optar por ativar configurações de política de grupo para **Windows Update**, **Windows Defender**e o **Firewall de rede**.  
  
7.  Clique em **concluir** para implementar as configurações de política de grupo.  
  
##  <a name="BKMK_7"></a>Conectar a um computador de rede usando uma sessão de área de trabalho remota  
 Para acessar remotamente seu computador de rede do Windows Server Essentials quando você estiver fora do escritório, use seu navegador da Web para seu site de acesso via Web remoto s organização fazer logon e, no **computadores** guia, clique no nome do computador.  
  
 O **Status** coluna mostra se você pode se conectar a um computador em sua rede e pode conter os seguintes valores:  
  
-   **Disponível**  
  
     O computador está ativado e está disponível para uma conexão remota. Mesmo se você vir esse status, você ainda pode não ser capaz de se conectar a este computador, se um firewall de terceiros bloqueia a conexão.  
  
-   **Offline ou dormindo**  
  
     O computador é desligado ou está em modo de suspensão ou hibernação. Se um computador está offline ou no modo de suspensão, o status é atualizado em tempo real para que você saiba quando o computador estiver disponível.  
  
-   **Sistema operacional sem suporte**  
  
     O sistema operacional no computador não oferece suporte a área de trabalho remota. Ele pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.  
  
-   **Conexão está desabilitado**  
  
     A conexão do computador está bloqueado por um firewall ou a área de trabalho remota está desabilitada no computador ou pela política de grupo. Ele pode levar até 6 horas para esse status ser atualizado no servidor, se houver uma alteração.  
  
##  <a name="BKMK_8"></a>Exibir propriedades de computador  
 O **dispositivos** seção do Windows Server Essentials painel exibe uma lista dos computadores da rede. A lista também fornece informações adicionais sobre cada computador.  
  
#### <a name="to-view-a-list-of-computers"></a>Para exibir uma lista de computadores  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação principal, clique em **dispositivos**.  
  
3.  O painel exibe uma lista atual de computadores.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Para exibir ou alterar as propriedades de um computador  
  
1.  Na lista de computadores, selecione a conta para o qual você deseja exibir ou alterar propriedades.  
  
2.  No **< Computername\ > tarefas** painel, clique em **exibir as propriedades de computador**. O **propriedades** aparecerá para os computadores.  
  
3.  Clique em uma guia para exibir as propriedades para aquele computador.  
  
4.  Para salvar as alterações feitas às propriedades do computador, clique em **aplicar**.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Use o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar contas de usuário usando o painel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
