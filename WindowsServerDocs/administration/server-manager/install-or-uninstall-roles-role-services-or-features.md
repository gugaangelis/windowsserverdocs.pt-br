---
title: Instalar ou desinstalar funções, serviços de função ou recursos
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 526acaadc257d5e8b1dea342756cdeeec240c1dd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435460"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Instalar ou desinstalar funções, serviços de função ou recursos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, o console do Gerenciador do servidor e os cmdlets do Windows PowerShell para o Gerenciador do servidor permitir a instalação de funções e recursos em servidores locais ou remotos, ou discos de rígidos virtuais (VHDs) offline. Você pode instalar várias funções e recursos em um único servidor remoto ou VHD offline em uma única adicionar funções e recursos do assistente ou sessão do Windows PowerShell.  
  
> [!IMPORTANT]  
> Gerenciador de servidores não podem ser usados para gerenciar uma versão mais recente do sistema operacional Windows Server. Gerenciador do servidor em execução no Windows Server 2012 R2 ou Windows 8.1 não pode ser usado para instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2016.  
  
Você deve fazer logon em um servidor como um administrador para instalar ou desinstalar funções, serviços de função e recursos. Caso esteja conectado ao computador remoto com uma conta sem direitos de administrador no servidor de destino, clique com o botão direito no servidor de destino, no bloco **Servidores**, e clique em **Gerenciar como** para fornecer uma conta com direitos de administrador. O servidor no qual você deseja montar o VHD offline deve ser adicionado ao Gerenciador do Servidor, e você deve ter direitos de Administrador nesse servidor.  
  
