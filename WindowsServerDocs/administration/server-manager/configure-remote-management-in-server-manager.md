---
title: Configurar o gerenciamento remoto no Gerenciador do servidor
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63d90d52b55357b5de823f2ca5e0a9fa2a3468e6
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222995"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurar o gerenciamento remoto no Gerenciador do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, você pode usar o Gerenciador do servidor para executar tarefas de gerenciamento em servidores remotos. gerenciamento remoto é habilitado por padrão nos servidores que estão executando o Windows Server 2016. Para gerenciar um servidor remotamente usando o Gerenciador do servidor, você pode adicionar o servidor ao pool de servidores do Gerenciador do servidor.

Você pode usar o Gerenciador de servidores para gerenciar servidores remotos que estão executando versões antigas do Windows Server, mas as seguintes atualizações são necessárias para gerenciar totalmente os sistemas operacionais mais antigos.

Para gerenciar servidores que estão executando versões do Windows Server anteriores ao Windows Server 2016, instale o software e atualizações para tornar as versões mais antigas do Windows Server gerenciável usando o Gerenciador do servidor no Windows Server 2016 a seguir.

|Sistema operacional|Software necessário|Capacidade de gerenciamento|
|----------|-----------|---------|
| Windows Server 2012 R2 ou Windows Server 2012 |-   [.NET Framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058). O pacote de download do Windows Management Framework 5.0 atualiza os provedores do Windows Management Instrumentation (WMI) no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2. Os provedores WMI atualizados permitem ao Gerenciador do servidor coletar informações sobre as funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização for aplicada, servidores que executam o Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 têm um status de gerenciabilidade **não está acessível**.<br />-A atualização de desempenho associada [artigo 2682011 da Base de dados de Conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) não é mais necessário em servidores que executam o Windows Server 2012 R2 ou Windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). O pacote de download do Windows Management Framework 4.0 atualiza os provedores do Windows Management Instrumentation (WMI) no Windows Server 2008 R2. Os provedores WMI atualizados permitem ao Gerenciador do servidor coletar informações sobre as funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização for aplicada, servidores que executam o Windows Server 2008 R2 têm um status de gerenciabilidade **não está acessível**.<br />-A atualização de desempenho associada [artigo 2682011 da Base de dados de Conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite que o Gerenciador do servidor coletar dados de desempenho do Windows Server 2008 R2.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) pacote de download do Windows Management Framework 3.0 atualiza os provedores do Windows Management Instrumentation (WMI) no Windows Server 2008. Os provedores WMI atualizados permitem ao Gerenciador do servidor coletar informações sobre as funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização for aplicada, servidores que executam o Windows Server 2008 tem um status de gerenciabilidade **não acessível – verificar as versões anteriores executam o Windows Management Framework 3.0**.<br />-A atualização de desempenho associada [artigo 2682011 da Base de dados de Conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite que o Gerenciador do servidor coletar dados de desempenho do Windows Server 2008.||

Para obter informações detalhadas sobre como adicionar servidores que estão em grupos de trabalho para gerenciar ou gerenciar servidores remotos de um computador de grupo de trabalho que está executando o Server Manager, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).

## <a name="enabling-or-disabling-remote-management"></a>Habilitando ou desabilitando o gerenciamento remoto
No Windows Server 2016, o gerenciamento remoto é habilitado por padrão. Antes que você pode se conectar a um computador que está executando o Windows Server 2016 remotamente usando o Gerenciador do servidor, gerenciamento remoto do Gerenciador do servidor deve ser habilitado no computador de destino se ele tiver sido desabilitado. Os procedimentos nesta seção descrevem como desabilitar o gerenciamento remoto, e como reabilitar o gerenciamento remoto se tiver sido desabilitado. No console do Gerenciador do servidor, o status de gerenciamento remoto para o servidor local é exibido na **propriedades** área da **servidor Local** página.

