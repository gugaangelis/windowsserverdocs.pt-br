---
title: Instalar ou desinstalar funções, serviços de função ou recursos
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cca2e4c7ba2658c4d85b14ef61ef5f79fbc96345
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383194"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Instalar ou desinstalar funções, serviços de função ou recursos

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, o console do Gerenciador do Servidor e os cmdlets do Windows PowerShell para Gerenciador do Servidor permitem a instalação de funções e recursos em servidores locais ou remotos ou VHDs (discos rígidos virtuais) offline. Você pode instalar várias funções e recursos em um único servidor remoto ou VHD offline em um único assistente para adicionar funções e recursos ou na sessão do Windows PowerShell.  
  
> [!IMPORTANT]  
> Gerenciador de servidores não podem ser usados para gerenciar uma versão mais recente do sistema operacional Windows Server. Gerenciador do Servidor em execução no Windows Server 2012 R2 ou Windows 8.1 não podem ser usadas para instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2016.  
  
Você deve estar conectado a um servidor como administrador para instalar ou desinstalar funções, serviços de função e recursos. Caso esteja conectado ao computador remoto com uma conta sem direitos de administrador no servidor de destino, clique com o botão direito no servidor de destino, no bloco **Servidores**, e clique em **Gerenciar como** para fornecer uma conta com direitos de administrador. O servidor no qual você deseja montar o VHD offline deve ser adicionado ao Gerenciador do Servidor, e você deve ter direitos de Administrador nesse servidor.  
  
