---
title: Gerenciar vários servidores remotos com o Gerenciador do servidor
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4db3e486cec5cb065bfd202b7da2547be41a01a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847607"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>Gerenciar vários servidores remotos com o Gerenciador do servidor

>Aplica-se a: Windows Server 2016

Gerenciador do servidor é um console de gerenciamento do Windows Server 2012 R2 e Windows Server 2012 que ajuda os profissionais de TI a provisionar e gerenciar servidores locais e remotos baseados no Windows suas áreas de trabalho, sem a necessidade de qualquer acesso físico aos servidores, ou o preciso habilitar conexões de protocolo rdP de área de trabalho remota para cada servidor. Embora o Gerenciador do servidor esteja disponível no Windows Server 2008 R2 e Windows Server 2008, o Gerenciador do servidor foi atualizado no Windows Server 2012, para dar suporte ao Gerenciamento multisservidor remoto e ajudar a aumentar o número de servidores, que um administrador pode gerenciar.

Em nossos testes, o Gerenciador do servidor no Windows Server 2012 R2 e Windows Server 2012 pode ser usado para gerenciar até 100 servidores configurados com uma carga de trabalho típica. O número de servidores que podem ser gerenciadas usando um único console do Gerenciador de servidores pode variar dependendo da quantidade de dados que você solicita do servidores gerenciados e recursos de hardware e rede disponíveis para o computador que executa o Gerenciador de servidores. Conforme a quantidade de dados que você deseja exibir se aproxima de capacidade do recurso desse computador, você pode enfrentar respostas lentas do Gerenciador de servidores e atrasos na finalização de atualizações. Para ajudar a aumentar o número de servidores que você pode gerenciar usando o Gerenciador de servidores, é recomendável limitar os dados de evento que o Gerenciador do servidor obtém dos seus servidores gerenciados usando as configurações na caixa de diálogo **Configurar dados de eventos** . É possível abrir Configurar Dados do Evento no menu **Tarefas** do bloco **Eventos** . Se você precisar gerenciar um número de nível empresarial de servidores em sua organização, é recomendável avaliar produtos da [Microsoft System Center suite](https://go.microsoft.com/fwlink/p/?LinkId=239437).

Este tópico e seus subtópicos fornecem informações sobre como usar os recursos no console do Gerenciador do servidor. Este tópico contém as seguintes seções.

-   [Examine as considerações iniciais e os requisitos do sistema](#BKMK_1.1)

-   [Tarefas que você pode executar no Gerenciador do servidor](#BKMK_tasks)

-   [Iniciar o Gerenciador do servidor](#BKMK_start)

-   [Reiniciar servidores remotos](#BKMK_restart)

-   [Exportar configurações do Gerenciador do servidor para outros computadores](#BKMK_export)

## <a name="BKMK_1.1"></a>Examine as considerações iniciais e os requisitos do sistema
As seções a seguir listam algumas considerações iniciais que você precisa analisar, bem como requisitos de hardware e software para o Gerenciador do servidor.

### <a name="hardware-requirements"></a>Requisitos de hardware
Gerenciador do servidor é instalado por padrão com todas as edições do Windows Server 2012 R2 e Windows Server 2012. Não existe nenhum requisito de hardware adicional para o Gerenciador do servidor.

### <a name="BKMK_softconfig"></a>Requisitos de software e configuração
Gerenciador do servidor é instalado por padrão em todas as edições do Windows Server 2012. Embora você possa usar o Gerenciador de servidores para gerenciar [opções de instalação Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) do Windows Server 2012 e Windows Server 2008 R2 que são executados em computadores remotos, o Gerenciador do servidor não é executado diretamente na instalação do Server Core Opções.

Para gerenciar totalmente servidores remotos que executam o Windows Server 2008 ou Windows Server 2008 R2, instale as seguintes atualizações nos servidores que você deseja gerenciar, na ordem mostrada.

Para gerenciar servidores que executam o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 usando o Gerenciador do servidor no Windows Server 2012 R2, aplique as seguintes atualizações aos sistemas operacionais mais antigos.

-   [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). O pacote de download do Windows Management Framework 4.0 atualiza os provedores do Windows Management Instrumentation (WMI) no Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008. Os provedores WMI atualizados permitem ao Gerenciador do servidor coletar informações sobre as funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização for aplicada, servidores que executam o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 tem um status de gerenciabilidade **não está acessível**.

-   A atualização de desempenho associada [artigo 2682011 da Base de dados de Conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite que o Gerenciador do servidor para coletar dados de desempenho do Windows Server 2008 e Windows Server 2008 R2. Essa atualização de desempenho não é necessária nos servidores que executam o Windows Server 2012.

Para gerenciar servidores que executam o Windows Server 2008 R2 ou Windows Server 2008, aplique as seguintes atualizações aos sistemas operacionais mais antigos.

-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) pacote de download do Windows Management Framework 3.0 atualiza os provedores do Windows Management Instrumentation (WMI) no Windows Server 2008 e Windows Server 2008 R2. Os provedores WMI atualizados permitem ao Gerenciador do servidor coletar informações sobre as funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização for aplicada, servidores que executam o Windows Server 2008 ou Windows Server 2008 R2 têm um status de gerenciabilidade **não acessível – verificar as versões anteriores executam o Windows Management Framework 3.0**.

-   A atualização de desempenho associada [artigo 2682011 da Base de dados de Conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite que o Gerenciador do servidor para coletar dados de desempenho do Windows Server 2008 e Windows Server 2008 R2.

Gerenciador do servidor é executado na Interface gráfica Minimal Server; ou seja, quando o recurso Shell gráfico de servidor foi desinstalado. O recurso Server Graphical Shell está instalado por padrão no Windows Server 2012 R2 e Windows Server 2012. Se você desinstalar o Server Graphical Shell, o console do Gerenciador do servidor é executado, mas alguns aplicativos ou ferramentas disponíveis no console não estão disponíveis. Navegadores da Internet não podem executar sem o Server Graphical Shell, portanto, páginas e aplicativos, como Ajuda em HTML (a Ajuda mmc F1, por exemplo) não pode ser aberta. Não é possível abrir caixas de diálogo para configurar comentários e atualizações automáticas do Windows quando o Server Graphical Shell não está instalado; comandos que abrem essas caixas de diálogo no console do Gerenciador do servidor são redirecionados para executar **sconfig. cmd**.

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gerenciar computadores remotos em um computador cliente
O console do Gerenciador do servidor está incluído no [ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/?LinkID=304145) para Windows 8.1 e [ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/p/?LinkID=238560) para o Windows 8. Observe que, quando as ferramentas de administração de servidor remoto é instalada em um computador cliente, você não pode gerenciar o computador local usando o Gerenciador do servidor. Gerenciador do servidor não pode ser usado para gerenciar computadores ou dispositivos que estão executando um sistema operacional cliente do Windows. Você pode usar o Gerenciador de servidores para gerenciar somente servidores baseados em Windows.

|Sistema de operacional de origem do Gerenciador do servidor|Direcionado no Windows Server 2012 R2 |Direcionado no Windows Server 2012 |Direcionado no Windows Server 2008 R2 ou Windows Server 2008 |Direcionado no Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 ou Windows Server 2012 |Sem suporte|Suporte completo|Depois de atender aos [Requisitos de software e configuração](#BKMK_softconfig), é possível executar a maioria das tarefas de gerenciamento, mas não a instalação ou desinstalação de funções ou recursos|Suporte limitado, somente status online e offline|
|Windows 8.1 ou Windows Server 2012 R2 |Suporte completo|Suporte completo|Depois de atender aos [Requisitos de software e configuração](#BKMK_softconfig), é possível executar a maioria das tarefas de gerenciamento, mas não a instalação ou desinstalação de funções ou recursos|Suporte limitado, somente status online e offline|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar o Gerenciador do Servidor em um computador cliente

1.  Siga as instruções em [implantar ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/?LinkID=238562) para instalar o Remote Server Administration Tools para Windows 8.1 ou o servidor remoto administração ferramentas para Windows 8.

2.  Sobre o **inicie** tela, clique em **Gerenciador do servidor**. O **Gerenciador do Servidor** está disponível depois que você instalar as Ferramentas de Administração de Servidor Remoto.

3.  Se nem o **ferramentas administrativas** nem a **Gerenciador do servidor** blocos são exibidos na **iniciar** tela depois de instalar as ferramentas de administração de servidor remoto, e pesquisando o Gerenciador do servidor para o **iniciar** tela não mostra os resultados, verifique o **Mostrar ferramentas administrativas** configuração está ativada. Para exibir essa configuração, passe o cursor do mouse sobre o canto superior direito dos **inicie** tela e, em seguida, clique em **configurações**. Se **Mostrar ferramentas administrativas** estiver desativado, ative a configuração para exibir as ferramentas que você instalou como parte das Ferramentas de Administração de Servidor Remoto.

Para obter mais informações sobre como executar o servidor remoto administração ferramentas para Windows 8 para gerenciar servidores remotos, consulte [ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/?LinkID=221055) no Wiki do TechNet.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurar o gerenciamento remoto nos servidores que você deseja gerenciar

> [!IMPORTANT]
> Por padrão, o gerenciamento remoto do Gerenciador do servidor e o Windows PowerShell está habilitado no Windows Server 2012 R2 e Windows Server 2012.

Para executar tarefas de gerenciamento em servidores remotos usando o Gerenciador do servidor, servidores remotos que você deseja gerenciar devem ser configurados para permitir o gerenciamento remoto usando o Gerenciador do servidor e do Windows PowerShell. Se o gerenciamento remoto tiver sido desabilitado no Windows Server 2012 R2 ou Windows Server 2012 e deseja habilitá-la novamente, execute as seguintes etapas.

##### <a name="BKMK_windows"></a>Para configurar o gerenciamento remoto do Gerenciador do servidor no Windows Server 2012 R2 ou Windows Server 2012 por meio da interface do Windows

1.  > [!NOTE]
    > As configurações que são controladas pelo **configurar o gerenciamento remoto** caixa de diálogo não afetam partes do Gerenciador de servidores que usam DCOM para comunicações remotas.

    Siga um destes procedimentos para abrir o Gerenciador do servidor se ele não ainda estiver aberto.

    -   Na barra de tarefas do Windows, clique no botão de Gerenciador do servidor.

    -   Sobre o **inicie** tela, clique em **Gerenciador do servidor**.

2.  No **propriedades** área da **servidores locais** , clique no valor de hyperlink para a **gerenciamento remoto** propriedade.

3.  Faça o seguinte e clique em **OK**.

    -   Para evitar que o computador seja gerenciado remotamente usando o Gerenciador do servidor (ou o Windows PowerShell se ele estiver instalado), desmarque a **habilitar o gerenciamento remoto deste servidor por outros computadores** caixa de seleção.

    -   Para permitir que este computador seja gerenciado remotamente usando o Gerenciador do servidor ou o Windows PowerShell, selecione **habilitar o gerenciamento remoto deste servidor por outros computadores**.

##### <a name="BKMK_ps"></a>Para habilitar o gerenciamento remoto do Gerenciador do servidor no Windows Server 2012 R2 ou Windows Server 2012 usando o Windows PowerShell

1.  Siga um destes procedimentos.

    -   Para executar o Windows PowerShell como administrador a partir o **iniciar** tela, clique com botão direito do **Windows PowerShell** lado a lado e, em seguida, clique em **executar como administrador**.

    -   Para executar o Windows PowerShell como administrador na área de trabalho, clique com botão direito do **Windows PowerShell** atalho na barra de tarefas e clique **executar como administrador**.

2.  Digite o seguinte e, em seguida, pressione **Enter** habilitar todas as exceções de regra de firewall necessárias.

    **SMremoting.exe configurar-habilitar**

    > [!NOTE]
    > Esse comando também funciona em um prompt de comando aberto com direitos de usuário com privilégios elevados (Executar como Administrador).

    Se habilitar o gerenciamento remoto falhar, consulte [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) no Microsoft TechNet para solução de problemas e práticas recomendadas.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor e do Windows PowerShell em sistemas operacionais mais antigos

-   Siga um destes procedimentos.

    -   Para habilitar o gerenciamento remoto nos servidores que executam o Windows Server 2008 R2, consulte [com o Gerenciador do servidor de gerenciamento remoto](https://go.microsoft.com/fwlink/?LinkID=137378) na Ajuda do Windows Server 2008 R2.

    -   Para habilitar o gerenciamento remoto nos servidores que executam o Windows Server 2008, consulte [habilitar e usar comandos remotos no Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

    -   Para habilitar o gerenciamento remoto em servidores com o Windows Server 2003, habilite as exceções DCOM de WMI no Firewall do Windows. Para obter mais informações sobre como fazer isso em servidores com o Windows Server 2003, consulte [Conectando através do firewall do Windows](https://msdn.microsoft.com/library/aa389286.aspx) no MSDN.

## <a name="BKMK_tasks"></a>Tarefas que você pode executar no Gerenciador do servidor
Gerenciador do servidor torna a administração de servidor mais eficiente permitindo que os administradores executar as tarefas na tabela a seguir usando uma única ferramenta. No Windows Server 2012 R2 e Windows Server 2012, os usuários padrão de um servidor e os membros do grupo Administradores podem realizar tarefas de gerenciamento no Gerenciador do servidor, mas por padrão, os usuários padrão são impedidos de executar algumas tarefas, conforme mostrado no tabela a seguir.

Os administradores podem usar dois cmdlets do Windows PowerShell no módulo de cmdlet do Gerenciador do servidor, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) e [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), para controle ainda mais o acesso de usuário padrão a alguns dados adicionais. O **Enable-ServerManagerStandardUserremoting** cmdlet pode fornecer acesso de usuários padrão, não-administrador, um ou mais eventos, serviços, contador de desempenho e dados de inventário de funções e recursos.

> [!IMPORTANT]
> Gerenciador de servidores não podem ser usados para gerenciar uma versão mais recente do sistema operacional Windows Server. Gerenciador do servidor em execução no Windows Server 2012 ou Windows 8 não pode ser usado para gerenciar servidores que executam o Windows Server 2012 R2.

|Descrição da tarefa|Administradores (inclusive a conta de Administrador interno)|Usuários do servidor padrão|
|----------|----------------------------------|-------------|
|Adicionar servidores remotos a um pool de servidores que o Gerenciador do servidor pode ser usado para gerenciar.|Sim|Não|
|criar e editar grupos personalizados de servidores, como servidores que estão em uma localização geográfica específica ou atender a uma finalidade específica.|Sim|Sim|
|Instalar ou desinstalar funções, serviços de função e recursos no local ou em servidores remotos que estão executando o Windows Server 2012 R2 ou Windows Server 2012. Para obter definições de funções, serviços de função e recursos, consulte [funções, serviços de função e recursos](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Sim|Não|
|Exibir e fazer alterações em funções e recursos do servidor instalados em servidores locais ou remotos. **Observação:** No Gerenciador do servidor, os dados de funções e recursos são exibidos no idioma base do sistema, também chamado de idioma de GUI padrão do sistema ou o idioma selecionado durante a instalação do sistema operacional.|Sim|Os usuários padrão podem exibir e gerenciar funções e recursos, além de executar tarefas como exibir eventos de função, mas não podem adicionar ou remover serviços de função.|
|Inicie as ferramentas de gerenciamento, como o Windows PowerShell ou o snap-ins do mmc. Você pode iniciar uma sessão do Windows PowerShell destinada a um servidor remoto clicando com o servidor na **servidores** lado a lado e, em seguida, clicando em **Windows PowerShell**. Você pode iniciar snap-ins do mmc do **ferramentas** menu do console do Gerenciador do servidor e, em seguida, apontar o mmc para um computador remoto depois que o snap-in estiver aberto.|Sim|Sim|
|Gerenciar servidores remotos com credenciais diferentes clicando com o botão direito do mouse no bloco **Servidores** e clicando em **Gerenciar como**. Você pode usar **Gerenciar como** para tarefas gerais de gerenciamento de servidores e Serviços de Arquivo e Armazenamento.|Sim|Não|
|Executar tarefas de gerenciamento associadas com o ciclo de vida operacional de servidores, como iniciar ou parar serviços; e iniciar outras ferramentas que permitem que você configure um servidor de configurações de rede, usuários e grupos e conexões de área de trabalho remota.|Sim|Os usuários padrão não podem iniciar ou parar serviços. Eles podem alterar o nome do servidor local, grupo de trabalho, ou associação de domínio e configurações de área de trabalho remota, mas for solicitados pelo controle de conta de usuário para fornecer as credenciais de administrador para que poderem concluir essas tarefas. Eles não podem alterar as configurações de gerenciamento remoto.|
|Executar tarefas de gerenciamento associadas ao ciclo de vida operacional de funções instaladas em servidores, incluindo a verificação de funções para garantir a conformidade com as práticas recomendadas.|Sim|Os usuários padrão não podem executar varreduras do analisador de práticas recomendadas.|
|Determinar o status do servidor, identificar eventos críticos e analisar e solucionar falhas ou problemas de configuração.|Sim|Sim|
|Personalize os eventos, dados de desempenho, serviços e resultados do analisador de práticas recomendadas sobre o qual você deseja ser alertado no dashboard do Gerenciador do servidor.|Sim|Sim|
|Reiniciar servidores.|Sim|Não|
|Atualize os dados que são exibidos no console do Gerenciador do servidor sobre servidores gerenciados.|Sim|Não|

> [!NOTE]
> Gerenciador de servidores podem receber status apenas online ou offline dos servidores que estão executando o Windows Server 2003. Gerenciador do servidor não pode ser usado para adicionar funções e recursos aos servidores que estão executando o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_start"></a>Iniciar o Gerenciador do servidor
Gerenciador do servidor é iniciado automaticamente por padrão nos servidores que estão executando o Windows Server 2012, quando um membro do grupo Administradores faz logon em um servidor. Se você fechar o Gerenciador do servidor, reinicie-o em uma das seguintes maneiras. Esta seção também contém etapas para alterar o comportamento padrão e impedindo que o Gerenciador do servidor seja iniciado automaticamente.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Para iniciar o Gerenciador do servidor na tela inicial

-   Sobre o Windows **inicie** tela, clique no **Gerenciador do servidor** lado a lado.

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Para iniciar o Gerenciador do Servidor na área de trabalho do Windows

-   Na barra de tarefas do Windows, clique em **Gerenciador de Arquivos**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Para impedir que o Gerenciador do Servidor seja iniciado automaticamente

1.  No console do Gerenciador do servidor, sobre o **gerenciar** menu, clique em **propriedades do Gerenciador do servidor**.

2.  Na caixa de diálogo **Propriedades do Gerenciador do Servidor** , marque a caixa de seleção **Não iniciar o Gerenciador do Servidor automaticamente no logon**. Clique em **OK**.

3.  Como alternativa, você pode impedir que o Gerenciador do servidor iniciado automaticamente habilitando a configuração de política de grupo **iniciar o Gerenciador do servidor automaticamente no logon**. O caminho para essa configuração de política, no console do editor de diretiva de Grupo Local, é o computador computador\Modelos administrativos\sistema\gerenciador do servidor.

## <a name="BKMK_restart"></a>Reiniciar servidores remotos
Você pode reiniciar um servidor remoto a partir de **servidores** lado a lado de uma página de função ou grupo no Gerenciador do servidor.

> [!IMPORTANT]
> Ao reiniciar um servidor remoto, o servidor é forçado a reiniciar, mesmo que ainda haja usuários conectados ao servidor remoto e programas abertos com dados não salvos. Esse comportamento é diferente de fechar ou reiniciar o computador local, quando seria solicitado que você salvasse dados não salvos do programa e verificasse se deseja forçar o logoff dos usuários conectados. Verifique se você pode forçar o logoff de outros usuários de servidores remotos e se pode descartar os dados não salvos em programas em execução nos servidores remotos.
> 
> Se ocorrer uma atualização automática no Gerenciador do servidor enquanto um servidor gerenciado é desligar e reiniciar, o status de atualização e capacidade de gerenciamento podem ocorrer erros no servidor gerenciado, porque o Gerenciador do servidor não pode se conectar ao servidor remoto até que ela seja concluída reiniciando.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Para reiniciar servidores remotos no Gerenciador do Servidor

1.  Abra uma página de home de grupo de funções ou servidores no Gerenciador do servidor.

2.  Selecione um ou mais servidores remotos que você adicionou ao Gerenciador do servidor. Pressione e segure **Ctrl** enquanto clica para selecionar vários servidores de uma vez. Para obter mais informações sobre como adicionar servidores ao pool de servidores do Gerenciador do servidor, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).

3.  Clique com o botão direito do mouse nos servidores selecionados e clique em **Reiniciar Servidor**.

## <a name="BKMK_export"></a>Exportar configurações do Gerenciador do servidor para outros computadores
No Gerenciador do servidor, sua lista de servidores gerenciados, as alterações nas configurações de console do Gerenciador do servidor e grupos personalizados que você criou são armazenados em dois arquivos a seguir. Você pode reutilizar essas configurações em outros computadores que estão executando a mesma versão do Gerenciador do servidor (não em computadores que estão executando a opção de instalação Server Core) ou o Windows 8. Ferramentas de administração de servidor remoto deve estar em execução em computadores cliente no Windows para exportar configurações do Gerenciador do servidor para esses computadores.

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   As credenciais Gerenciar como (ou alternativas) para servidores de seu pool de servidores não são armazenadas no perfil móvel. Os usuários do Gerenciador do servidor devem adicioná-los em cada computador do qual deseja gerenciar.
> -   O perfil móvel do compartilhamento de rede é criado somente depois que o usuário faz logon na rede e depois faz logoff pela primeira vez. O arquivo **Serverlist.xml** é criado nesse momento.

Exportar configurações do Gerenciador do servidor, verifique as configurações do Gerenciador de servidores portáteis ou usá-los em outros computadores em uma das duas maneiras a seguir.

-   Para exportar as configurações para outro computador ingressado no domínio, configure o usuário do Gerenciador do servidor para ter um perfil móvel no active directory de usuários e computadores. Você deve ser um administrador de domínio para alterar as propriedades de usuário no active directory de usuários e computadores.

-   Para exportar as configurações para outro computador em um grupo de trabalho, copie os dois arquivos anteriores para o mesmo local no computador do qual você deseja gerenciar usando o Gerenciador do servidor.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Para exportar as configurações do Gerenciador do Servidor para outros computadores associados ao domínio

1.  Em computadores e usuários do Active Directory, abra o **propriedades** caixa de diálogo para um usuário do Gerenciador do servidor.

2.  Sobre o **perfil** guia, adicione um caminho para um compartilhamento de rede para armazenar o perfil do usuário.

3.  Siga um destes procedimentos.

    -   Nos EUA Inglês (en-us) compilações, muda para o **ServerList** arquivo são salvas automaticamente no perfil. Vá para a próxima etapa.

    -   Em outras compilações, copie os dois seguintes arquivos do computador que está executando o Gerenciador do servidor para o compartilhamento de rede que faz parte do perfil móvel do usuário.

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Clique em **OK** para salvar suas alterações e fechar a caixa de diálogo **Propriedades** .

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Para exportar as configurações do Gerenciador do Servidor para computadores em grupos de trabalho

-   Em um computador do qual você deseja gerenciar servidores remotos, substitua os dois arquivos a seguir com os mesmos arquivos de outro computador que está executando o Gerenciador do servidor, e que tem as configurações desejadas.

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config