As contas do administrador local, exceto a conta do Administrador interno, podem não ter direito de gerenciar um servidor remotamente, mesmo que o gerenciamento remoto esteja habilitado. O controle de conta de usuário (UAC) remoto **LocalAccountTokenFilterPolicy** configuração do registro deve ser configurada para permitir que contas locais do grupo administradores diferentes da conta de administrador interno gerenciem remotamente o servidor.

No Windows Server 2016, o Gerenciador do servidor se baseia em Windows remote Management (WinRM) e o Distributed component Object model (DCOM) para comunicações remotas. As configurações que são controladas pelo **configurar o gerenciamento remoto** caixa de diálogo afetam apenas partes do Gerenciador do servidor e do Windows PowerShell que usam WinRM para comunicações remotas. Elas não afetam partes do Gerenciador de servidores que usam DCOM para comunicações remotas. Por exemplo, o Gerenciador do servidor usa WinRM para se comunicar com servidores remotos que estão executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, mas usa DCOM para se comunicar com servidores que executam o Windows Server 2008 e Windows Server 2008 R2 mas não têm o [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881) ou [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) atualizações aplicadas. Console de gerenciamento Microsoft (mmc) e outras ferramentas de gerenciamento herdadas usam o DCOM. Para obter mais informações sobre como alterar essas configurações, consulte [para configurar o mmc ou outro gerenciamento remoto de ferramenta referente ao DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom) neste tópico.

> [!NOTE]
> Os procedimentos nessa seção podem ser concluídos somente nos computadores que estão executando o Windows Server. Você não pode habilitar ou desabilitar o gerenciamento remoto em um computador que está executando o Windows 10, usando esses procedimentos, porque o sistema operacional cliente não podem ser gerenciado usando o Gerenciador do servidor.