Para obter mais informações sobre quais funções, serviços de função e recursos são, consulte [funções, serviços de função e recursos](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
Este tópico contém as seguintes seções.  
  
-   [Instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [Instalar funções, serviços de função e recursos usando os cmdlets do Windows PowerShell](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Remover funções, serviços de função e recursos usando o assistente para remover funções e recursos](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [Remover funções, serviços de função e recursos usando os cmdlets do Windows PowerShell](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Instalar funções e recursos em vários servidores executando um script do Windows PowerShell](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [Instalar o .NET Framework 3.5 e outros recursos sob demanda](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>Instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos  
Em uma única sessão do assistente Adicionar funções e recursos, você pode instalar funções, serviços de função e recursos no servidor local, um servidor remoto que foi adicionado ao Gerenciador do Servidor ou um VHD offline. Para obter mais informações sobre como adicionar um servidor ao Gerenciador do Servidor para gerenciar, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Se estiver executando Gerenciador do Servidor no Windows Server 2016 ou no Windows 10, você poderá usar o assistente para adicionar funções e recursos para instalar funções e recursos somente em servidores e VHDs offline que estejam executando o Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Para instalar funções e recursos usando o assistente para adicionar funções e recursos  
  
1.  Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.  
  
    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.  
  
    -   Na tela **Iniciar** do Windows, clique no bloco **Gerenciador do servidor** .  
  
2.  No menu **gerenciar** , clique em **adicionar funções e recursos**.  
  
3.  Na página **Antes de começar**, verifique se o servidor de destino e o ambiente de rede estão preparados para a função e o recurso que você vai instalar. Clique em **Avançar**.  
  
4.  Na página **Selecionar tipo de instalação** , escolha a **Instalação baseada em função ou recurso** para instalar todas as partes das funções ou dos recursos em um único servidor, ou a **Instalação de Serviços de Área de Trabalho Remota** para instalar uma infraestrutura de área de trabalho baseada em máquina virtual ou uma infraestrutura de área de trabalho baseada em sessão para os Serviços de Área de Trabalho Remota. A opção **Instalação dos Serviços de Área de Trabalho Remota** distribui partes lógicas da função Serviços de Área de Trabalho Remota por servidores diferentes, conforme necessário para os administradores. Clique em **Avançar**.  
  
5.  Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores ou um VHD offline. Para selecionar um VHD offline como servidor de destino, primeiro selecione o servidor no qual deseja montar o VHD e selecione o arquivo VHD. Para obter informações sobre como adicionar servidores ao pool de servidores, consulte [adicionar servidores a Gerenciador do servidor](add-servers-to-server-manager.md). Após selecionar o servidor de destino, clique em **Avançar**.  
  
    > [!NOTE]  
    > Para instalar funções e recursos em VHDs offline, os VHDs de destino devem atender aos requisitos a seguir.  
    >   
    > -   Os VHDs devem estar executando o lançamento do Windows Server que corresponde à versão do Gerenciador do Servidor que você está executando. Consulte a observação no início de [instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
    > -   Os VHDs não podem ter mais de um volume ou partição do sistema.  
    > -   A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
    >   
    >     -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
    >     -   Acesso de **controle total** na guia **segurança** , na caixa de diálogo **Propriedades** do arquivo ou da pasta.  
  
6.  Selecione funções, selecione serviços de função para a função (se aplicável) e clique em **Avançar** para selecionar recursos.  
  
    Conforme você prosseguir, o assistente Adicionar funções e recursos o informará automaticamente se forem encontrados conflitos no servidor de destino que podem impedir funções ou recursos selecionados da instalação ou da operação normal. Você também receberá um aviso para adicionar quaisquer funções, serviços de função ou recursos que sejam exigidos pelas funções ou recursos que você selecionou.  
  
    Além disso, se você pretende gerenciar a função remotamente, seja de outro servidor ou de um computador cliente com Windows que executa as Ferramentas de Administração de Servidor Remoto, pode optar por não instalar as ferramentas de gerenciamento e os snap-ins das funções no servidor de destino. Por padrão, no Assistente para adicionar funções e recursos, as ferramentas de gerenciamento são selecionadas para instalação.  
  
7.  Na página **Confirmar seleções de instalação** , confira as seleções de função, recurso e servidor. Se já estiver pronto para instalar, clique em **Instalar**.  
  
    Você também pode exportar suas seleções para um arquivo de configuração baseado em XML que pode ser usado para instalações autônomas com o Windows PowerShell. Para exportar a configuração especificada nesta sessão do assistente para adicionar funções e recursos, clique em **exportar definições de configuração**e salve o arquivo XML em um local conveniente.  
  
    O comando **Especificar um caminho de origem alternativo** na página **Confirmar seleções de instalação** permite especificar um caminho de origem alternativo para os arquivos obrigatórios na instalação de funções e recursos no servidor selecionado. No Windows Server 2012 e versões posteriores do Windows Server, os [recursos sob demanda](https://go.microsoft.com/fwlink/p/?LinkID=241573) permitem reduzir a quantidade de espaço em disco usado pelo sistema operacional, removendo os arquivos de função e recurso de servidores que são gerenciados exclusivamente remotamente. Se você tiver removido arquivos de funções e recursos de um servidor usando o cmdlet `Uninstall-WindowsFeature -remove`, será possível instalar funções e recursos no servidor futuramente, especificando um caminho de origem alternativo, ou um compartilhamento no qual os arquivos de funções e recursos obrigatórios estão armazenados. O caminho de origem ou o compartilhamento de arquivos deve conceder permissões de **leitura** ao grupo **todos** (não recomendado por motivos de segurança) ou à conta de computador (*domínio*\\*ServerName*$) do servidor de destino; conceder acesso à conta de usuário não é suficiente. Para obter mais informações sobre Recursos sob Demanda, consulte [Opções de instalação do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Você pode especificar um arquivo WIM como uma fonte de arquivo de recurso alternativo ao instalar funções, serviços de função e recursos em um servidor físico em execução. O caminho de origem de um arquivo WIM deve ser o formato a seguir, com **WIM** como prefixo e o índice no qual os arquivos de recursos estão localizados como um sufixo: **WIM:e:\sources\install.wim:4**. No entanto, você não pode usar um arquivo WIM diretamente como uma fonte para instalar funções, serviços de função e recursos para um VHD offline; Você deve montar o VHD offline e apontar para seu caminho de montagem para arquivos de origem ou deve apontar para uma pasta que contém uma cópia do conteúdo do arquivo WIM.  
  
8.  Depois de clicar em **instalar**, a página **progresso da instalação** exibe o progresso da instalação, os resultados e as mensagens, como avisos, falhas ou etapas de configuração pós-instalação necessárias para as funções ou os recursos que você instalou. No Windows Server 2012 e versões posteriores do Windows Server, você pode fechar o assistente para adicionar funções e recursos enquanto a instalação ainda estiver em andamento e exibir os resultados da instalação ou outras mensagens na área de **notificações** na parte superior do console do Gerenciador do servidor. Clique no ícone sinalizador de **notificações** para ver mais detalhes sobre instalações ou outras tarefas que você está executando em Gerenciador do servidor.  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Instalar funções, serviços de função e recursos usando os cmdlets do Windows PowerShell  
Os cmdlets de implantação Gerenciador do Servidor para o Windows PowerShell funcionam da mesma forma que o assistente Adicionar funções e recursos baseado em GUI e o assistente para remover funções e recursos, com uma diferença importante. No Windows PowerShell, ao contrário do assistente para adicionar funções e recursos, as ferramentas de gerenciamento e os snap-ins para uma função não são incluídos por padrão. Para incluir ferramentas de gerenciamento como parte da instalação de uma função, adicione o parâmetro `IncludeManagementTools` ao cmdlet. Se você estiver instalando funções e recursos em um servidor que esteja executando a opção de instalação Server Core do Windows Server 2012 ou versões posteriores, poderá adicionar as ferramentas de gerenciamento de uma função a uma instalação, mas as ferramentas de gerenciamento baseadas em GUI e os snap-ins não poderão ser instalados em servidores que executam a opção de instalação Server Core do Windows Server. Somente as ferramentas de gerenciamento de linha de comando e do Windows PowerShell podem ser instaladas na opção de instalação Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Para instalar funções e recursos usando o cmdlet Install-WindowsFeature  
  
1. Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
   > [!NOTE]  
   > Se você estiver instalando funções e recursos em um servidor remoto, não será necessário executar o Windows PowerShell com direitos de usuário elevados.  
  
   -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
   -   Na tela **Iniciar** do Windows, clique com o botão direito do mouse no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.  
  
2. Digite **Get-WindowsFeature** e pressione **Enter** para exibir uma lista de funções e recursos disponíveis e instalados no servidor local. Se o computador local não for um servidor ou se você quiser obter informações sobre um servidor remoto, execute **Get-WindowsFeature-computername <** <em>computer_name</em> **>** , em que *computer_name* representa o nome de um computador remoto que está executando o Windows Server 2016. Os resultados do cmdlet contêm os nomes de comando de funções e recursos que você adiciona ao cmdlet na etapa 4.  
  
   > [!NOTE]  
   > No Windows PowerShell 3,0 e versões posteriores do Windows PowerShell, não é necessário importar o módulo do cmdlet Gerenciador do Servidor para a sessão do Windows PowerShell antes de executar os cmdlets que fazem parte do módulo. Um módulo é importado automaticamente durante a primeira execução de um cmdlet que faça parte do módulo. Além disso, nem os cmdlets do Windows PowerShell nem os nomes dos recursos usados com os cmdlets diferenciam maiúsculas de minúsculas.  
  
3. Digite **Get-Help install-WindowsFeature**e pressione **Enter** para exibir a sintaxe e os parâmetros aceitos para o cmdlet `Install-WindowsFeature`.  
  
4. Digite o seguinte e pressione **Enter**, em que *feature_name* representa o nome do comando de uma função ou recurso que você deseja instalar (obtido na etapa 2) e *computer_name* representa um computador remoto no qual você deseja instalar funções e recursos. Separe vários valores para *nome_do_recurso* usando vírgulas. O parâmetro `Restart` reiniciará o servidor de destino automaticamente se for exigido pela instalação da função ou do recurso.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para instalar funções e recursos em um VHD offline, adicione os dois parâmetros `computerName` e `VHD` . Se você não adicionar o parâmetro `computerName`, o cmdlet vai considerar que o computador local está montado para acessar o VHD. O parâmetro `computerName` contém o nome do servidor em que será montado o VHD, e o parâmetro `VHD` contém o caminho para o arquivo VHD no servidor especificado.  
  
   > [!NOTE]  
   > Você deve adicionar o parâmetro `computerName` se estiver executando o cmdlet de um computador que esteja executando um sistema operacional cliente Windows.  
   >   
   > Para instalar funções e recursos em VHDs offline, os VHDs de destino devem atender aos requisitos a seguir.  
   >   
   > -   Os VHDs devem estar executando o lançamento do Windows Server que corresponde à versão do Gerenciador do Servidor que você está executando. Consulte a observação no início de [instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
   > -   Os VHDs não podem ter mais de um volume ou partição do sistema.  
   > -   A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
   >   
   >     -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
   >     -   Acesso de **controle total** na guia **segurança** , na caixa de diálogo **Propriedades** do arquivo ou da pasta.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Exemplo:** O cmdlet a seguir instala a função de serviços de domínio do Active Directory e o recurso de gerenciamento de Política de Grupo em um servidor remoto, ContosoDC1. As ferramentas de gerenciamento e os snap-ins são adicionados usando o parâmetro `IncludeManagementTools`, e o servidor de destino é reiniciado automaticamente quando a instalação exige a reinicialização dos servidores.  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Quando a instalação for concluída, verifique a instalação abrindo a página **todos os servidores** em Gerenciador do servidor, selecionando um servidor no qual você instalou funções e recursos e exibindo o bloco **funções e recursos** na página do servidor selecionado. Você também pode executar o cmdlet `Get-WindowsFeature` direcionado ao servidor selecionado (Get-WindowsFeature-ComputerName <*computer_name*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>Remover funções, serviços de função e recursos usando o assistente para remover funções e recursos  
Você deve estar conectado a um servidor como administrador para desinstalar funções, serviços de função e recursos. Caso esteja conectado ao computador remoto com uma conta sem direitos de administrador no servidor de destino de desinstalação, clique com o botão direito no servidor de destino, no bloco **Servidores** , e clique em **Gerenciar como** para fornecer uma conta com direitos de administrador. O servidor no qual você deseja montar o VHD offline deve ser adicionado ao Gerenciador do Servidor, e você deve ter direitos de Administrador nesse servidor.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Para remover funções e recursos usando o assistente para remover funções e recursos  
  
1.  Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.  
  
    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.  
  
    -   Na **tela inicial** do Windows, clique no bloco **Gerenciador do Servidor** .  
  
2.  No menu **Gerenciar**, clique em **Remover Funções e Recursos**.  
  
3.  Na página **Antes de começar**, verifique se está preparado para remover funções ou recursos de um servidor. Clique em **Avançar**.  
  
4.  Na página **selecionar servidor de destino** , selecione um servidor no pool de servidores ou selecione um VHD offline. Para selecionar um VHD offline, primeiro selecione o servidor no qual deseja montar o VHD e depois selecione o arquivo VHD.  
  
    > [!NOTE]  
    > A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
    >   
    > -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
    > -   Acesso de **controle total** na guia **Segurança**, caixa de diálogo **Propriedades** da pasta ou arquivo.  
  
    Para obter informações sobre como adicionar servidores ao pool de servidores, consulte [adicionar servidores a Gerenciador do servidor](add-servers-to-server-manager.md). Após selecionar o servidor de destino, clique em **Avançar**.  
  
    > [!NOTE]  
    > Você pode usar o assistente para remover funções e recursos para remover funções e recursos de servidores que executam a mesma versão do Windows Server que dá suporte à versão do Gerenciador do Servidor que você está usando. Não é possível remover funções, serviços de função ou recursos de servidores que executam o Windows Server 2016, se você estiver executando o Gerenciador do Servidor no Windows Server 2012 R2, no Windows Server 2012 ou no Windows 8. Você não pode usar o assistente para remover funções e recursos para remover funções e recursos de servidores que executam o Windows Server 2008 ou o Windows Server 2008 R2.  
  
5.  Selecione funções, selecione serviços de função para a função (se aplicável) e clique em **Avançar** para selecionar recursos.  
  
    Conforme você prosseguir, o assistente para remover funções e recursos automaticamente solicitará que você remova quaisquer funções, serviços de função ou recursos que não podem ser executados sem as funções ou os recursos que você está removendo.  
  
    Além disso, você pode optar por remover as ferramentas de gerenciamento e os snap-ins para funções no servidor de destino. Por padrão, no Assistente para remover funções e recursos, as ferramentas de gerenciamento são selecionadas para remoção. Você pode deixar as ferramentas de gerenciamento e os snap-ins se planeja usar o servidor selecionado para gerenciar a função em outros servidores remotos.  
  
6.  Na página **Confirmar seleções de remoção** , confira as seleções de função, recurso e servidor. Se você estiver pronto para remover as funções ou recursos, clique em **remover**.  
  
7.  Depois de clicar em **remover**, a página **progresso da remoção** exibe o progresso, os resultados e as mensagens de remoção, como avisos, falhas ou etapas de configuração após a remoção que são necessárias, como reiniciar o servidor de destino. No Windows Server 2012 e versões posteriores do Windows Server, você pode fechar o assistente para remover funções e recursos enquanto a remoção ainda estiver em andamento e exibir os resultados da remoção ou outras mensagens na área de **notificações** na parte superior do console do Gerenciador do servidor. Clique no sinalizador **notificações** para ver mais detalhes sobre as remoções ou outras tarefas que você está executando em Gerenciador do servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Remover funções, serviços de função e recursos usando os cmdlets do Windows PowerShell  
Os cmdlets de implantação Gerenciador do Servidor para o Windows PowerShell funcionam de forma semelhante ao Assistente para remover funções e recursos baseados em GUI, com uma diferença importante. No Windows PowerShell, ao contrário do assistente para remover funções e recursos, as ferramentas de gerenciamento e os snap-ins para uma função não são removidos por padrão. Para remover ferramentas de gerenciamento como parte da remoção de uma função, adicione o parâmetro `IncludeManagementTools` ao cmdlet. Se você estiver desinstalando funções e recursos de um servidor que está executando a opção de instalação Server Core do Windows Server 2012 ou uma versão posterior do Windows Server, esse parâmetro removerá as ferramentas de linha de comando e gerenciamento do Windows PowerShell para o especificado funções e recursos.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Para remover funções e recursos usando o cmdlet Uninstall-WindowsFeature  
  
1. Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
   > [!NOTE]  
   > Se você estiver desinstalando funções e recursos de um servidor remoto, não será necessário executar o Windows PowerShell com direitos de usuário elevados.  
  
   -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
   -   Na tela **inicial** do Windows, clique com o botão direito do mouse no bloco Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.  
  
2. Digite **Get-WindowsFeature** e pressione **Enter** para exibir uma lista de funções e recursos disponíveis e instalados no servidor local. Se o computador local não for um servidor ou se você quiser obter informações sobre um servidor remoto, execute **Get-WindowsFeature-computername <** <em>computer_name</em> **>** , em que *computer_name* representa o nome de um computador remoto que está executando o Windows Server 2016. Os resultados do cmdlet contêm os nomes de comando de funções e recursos que você adiciona ao cmdlet na etapa 4.  
  
   > [!NOTE]  
   > No Windows PowerShell 3,0 e versões posteriores do Windows PowerShell, não é necessário importar o módulo do cmdlet Gerenciador do Servidor para a sessão do Windows PowerShell antes de executar os cmdlets que fazem parte do módulo. Um módulo é importado automaticamente durante a primeira execução de um cmdlet que faça parte do módulo. Além disso, nem os cmdlets do Windows PowerShell nem os nomes dos recursos usados com os cmdlets diferenciam maiúsculas de minúsculas.  
  
3. Digite **Get-Help Uninstall-WindowsFeature**e pressione **Enter** para exibir a sintaxe e os parâmetros aceitos para o cmdlet `Uninstall-WindowsFeature`.  
  
4. Digite o seguinte e pressione **Enter**, em que *nome_do_recurso* representa o nome do comando de uma função ou recurso que deseja remover (obtido na etapa 2) e *nome_do_computador* representa o computador remoto do qual as funções e os recursos serão removidos. Separe vários valores para *nome_do_recurso* usando vírgulas. O parâmetro `Restart` reiniciará os servidores de destino automaticamente se for exigido pela remoção da função ou do recurso.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para remover funções e recursos de um VHD offline, adicione os dois parâmetros `computerName` e `VHD`. Se você não adicionar o parâmetro `computerName`, o cmdlet vai considerar que o computador local está montado para acessar o VHD. O parâmetro `computerName` contém o nome do servidor em que será montado o VHD, e o parâmetro `VHD` contém o caminho para o arquivo VHD no servidor especificado.  
  
   > [!NOTE]  
   > Você deve adicionar o parâmetro `computerName` se estiver executando o cmdlet de um computador que esteja executando um sistema operacional cliente Windows.  
   >   
   > A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
   >   
   > -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
   > -   Acesso de **controle total** na guia **segurança** , na caixa de diálogo **Propriedades** do arquivo ou da pasta.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Exemplo:** O cmdlet a seguir remove a função dos serviços de domínio do Active Directory e o recurso de gerenciamento de Política de Grupo de um servidor remoto, ContosoDC1. As ferramentas de gerenciamento e os snap-ins também são removidos, e o servidor de destino é reiniciado automaticamente quando a remoção exige a reinicialização dos servidores.  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Quando a remoção for concluída, verifique se as funções e os recursos foram removidos abrindo a página **todos os servidores** no Gerenciador do servidor, selecionando o servidor do qual você removeu funções e recursos e exibindo o bloco **funções e recursos** na página do servidor selecionado. Você também pode executar o cmdlet `Get-WindowsFeature` direcionado ao servidor selecionado (Get-WindowsFeature-ComputerName <*computer_name*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>Instalar funções e recursos em vários servidores executando um script do Windows PowerShell  
Embora você não possa usar o assistente para adicionar funções e recursos para instalar funções, serviços de função e recursos em mais de um servidor de destino em uma única sessão de assistente, você pode usar um script do Windows PowerShell para instalar funções, serviços de função e recursos em vários destinos servidores que você está gerenciando usando Gerenciador do Servidor. O script que você usa para executar a implantação em lote, como esse processo é chamado, aponta para um arquivo de configuração XML que você pode criar facilmente usando o assistente para adicionar funções e recursos e clicando em **exportar definições de configuração** depois de avançar pelo Assistente para a página **confirmar seleções de instalação** do assistente para adicionar funções e recursos.  
  
> [!IMPORTANT]  
> Todos os servidores de destino especificados no seu script devem estar executando o lançamento do Windows Server que corresponde à versão do Gerenciador do Servidor que você está executando no computador local. Por exemplo, se você estiver executando Gerenciador do Servidor no Windows 10, poderá instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2016. Se as ferramentas de gerenciamento baseadas em GUI forem adicionadas à instalação, o processo de instalação converterá automaticamente os servidores de destino que estão executando a opção de instalação Server Core do Windows Server para a opção de instalação completa (servidor com uma GUI completa, também conhecida como como um shell gráfico do servidor em execução).  
>   
> O script fornecido nesta seção é um exemplo de como a implantação do lote pode ser executada usando o cmdlet `Install-WindowsFeature` e um script do Windows PowerShell. Há outros scripts e métodos que podem ser usados para realizar implantação em lote em vários servidores. Para pesquisar ou fornecer outros scripts para implantar funções e recursos, pesquise o [Repositório da Central de Scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Para instalar funções e recursos em vários servidores  
  
1.  Se você ainda não tiver feito isso, crie um arquivo de configuração XML que contenha as funções, os serviços de função e os recursos que você deseja instalar em vários servidores. Você pode criar esse arquivo de configuração executando o assistente para adicionar funções e recursos, selecionando funções, serviços de função e recursos desejados e clicando em **exportar definições de configuração** depois de avançar o assistente para a página **confirmar seleções de instalação** . Salve o arquivo de configuração em um local conveniente. Não será necessário clicar em **Instalar** nem concluir o assistente caso esteja executando-o somente para criar um arquivo de configuração.  
  
2.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Na tela **inicial** do Windows, clique com o botão direito do mouse no bloco Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.  
  
3.  Copie e cole o script a seguir em sua sessão do Windows PowerShell.  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    Se necessário, os servidores de destino serão automaticamente reiniciado pelas funções e recursos selecionados.  
  
4.  Execute a função utilizando o procedimento a seguir.  
  
    1.  Crie uma variável para armazenar os nomes dos computadores de destino, separados por vírgulas. No exemplo a seguir, a variável `$ServerNames` armazena os nomes dos servidores de destino *Contoso_01* e *Contoso_02*. Pressione **Enter**.  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  Para executar a função, digite o comando a seguir e pressione **Enter**, onde `$ServerNames` é um exemplo da variável criada na etapa anterior e *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* é um exemplo de caminho para o arquivo de configuração criado na etapa 1.  
  
        **Invoke-WindowsFeatureBatchDeployment-ComputerName $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Quando a instalação for concluída, verifique a instalação abrindo a página **todos os servidores** em Gerenciador do servidor, selecionando um servidor no qual você instalou funções e recursos e exibindo o bloco **funções e recursos** na página do servidor selecionado. Você também pode executar o cmdlet `Get-WindowsFeature` direcionado a um servidor específico (`Get-WindowsFeature -computerName` <*computer_name*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>Instalar o .NET Framework 3.5 e outros recursos sob demanda  
a partir do Windows Server 2012 e do Windows 8, os arquivos de recurso para .NET Framework 3,5 (que inclui .NET Framework 2,0 e .NET Framework 3,0) não estão disponíveis no computador local por padrão. Os arquivos foram removidos. Os arquivos dos recursos que foram removidos na configuração Recursos sob Demanda, juntamente com os arquivos de recursos do .NET Framework 3.5, estão disponíveis através do Windows Update. Por padrão, se os arquivos de recurso não estiverem disponíveis no servidor de destino que está executando o Windows Server 2012 ou versões posteriores, o processo de instalação pesquisará os arquivos ausentes conectando-se ao Windows Update. Você pode substituir o comportamento padrão definindo uma configuração de Política de Grupo ou especificando um caminho de origem alternativo durante a instalação, independentemente de você estar instalando usando a GUI do assistente Adicionar funções e recursos ou uma linha de comando.  
  
É possível instalar o .NET Framework 3.5 de uma das seguintes maneiras.  
  
-   Consulte [Para instalar o .NET Framework 3.5 executando o cmdlet Install-WindowsFeature](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) para adicionar o parâmetro `Source` e especifique a fonte da qual obter os arquivos de recursos do .NET Framework 3.5. Se você não adicionar o parâmetro `Source`, o processo de instalação primeiro determinará se foi especificado pelas configurações de Política de Grupo um caminho para os arquivos de recursos e, se nenhum caminho for encontrado, usará o Windows Update para procurar os arquivos de recursos ausentes.  
  
-   Use [para instalar o .NET Framework 3,5 usando o assistente para adicionar funções e recursos](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) para especificar um local de arquivo de origem alternativo na página **confirmar opções de instalação** do assistente para adicionar funções e recursos.  
  
-   Consulte [Para instalar o .NET Framework 3.5 usando o DISM](#to-install-net-framework-35-by-using-dism) para obter os arquivos do Windows Update por padrão ou especificando um caminho de origem para a mídia de instalação.  
  
[Configurar fontes alternativas para os arquivos de recursos na Política de Grupo](#configure-alternate-sources-for-feature-files-in-group-policy) para o .NET Framework 3.5 ou outros recursos, se os arquivos de recursos não forem encontrados no computador local.  
  
> [!IMPORTANT]  
> Quando você instala arquivos de recursos de uma fonte remota, o caminho de origem ou o compartilhamento de arquivos deve conceder as permissões **Leitura** ao grupo **Todos** (não é recomendável por questões de segurança) ou à conta de computador (sistema local) do servidor de destino. Permitir acesso à conta de usuário apenas não é suficiente.  
>   
> Os servidores que estão em grupos de trabalho não podem acessar compartilhamentos de arquivos externos, mesmo que a conta do computador do servidor do grupo tenha permissões **Leitura** no compartilhamento externo. Locais de origem alternativos que funcionam para servidores de grupos de trabalho incluem mídias de instalação, o Windows Update e arquivos VHD ou WIM armazenados no servidor de grupo de trabalho local.  
>   
> Você pode especificar um arquivo WIM como uma fonte de arquivo de recurso alternativo ao instalar funções, serviços de função e recursos em um servidor físico em execução. O caminho de origem de um arquivo WIM deve ser o formato a seguir, com **WIM** como prefixo e o índice no qual os arquivos de recursos estão localizados como um sufixo: **WIM:e:\sources\install.wim:4**. No entanto, você não pode usar um arquivo WIM diretamente como uma fonte para instalar funções, serviços de função e recursos para um VHD offline; Você deve montar o VHD offline e apontar para seu caminho de montagem para arquivos de origem ou deve apontar para uma pasta que contém uma cópia do conteúdo do arquivo WIM.  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>Para instalar o .NET Framework 3.5 executando o cmdlet Install-WindowsFeature  
  
1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    > [!NOTE]  
    > Se você estiver instalando funções e recursos de um servidor remoto, não será necessário executar o Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Na tela **inicial** do Windows, clique com o botão direito do mouse no bloco Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.  
  
    -   Em um servidor que esteja executando a opção de instalação Server Core do Windows Server 2012 R2 ou Windows Server 2012, digite **PowerShell** em um prompt de comando e pressione **Enter**.  
  
2.  Digite o comando a seguir e pressione **Enter**. No seguinte exemplo, os arquivos de origem estão localizados em um repositório lado a lado (abreviado para **SxS**) na mídia de instalação na unidade D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Para que o comando utilize o Windows Update como a fonte para os arquivos de recursos ausentes, ou se uma fonte padrão já foi configurada usando a Política de Grupo, você não precisa adicionar o parâmetro `Source` , exceto para especificar uma fonte diferente.  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>Para instalar o .NET Framework 3,5 usando o assistente para adicionar funções e recursos  
  
1. No menu **gerenciar** no Gerenciador do servidor, clique em **adicionar funções e recursos**.  
  
2. Selecione um servidor de destino que esteja executando o Windows Server 2016.  
  
3. Na página **selecionar recursos** do assistente para adicionar funções e recursos, selecione **.NET Framework 3,5**.  
  
4. Se o computador local tiver permissão para fazer isso usando as configurações de Política de Grupo, o processo de instalação tentará obter os arquivos de recursos ausentes através do Windows Update. Clique em **Instalar**; não é preciso avançar para a próxima etapa.  
  
   se Política de Grupo configurações não permitirem isso, ou se quiser usar outra fonte para os arquivos de recurso .NET Framework 3,5, na página **confirmar seleções de instalação** do assistente, clique em **especificar um caminho de origem alternativo**.  
  
5. Indique o caminho para o repositório lado a lado (chamado de **SxS**) na mídia de instalação ou para um arquivo WIM. No seguinte exemplo, a mídia de instalação está localizada na unidade D.  
  
   **\\ D:\Sources\SxS**  
  
   Para especificar um arquivo WIM, adicione o prefixo **WIM:** e acrescente o índice da imagem para usar no arquivo WIM como um sufixo, conforme mostrado no exemplo a seguir.  
  
   **Wim:\\\\** <em>server_name</em> **\share\install.wim: 3**  
  
6. Clique em **OK**e em **Instalar**.  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>Para instalar o .NET Framework 3.5 usando o DISM  
  
1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    > [!NOTE]  
    > Se você estiver instalando funções e recursos de um servidor remoto, não será necessário executar o Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Na tela **inicial** do Windows, clique com o botão direito do mouse no bloco Windows PowerShell e, em seguida, na barra de aplicativos, clique em **Executar como administrador**.  
  
    -   Em um servidor que esteja executando a opção de instalação Server Core, digite **PowerShell** em um prompt de comando e pressione **Enter**.  
  
2.  Execute um dos seguintes comandos DISM.  
  
    -   Se o computador tiver acesso ao Windows Update ou se um local do arquivo de origem padrão já tiver sido configurado no Política de Grupo, execute o comando a seguir.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Se o computador tiver acesso à mídia de instalação, execute um comando semelhante ao seguinte. No seguinte exemplo, a mídia de instalação do sistema operacional está localizada na unidade D. O parâmetro `LimitAccess` impede que o comando tente entrar em contato com o Windows Update ou com um servidor que esteja executando o WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > O comando DISM diferencia maiúsculas de minúsculas.  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>Configurar fontes alternativas para os arquivos de recursos na Política de Grupo  
A configuração de Política de Grupo descrita nesta seção especifica os locais de origem autorizados para os arquivos do .NET Framework 3.5 e outros arquivos de recursos que foram removidos como parte da configuração Recursos sob Demanda. A configuração de política **especifica as configurações para a instalação de componente opcional e o reparo de componentes** está localizado na pasta do **computador \ Configuração do computador\modelos Templates\System** no console de gerenciamento de política de grupo ou no editor de política de grupo local.  
  
> [!NOTE]  
> Você deve ser membro do grupo Administradores para alterar as configurações de Política de Grupo no computador local. Se as configurações de Política de Grupo do computador que você deseja gerenciar forem controladas no nível do domínio, você deverá ser membro do grupo Administradores de Domínio para alterar as configurações de Política de Grupo.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Para configurar um caminho de origem alternativo padrão na Política de Grupo  
  
1. No editor de Política de Grupo local ou Console de Gerenciamento de Política de Grupo, abra a configuração de política a seguir.  
  
   **configuração do Computador\modelos Templates\System\Specify para instalação de componente opcional e reparo de componente**  
  
2. Sselect **habilitado** para habilitar a configuração de política, se ela ainda não estiver habilitada.  
  
3. Na caixa de texto **Caminho de arquivo de origem alternativo** da área **Opções**, especifique o caminho totalmente qualificado para uma pasta compartilhada ou um arquivo WIM. Para especificar um arquivo WIM como um local de arquivo de origem alternativo, adicione o prefixo **WIM:** ao caminho e acrescente o índice da imagem para usar no arquivo WIM como um sufixo. Veja a seguir exemplos de valores que você pode especificar.  
  
   - caminho para uma pasta compartilhada: **\\\\** <em>server_name</em> **\share\\** <em>Folder_Name</em>  
  
   - caminho para um arquivo WIM, no qual **3** representa o índice da imagem na qual os arquivos de recurso são encontrados: **WIM:\\\\** <em>server_name</em> **\share\install.wim: 3**  
  
4. Se você não quiser que os computadores que são controlados por essa configuração de política procurem arquivos de recursos ausentes no Windows Update, selecione **nunca tentar baixar conteúdo de Windows Update**.  
  
5. Se os computadores controlados por essa configuração de política normalmente recebem atualizações pelo WSUS, mas você prefere usar o Windows Update em vez do WSUS para localizar os arquivos de recursos ausentes, selecione **Contatar diretamente o Windows Update para baixar conteúdo de reparo em vez do WSUS (Windows Server Update Services)** .  
  
6. Clique em **OK** quando terminar de alterar essa configuração de política e feche o Editor de Política de Grupo.  
  
## <a name="see-also"></a>Consulte também  
[Opções de instalação do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considerações de implantação do Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Como habilitar ou desabilitar recursos do Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


