---
title: Configurar o gerenciamento remoto no Gerenciador do Servidor
description: Gerenciador do Servidor
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0d2369bd42fc884b1a401fc1450dbe9d1e47663
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895805"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurar o gerenciamento remoto no Gerenciador do Servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, você pode usar Gerenciador do Servidor para executar tarefas de gerenciamento em servidores remotos. o gerenciamento remoto é habilitado por padrão em servidores que executam o Windows Server 2016. Para gerenciar um servidor remotamente usando Gerenciador do Servidor, você adiciona o servidor ao pool de servidores do Gerenciador do Servidor.

Você pode usar Gerenciador do Servidor para gerenciar servidores remotos que executam versões mais antigas do Windows Server, mas as atualizações a seguir são necessárias para gerenciar totalmente esses sistemas operacionais mais antigos.

Para gerenciar servidores que executam versões anteriores ao Windows Server 2016, instale o software e as atualizações a seguir para fazer com que as versões mais antigas do Windows Server sejam gerenciadas usando Gerenciador do Servidor no Windows Server 2016.

|Sistema operacional|Software necessário|Capacidade de gerenciamento|
|----------|-----------|---------|
| Windows Server 2012 R2 ou Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). O pacote de download do Windows Management Framework 5,0 atualiza os provedores de Instrumentação de Gerenciamento do Windows (WMI) no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 têm um status de capacidade de gerenciamento **não acessível**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) não é mais necessária em servidores que executam o windows Server 2012 R2 ou o windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). O pacote de download do Windows Management Framework 4,0 atualiza os provedores de Instrumentação de Gerenciamento do Windows (WMI) no Windows Server 2008 R2. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2008 R2 têm um status de capacidade de gerenciamento **não acessível**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite Gerenciador do servidor coletar dados de desempenho do Windows Server 2008 R2.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) O pacote de download do Windows Management Framework 3,0 atualiza os provedores de Instrumentação de Gerenciamento do Windows (WMI) no Windows Server 2008. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2008 têm um status de capacidade de gerenciamento **não acessível-Verifique se as versões anteriores executam o Windows Management Framework 3,0**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite Gerenciador do servidor coletar dados de desempenho do Windows Server 2008.||

para obter informações detalhadas sobre como adicionar servidores que estão em grupos de trabalho para gerenciar ou gerenciar servidores remotos de um computador do grupo de trabalhos que está executando o Gerenciador do Servidor, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).

## <a name="enabling-or-disabling-remote-management"></a>Habilitando ou desabilitando o gerenciamento remoto
No Windows Server 2016, o gerenciamento remoto é habilitado por padrão. Antes de se conectar a um computador que esteja executando o Windows Server 2016 remotamente usando Gerenciador do Servidor, Gerenciador do Servidor gerenciamento remoto deve estar habilitado no computador de destino se ele tiver sido desabilitado. Os procedimentos nesta seção descrevem como desabilitar o gerenciamento remoto, e como reabilitar o gerenciamento remoto se tiver sido desabilitado. No console do Gerenciador do Servidor, o status do gerenciamento remoto para o servidor local é exibido na área **Propriedades** da página **servidor local** .

As contas do administrador local, exceto a conta do Administrador interno, podem não ter direito de gerenciar um servidor remotamente, mesmo que o gerenciamento remoto esteja habilitado. A configuração do registro **LocalAccountTokenFilterPolicy** do controle de conta de usuário remoto (UAC) deve ser configurada para permitir contas locais do grupo Administradores que não sejam a conta de administrador interno para gerenciar remotamente o servidor.

No Windows Server 2016, Gerenciador do Servidor se baseia no gerenciamento remoto do Windows (WinRM) e no Distributed Component Object Model (DCOM) para comunicações remotas. As configurações controladas pela caixa de diálogo **Configurar gerenciamento remoto** afetam apenas partes do Gerenciador do servidor e do Windows PowerShell que usam o WinRM para comunicações remotas. Eles não afetam partes de Gerenciador do Servidor que usam DCOM para comunicações remotas. Por exemplo, Gerenciador do Servidor usa o WinRM para se comunicar com servidores remotos que executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012, mas usa DCOM para se comunicar com servidores que executam o Windows Server 2008 e o Windows Server 2008 R2, mas que não têm o Windows [Management framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881) ou o [windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) atualizações aplicadas. O MMC (console de gerenciamento Microsoft) e outras ferramentas de gerenciamento herdadas usam DCOM. Para obter mais informações sobre como alterar essas configurações, consulte [para configurar o MMC ou outra ferramenta gerenciamento remoto sobre DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom) neste tópico.

> [!NOTE]
> Os procedimentos nessa seção podem ser concluídos somente nos computadores que estão executando o Windows Server. Você não pode habilitar ou desabilitar o gerenciamento remoto em um computador que esteja executando o Windows 10 usando esses procedimentos, pois o sistema operacional do cliente não pode ser gerenciado usando Gerenciador do Servidor.

