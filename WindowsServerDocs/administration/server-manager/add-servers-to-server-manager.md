---
title: Add Servers to Server Manager
description: Gerenciador do Servidor
ms.topic: article
ms.assetid: aab895f2-fe4d-4408-b66b-cdeadbd8969e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.localizationpriority: medium
ms.date: 02/01/2018
ms.openlocfilehash: 2280dd901756c033a16e5203ad60bc0b6cad5ce4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895829"
---
# <a name="add-servers-to-server-manager"></a>Add Servers to Server Manager

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, você pode gerenciar vários servidores remotos usando um único console do Gerenciador de servidores. Servidores que você deseja gerenciar usando o Gerenciador do servidor podem estar executando Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Observe que não é possível gerenciar uma versão mais recente do Windows Server com uma versão mais antiga do Gerenciador de servidores.

Este tópico descreve como adicionar servidores ao pool de servidores do Gerenciador de servidores.

> [!NOTE]
> Em nossos testes, o Server Manager no Windows Server 2012 e versões posteriores do Windows Server podem ser usados para gerenciar até 100 servidores que estão configurados com uma carga de trabalho típica. O número de servidores que podem ser gerenciadas usando um único console do Gerenciador de servidores pode variar dependendo da quantidade de dados que você solicita do servidores gerenciados e recursos de hardware e rede disponíveis para o computador que executa o Gerenciador de servidores. Conforme a quantidade de dados que você deseja exibir se aproxima de capacidade do recurso desse computador, você pode enfrentar respostas lentas do Gerenciador de servidores e atrasos na finalização de atualizações. Para ajudar a aumentar o número de servidores que você pode gerenciar usando o Gerenciador de servidores, é recomendável limitar os dados de evento que o Gerenciador do servidor obtém dos seus servidores gerenciados usando as configurações na caixa de diálogo **Configurar dados de eventos** . É possível abrir Configurar Dados do Evento no menu **Tarefas** do bloco **Eventos**. Se você precisar gerenciar um número de servidores de nível empresarial em sua organização, recomendamos a avaliação de produtos no [pacote do Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
>
> Gerenciador de servidores podem receber status apenas online ou offline dos servidores que estão executando o Windows Server 2003. Embora você possa usar o Gerenciador de servidores para executar tarefas de gerenciamento em servidores que estão executando o Windows Server 2008 R2 ou Windows Server 2008, você não pode adicionar funções e recursos para servidores que estão executando o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 .
>
> Gerenciador de servidores não podem ser usados para gerenciar uma versão mais recente do sistema operacional Windows Server. Gerenciador de servidores executando o Windows Server 2012 R2, Windows Server 2012, Windows 8 ou Windows 8.1 não pode ser usado para gerenciar os servidores que estão executando o Windows Server 2016.

Este tópico inclui as seções a seguir.

-   [Adicionar servidores a serem gerenciados](#BKMK_add)

-   [Fornecer credenciais com o comando Gerenciar Como](#BKMK_creds)

## <a name="provide-credentials-with-the-manage-as-command"></a><a name=BKMK_creds></a>Fornecer credenciais com o comando Gerenciar Como
Como adicionar servidores remotos ao Gerenciador do servidor, alguns dos servidores que você adicionar podem exigir credenciais da conta de usuário diferente para acessar ou gerenciá-los. Para especificar as credenciais para um servidor gerenciado que serão diferentes daquele que você pode usar para fazer logon no computador no qual você está executando o Gerenciador de servidores, use o comando **Gerenciar como** depois de adicionar um servidor ao Gerenciador de servidores, que pode ser acessada clicando com o entrada para um servidor gerenciado em blocos de **servidores** da home page de uma função ou grupo. Ao clicar em **Gerenciar Como** a caixa de diálogo **Segurança do Windows** é exibida, na qual você pode fornecer um nome de usuário que tenha direitos de acesso no servidor gerenciado, em um dos formatos a seguir.

-   *Nome de usuário*

-   *Nome de usuário*@example.domain.com

-   *Domínio* \\ do *Nome de usuário*

A caixa de diálogo de **Segurança do Windows** que é aberta, o comando **Gerenciar como** não é possível aceitar credenciais de cartão inteligente; Não há suporte para fornecer credenciais de cartão inteligente por meio do Gerenciador de servidores. As credenciais que você fornecer para um servidor gerenciado usando o comando **Gerenciar como** são armazenados em cache e persistem enquanto você está gerenciando o servidor usando o mesmo computador no qual você está executando atualmente o Gerenciador do servidor ou desde que você não substitua-las pela especificar credenciais em branco ou diferentes para o mesmo servidor. Se você exportar as configurações do Gerenciador do servidor para outros computadores ou configurar seu perfil do domínio para ser roaming para permitir que as configurações do Gerenciador do servidor a ser usado em outros computadores, **Gerenciar como** credenciais para servidores em seu pool de servidores não são armazenadas no roaming perfil. Os usuários do Gerenciador do servidor devem adicioná-los em cada computador do qual deseja gerenciar.

Depois de adicionar servidores para gerenciar seguindo os procedimentos neste tópico, mas antes de usar o comando **Gerenciar Como** para especificar credenciais alternativas que podem ser necessárias para gerenciar um servidor que você adicionou, os seguintes erros de status de capacidade de gerenciamento podem ser exibidos para o servidor:

-   Erro de resolução de destino do Kerberos

-   Erro de autenticação do Kerberos

-   Acesso online negado

> [!NOTE]
> Funções e recursos que não dão suporte ao comando **gerenciar como** incluem serviços de área de trabalho remota (RDS) e servidor IPAM (gerenciamento de endereço IP). Se você não pode gerenciar o servidor remoto do RDS ou IPAM usando as mesmas credenciais que você está usando no computador no qual você está executando o Gerenciador de servidores, adicionando a conta que você normalmente usa para gerenciar esses servidores remotos ao grupo Administradores no computador de teste que está executando o Gerenciador de servidores. Em seguida, faça logon no computador que está executando o Gerenciador de servidores com a conta usada para gerenciar o servidor remoto que está executando o rdS ou IPAM.

## <a name="add-servers-to-manage"></a><a name=BKMK_add></a>Adicionar servidores para gerenciamento
Você pode adicionar servidores ao Gerenciador do servidor para gerenciar usando qualquer um dos três métodos na caixa de diálogo **Adicionar servidores** .

-   **Active Directory Domain Services** adicionar servidores para gerenciar que o Active Directory encontra no mesmo domínio que o computador local.

-   **Entrada do Sistema de Nomes de Domínio (DNS)** Pesquise servidores para gerenciar por nome do computador ou endereço IP.

-   **Importar vários servidores** Especifique vários servidores a serem importados em um arquivo que contenha servidores listados por nome do computador ou endereço IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Para adicionar servidores ao pool de servidores

1.  Se o Gerenciador do Servidor já estiver aberto, vá para a etapa seguinte. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.

    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.

    -   Na tela **Iniciar** do Windows, clique no bloco Gerenciador do servidor.

2.  No menu **gerenciar** , clique em **adicionar servidores**.

3.  Execute uma delas.

    -   Na guia **Active Directory** , selecione os servidores que estão no domínio atual. Pressione **Ctrl** enquanto seleciona vários servidores. Clique no botão de seta para a direita para mover os servidores selecionados para a lista **selecionada** .

    -   Na guia **DNS**, digite os primeiros caracteres do nome ou endereço IP de um computador e pressione **Enter** ou clique em **Pesquisar**. Selecione os servidores que você deseja adicionar e, em seguida, clique no botão de seta para a direita.

    -   Na guia **importar** , procure um arquivo de texto que contenha os nomes DNS ou endereços IP dos computadores que você deseja adicionar, um nome ou endereço IP por linha.

4.  Após concluir a adição de servidores, clique em **OK**.

### <a name="add-and-manage-servers-in-workgroups"></a>Adicionar e gerenciar servidores em grupos de trabalho
Embora a adição de servidores que estão em grupos de trabalho ao Gerenciador de servidores possa ser bem-sucedida, depois que eles são adicionados, a coluna de **Capacidade de gerenciamento** do bloco **Servidores** em uma página de função ou grupo que inclui um servidor de grupo de trabalho pode exibir erros de **Credenciais inválidas** que ocorrem ao tentar se conectar ao ou coletar dados do servidor remoto do grupo de trabalho.

Esses erros ou erros semelhantes podem ocorrer nas condições a seguir.

-   O servidor gerenciado está no mesmo grupo de trabalho do computador que está executando o Gerenciador do servidor.

-   O servidor gerenciado está em outro grupo de trabalho do computador que está executando o Gerenciador do servidor.

-   Um dos computadores está um grupo de trabalho, enquanto o outro está em um domínio.

-   O computador que está executando o Gerenciador do servidor está em um grupo de trabalho e os servidores remotos gerenciados estão em uma sub-rede diferente.

-   Ambos os computadores estão em domínios, mas não há nenhuma relação de confiança entre os dois domínios.

-   Ambos os computadores estão em domínios, mas há apenas uma relação de confiança unidirecional entre os dois domínios.

-   O servidor que você deseja gerenciar foi adicionado usando seu endereço IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Para adicionar servidores de grupo de trabalho remotos ao Gerenciador do Servidor

1.  No computador que está executando o Gerenciador do servidor, adicione o nome do servidor de grupo de trabalho à lista **TrustedHosts**. Este é um requisito da autenticação NTLM. Por exemplo, para adicionar o nome do computador a uma lista existente de hosts confiáveis, adicione o parâmetro `Concatenate` ao comando. Por exemplo, para adicionar o computador `Server01` a uma lista existente de hosts confiáveis, use o comando a seguir.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determine se o servidor de grupo de trabalho que você deseja gerenciar está na mesma sub-rede que o computador no qual você está executando o Gerenciador do Servidor.

    Se os dois computadores estiverem na mesma sub-rede ou se o perfil de rede do servidor de grupo de trabalho estiver definido como **privado** no **centro de rede e compartilhamento**, vá para a próxima etapa.

    Se eles não estiverem na mesma sub-rede ou se o perfil de rede do servidor do grupo de trabalho não estiver definido como **privado**, no servidor de grupo de trabalho, altere a configuração de entrada do **gerenciamento remoto do Windows (http-in)** no firewall do Windows para permitir explicitamente conexões de computadores remotos, adicionando os nomes de computador na guia **computadores** da caixa de diálogo **Propriedades** da configuração.

3.  > [!IMPORTANT]
    > A execução do cmdlet nesta etapa substitui as medidas de UAC (Controle de Conta do Usuário) que impedem processos elevados de executar em computadores do grupo de trabalho, a menos que o Administrador interno ou a conta do Sistema esteja executando os processos. O cmdlet permite que membros do grupo de Administradores gerenciem o servidor de grupo de trabalho sem fazer logon como o Administrador interno. Dar permissão para que usuários adicionais gerenciem o servidor de grupo de trabalho pode reduzir sua segurança; no entanto, isso é mais seguro do que fornecer credenciais da conta de Administrador interno, em que pode haver árias pessoas gerenciando o servidor de grupo de trabalho.

    Para substituir as restrições de UAC em processos executados com privilégios elevados nos computadores de grupo de trabalho, crie uma entrada de Registro chamada **LocalAccountTokenFilterPolicy** no servidor de grupo trabalho executando o cmdlet a seguir.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  No computador no qual você está executando o Gerenciador do Servidor, abra a página **Todos os servidores**.

5.  Se o computador que está executando o Gerenciador do Servidor e o servidor de grupo de trabalho de destino estiverem no mesmo grupo de trabalho, vá para a última etapa. Se os dois computadores não estiverem no mesmo grupo de trabalho, clique com o botão direito do mouse no servidor de grupo de trabalho de destino no bloco **Servidores** e clique em **Gerenciar como**.

6.  Faça logon no servidor de grupo de trabalho usando a conta de Administrador interno para o servidor de grupo de trabalho.

7.  Verifique se o Gerenciador do Servidor é capaz de se conectar a e coletar dados do servidor de grupo de trabalho ao atualizar a página **Todos os servidores** e, em seguida, exibindo o status de capacidade de gerenciamento do servidor de grupo de trabalho.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Para adicionar servidores remotos quando o Gerenciador do Servidor está executando em um computador de grupo de trabalho

1.  No computador que está executando o Gerenciador do Servidor, adicione servidores remotos à lista **TrustedHosts** do computador local em uma sessão do Windows PowerShell. Por exemplo, para adicionar o nome do computador a uma lista existente de hosts confiáveis, adicione o parâmetro `Concatenate` ao comando. Por exemplo, para adicionar o computador `Server01` a uma lista existente de hosts confiáveis, use o comando a seguir.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determine se o servidor que você deseja gerenciar está na mesma sub-rede que o computador do grupo de trabalho no qual você está executando o Gerenciador do Servidor.

    Se os dois computadores estiverem na mesma sub-rede ou se o perfil de rede do grupo de trabalho do computador estiver definido como **Privada** na **Central de Rede e Compartilhamento**, vá para a próxima etapa.

    Se elas não estiverem na mesma sub-rede ou se o perfil de rede do grupo de trabalho do computador não estiver definido como **particular**, no computador de grupo de trabalho que está executando o Gerenciador do servidor, altere a configuração do **gerenciamento remoto do Windows (HTTP-entrada)** entrada no Firewall do Windows para permitir explicitamente conexões de computadores remotos adicionando os nomes de computador na guia **computadores** da caixa de diálogo **Propriedades** da configuração.

3.  No computador no qual você está executando o Gerenciador do Servidor, abra a página **Todos os servidores**.

4.  Verifique se o Gerenciador de servidores é capaz de se conectar a e coletar dados do servidor remoto atualizar a página de **Todos os servidores** e, em seguida, exibindo o status de capacidade de gerenciamento de servidor remoto. Se o bloco **Servidores** ainda exibir um erro de gerenciamento para o servidor remoto, vá para a próxima etapa.

5.  Faça logoff do computador no qual você está executando o Gerenciador do servidor e, em seguida, faça logon novamente usando a conta de administrador interna. Repita a etapa anterior, para verificar se o Gerenciador de servidores é capaz de se conectar a e coletar dados do servidor remoto.

Se você seguiu os procedimentos nesta seção e continuar tendo problemas para gerenciar computadores do grupo de trabalho ou gerenciar outros computadores em computadores do grupo de trabalho, consulte [about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx) no site da Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Adicionar e gerenciar servidores em clusters
Você pode usar o Gerenciador do Servidor para gerenciar os servidores que estão em clusters de failover (também chamados de clusters de servidores ou MSCS). Os servidores nos clusters de failover (se os nós do cluster são físicos ou virtuais) têm algumas limitações de gerenciamento e comportamentos exclusivos no Gerenciador do Servidor.

-   Os servidores físicos e virtuais em clusters são adicionados automaticamente ao Gerenciador do Servidor quando um servidor do cluster é adicionado ao Gerenciador do Servidor. Da mesma forma, quando você remove um servidor de cluster do Gerenciador do Servidor, você deverá remover os outros servidores no cluster.

-   O Gerenciador do Servidor não exibe dados para servidores virtuais agrupados, pois os dados são dinâmicos e idênticos aos dados do servidor no qual o nó de cluster virtual está hospedado. Você pode selecionar o servidor que hospeda o servidor virtual para exibir seus dados.

-   Se você adicionar um servidor ao Gerenciador do Servidor usando o nome do objeto do servidor virtual do cluster; o nome do objeto virtual é exibido no Gerenciador do Servidor em vez do nome do servidor físico (esperado).

-   Não é possível instalar funções e recursos em um servidor virtual clusterizado.

## <a name="see-also"></a>Consulte Também
[Gerenciador do servidor](server-manager.md) 
 [criar e gerenciar grupos de servidores](create-and-manage-server-groups.md)
