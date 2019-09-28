---
title: Gerenciador do Servidor
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d996ef40-8bcc-42b0-b6ae-806b828223f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 373e2063622317905686b1c5fc74425943abd9ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383061"
---
# <a name="server-manager"></a>Gerenciador do Servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gerenciador do Servidor é um console de gerenciamento do Windows Server que ajuda os profissionais de ti a provisionar e gerenciar servidores locais e remotos baseados no Windows a partir de seus desktops, sem a necessidade de acesso físico aos servidores ou a necessidade de habilitar Área de Trabalho Remota conexões de protocolo (rdP) para cada servidor. Embora Gerenciador do Servidor esteja disponível no Windows Server 2008 R2 e no Windows Server 2008, o Gerenciador do Servidor foi atualizado no Windows Server 2012 para oferecer suporte ao gerenciamento remoto, multisservidor e ajudar a aumentar o número de servidores que um administrador pode gerenciar.

Em nossos testes, Gerenciador do Servidor no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012 podem ser usados para gerenciar até 100 servidores, dependendo das cargas de trabalho que os servidores estão executando. O número de servidores que podem ser gerenciadas usando um único console do Gerenciador de servidores pode variar dependendo da quantidade de dados que você solicita do servidores gerenciados e recursos de hardware e rede disponíveis para o computador que executa o Gerenciador de servidores. Conforme a quantidade de dados que você deseja exibir se aproxima de capacidade do recurso desse computador, você pode enfrentar respostas lentas do Gerenciador de servidores e atrasos na finalização de atualizações. Para ajudar a aumentar o número de servidores que você pode gerenciar usando o Gerenciador de servidores, é recomendável limitar os dados de evento que o Gerenciador do servidor obtém dos seus servidores gerenciados usando as configurações na caixa de diálogo **Configurar dados de eventos** . É possível abrir Configurar Dados do Evento no menu **Tarefas** do bloco **Eventos** . Se você precisar gerenciar um número de nível empresarial de servidores em sua organização, é recomendável avaliar produtos da [Microsoft System Center suite](https://go.microsoft.com/fwlink/p/?LinkId=239437).

Este tópico e seus subtópicos fornecem informações sobre como usar recursos no console do Gerenciador do Servidor. Este tópico contém as seguintes seções.

-   [Examine as considerações iniciais e os requisitos do sistema](#review-initial-considerations-and-system-requirements)

-   [Tarefas que podem ser executadas no Gerenciador do Servidor](#tasks-that-you-can-perform-in-server-manager)

-   [iniciar Gerenciador do Servidor](#start-server-manager)

-   [Reiniciar servidores remotos](#restart-remote-servers)

-   [Exportar configurações de Gerenciador do Servidor para outros computadores](#export-server-manager-settings-to-other-computers)

## <a name="review-initial-considerations-and-system-requirements"></a>Examinar as considerações iniciais e os requisitos do sistema
As seções a seguir listam algumas considerações iniciais que você precisa examinar, bem como os requisitos de hardware e software para Gerenciador do Servidor.

### <a name="hardware-requirements"></a>Requisitos de hardware
O Gerenciador do Servidor é instalado por padrão com todas as edições do Windows Server 2016. Não existem requisitos de hardware adicionais para Gerenciador do Servidor.

### <a name="software-and-configuration-requirements"></a>Requisitos de software e configuração
O Gerenciador do Servidor é instalado por padrão com todas as edições do Windows Server 2016. Você pode usar Gerenciador do Servidor no Windows Server 2016 para gerenciar [as opções de instalação do Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) do windows Server 2016, do windows Server 2012 e do windows Server 2008 R2 que estão sendo executados em computadores remotos. Gerenciador do Servidor executa na opção de instalação Server Core do Windows Server 2016.

O Gerenciador do Servidor é executado na interface gráfica mínima do servidor; ou seja, quando o recurso de shell gráfico do servidor não está instalado. O recurso de shell gráfico do servidor não é instalado por padrão no Windows Server 2016. Se você não estiver executando o shell gráfico do servidor, o console do Gerenciador do Servidor será executado, mas alguns aplicativos ou ferramentas disponíveis no console do não estarão disponíveis. Os navegadores da Internet não podem ser executados sem o shell gráfico do servidor, portanto, as páginas da Web e os aplicativos, como a ajuda HTML (a ajuda F1 do MMC, por exemplo) não podem ser abertos. Não é possível abrir caixas de diálogo para configurar a atualização automática do Windows e comentários quando o shell gráfico do servidor não está instalado; comandos que abrem essas caixas de diálogo no console do Gerenciador do Servidor são redirecionados para executar **sconfig. cmd**.

Para gerenciar servidores que executam versões anteriores ao Windows Server 2016, instale o software e as atualizações a seguir para fazer com que as versões mais antigas do Windows Server sejam gerenciadas usando Gerenciador do Servidor no Windows Server 2016.

|Sistema operacional|Software necessário|
|----------|-----------|
| Windows Server 2012 R2 ou Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). O pacote de download do Windows Management Framework 5,0 atualiza os provedores de Instrumentação de Gerenciamento do Windows (WMI) no Windows Server 2012 R2 e no Windows Server 2012. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2012 R2 ou o Windows Server 2012 têm um status de capacidade de gerenciamento **não acessível**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) não é mais necessária em servidores que executam o windows Server 2012 R2 ou o windows Server 2012.|
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). O pacote de download do Windows Management Framework 4,0 atualiza os provedores de Instrumentação de Gerenciamento do Windows (WMI) no Windows Server 2008 R2. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2008 R2 têm um status de capacidade de gerenciamento **não acessível**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite Gerenciador do servidor coletar dados de desempenho do Windows Server 2008 R2.|
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) the Windows management Framework 3,0 download Package updates Instrumentação de gerenciamento do Windows (WMI) Providers no Windows Server 2008. Os provedores WMI atualizados permitem que Gerenciador do Servidor coletem informações sobre funções e recursos que estão instalados nos servidores gerenciados. Até que a atualização seja aplicada, os servidores que executam o Windows Server 2008 têm um status de capacidade de gerenciamento **não acessível-Verifique se as versões anteriores executam o Windows Management Framework 3,0**.<br />-A atualização de desempenho associada ao [artigo 2682011 da base de dados de conhecimento](https://go.microsoft.com/fwlink/p/?LinkID=245487) permite Gerenciador do servidor coletar dados de desempenho do Windows Server 2008.|

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gerenciar computadores remotos em um computador cliente
O console do Gerenciador do Servidor está incluído no [ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/?LinkID=404281) para Windows 10. Observe que quando o Ferramentas de Administração de Servidor Remoto é instalado em um computador cliente, não é possível gerenciar o computador local usando Gerenciador do Servidor; Gerenciador do Servidor não pode ser usado para gerenciar computadores ou dispositivos que estejam executando um sistema operacional cliente Windows. Você só pode usar Gerenciador do Servidor para gerenciar servidores baseados no Windows.

|Sistema operacional de origem Gerenciador do Servidor|Destinado ao Windows Server 2016|Destinado ao Windows Server 2012 R2 |Destinado ao Windows Server 2012 |Destinado ao Windows Server 2008 R2 ou ao Windows Server 2008 |Direcionado no Windows Server 2003|
|-------------------------------|--------------------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 10 ou Windows Server 2016|Suporte completo|Suporte completo|Suporte completo|Depois de atender aos [Requisitos de software e configuração](#software-and-configuration-requirements), é possível executar a maioria das tarefas de gerenciamento, mas não a instalação ou desinstalação de funções ou recursos|Sem suporte|
|Windows 8.1 ou Windows Server 2012 R2 |Sem suporte|Suporte completo|Suporte completo|Depois de atender aos [Requisitos de software e configuração](#software-and-configuration-requirements), é possível executar a maioria das tarefas de gerenciamento, mas não a instalação ou desinstalação de funções ou recursos|Suporte limitado, somente status online e offline|
|Windows 8 ou Windows Server 2012 |Sem suporte|Sem suporte|Suporte completo|Depois de atender aos [Requisitos de software e configuração](#software-and-configuration-requirements), é possível executar a maioria das tarefas de gerenciamento, mas não a instalação ou desinstalação de funções ou recursos|Suporte limitado, somente status online e offline|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar o Gerenciador do Servidor em um computador cliente

1.  Siga as instruções em [ferramentas de administração de servidor remoto](../../remote/remote-server-administration-tools.md) para instalar o ferramentas de administração de servidor remoto para Windows 10.

2.  Na tela **Iniciar** , clique em **Gerenciador do servidor**. O **Gerenciador do Servidor** está disponível depois que você instalar as Ferramentas de Administração de Servidor Remoto.

3.  se nem as **Ferramentas administrativas** nem os blocos de **Gerenciador do servidor** são exibidos na tela **inicial** após a instalação do Ferramentas de Administração de Servidor Remoto, e a pesquisa de Gerenciador do servidor na tela **inicial** não é exibida resultados, verifique se a configuração **Mostrar ferramentas administrativas** está ativada. Para exibir essa configuração, passe o cursor do mouse sobre o canto superior direito da tela **inicial** e clique em **configurações**. Se **Mostrar ferramentas administrativas** estiver desativado, ative a configuração para exibir as ferramentas que você instalou como parte das Ferramentas de Administração de Servidor Remoto.

para obter mais informações sobre como executar Ferramentas de Administração de Servidor Remoto para o Windows 10 para gerenciar servidores remotos, consulte [ferramentas de administração de servidor remoto](https://go.microsoft.com/fwlink/?LinkID=221055) no wiki do TechNet.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurar o gerenciamento remoto nos servidores que você deseja gerenciar

> [!IMPORTANT]
> Por padrão, o Gerenciador do Servidor e o gerenciamento remoto do Windows PowerShell estão habilitados no Windows Server 2016.

Para executar tarefas de gerenciamento em servidores remotos usando Gerenciador do Servidor, os servidores remotos que você deseja gerenciar devem ser configurados para permitir o gerenciamento remoto usando o Gerenciador do Servidor e o Windows PowerShell. Se o gerenciamento remoto tiver sido desabilitado no Windows Server 2012 R2 ou no Windows Server 2012 e você quiser habilitá-lo novamente, execute as etapas a seguir.

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a>Para configurar Gerenciador do Servidor gerenciamento remoto no Windows Server 2012 R2 ou no Windows Server 2012 usando a interface do Windows

1.  > [!NOTE]
    > As configurações controladas pela caixa de diálogo **Configurar gerenciamento remoto** não afetam partes de Gerenciador do servidor que usam DCOM para comunicações remotas.

    Siga um destes procedimentos para abrir Gerenciador do Servidor se ele ainda não estiver aberto.

    -   Na barra de tarefas do Windows, clique no botão Gerenciador do Servidor.

    -   Na tela **Iniciar** , clique em **Gerenciador do servidor**.

2.  Na área **Propriedades** da página **servidores locais** , clique no valor de hiperlink para a propriedade **gerenciamento remoto** .

3.  Faça o seguinte e clique em **OK**.

    -   Para impedir que este computador seja gerenciado remotamente usando Gerenciador do Servidor (ou o Windows PowerShell, se estiver instalado), desmarque a caixa de seleção **habilitar gerenciamento remoto deste servidor em outros computadores** .

    -   Para permitir que este computador seja gerenciado remotamente usando o Gerenciador do Servidor ou o Windows PowerShell, selecione **habilitar o gerenciamento remoto deste servidor em outros computadores**.

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a>Para habilitar Gerenciador do Servidor gerenciamento remoto no Windows Server 2012 R2 ou no Windows Server 2012 usando o Windows PowerShell

1.  Siga um destes procedimentos.

    -   Para executar o Windows PowerShell como administrador na tela **inicial** , clique com o botão direito do mouse no bloco **Windows PowerShell** e clique em **Executar como administrador**.

    -   Para executar o Windows PowerShell como administrador na área de trabalho, clique com o botão direito do mouse no atalho do **Windows PowerShell** na barra de tarefas e clique em **Executar como administrador**.

2.  Digite o seguinte e pressione **Enter** para habilitar todas as exceções de regra de firewall necessárias.

    **Configure-SMRemoting. exe-Enable**

    > [!NOTE]
    > Esse comando também funciona em um prompt de comando aberto com direitos de usuário com privilégios elevados (Executar como Administrador).

    se a habilitação do gerenciamento remoto falhar, consulte [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) no Microsoft TechNet para obter dicas de solução de problemas e práticas recomendadas.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Para habilitar o gerenciamento remoto do Gerenciador do Servidor e do Windows PowerShell em sistemas operacionais mais antigos

-   Siga um destes procedimentos.

    -   Para habilitar o gerenciamento remoto em servidores que executam o Windows Server 2008 R2, consulte [gerenciamento remoto com Gerenciador do servidor](https://go.microsoft.com/fwlink/?LinkID=137378) na ajuda do Windows Server 2008 R2.

    -   Para habilitar o gerenciamento remoto em servidores que executam o Windows Server 2008, consulte [habilitar e usar comandos remotos no Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

## <a name="tasks-that-you-can-perform-in-server-manager"></a>Tarefas que você pode realizar no Gerenciador de Servidores
Gerenciador do Servidor torna a administração do servidor mais eficiente, permitindo que os administradores realizem tarefas na tabela a seguir usando uma única ferramenta. No Windows Server 2012 R2 e no Windows Server 2012, os usuários padrão de um servidor e os membros do grupo Administradores podem executar tarefas de gerenciamento no Gerenciador do Servidor, mas por padrão, os usuários padrão são impedidos de executar algumas tarefas, conforme mostrado no tabela a seguir.

Os administradores podem usar dois cmdlets do Windows PowerShell no módulo cmdlet Gerenciador do Servidor, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) e [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), para controlar ainda mais o acesso de usuário padrão a alguns dados adicionais. O cmdlet **Enable-ServerManagerStandardUserremoting** pode fornecer um ou mais usuários padrão, não administradores, acesso aos dados de inventário de eventos, serviços, contadores de desempenho e funções e recursos.

> [!IMPORTANT]
> Gerenciador de servidores não podem ser usados para gerenciar uma versão mais recente do sistema operacional Windows Server. Gerenciador do Servidor em execução no Windows Server 2012 ou no Windows 8 não podem ser usadas para gerenciar servidores que executam o Windows Server 2012 R2.

|Descrição da tarefa|Administradores (inclusive a conta de Administrador interno)|Usuários do servidor padrão|
|----------|----------------------------------|-------------|
|Adicione servidores remotos a um pool de servidores que Gerenciador do Servidor podem ser usados para gerenciar o.|Sim|Não|
|Crie e edite grupos personalizados de servidores, como servidores que estão em uma localização geográfica específica ou que atendem a uma finalidade específica.|Sim|Sim|
|Instale ou desinstale funções, serviços de função e recursos no local ou em servidores remotos que estejam executando o Windows Server 2012 R2 ou o Windows Server 2012. Para obter definições de funções, serviços de função e recursos, consulte [funções, serviços de função e recursos](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Sim|Não|
|Exibir e fazer alterações em funções e recursos do servidor instalados em servidores locais ou remotos. **Observação:** No Gerenciador do Servidor, os dados de função e de recurso são exibidos no idioma base do sistema, também chamado de idioma de GUI padrão do sistema ou no idioma selecionado durante a instalação do sistema operacional.|Sim|Os usuários padrão podem exibir e gerenciar funções e recursos, além de executar tarefas como exibir eventos de função, mas não podem adicionar ou remover serviços de função.|
|Inicie ferramentas de gerenciamento, como o Windows PowerShell ou snap-ins do MMC. Você pode iniciar uma sessão do Windows PowerShell direcionada ao servidor remoto clicando com o botão direito do mouse no bloco **servidores** e clicando em **Windows PowerShell**. Você pode iniciar os snap-ins do MMC no menu **ferramentas** do console do Gerenciador do servidor e, em seguida, apontar o MMC para um computador remoto depois que o snap-in estiver aberto.|Sim|Sim|
|Gerenciar servidores remotos com credenciais diferentes clicando com o botão direito do mouse no bloco **Servidores** e clicando em **Gerenciar como**. Você pode usar **Gerenciar como** para tarefas gerais de gerenciamento de servidores e Serviços de Arquivo e Armazenamento.|Sim|Não|
|Executar tarefas de gerenciamento associadas ao ciclo de vida operacional de servidores, como iniciar ou interromper serviços; e inicie outras ferramentas que permitem que você defina as configurações de rede, os usuários e os grupos e as conexões de Área de Trabalho Remota de um servidor.|Sim|Os usuários padrão não podem iniciar ou parar serviços. Eles podem alterar o nome do servidor local, o grupo de trabalho ou a associação de domínio e as configurações de Área de Trabalho Remota, mas são solicitados pelo controle de conta de usuário para fornecer credenciais de administrador antes que eles possam concluir essas tarefas. Eles não podem alterar as configurações de gerenciamento remoto.|
|Executar tarefas de gerenciamento associadas ao ciclo de vida operacional de funções instaladas em servidores, incluindo a verificação de funções para garantir a conformidade com as práticas recomendadas.|Sim|Os usuários padrão não podem executar verificações de Analisador de Práticas Recomendadas.|
|Determinar o status do servidor, identificar eventos críticos e analisar e solucionar falhas ou problemas de configuração.|Sim|Sim|
|Personalize os eventos, dados de desempenho, serviços e Analisador de Práticas Recomendadas resultados sobre os quais você deseja ser alertado no painel de Gerenciador do Servidor.|Sim|Sim|
|Reiniciar servidores.|Sim|Não|
|Atualize os dados que são exibidos no console do Gerenciador do Servidor sobre servidores gerenciados.|Sim|Não|

> [!NOTE]
> Gerenciador do Servidor não pode ser usado para adicionar funções e recursos a servidores que executam o Windows Server 2008 R2 ou o Windows Server 2008.

## <a name="start-server-manager"></a>iniciar Gerenciador do Servidor
O Gerenciador do Servidor é iniciado automaticamente por padrão em servidores que executam o Windows Server 2016 quando um membro do grupo Administradores faz logon em um servidor. Se você fechar Gerenciador do Servidor, reinicie-o de uma das maneiras a seguir. Esta seção também contém etapas para alterar o comportamento padrão e impedir que Gerenciador do Servidor sejam iniciadas automaticamente.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Para iniciar a Gerenciador do Servidor na tela inicial

-   Na tela **Iniciar** do Windows, clique no bloco **Gerenciador do servidor** .

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Para iniciar o Gerenciador do Servidor na área de trabalho do Windows

-   Na barra de tarefas do Windows, clique em **Gerenciador de Arquivos**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Para impedir que o Gerenciador do Servidor seja iniciado automaticamente

1.  No console do Gerenciador do Servidor, no menu **gerenciar** , clique em **Propriedades de Gerenciador do servidor**.

2.  Na caixa de diálogo **Propriedades do Gerenciador do Servidor** , marque a caixa de seleção **Não iniciar o Gerenciador do Servidor automaticamente no logon**. Clique em **OK**.

3.  Como alternativa, você pode impedir a inicialização automática de Gerenciador do Servidor habilitando a configuração de Política de Grupo, **não iniciar Gerenciador do servidor automaticamente no logon**. O caminho para essa configuração de política, no console do editor de Política de Grupo local, é o Computador\modelos Templates\System\Server Manager.

## <a name="restart-remote-servers"></a>Reiniciar servidores remotos
Você pode reiniciar um servidor remoto a partir do bloco **servidores** de uma página de função ou grupo no Gerenciador do servidor.

> [!IMPORTANT]
> Ao reiniciar um servidor remoto, o servidor é forçado a reiniciar, mesmo que ainda haja usuários conectados ao servidor remoto e programas abertos com dados não salvos. Esse comportamento é diferente de fechar ou reiniciar o computador local, quando seria solicitado que você salvasse dados não salvos do programa e verificasse se deseja forçar o logoff dos usuários conectados. Verifique se você pode forçar o logoff de outros usuários de servidores remotos e se pode descartar os dados não salvos em programas em execução nos servidores remotos.
> 
> Se ocorrer uma atualização automática no Gerenciador do Servidor enquanto um servidor gerenciado estiver desligando, os erros de status de atualização e de capacidade de gerenciamento poderão ocorrer para o servidor gerenciado, porque Gerenciador do Servidor não pode se conectar ao servidor remoto até que ele seja concluído reiniciar.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Para reiniciar servidores remotos no Gerenciador do Servidor

1.  Abra uma função ou grupo de servidores home page em Gerenciador do Servidor.

2.  Selecione um ou mais servidores remotos que você adicionou ao Gerenciador do Servidor. Pressione e segure **Ctrl** enquanto clica para selecionar vários servidores de uma vez. Para obter mais informações sobre como adicionar servidores ao pool de servidores do Gerenciador do Servidor, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).

3.  Clique com o botão direito do mouse nos servidores selecionados e clique em **Reiniciar Servidor**.

## <a name="export-server-manager-settings-to-other-computers"></a>Exportar configurações de Gerenciador do Servidor para outros computadores
No Gerenciador do Servidor, sua lista de servidores gerenciados, alterações em Gerenciador do Servidor configurações do console e grupos personalizados que você criou são armazenados nos dois arquivos a seguir. Você pode reutilizar essas configurações em outros computadores que estejam executando a mesma versão do Gerenciador do Servidor (ou Windows 10 com o Ferramentas de Administração de Servidor Remoto instalado). Ferramentas de Administração de Servidor Remoto deve estar em execução em computadores baseados no cliente Windows para exportar configurações de Gerenciador do Servidor para esses computadores.

-   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.xml

-   %*AppData*% \ Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   As credenciais Gerenciar como (ou alternativas) para servidores de seu pool de servidores não são armazenadas no perfil móvel. Os usuários do Gerenciador do servidor devem adicioná-los em cada computador do qual deseja gerenciar.
> -   O perfil móvel do compartilhamento de rede é criado somente depois que o usuário faz logon na rede e depois faz logoff pela primeira vez. O arquivo **Serverlist.xml** é criado nesse momento.

Você pode exportar Gerenciador do Servidor configurações, tornar Gerenciador do Servidor configurações portáteis ou usá-las em outros computadores de uma das duas maneiras a seguir.

-   Para exportar configurações para outro computador ingressado no domínio, configure o Gerenciador do Servidor usuário para ter um perfil móvel em usuários e computadores do Active Directory. Você deve ser um administrador de domínio para alterar as propriedades do usuário em usuários e computadores do Active Directory.

-   Para exportar configurações para outro computador em um grupo de trabalho, copie os dois arquivos anteriores para o mesmo local no computador do qual você deseja gerenciar usando Gerenciador do Servidor.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Para exportar as configurações do Gerenciador do Servidor para outros computadores associados ao domínio

1.  Em usuários e computadores do Active Directory, abra a caixa de diálogo **Propriedades** de um usuário Gerenciador do servidor.

2.  Na guia **perfil** , adicione um caminho a um compartilhamento de rede para armazenar o perfil do usuário.

3.  Siga um destes procedimentos.

    -   Nos EUA Compilações em inglês (en-US), as alterações no arquivo **ServerList. xml** são salvas automaticamente no perfil. Vá para a próxima etapa.

    -   Em outras compilações, copie os dois arquivos a seguir do computador que está executando Gerenciador do Servidor para o compartilhamento de rede que faz parte do perfil móvel do usuário.

        -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*LocalAppData*% \ Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Clique em **OK** para salvar suas alterações e fechar a caixa de diálogo **Propriedades** .

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Para exportar as configurações do Gerenciador do Servidor para computadores em grupos de trabalho

-   Em um computador do qual você deseja gerenciar servidores remotos, substitua os dois arquivos a seguir pelos mesmos arquivos de outro computador que esteja executando o Gerenciador do Servidor e que tenha as configurações desejadas.

    -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*LocalAppData*% \ Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config