-   Para habilitar o gerenciamento remoto do WinRM, selecione um dos procedimentos a seguir.

    -   [Para habilitar o gerenciamento remoto do Gerenciador do servidor usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [Para habilitar o gerenciamento remoto do Gerenciador do servidor usando o Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [Para habilitar o gerenciamento remoto do Gerenciador do servidor usando a linha de comando](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [Para habilitar o Gerenciador do servidor e do Windows PowerShell gerenciamento remoto em versões anteriores do Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   Para desabilitar o gerenciamento remoto do WinRM e o Gerenciador do servidor, selecione um dos procedimentos a seguir.

    -   [Para desabilitar o gerenciamento remoto usando a diretiva de grupo](#to-disable-remote-management-by-using-group-policy)

    -   [Para desabilitar o gerenciamento remoto usando um arquivo de resposta durante a instalação autônoma](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   Para configurar o gerenciamento remoto DCOM, consulte [Configurar o gerenciamento remoto DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom).

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a interface do Windows

1.  > [!NOTE]
    > As configurações que são controladas pelo **configurar o gerenciamento remoto** caixa de diálogo não afetam partes do Gerenciador de servidores que usam DCOM para comunicações remotas.

    No computador que você deseja gerenciar remotamente, abra o Gerenciador do servidor, se ele não ainda estiver aberto. Na barra de tarefas do Windows, clique em **Gerenciador de Arquivos**. Sobre o **iniciar** tela, clique no **Gerenciador do servidor** lado a lado.

2.  No **propriedades** área da **servidores locais** , clique no valor de hyperlink para a **gerenciamento remoto** propriedade.

3.  Faça o seguinte e clique em **OK**.

    -   Para evitar que o computador seja gerenciado remotamente usando o Gerenciador do servidor (ou o Windows PowerShell se ele estiver instalado), desmarque a **habilitar o gerenciamento remoto deste servidor por outros computadores** caixa de seleção.

    -   Para permitir que este computador seja gerenciado remotamente usando o Gerenciador do servidor ou o Windows PowerShell, selecione **habilitar o gerenciamento remoto deste servidor por outros computadores**.

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>Habilitar o gerenciamento remoto do Gerenciador de Servidores usando o Windows PowerShell

1.  No computador que você deseja gerenciar remotamente, siga um destes procedimentos para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.

    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.

    -   Sobre o Windows **inicie** tela, clique com botão direito **Windows PowerShell**e, em seguida, na barra de aplicativos, clique em **executar como administrador**.

2.  Digite o seguinte e, em seguida, pressione **Enter** habilitar todas as exceções de regra de firewall necessárias.

    **SMremoting.exe configurar-habilitar**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a linha de comando

1.  Abra uma sessão de prompt de comando com direitos privilegiados de usuário no computador que você deseja gerenciar remotamente. Para fazer isso, no **iniciar** tela, digite **cmd**, clique com botão direito a **prompt de comando** bloco quando ele for exibido no **aplicativos** resultados, e em seguida, na barra de aplicativos, clique em **executar como administrador**.

2.  Execute o seguinte arquivo executável.

    **%windir%\system32\Configure-SMremoting.exe**

3.  Siga um destes procedimentos:

    -   Para desabilitar o gerenciamento remoto, digite **SMremoting.exe configurar-desabilitar**, em seguida, pressione **Enter**.

    -   Para habilitar o gerenciamento remoto, digite **SMremoting.exe configurar-habilitar**, em seguida, pressione **Enter**.

    -   Para exibir a configuração de gerenciamento remoto atual, digite **SMremoting.exe configurar-obter**, e pressione ENTER.

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>Habilitar o Gerenciador de Servidores e o gerenciamento remoto do Windows PowerShell em versões anteriores do Windows Server

-   Siga um destes procedimentos:

    -   Para habilitar o gerenciamento remoto nos servidores que executam o Windows Server 2012, consulte [para habilitar o gerenciamento remoto do Gerenciador do servidor usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) neste tópico.

    -   Para habilitar o gerenciamento remoto nos servidores que executam o Windows Server 2008 R2, consulte [com o Gerenciador do servidor de gerenciamento remoto](https://go.microsoft.com/fwlink/?LinkID=137378) na Ajuda do Windows Server 2008 R2.

    -   Para habilitar o gerenciamento remoto nos servidores que executam o Windows Server 2008, consulte [habilitar e usar comandos remotos no Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>Para configurar o mmc ou outro gerenciamento remoto de ferramenta referente ao DCOM

1.  Faça o seguinte para abrir o Firewall do Windows com o Firewall snap-in de Segurança Avançada.

    -   No **propriedades** área da **servidor Local** no Gerenciador do servidor, clique no valor de hipertexto para a **Firewall do Windows** propriedade e depois clique em  **Configurações avançadas**.

    -   No **inicie** tela, digite **WF. msc**e clique no bloco snap-in quando ele for exibido no **aplicativos** resultados.

2.  No painel da árvore, selecione **Regras de entrada**.

3.  verificar se as exceções às seguintes regras de firewall estão habilitadas e não foram desabilitadas pelas configurações de diretiva de grupo. Se algum deles não foi habilitado, vá para a próxima etapa.

    -   Acesso à Rede COM+ (DCOM-In)

    -   Gerenciamento remoto do Log de eventos (NP-In)

    -   Gerenciamento remoto de Log de eventos (RPC)

    -   Gerenciamento remoto do Log de eventos (RPC-EPMAP)

4.  Clique com o botão direito do mouse nas regras que não estão habilitadas, e clique no menu de contexto **Habilitar regra** .

5.  Feche o Firewall do Windows com o snap-in de Segurança Avançada

### <a name="to-disable-remote-management-by-using-group-policy"></a>Para desabilitar o gerenciamento remoto usando a Política de Grupo

1.  Siga um destes procedimentos para abrir o editor de diretiva de Grupo Local.

    -   Em um servidor que está executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, na **inicie** tela, digite **gpedit. msc**e, em seguida, clique no **gpedit** lado a lado Quando ele for exibido.

    -   Em um servidor que esteja executando o Windows Server 2008 R2 ou Windows Server 2008, além de **executados** caixa de diálogo, digite **gpedit. msc**e, em seguida, pressione **Enter**.

2.  Abra **computador Configuration\Administrative Templates\Windows Windows\Windows remote Management (WinRM) \WinRM serviço**.

3.  No painel de conteúdo, clique duas vezes em **Permitir gerenciamento de servidor remoto via WinRM**.

4.  Na caixa de diálogo da configuração de política **Permitir gerenciamento de servidor remoto via WinRM** , selecione **Desabilitado** para desabilitar o gerenciamento remoto. Clique em **OK** para salvar suas alterações e fechar a caixa de diálogo de configuração da política.

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>Para desabilitar o gerenciamento remoto usando um arquivo de resposta durante a instalação autônoma

1.  Crie um arquivo de resposta de instalação autônoma para instalações do Windows Server 2016 usando o Gerenciador de imagem de sistema do Windows (Windows SIM). Para obter mais informações sobre como criar um arquivo de resposta e usar o Windows SIM, consulte [o que é o Gerenciador de imagem de sistema do Windows?](https://technet.microsoft.com/library/cc766347.aspx) e [passo a passo: Implantação básica do Windows para profissionais de TI](https://technet.microsoft.com/library/dd349348.aspx).

2.  Em seu arquivo de resposta, localize a configuração **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Para desabilitar o gerenciamento remoto do Gerenciador do servidor por padrão em todos os servidores aos quais você deseja aplicar o arquivo de resposta, defina **Microsoft \EnableServerremoteManagement** para **False** .

    > [!NOTE]
    > Essa configuração desabilita o gerenciamento remoto como parte do processo de instalação do sistema operacional. Essa configuração não impede que um administrador habilitar o gerenciamento remoto do Gerenciador do servidor em um servidor após a conclusão da instalação do sistema operacional. Os administradores podem habilitar o gerenciamento remoto novamente usando as etapas no Gerenciador do servidor [para configurar o gerenciamento remoto do Gerenciador do servidor usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) ou [para habilitar o gerenciamento remoto do Gerenciador do servidor usando Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell) neste tópico.
    > 
    > Se você desabilitar o gerenciamento remoto por padrão como parte de uma instalação autônoma e não habilitar o gerenciamento remoto no servidor novamente após a instalação, os servidores aos quais esse arquivo de resposta é aplicado não podem ser gerenciados totalmente usando o Gerenciador do servidor. Os servidores que estão executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (e têm o gerenciamento remoto desabilitado por padrão) gerar erros de status de capacidade de gerenciamento no console do Gerenciador do servidor depois de serem adicionados ao servidor do Server Manager pool.

## <a name="windows-remote-management-winrm-listener-settings"></a>Configurações do ouvinte do Windows remotas Management (WinRM)
Gerenciador do servidor depende de configurações do ouvinte WinRM padrão nos servidores remotos que você deseja gerenciar. Se o mecanismo de autenticação padrão ou o número de porta do ouvinte WinRM em um servidor remoto foi alterado de configurações padrão, o Gerenciador do servidor não pode se comunicar com o servidor remoto.

A lista a seguir mostra as configurações do ouvinte WinRM padrão para gerenciamento usando o Gerenciador do servidor.

-   O serviço WinRM está executando.

-   Um ouvinte WinRM é criado para aceitar as solicitações HTTP através do número da porta 5985.

-   O número da porta 5985 é habilitado nas configurações do Firewall do Windows para permitir as solicitações através do WinRM.

-   Os tipos de autenticação **Kerberos** e **Negotiate** estão habilitados.

O número da porta padrão é 5985 para que o WinRM se comunique com um computador remoto.

Para obter mais informações sobre como configurar as configurações do ouvinte WinRM, em um prompt de comando, digite **winrm help config**, e pressione ENTER.

## <a name="see-also"></a>Consulte também
[Adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md)
[Windows PowerShell: about_remote_Troubleshooting no Windows Server TechCenter](https://technet.microsoft.com/library/dd347642.aspx)
[descrição de controle de conta de usuário](https://support.microsoft.com/kb/951016)