Para obter mais informações sobre o que são funções, serviços de função e recursos, consulte [funções, serviços de função e recursos](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
Este tópico contém as seguintes seções.  
  
-   [Instalar funções, serviços de função e recursos usando o Assistente de recursos e adicionar funções](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [Instalar funções, serviços de função e recursos usando os cmdlets do Windows PowerShell](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [remover funções, serviços de função e recursos usando o remover Assistente de funções e recursos](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [Remover funções, serviços de função e recursos usando os cmdlets do Windows PowerShell](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Instalar funções e recursos em vários servidores executando um script do Windows PowerShell](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [Instalar o .NET Framework 3.5 e outros recursos sob demanda](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>Instalar funções, serviços de função e recursos usando o Assistente de recursos e adicionar funções  
Em uma única sessão no adicionar funções e recursos do assistente, você pode instalar funções, serviços de função e recursos no servidor local, um servidor remoto que foi adicionado ao Gerenciador do servidor ou um VHD offline. Para obter mais informações sobre como adicionar um servidor ao Gerenciador do servidor para gerenciar, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Se você estiver executando o Gerenciador do servidor no Windows Server 2016 ou Windows 10, você pode usar o Assistente de recursos e adicionar funções para instalar funções e recursos apenas em servidores e VHDs offline que estão executando o Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Para instalar funções e recursos usando o Assistente de recursos e adicionar funções  
  
1.  Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.  
  
    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.  
  
    -   Sobre o Windows **inicie** tela, clique no **Gerenciador do servidor** lado a lado.  
  
2.  Sobre o **Manage** menu, clique em **adicionar funções e recursos**.  
  
3.  Na página **Antes de começar**, verifique se o servidor de destino e o ambiente de rede estão preparados para a função e o recurso que você vai instalar. Clique em **Avançar**.  
  
4.  Na página **Selecionar tipo de instalação** , escolha a **Instalação baseada em função ou recurso** para instalar todas as partes das funções ou dos recursos em um único servidor, ou a **Instalação de Serviços de Área de Trabalho Remota** para instalar uma infraestrutura de área de trabalho baseada em máquina virtual ou uma infraestrutura de área de trabalho baseada em sessão para os Serviços de Área de Trabalho Remota. A opção **Instalação dos Serviços de Área de Trabalho Remota** distribui partes lógicas da função Serviços de Área de Trabalho Remota por servidores diferentes, conforme necessário para os administradores. Clique em **Avançar**.  
  
5.  Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores ou um VHD offline. Para selecionar um VHD offline como servidor de destino, primeiro selecione o servidor no qual deseja montar o VHD e selecione o arquivo VHD. Para obter informações sobre como adicionar servidores ao pool de servidores, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md). Após selecionar o servidor de destino, clique em **Avançar**.  
  
    > [!NOTE]  
    > Para instalar funções e recursos em VHDs offline, os VHDs de destino devem atender aos requisitos a seguir.  
    >   
    > -   VHDs devem estar executando a versão do Windows Server que corresponde à versão do Gerenciador do servidor estiver executando. Consulte a observação no início da [instalar funções, serviços de função e recursos usando o Assistente de recursos e adicionar funções](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
    > -   Os VHDs não podem ter mais de um volume ou partição do sistema.  
    > -   A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
    >   
    >     -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
    >     -   **Controle total** acessar na **Security** guia, arquivo ou pasta **propriedades** caixa de diálogo.  
  
6.  Selecione funções, selecione serviços de função para a função (se aplicável) e clique em **Avançar** para selecionar recursos.  
  
    Conforme você prosseguir, adicionar Assistente de funções e recursos automaticamente informa se o conflito foi encontrado no servidor de destino que pode impedir a funções selecionadas ou recursos de instalação ou operação normal. Você também receberá um aviso para adicionar quaisquer funções, serviços de função ou recursos que sejam exigidos pelas funções ou recursos que você selecionou.  
  
    Além disso, se você pretende gerenciar a função remotamente, seja de outro servidor ou de um computador cliente com Windows que executa as Ferramentas de Administração de Servidor Remoto, pode optar por não instalar as ferramentas de gerenciamento e os snap-ins das funções no servidor de destino. Por padrão, a adicionar Assistente de funções e recursos, ferramentas de gerenciamento são selecionadas para instalação.  
  
7.  Na página **Confirmar seleções de instalação** , confira as seleções de função, recurso e servidor. Se já estiver pronto para instalar, clique em **Instalar**.  
  
    Você também pode exportar suas seleções para um arquivo de configuração com base em XML que você pode usar para instalações autônomas com o Windows PowerShell. Para exportar a configuração especificada nesta adicionar funções e a sessão do Assistente de recursos, clique em **exportar definições de configuração**e, em seguida, salve o arquivo XML em um local conveniente.  
  
    O comando **Especificar um caminho de origem alternativo** na página **Confirmar seleções de instalação** permite especificar um caminho de origem alternativo para os arquivos obrigatórios na instalação de funções e recursos no servidor selecionado. No Windows Server 2012 e versões posteriores do Windows Server [recursos sob demanda](https://go.microsoft.com/fwlink/p/?LinkID=241573) permitem reduzir a quantidade de espaço em disco usado pelo sistema operacional, removendo arquivos de recursos e dos servidores gerenciados exclusivamente remotamente. Se você tiver removido arquivos de funções e recursos de um servidor usando o cmdlet `Uninstall-WindowsFeature -remove`, será possível instalar funções e recursos no servidor futuramente, especificando um caminho de origem alternativo, ou um compartilhamento no qual os arquivos de funções e recursos obrigatórios estão armazenados. O compartilhamento de arquivo ou caminho de origem deve conceder **leitura** permissões para o **Everyone** grupo (não recomendado por motivos de segurança), ou para a conta de computador (*domínio* \\ *SERverNAME*$) do servidor de destino; permitir acesso à conta de usuário não é suficiente. Para obter mais informações sobre Recursos sob Demanda, consulte [Opções de instalação do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Você pode especificar um arquivo WIM como uma fonte de arquivo de recurso alternativo quando você estiver instalando funções, serviços de função e recursos em um servidor físico em funcionamento. O caminho de origem para um arquivo WIM deve ser no seguinte formato, com **WIM** como um prefixo e o índice no qual os arquivos de recursos estão localizados como um sufixo: **WIM:e:\sources\install.wim:4**. No entanto, você não pode usar um arquivo WIM diretamente como fonte para a instalação de funções, serviços de função e recursos em um VHD offline; Você deve montar o VHD offline e apontar para seu caminho de montagem para arquivos de origem, ou você deve apontar para uma pasta que contém uma cópia do conteúdo do arquivo WIM.  
  
8.  Depois de clicar em **instale**, o **progresso da instalação** página exibe o progresso da instalação, resultados e mensagens, como avisos, falhas ou etapas de configuração de pós-instalação que são necessário para as funções ou recursos instalados. No Windows Server 2012 e versões posteriores do Windows Server, você pode fechar o Assistente de recursos e adicionar funções enquanto a instalação ainda está em andamento e exibir resultados da instalação ou outras mensagens na **notificações** área na parte superior do console do Gerenciador do servidor. Clique o **notificações** ícone de sinalizador para ver mais detalhes sobre as instalações ou outras tarefas que você está executando no Gerenciador do servidor.  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Instalar funções, serviços de função e recursos usando os cmdlets do Windows PowerShell  
Os cmdlets de implantação do Gerenciador do servidor para a função do Windows PowerShell da mesma forma que a interface gráfica do usuário com base em Adicionar funções e Assistente de recursos e remover funções e recursos do assistente, com uma diferença importante. No Windows PowerShell, ao contrário a adicionar Assistente de funções e recursos, ferramentas de gerenciamento e snap-ins para uma função não são incluídos por padrão. Para incluir ferramentas de gerenciamento como parte da instalação de uma função, adicione o parâmetro `IncludeManagementTools` ao cmdlet. Se você estiver instalando funções e recursos em um servidor que está executando a opção de instalação Server Core do Windows Server 2012 ou versões posteriores, você pode adicionar ferramentas de gerenciamento da função a uma instalação, mas o snap-ins e ferramentas de gerenciamento baseado em GUI não podem ser instalados em servidores que estão executando a opção de instalação Server Core do Windows Server. Somente linha de comando e ferramentas de gerenciamento do Windows PowerShell podem ser instaladas na opção de instalação Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Para instalar funções e recursos usando o cmdlet Install-WindowsFeature  
  
1. Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
   > [!NOTE]  
   > Se você estiver instalando funções e recursos em um servidor remoto, você precisa executar o Windows PowerShell com direitos de usuário elevados.  
  
   -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
   -   Sobre o Windows **inicie** tela, clique com botão direito no bloco para o Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.  
  
2. Digite **Get-WindowsFeature** e pressione **Enter** para exibir uma lista de funções e recursos disponíveis e instalados no servidor local. Se o computador local não é um servidor, ou se desejar obter informações sobre um servidor remoto, execute **Get-WindowsFeature - computerName <** <em>computer_name</em> **>** , no qual *nome_do_computador* representa o nome de um computador remoto que esteja executando o Windows Server 2016. Os resultados do cmdlet contêm os nomes de comando das funções e recursos que você adiciona ao cmdlet na etapa 4.  
  
   > [!NOTE]  
   > No Windows PowerShell 3.0 e versões posteriores do Windows PowerShell, não é necessário para importar o módulo de cmdlet do Gerenciador do servidor para a sessão do Windows PowerShell antes de executar os cmdlets que fazem parte do módulo. Um módulo é importado automaticamente durante a primeira execução de um cmdlet que faça parte do módulo. Além disso, nem cmdlets do Windows PowerShell, nem os nomes de recurso usados com os cmdlets diferenciam maiusculas de minúsculas.  
  
3. tipo de **Get-help Install-WindowsFeature**, em seguida, pressione **Enter** para exibir a sintaxe e os parâmetros aceitos para o `Install-WindowsFeature` cmdlet.  
  
4. Digite o seguinte e, em seguida, pressione **Enter**, onde *nome_do_recurso* representa o nome do comando de uma função ou recurso que você deseja instalar (obtido na etapa 2), e *nome_do_computador* representa um computador remoto no qual você deseja instalar funções e recursos. Separe vários valores para *nome_do_recurso* usando vírgulas. O parâmetro `Restart` reiniciará o servidor de destino automaticamente se for exigido pela instalação da função ou do recurso.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para instalar funções e recursos em um VHD offline, adicione os dois parâmetros `computerName` e `VHD` . Se você não adicionar o parâmetro `computerName`, o cmdlet vai considerar que o computador local está montado para acessar o VHD. O parâmetro `computerName` contém o nome do servidor em que será montado o VHD, e o parâmetro `VHD` contém o caminho para o arquivo VHD no servidor especificado.  
  
   > [!NOTE]  
   > Você deve adicionar o `computerName` parâmetro se você estiver executando o cmdlet em um computador que está executando um sistema operacional cliente do Windows.  
   >   
   > Para instalar funções e recursos em VHDs offline, os VHDs de destino devem atender aos requisitos a seguir.  
   >   
   > -   VHDs devem estar executando a versão do Windows Server que corresponde à versão do Gerenciador do servidor estiver executando. Consulte a observação no início da [instalar funções, serviços de função e recursos usando o Assistente de recursos e adicionar funções](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
   > -   Os VHDs não podem ter mais de um volume ou partição do sistema.  
   > -   A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
   >   
   >     -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
   >     -   **Controle total** acessar na **Security** guia, arquivo ou pasta **propriedades** caixa de diálogo.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Exemplo:** O seguinte cmdlet instala a função Serviços de domínio active directory e o recurso Gerenciamento de diretiva de grupo em um servidor remoto, ContosoDC1. As ferramentas de gerenciamento e os snap-ins são adicionados usando o parâmetro `IncludeManagementTools`, e o servidor de destino é reiniciado automaticamente quando a instalação exige a reinicialização dos servidores.  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Quando a instalação for concluída, verifique a instalação abrindo a **todos os servidores** página no Gerenciador do servidor, selecionando um servidor no qual você instalou as funções e recursos e exibindo o **funções e recursos** lado a lado na página para o servidor selecionado. Você também pode executar o `Get-WindowsFeature` cmdlet do direcionado ao servidor selecionado (Get-WindowsFeature - computerName <*computer_name*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>remover funções, serviços de função e recursos usando o remover Assistente de funções e recursos  
Você deve fazer logon em um servidor como administrador para desinstalar funções, serviços de função e recursos. Caso esteja conectado ao computador remoto com uma conta sem direitos de administrador no servidor de destino de desinstalação, clique com o botão direito no servidor de destino, no bloco **Servidores** , e clique em **Gerenciar como** para fornecer uma conta com direitos de administrador. O servidor no qual você deseja montar o VHD offline deve ser adicionado ao Gerenciador do Servidor, e você deve ter direitos de Administrador nesse servidor.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Para remover funções e recursos usando o remover Assistente de funções e recursos  
  
1.  Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.  
  
    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.  
  
    -   Na **tela inicial** do Windows, clique no bloco **Gerenciador do Servidor** .  
  
2.  No menu **Gerenciar**, clique em **Remover Funções e Recursos**.  
  
3.  Na página **Antes de começar**, verifique se está preparado para remover funções ou recursos de um servidor. Clique em **Avançar**.  
  
4.  Sobre o **Selecionar servidor de destino** página, selecione um servidor no pool de servidores ou selecione um VHD offline. Para selecionar um VHD offline, primeiro selecione o servidor no qual deseja montar o VHD e depois selecione o arquivo VHD.  
  
    > [!NOTE]  
    > A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
    >   
    > -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
    > -   Acesso de **controle total** na guia **Segurança**, caixa de diálogo **Propriedades** da pasta ou arquivo.  
  
    Para obter informações sobre como adicionar servidores ao pool de servidores, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md). Após selecionar o servidor de destino, clique em **Avançar**.  
  
    > [!NOTE]  
    > Você pode usar a remover funções e recursos do Assistente para remover funções e recursos de servidores que estão executando a mesma versão do Windows Server suporta a versão do Gerenciador do servidor que você está usando. É possível remover funções, serviços de função ou recursos de servidores que executam o Windows Server 2016, se você estiver executando o Gerenciador do servidor no Windows Server 2012 R2, Windows Server 2012 ou Windows 8. Não é possível usar a remover funções e recursos do Assistente para remover funções e recursos de servidores que executam o Windows Server 2008 ou Windows Server 2008 R2.  
  
5.  Selecione funções, selecione serviços de função para a função (se aplicável) e clique em **Avançar** para selecionar recursos.  
  
    Conforme você prosseguir, a remover funções e recursos do assistente solicita automaticamente que você remova todas as funções, serviços de função ou recursos que não podem ser executados sem as funções ou recursos que você estão removendo.  
  
    Além disso, você pode optar por remover as ferramentas de gerenciamento e os snap-ins das funções no servidor de destino. Por padrão, na remover funções e Assistente de recursos, ferramentas de gerenciamento são selecionadas para remoção. Você pode deixar as ferramentas de gerenciamento e os snap-ins se planeja usar o servidor selecionado para gerenciar a função em outros servidores remotos.  
  
6.  Na página **Confirmar seleções de remoção** , confira as seleções de função, recurso e servidor. Se você estiver pronto para remover funções ou recursos, clique em **remover**.  
  
7.  Depois de clicar em **remova**, o **progresso da remoção** página exibe o progresso da remoção, resultados e mensagens, como avisos, falhas ou etapas de configuração pós-remoção que são necessárias, como reiniciando o servidor de destino. No Windows Server 2012 e versões posteriores do Windows Server, você pode fechar a remover funções e recursos do assistente enquanto a remoção ainda está em andamento e exibir os resultados da remoção ou outras mensagens na **notificações** área na parte superior das Console do Gerenciador do servidor. Clique o **notificações** sinalizador para ver mais detalhes sobre remoções ou outras tarefas que você está executando no Gerenciador do servidor.  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Remover funções, serviços de função e recursos usando os cmdlets do Windows PowerShell  
Os cmdlets de implantação do Gerenciador do servidor para a função do Windows PowerShell da mesma forma que o baseado em GUI remover funções e recursos do assistente, com uma diferença importante. No Windows PowerShell, ao contrário de na remover funções e Assistente de recursos, ferramentas de gerenciamento e snap-ins para uma função não são removidos por padrão. Para remover ferramentas de gerenciamento como parte da remoção de uma função, adicione o parâmetro `IncludeManagementTools` ao cmdlet. Se você estiver desinstalando funções e recursos de um servidor que está executando a opção de instalação Server Core do Windows Server 2012 ou uma versão posterior do Windows Server, esse parâmetro removerá a linha de comando e ferramentas de gerenciamento do Windows PowerShell para especificado funções e recursos.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Para remover funções e recursos usando o cmdlet Uninstall-WindowsFeature  
  
1. Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
   > [!NOTE]  
   > Se você estiver desinstalando funções e recursos de um servidor remoto, você precisa executar o Windows PowerShell com direitos de usuário elevados.  
  
   -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
   -   Sobre o Windows **inicie** tela, clique com botão direito no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.  
  
2. Digite **Get-WindowsFeature** e pressione **Enter** para exibir uma lista de funções e recursos disponíveis e instalados no servidor local. Se o computador local não é um servidor, ou se desejar obter informações sobre um servidor remoto, execute **Get-WindowsFeature - computerName <** <em>computer_name</em> **>** , no qual *nome_do_computador* representa o nome de um computador remoto que esteja executando o Windows Server 2016. Os resultados do cmdlet contêm os nomes de comando das funções e recursos que você adiciona ao cmdlet na etapa 4.  
  
   > [!NOTE]  
   > No Windows PowerShell 3.0 e versões posteriores do Windows PowerShell, não é necessário para importar o módulo de cmdlet do Gerenciador do servidor para a sessão do Windows PowerShell antes de executar os cmdlets que fazem parte do módulo. Um módulo é importado automaticamente durante a primeira execução de um cmdlet que faça parte do módulo. Além disso, nem cmdlets do Windows PowerShell, nem os nomes de recurso usados com os cmdlets diferenciam maiusculas de minúsculas.  
  
3. tipo de **Get-help Uninstall-WindowsFeature**, em seguida, pressione **Enter** para exibir a sintaxe e os parâmetros aceitos para o `Uninstall-WindowsFeature` cmdlet.  
  
4. Digite o seguinte e pressione **Enter**, em que *nome_do_recurso* representa o nome do comando de uma função ou recurso que deseja remover (obtido na etapa 2) e *nome_do_computador* representa o computador remoto do qual as funções e os recursos serão removidos. Separe vários valores para *nome_do_recurso* usando vírgulas. O parâmetro `Restart` reiniciará os servidores de destino automaticamente se for exigido pela remoção da função ou do recurso.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Para remover funções e recursos de um VHD offline, adicione os dois parâmetros `computerName` e `VHD`. Se você não adicionar o parâmetro `computerName`, o cmdlet vai considerar que o computador local está montado para acessar o VHD. O parâmetro `computerName` contém o nome do servidor em que será montado o VHD, e o parâmetro `VHD` contém o caminho para o arquivo VHD no servidor especificado.  
  
   > [!NOTE]  
   > Você deve adicionar o `computerName` parâmetro se você estiver executando o cmdlet em um computador que está executando um sistema operacional cliente do Windows.  
   >   
   > A pasta compartilhada de rede na qual o arquivo VHD é armazenado deve conceder os seguintes direitos de acesso à conta do computador (ou sistema local) do servidor selecionado para montagem do VHD. O acesso à conta somente do usuário não é suficiente. O compartilhamento pode fornecer permissões de **Leitura** e **Gravação** ao grupo **Todos** para conceder acesso ao VHD. Porém, por motivos de segurança, isso não é recomendado.  
   >   
   > -   Acesso de **leitura/gravação** na caixa de diálogo **Compartilhamento de Arquivos**.  
   > -   **Controle total** acessar na **Security** guia, arquivo ou pasta **propriedades** caixa de diálogo.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Exemplo:** O cmdlet a seguir remove a função Serviços de domínio active directory e o recurso Gerenciamento de diretiva de grupo de um servidor remoto, ContosoDC1. As ferramentas de gerenciamento e os snap-ins também são removidos, e o servidor de destino é reiniciado automaticamente quando a remoção exige a reinicialização dos servidores.  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Concluída a remoção, verifique se que as funções e recursos foram removidos abrindo a **todos os servidores** página no Gerenciador do servidor, selecionando o servidor do qual você removeu as funções e recursos e exibindo o **funções e Recursos** lado a lado na página para o servidor selecionado. Você também pode executar o `Get-WindowsFeature` cmdlet do direcionado ao servidor selecionado (Get-WindowsFeature - computerName <*computer_name*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>Instalar funções e recursos em vários servidores executando um script do Windows PowerShell  
Embora você não pode usar o Assistente de recursos e adicionar funções para instalar funções, serviços de função e recursos em mais de um servidor de destino em uma única sessão de assistente, você pode usar um script do Windows PowerShell para instalar funções, serviços de função e recursos em múltiplos de destino servidores que você está gerenciando usando o Gerenciador do servidor. O script utilizado para realizar implantação em lote, como esse processo é chamado, aponta para um arquivo de configuração XML que você pode criar facilmente usando o Assistente de recursos e adicionar funções e, em seguida, clicando em **exportar definições de configuração** depois aprimorando o Assistente para o **confirmar seleções de instalação** página do Assistente de recursos e adicionar funções.  
  
> [!IMPORTANT]  
> Todos os servidores de destino que são especificados em seu script devem estar executando a versão do Windows Server que corresponde à versão do Gerenciador do servidor estão em execução no computador local. Por exemplo, se você estiver executando o Gerenciador do servidor no Windows 10, você pode instalar funções, serviços de função e recursos em servidores que executam o Windows Server 2016. Caso sejam adicionadas ferramentas de gerenciamento baseado em GUI à instalação, o processo de instalação automaticamente converte servidores de destino que estão executando a opção de instalação Server Core do Windows Server para a opção de instalação completa (servidor com GUI completa, também conhecida como em execução Shell gráfico de servidor).  
>   
> O script fornecido nesta seção é um exemplo de como a implantação em lote pode ser realizada usando o `Install-WindowsFeature` cmdlet e um script do Windows PowerShell. Há outros scripts e métodos que podem ser usados para realizar implantação em lote em vários servidores. Para pesquisar ou fornecer outros scripts para implantar funções e recursos, pesquise o [Repositório da Central de Scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Para instalar funções e recursos em vários servidores  
  
1.  Se você ainda não fez isso, crie um arquivo de configuração XML que contém as funções, serviços de função e recursos que deseja instalar em vários servidores. Você pode criar esse arquivo de configuração executando o Assistente de recursos e adicionar funções, selecionando funções, serviços de função e recursos que você quer, e clicando em **exportar definições de configuração** após Avançar no Assistente para o **Confirmar seleções de instalação** página. Salve o arquivo de configuração em um local conveniente. Não será necessário clicar em **Instalar** nem concluir o assistente caso esteja executando-o somente para criar um arquivo de configuração.  
  
2.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Sobre o Windows **inicie** tela, clique com botão direito no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.  
  
3.  Copie e cole o seguinte script na sessão do Windows PowerShell.  
  
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
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Quando a instalação for concluída, verifique a instalação abrindo a **todos os servidores** página no Gerenciador do servidor, selecionando um servidor no qual você instalou as funções e recursos e exibindo o **funções e recursos** lado a lado na página para o servidor selecionado. Você também pode executar o `Get-WindowsFeature` cmdlet do direcionado a um servidor específico (`Get-WindowsFeature -computerName` <*nome_do_computador*>) para exibir uma lista de funções e recursos que estão instalados no servidor.  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>Instalar o .NET Framework 3.5 e outros recursos sob demanda  
começando com o Windows Server 2012 e Windows 8, os arquivos de recursos do .NET Framework 3.5 (que inclui o .NET Framework 2.0 e o .NET Framework 3.0) não estão disponíveis no computador local por padrão. Os arquivos foram removidos. Os arquivos dos recursos que foram removidos na configuração Recursos sob Demanda, juntamente com os arquivos de recursos do .NET Framework 3.5, estão disponíveis através do Windows Update. Por padrão, se os arquivos de recursos não estão disponíveis no servidor de destino que esteja executando o Windows Server 2012 ou versões posteriores, o processo de instalação procura os arquivos ausentes conectando-se ao Windows Update. Você pode substituir o comportamento padrão definindo uma configuração de diretiva de grupo ou especificando um caminho de origem alternativo durante a instalação, se você estiver instalando por meio de adicionar funções e recursos do Assistente de GUI ou uma linha de comando.  
  
É possível instalar o .NET Framework 3.5 de uma das seguintes maneiras.  
  
-   Consulte [Para instalar o .NET Framework 3.5 executando o cmdlet Install-WindowsFeature](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) para adicionar o parâmetro `Source` e especifique a fonte da qual obter os arquivos de recursos do .NET Framework 3.5. Se você não adicionar o parâmetro `Source`, o processo de instalação primeiro determinará se foi especificado pelas configurações de Política de Grupo um caminho para os arquivos de recursos e, se nenhum caminho for encontrado, usará o Windows Update para procurar os arquivos de recursos ausentes.  
  
-   Use [para instalar o .NET Framework 3.5 usando o Assistente de recursos e adicionar funções](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) para especificar um local de arquivo de origem alternativo na **Confirmar Opções de instalação** página do Assistente de recursos e adicionar funções.  
  
-   Consulte [Para instalar o .NET Framework 3.5 usando o DISM](#to-install-net-framework-35-by-using-dism) para obter os arquivos do Windows Update por padrão ou especificando um caminho de origem para a mídia de instalação.  
  
[Configurar fontes alternativas para os arquivos de recursos na Política de Grupo](#configure-alternate-sources-for-feature-files-in-group-policy) para o .NET Framework 3.5 ou outros recursos, se os arquivos de recursos não forem encontrados no computador local.  
  
> [!IMPORTANT]  
> Quando você instala arquivos de recursos de uma fonte remota, o caminho de origem ou o compartilhamento de arquivos deve conceder as permissões **Leitura** ao grupo **Todos** (não é recomendável por questões de segurança) ou à conta de computador (sistema local) do servidor de destino. Permitir acesso à conta de usuário apenas não é suficiente.  
>   
> Os servidores que estão em grupos de trabalho não podem acessar compartilhamentos de arquivos externos, mesmo que a conta do computador do servidor do grupo tenha permissões **Leitura** no compartilhamento externo. Locais de origem alternativos que funcionam para servidores de grupos de trabalho incluem mídias de instalação, o Windows Update e arquivos VHD ou WIM armazenados no servidor de grupo de trabalho local.  
>   
> Você pode especificar um arquivo WIM como uma fonte de arquivo de recurso alternativo quando você estiver instalando funções, serviços de função e recursos em um servidor físico em funcionamento. O caminho de origem para um arquivo WIM deve ser no seguinte formato, com **WIM** como um prefixo e o índice no qual os arquivos de recursos estão localizados como um sufixo: **WIM:e:\sources\install.wim:4**. No entanto, você não pode usar um arquivo WIM diretamente como fonte para a instalação de funções, serviços de função e recursos em um VHD offline; Você deve montar o VHD offline e apontar para seu caminho de montagem para arquivos de origem, ou você deve apontar para uma pasta que contém uma cópia do conteúdo do arquivo WIM.  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>Para instalar o .NET Framework 3.5 executando o cmdlet Install-WindowsFeature  
  
1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    > [!NOTE]  
    > Se você estiver instalando funções e recursos de um servidor remoto, você precisa executar o Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Sobre o Windows **inicie** tela, clique com botão direito no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.  
  
    -   Em um servidor que está executando a opção de instalação Server Core do Windows Server 2012 R2 ou Windows Server 2012, digite **PowerShell** em um prompt de comando e pressione **Enter**.  
  
2.  Digite o seguinte comando e pressione **Enter**. No seguinte exemplo, os arquivos de origem estão localizados em um repositório lado a lado (abreviado para **SxS**) na mídia de instalação na unidade D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Para que o comando utilize o Windows Update como a fonte para os arquivos de recursos ausentes, ou se uma fonte padrão já foi configurada usando a Política de Grupo, você não precisa adicionar o parâmetro `Source` , exceto para especificar uma fonte diferente.  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>Para instalar o .NET Framework 3.5 usando o Assistente de recursos e adicionar funções  
  
1. Sobre o **Manage** menu no Gerenciador do servidor, clique em **adicionar funções e recursos**.  
  
2. Selecione um servidor de destino que está executando o Windows Server 2016.  
  
3. Sobre o **selecionar recursos** página de adicionar funções e recursos do assistente, selecione **.NET Framework 3.5**.  
  
4. Se o computador local tiver permissão para fazer isso usando as configurações de Política de Grupo, o processo de instalação tentará obter os arquivos de recursos ausentes através do Windows Update. Clique em **Instalar**; não é preciso avançar para a próxima etapa.  
  
   Se as configurações de diretiva de grupo não derem permissão para isso, ou você deseja usar outra fonte para os arquivos de recursos do .NET Framework 3.5 na **confirmar seleções de instalação** página do assistente, clique em **especificar um caminho de origem alternativo** .  
  
5. Indique o caminho para o repositório lado a lado (chamado de **SxS**) na mídia de instalação ou para um arquivo WIM. No seguinte exemplo, a mídia de instalação está localizada na unidade D.  
  
   **D:\Sources\SxS\\**  
  
   Para especificar um arquivo WIM, adicione o prefixo **WIM:** e acrescente o índice da imagem para usar no arquivo WIM como um sufixo, conforme mostrado no exemplo a seguir.  
  
   **WIM:\\\\** <em>server_name</em> **\share\install.wim:3**  
  
6. Clique em **OK**e em **Instalar**.  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>Para instalar o .NET Framework 3.5 usando o DISM  
  
1.  Execute uma das ações a seguir para abrir uma sessão do Windows PowerShell com direitos de usuário elevados.  
  
    > [!NOTE]  
    > Se você estiver instalando funções e recursos de um servidor remoto, você precisa executar o Windows PowerShell com direitos de usuário elevados.  
  
    -   Na área de trabalho do Windows, clique com o botão direito do mouse no **Windows PowerShell** na barra de tarefas e clique em **Executar como Administrador**.  
  
    -   Sobre o Windows **inicie** tela, clique com botão direito no bloco do Windows PowerShell e, em seguida, na barra de aplicativos, clique em **executar como administrador**.  
  
    -   Em um servidor que está executando a opção de instalação Server Core, digite **PowerShell** em um prompt de comando e pressione **Enter**.  
  
2.  Execute um dos seguintes comandos DISM.  
  
    -   Se o computador tem acesso ao Windows Update, ou um local do arquivo de origem padrão já foi configurado na política de grupo, execute o comando a seguir.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Se o computador tem acesso à mídia de instalação, execute um comando semelhante ao seguinte. No seguinte exemplo, a mídia de instalação do sistema operacional está localizada na unidade D. O parâmetro `LimitAccess` impede que o comando tente entrar em contato com o Windows Update ou com um servidor que esteja executando o WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > O comando DISM diferencia maiúsculas de minúsculas.  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>Configurar fontes alternativas para os arquivos de recursos na Política de Grupo  
A configuração de Política de Grupo descrita nesta seção especifica os locais de origem autorizados para os arquivos do .NET Framework 3.5 e outros arquivos de recursos que foram removidos como parte da configuração Recursos sob Demanda. A configuração de política **especificar configurações para instalação de componentes opcionais e reparo de componentes** está localizado na **computador configuração do Computador\Modelos Administrativos\Sistema** pasta na política de grupo Editor de diretiva de Grupo Local ou de Console de gerenciamento.  
  
> [!NOTE]  
> Você deve ser membro do grupo Administradores para alterar as configurações de Política de Grupo no computador local. Se as configurações de Política de Grupo do computador que você deseja gerenciar forem controladas no nível do domínio, você deverá ser membro do grupo Administradores de Domínio para alterar as configurações de Política de Grupo.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Para configurar um caminho de origem alternativo padrão na Política de Grupo  
  
1. No editor de diretiva de Grupo Local ou o Console de gerenciamento de diretiva de grupo, abra a seguinte configuração de política.  
  
   **configuração do computador\Modelos administrativos\sistema\especificar configurações para instalação de componentes opcionais e reparo de componentes**  
  
2. Sselect **Enabled** para habilitar a configuração de política, se já não estiver habilitado.  
  
3. Na caixa de texto **Caminho de arquivo de origem alternativo** da área **Opções**, especifique o caminho totalmente qualificado para uma pasta compartilhada ou um arquivo WIM. Para especificar um arquivo WIM como um local de arquivo de origem alternativo, adicione o prefixo **WIM:** ao caminho e acrescente o índice da imagem para usar no arquivo WIM como um sufixo. Veja a seguir exemplos de valores que você pode especificar.  
  
   - caminho para uma pasta compartilhada: **\\ \\** <em>nome_do_servidor</em> **\share\\** <em>nome_da_pasta</em>  
  
   - caminho para um arquivo WIM, em que **3** representa o índice da imagem na qual os arquivos de recursos se encontram:  **WIM:\\\\** <em>server_name</em> **\share\install.wim:3**  
  
4. Se você não quiser que os computadores controlados por essa configuração de política para pesquisar arquivos de recurso ausente no Windows Update, selecione **nunca tentar baixar carga do Windows Update**.  
  
5. Se os computadores controlados por essa configuração de política normalmente recebem atualizações pelo WSUS, mas você prefere usar o Windows Update em vez do WSUS para localizar os arquivos de recursos ausentes, selecione **Contatar diretamente o Windows Update para baixar conteúdo de reparo em vez do WSUS (Windows Server Update Services)** .  
  
6. Clique em **OK** quando terminar de alterar essa configuração de política e feche o Editor de Política de Grupo.  
  
## <a name="see-also"></a>Consulte também  
[Opções de instalação do Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considerações de implantação do Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Como habilitar ou desabilitar os recursos do Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