-   Para habilitar o gerenciamento remoto do WinRM, selecione um dos procedimentos a seguir.

    -   [Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [Habilitar o gerenciamento remoto do Gerenciador de Servidores usando o Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a linha de comando](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [Habilitar o Gerenciador de Servidores e o gerenciamento remoto do Windows PowerShell em versões anteriores do Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   Para desabilitar o WinRM e Gerenciador do Servidor gerenciamento remoto, selecione um dos procedimentos a seguir.

    -   [Para desabilitar o gerenciamento remoto usando a Política de Grupo](#to-disable-remote-management-by-using-group-policy)

    -   [Para desabilitar o gerenciamento remoto usando um arquivo de resposta durante a instalação autônoma](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   Para configurar o gerenciamento remoto do DCOM, consulte [Para configurar o gerenciamento remoto do DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom).

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a interface do Windows

1.  > [!NOTE]
    > As configurações controladas pela caixa de diálogo **Configurar gerenciamento remoto** não afetam partes de Gerenciador do servidor que usam DCOM para comunicações remotas.

    No computador que você deseja gerenciar remotamente, abra Gerenciador do Servidor, se ainda não estiver aberto. Na barra de tarefas do Windows, clique em **Gerenciador de Arquivos**. Na tela **Iniciar** , clique no bloco **Gerenciador do servidor** .

2.  Na área **Propriedades** da página **servidores locais** , clique no valor de hiperlink para a propriedade **gerenciamento remoto** .

3.  Faça o seguinte e clique em **OK**.

    -   Para impedir que este computador seja gerenciado remotamente usando Gerenciador do Servidor (ou o Windows PowerShell, se estiver instalado), desmarque a caixa de seleção **habilitar gerenciamento remoto deste servidor em outros computadores** .

    -   Para permitir que este computador seja gerenciado remotamente usando o Gerenciador do Servidor ou o Windows PowerShell, selecione **habilitar o gerenciamento remoto deste servidor em outros computadores**.

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>Habilitar o gerenciamento remoto do Gerenciador de Servidores usando o Windows PowerShell

1.  No computador que você deseja gerenciar remotamente, execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.

    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.

    -   Na tela **Iniciar** do Windows, clique com o botão direito do mouse em **Windows PowerShell**e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.

2.  Digite o seguinte e pressione **Enter** para habilitar todas as exceções de regra de firewall necessárias.

    **Configure-SMremoting.exe-habilitar**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor usando a linha de comando

1.  Abra uma sessão de prompt de comando com direitos privilegiados de usuário no computador que você deseja gerenciar remotamente. Para fazer isso, na tela **Iniciar** , digite **cmd**, clique com o botão direito do mouse no bloco do **prompt de comando** quando ele for exibido nos resultados dos **aplicativos** e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.

2.  Execute o seguinte arquivo executável.

    **% WINDIR% \system32\Configure-SMremoting.exe**

3.  Realize um dos seguintes procedimentos:

    -   Para desabilitar o gerenciamento remoto, digite **Configure-SMremoting.exe-Disable**e pressione **Enter**.

    -   Para habilitar o gerenciamento remoto, digite **Configure-SMremoting.exe-Enable**e pressione **Enter**.

    -   Para exibir a configuração de gerenciamento remoto atual, digite **Configure-SMremoting.exe-Get**e pressione Enter.

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>Habilitar o Gerenciador de Servidores e o gerenciamento remoto do Windows PowerShell em versões anteriores do Windows Server

-   Realize um dos seguintes procedimentos:

    -   Para habilitar o gerenciamento remoto em servidores que executam o Windows Server 2012, consulte [para habilitar Gerenciador do servidor gerenciamento remoto usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) neste tópico.

    -   Para habilitar o gerenciamento remoto em servidores que executam o Windows Server 2008 R2, consulte [gerenciamento remoto com Gerenciador do servidor](https://go.microsoft.com/fwlink/?LinkID=137378) na ajuda do Windows Server 2008 R2.

    -   Para habilitar o gerenciamento remoto em servidores que executam o Windows Server 2008, consulte [habilitar e usar comandos remotos no Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>Para configurar o MMC ou o gerenciamento remoto de outra ferramenta por DCOM

1.  Faça o seguinte para abrir o Firewall do Windows com o Firewall snap-in de Segurança Avançada.

    -   Na área **Propriedades** da página **servidor local** em Gerenciador do servidor, clique no valor de hipertexto para a propriedade **Firewall do Windows** e clique em **Configurações avançadas**.

    -   Na tela **Iniciar** , digite **WF. msc**e, em seguida, clique no bloco snap-in quando ele for exibido nos resultados dos **aplicativos** .

2.  No painel da árvore, selecione **Regras de entrada**.

3.  Verifique se as exceções às regras de firewall a seguir estão habilitadas e se não foram desabilitadas pelas configurações de Política de Grupo. Se algum deles não foi habilitado, vá para a próxima etapa.

    -   Acesso à Rede COM+ (DCOM-In)

    -   Gerenciamento remoto do log de eventos (NP-in)

    -   Gerenciamento remoto do log de eventos (RPC)

    -   Gerenciamento remoto do log de eventos (RPC-EPMAP)

4.  Clique com o botão direito do mouse nas regras que não estão habilitadas, e clique no menu de contexto **Habilitar regra**.

5.  Feche o Firewall do Windows com o snap-in de Segurança Avançada

### <a name="to-disable-remote-management-by-using-group-policy"></a>Para desabilitar o gerenciamento remoto usando a Política de Grupo

1.  Siga um destes procedimentos para abrir o editor de Política de Grupo local.

    -   Em um servidor que esteja executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012, na tela **Iniciar** , digite **gpedit. msc**e, em seguida, clique no bloco **gpedit** quando ele for exibido.

    -   Em um servidor que esteja executando o Windows Server 2008 R2 ou o Windows Server 2008, na caixa de diálogo **executar** , digite **gpedit. msc**e pressione **Enter**.

2.  Abra o **Computador\modelos Administrativos\Componentes do serviço \WinRM do gerenciamento remoto do Windows\Windows (WinRM)**.

3.  No painel de conteúdo, clique duas vezes em **Permitir gerenciamento de servidor remoto via WinRM**.

4.  Na caixa de diálogo da configuração de política **Permitir gerenciamento de servidor remoto via WinRM**, selecione **Desabilitado** para desabilitar o gerenciamento remoto. Clique em **OK** para salvar suas alterações e fechar a caixa de diálogo de configuração da política.

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>Para desabilitar o gerenciamento remoto usando um arquivo de resposta durante a instalação autônoma

1.  Crie um arquivo de resposta de instalação autônoma para instalações do Windows Server 2016 usando o Windows SIM (Gerenciador de imagem de sistema do Windows). Para obter mais informações sobre como criar um arquivo de resposta e usar o Windows SIM, consulte [O que é o Gerenciador de Imagem de Sistema do Windows?](https://technet.microsoft.com/library/cc766347.aspx) e [Passo a passo: implantação básica do Windows para profissionais de TI](https://technet.microsoft.com/library/dd349348.aspx).

2.  No arquivo de resposta, localize a configuração **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Para desabilitar Gerenciador do Servidor gerenciamento remoto por padrão em todos os servidores aos quais você deseja aplicar o arquivo de resposta, defina **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** como **false**.

    > [!NOTE]
    > Essa configuração desabilita o gerenciamento remoto como parte do processo de instalação do sistema operacional. Definir essa configuração não impede que um administrador habilite Gerenciador do Servidor gerenciamento remoto em um servidor após a conclusão da instalação do sistema operacional. Os administradores podem habilitar Gerenciador do Servidor gerenciamento remoto novamente usando as etapas em [para configurar Gerenciador do servidor gerenciamento remoto usando a interface do Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) ou [para habilitar Gerenciador do servidor gerenciamento remoto usando o Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell) neste tópico.
    >
    > Se você desabilitar o gerenciamento remoto por padrão como parte de uma instalação autônoma e não habilitar o gerenciamento remoto no servidor novamente após a instalação, os servidores aos quais esse arquivo de resposta é aplicado não poderão ser totalmente gerenciados com o uso de Gerenciador do Servidor. Os servidores que executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 (e que têm o gerenciamento remoto desabilitado por padrão) geram erros de status de capacidade de gerenciamento no console do Gerenciador do Servidor depois de serem adicionados ao pool de servidores do Gerenciador do Servidor.

## <a name="windows-remote-management-winrm-listener-settings"></a>Configurações do ouvinte do Windows Remote Management (WinRM)
Gerenciador do Servidor se baseia nas configurações padrão do ouvinte do WinRM nos servidores remotos que você deseja gerenciar. Se o mecanismo de autenticação padrão ou o número da porta do ouvinte do WinRM em um servidor remoto tiver sido alterado das configurações padrão, Gerenciador do Servidor não poderá se comunicar com o servidor remoto.

A lista a seguir mostra as configurações padrão do ouvinte do WinRM para gerenciar usando Gerenciador do Servidor.

-   O serviço WinRM está executando.

-   Um ouvinte WinRM é criado para aceitar as solicitações HTTP através do número da porta 5985.

-   O número da porta 5985 é habilitado nas configurações do Firewall do Windows para permitir as solicitações através do WinRM.

-   Os tipos de autenticação **Kerberos** e **Negotiate** estão habilitados.

O número da porta padrão é 5985 para que o WinRM se comunique com um computador remoto.

para obter mais informações sobre como definir as configurações do ouvinte do WinRM, em um prompt de comando, digite **winrm help config**e pressione Enter.

## <a name="see-also"></a>Consulte Também
[Adicionar servidores a Gerenciador do servidor](add-servers-to-server-manager.md) 
 [Windows PowerShell: about_remote_Troubleshooting no TechCenter](https://technet.microsoft.com/library/dd347642.aspx) 
 do Windows Server [Descrição do controle de conta de usuário](https://support.microsoft.com/kb/951016)



