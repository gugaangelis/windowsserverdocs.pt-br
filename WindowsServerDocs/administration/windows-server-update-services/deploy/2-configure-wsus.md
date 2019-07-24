---
title: 'Etapa 2: configurar o WSUS'
description: Tópico do Windows Server Update Service (WSUS) - configurar o WSUS é a etapa dois em um processo de quatro etapas para implantar o WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae151c2a6f0f5ef72b263fbe7f1f0d26933452af
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810997"
---
# <a name="step-2-configure-wsus"></a>Etapa 2: Configurar o WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de instalar a função de servidor do WSUS no servidor, você precisará configurá-lo adequadamente. A lista de verificação a seguir resume as etapas envolvidas ao executar a configuração inicial para o servidor do WSUS.

|Tarefa|Descrição|
|----|--------|
|[2.1. Configurar conexões de rede](#21-configure-network-connections)|Configure a rede de cluster usando o Assistente de Configuração de Rede.|
|[2.2. Configurar o WSUS usando o Assistente de configuração do WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|Use o assistente de configuração do WSUS para executar a configuração básica do WSUS.|
|[2.3. Configurar grupos de computadores do WSUS](#23-configure-wsus-computer-groups)|Crie grupos de computadores no console de administração do WSUS para gerenciar atualizações na sua organização.|
|[2.4. Configurar atualizações do cliente](#24-configure-client-updates)|Especifique como e quando as atualizações automáticas são aplicadas aos computadores cliente.|
|[2.5. Proteger o WSUS com o protocolo SSL](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|Configure o protocolo SSL para ajudar a proteger o WSUS (Windows Server Update Services).|

## <a name="21-configure-network-connections"></a>2.1. Configurar as conexões de rede
Para começar o processo de configuração, verifique se sabe as respostas às seguintes perguntas:

1.  O firewall do servidor está configurado para permitir que clientes acessem o servidor?

2.  Este computador pode se conectar ao servidor upstream (como aquele designado para baixar atualizações do Microsoft Update)?

3.  Você tem o nome do servidor proxy e as credenciais do usuário referentes ao servidor proxy, se precisar desses dados?

Por padrão, o WSUS está configurado para usar o Microsoft Update como local de onde serão obtidas as atualizações. Se você tiver um servidor proxy na rede, você pode configurar o WSUS para usar o servidor proxy. Se houver um firewall corporativo entre o WSUS e a Internet, você talvez precise configurar o firewall para garantir que o WSUS possa obter atualizações.

> [!TIP]
> Apesar de ser necessária conectividade à Internet para baixar atualizações do Microsoft Update, o WSUS permite importá-las para redes que não está conectadas à Internet.

Quando você tiver as respostas a essas perguntas, poderá começar a configurar as seguintes definições de rede do WSUS:

-   **Atualizações** Especifique a maneira como esse servidor obterá atualizações (no Microsoft Update ou em outro servidor do WSUS).

-   **Proxy** se você identificou que o WSUS precisa usar um servidor proxy para ter acesso à Internet, você precisa definir as configurações de proxy no servidor do WSUS.

-   **Firewall** se você identificou que o WSUS está protegido por um firewall corporativo, há algumas etapas adicionais que devem ser feitas no dispositivo de borda para permitir adequadamente o tráfego do WSUS.

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. Conexão do servidor do WSUS com a Internet
Se houver um firewall corporativo entre o WSUS e a Internet, convém configurar o firewall para garantir que o WSUS possa obter atualizações. Para obter atualizações do Microsoft Update, o servidor do WSUS usa a porta 443 para o protocolo HTTPS. Embora a maioria dos firewalls corporativos permita esse tipo de tráfego, há algumas empresas restringem o acesso à Internet proveniente dos servidores devido a políticas de segurança da empresa. Se sua empresa restringe o acesso, você precisará obter autorização para permitir o acesso à Internet do WSUS para a lista de URLs a seguir:

- http://windowsupdate.microsoft.com

- http://*.windowsupdate.microsoft.com

- https://*.windowsupdate.microsoft.com

- http://*.update.microsoft.com

- http://*.update.microsoft.com

- http://*.windowsupdate.com

- http://download.windowsupdate.com

- https://download.microsoft.com

- http://*.download.windowsupdate.com

- http://wustat.windows.com

- http://ntservicepack.microsoft.com

- http://go.microsoft.com

- http://dl.delivery.mp.microsoft.com

- https://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> Para um cenário em que o WSUS não consegue obter atualizações devido a configurações de firewall, consulte [artigo 885819](https://support.microsoft.com/kb/885819) na Base de dados de Conhecimento Microsoft.

A seção a seguir descreve como configurar um firewall corporativo posicionado entre o WSUS e a Internet. Como o WSUS inicia todo o tráfego de rede, não é necessário configurar o Firewall do Windows no servidor do WSUS. Apesar de a conexão entre o Microsoft Update e o WSUS exigir que as portas 80 e 443 estejam abertas, você pode configurar vários servidores do WSUS para que sejam sincronizadas com uma porta personalizada.

### <a name="212-connection-between-wsus-servers"></a>2.1.2. Conexão entre servidores do WSUS
Os servidores do WSUS upstream e downstream sincronizarão na porta configurada pelo Administrador do WSUS. Por padrão, essas portas são configuradas da seguinte maneira:

-   No WSUS 3.2 e anteriores, a porta 80 para HTTP e 443 para HTTPS

-   No WSUS 6.2 e posteriores (pelo menos Windows Server 2012), a porta 8530 para HTTP e 8531 para HTTPS são usadas

O firewall no servidor do WSUS deve ser configurado para permitir tráfego de entrada nessas portas.

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. Conexão entre clientes (Windows Update Agent) e servidores do WSUS
As portas e interfaces de escuta são configuradas em sites do IIS para o WSUS e em quaisquer configurações de Política de Grupo usadas para configurar PCs cliente. As portas padrão são as mesmas especificadas na seção anterior **Conexão entre servidores do WSUS**e o firewall no servidor do WSUS também deve ser configurado para permitir o tráfego de entrada nessas portas.

## <a name="configure-the-proxy-server"></a>Configurar o servidor proxy
Se a rede corporativa usar servidores proxy, eles devem dar suporte aos protocolos HTTP e SSL e usar a autenticação básica ou a autenticação do Windows. Esses requisitos podem ser atendidos usando uma das seguintes configurações:

1.  Um servidor proxy único que dá suporte a dois canais de protocolo. Nesse caso, defina um canal para usar HTTP e o outro canal para usar HTTPS.

    > [!NOTE]
    > Você pode configurar um servidor proxy que lida com ambos os protocolos do WSUS durante a instalação do software de servidor do WSUS.

2.  Dois servidores proxy, cada um deles dá suporte a um único protocolo. Nesse caso, um servidor proxy é configurado para usar HTTP e o outro servidor proxy é configurado para usar HTTPS.

Para configurar dois servidores proxy, cada um dos quais lidará com um protocolo para o WSUS, use o procedimento a seguir:

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>Para configurar o WSUS para usar dois servidores proxy

1.  Faça logon no computador que deve ser o servidor do WSUS usando uma conta que seja membro do grupo de Administradores local.

2.  Instale a função de servidor do WSUS Durante o Assistente de Configuração do WSUS (discutido na próxima seção) não especifique um servidor proxy.

3.  Abra um prompt de comando (Cmd.exe) como um administrador. Para abrir um prompt de comando como administrador, acesse **Iniciar**. Na **Iniciar pesquisa**, digite **prompt de comando**. na parte superior do menu Iniciar, clique com botão direito **prompt de comando**e, em seguida, clique em **executar como administrador**. Se o **User Account Control** caixa de diálogo for exibida, insira as credenciais adequadas (se solicitado), confirme se a ação exibida é que você deseja e, em seguida, clique em **continuar**.

4.  Na janela do prompt de comando, vá para a pasta C:\Program Files\Update Services\Tools. Digite o seguinte comando:

    **WSUSUTIL ConfigureSSlproxy [< proxy_server proxy_port >]-habilitar**, onde:

    1.  proxy_server é o nome do servidor proxy que dá suporte ao HTTPS.

    2.  proxy_port é o número da porta do servidor proxy.

5.  Feche a janela de prompt de comando.

Para adicionar o servidor proxy que usa o protocolo HTTP à configuração do WSUS, use o procedimento a seguir:

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>Para adicionar um servidor proxy que usa o protocolo HTTP

1.  Abra o Console de Administração do WSUS.

2.  No painel esquerdo, expanda o nome do servidor e, em seguida, clique em **Opções**.

3.  No painel **Opções**, clique em **Origem da Atualização e Servidor de Atualização** e clique na guia **Servidor Proxy**.

4.  Use as opções a seguir para modificar a configuração do servidor proxy existente:

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>Para alterar ou adicionar um servidor proxy na configuração do WSUS

    1.  Marque a caixa de seleção de **Utilizar um servidor proxy ao sincronizar**.

    2.  Na caixa de texto **Nome do Servidor Proxy**, digite o nome do servidor proxy.

    3.  Na caixa de texto **Número da Porta Proxy** , digite o número da porta do servidor proxy O número da porta padrão é 80.

    4.  FF o servidor proxy exige que você use uma conta de usuário específico, selecione o **usar as credenciais do usuário para se conectar ao servidor proxy** caixa de seleção. Digite o nome de usuário necessária, o domínio e a senha nas caixas de texto correspondentes.

    5.  Se o servidor proxy der suporte à autenticação básica, marque a caixa de seleção **Permitir autenticação básica (a senha é enviada em texto não criptografado)** .

    6.  Clique em **OK**.

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>Para remover um servidor proxy da configuração do WSUS

    1.  Para remover um servidor proxy da configuração do WSUS, desmarque a caixa de seleção de **Utilizar um servidor proxy ao sincronizar**.

    2.  Clique em **OK**.

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. Configurar o WSUS usando o Assistente de Configuração do WSUS
Este procedimento pressupõe que você esteja usando o Assistente de Configuração do WSUS, que aparece na primeira vez em que você inicia o Console de Gerenciamento do WSUS. Mais adiante neste tópico, você saberá como executar essas configurações usando a página **Opções**:

#### <a name="to-configure-wsus"></a>Para configurar o WSUS

1.  No painel de navegação do Gerenciador do Servidor, clique em **Painel**, clique em **Ferramentas** e clique em **Windows Server Update Services**.

    > [!NOTE]
    > Se o **concluir a instalação do WSUS** caixa de diálogo for exibida, clique em **executar**. No **concluir a instalação do WSUS** caixa de diálogo, clique em **fechar** quando a instalação for concluída com êxito.

2.  O Assistente do Windows Server Update Services é aberto. Na página **Antes de Começar** , analise a informação e clique em **Avançar**.

3.  Leia as instruções na página **Ingressar no Programa de Aperfeiçoamento do Microsoft Update** e avalie se quer participar. Se você deseja participar do programa. Mantenha a seleção padrão, ou desmarque a caixa de seleção e, em seguida, clique em **próxima**.

4.  Na página **Escolher Servidor Upstream** , há duas opções:

    1.  Sincronizar as atualizações com o Microsoft Update

    2.  Sincronizar de outro servidor do Windows Server Update Services

        -   Se você optar por sincronizar a partir de outro servidor do WSUS, especifique o nome do servidor e a porta na qual este servidor irá se comunicar com o servidor upstream.

        -   Para usar SSL, marque a caixa de seleção **Usar SSL ao sincronizar informações de atualização**. Os servidores usarão a porta 443 para sincronização. (Verifique se esse servidor e o servidor upstream dão suporte a SSL).

        -   Se esse for um servidor de réplica, selecione a **esta é uma réplica do servidor upstream** caixa de seleção.

5.  Depois de selecionar as opções adequadas para sua implantação, clique em **Avançar** para continuar.

6.  Na página **Especificar Servidor Proxy**, marque a caixa de seleção **Utilizar um servidor proxy ao sincronizar** e digite o nome do servidor proxy e o número da porta (porta 80, por padrão) nas caixas correspondentes.

    > [!IMPORTANT]
    > Você precisa concluir essa etapa se identificou que o WSUS precisa de um servidor proxy para ter acesso à Internet.

7.  Se você deseja se conectar ao servidor proxy usando credenciais de usuário específicas, selecione a **usar as credenciais do usuário para se conectar ao servidor proxy** caixa de seleção e, em seguida, digite o nome de usuário, domínio e senha do usuário correspondente caixas. Se você quiser habilitar a autenticação básica para o usuário que está se conectando ao servidor proxy, selecione a **permitir autenticação básica (senha enviada em texto não criptografado)** caixa de seleção.

8.  Clique em **Avançar**. Sobre o **conectar ao servidor Upstream** , clique em **comece conectando-se**.

9. Quando a conexão for estabelecida, clique em **Avançar** para continuar.

10. Sobre o **escolher idiomas** página, você tem a opção de selecionar os idiomas em que o WSUS receberá atualizações – todos os idiomas ou um subconjunto de idiomas. Selecionar um subconjunto de idiomas economizará espaço em disco, mas é importante escolher todos os idiomas que são necessários para todos os clientes desse servidor do WSUS. Se você optar por obter atualizações somente para idiomas específicos, selecione **baixar atualizações somente nestes idiomas**e, em seguida, selecione os idiomas para os quais deseja atualizações; caso contrário, deixe a seleção padrão.

    > [!WARNING]
    > Se você escolher a opção **Baixar atualizações somente nestes idiomas** e este servidor tiver um servidor downstream do WSUS conectado a ele, essa opção forçará o servidor downstream a também usar somente os idiomas escolhidos.

11. Depois de selecionar as opções de idioma adequadas para sua implantação, clique em **Avançar** para continuar.

12. A página **Escolher Produtos** permite especificar os produtos para os quais você deseja atualizações. Selecione as categorias de produto, como Windows, ou produtos específicos, como o Windows Server 2008. Selecionar uma categoria de produto seleciona todos os produtos dessa categoria.

13. Selecione as opções de produto adequadas para sua implantação e, em seguida, clique em **próxima**.

14. Na página **Escolher Classificações** , escolha as classificações de atualização que deseja obter. Escolha todas as classificações ou um subconjunto delas e clique em **Avançar**.

15. A página **Definir Agenda de Sincronização** permite que você selecione se executará a sincronização em modo manual ou automático.

    -   Se você escolher **sincronizar manualmente**, você deve iniciar o processo de sincronização no Console de administração do WSUS.

    -   Se você escolher **sincronizar automaticamente**, o servidor do WSUS será sincronizado em intervalos definidos.

    Defina o tempo para a **Primeira sincronização**e especifique o número de **Sincronizações por dia** que deseja que este servidor execute. Por exemplo, se você especificar que deve haver quatro sincronizações por dia, começando às 3h00, as sincronizações ocorrerão às 3h00, 9h00, 15h00 e 21h00.

16. Depois de selecionar as opções de sincronização adequadas para sua implantação, clique em **Avançar** para continuar.

17. Na página **Concluído** , você tem a opção de começar a sincronização imediatamente marcando a caixa de seleção **Começar a sincronização inicial** . Se você não selecionar essa opção, você precisa usar o Console de gerenciamento do WSUS para executar a sincronização inicial. Clique em **Avançar** se quiser ler mais sobre configurações adicionais ou clique em **Concluir** para concluir este assistente e a configuração inicial do WSUS.

18. Depois de clicar em **Concluir**, será exibido o Console de Gerenciamento do WSUS.

Agora que você executou a configuração básica do WSUS, leia as próximas seções para saber mais detalhes sobre como alterar as configurações usando o Console de Gerenciamento do WSUS.

## <a name="23-configure-wsus-computer-groups"></a>2.3. Configurar grupos de computadores do WSUS
grupos de computadores são uma parte importante das implantações do Windows Server Update Services (WSUS). Os grupos de computadores permitem testar e direcionar atualizações para computadores específicos. Há dois grupos de computadores padrão: Todos os computadores e computadores não atribuídos. Por padrão, quando cada computador cliente contata pela primeira vez o servidor do WSUS, o servidor adiciona esse computador cliente a ambos os grupos.

Você pode criar quantos grupos de computadores personalizados precisar para gerenciar atualizações em sua organização. Como prática recomendada, crie pelo menos um grupo de computadores para testar as atualizações antes de implantá-las em outros computadores de sua organização.

Use o procedimento a seguir para criar um novo grupo e atribuir um computador a esse grupo:

#### <a name="to-create-a-computer-group"></a>Para criar um grupo de computadores

1.  No Console de administração do WSUS, sob **serviços de atualização**, expanda o servidor do WSUS, expanda **computadores**, clique com botão direito **todos os computadores**e, em seguida, clique em **adicionar grupo do computador**.

2.  No **adicionar grupo do computador** na caixa **nome**, especifique o nome do novo grupo e clique em seguida **adicionar**.

3.  Clique em **computadores**e, em seguida, selecione os computadores que você deseja atribuir a esse novo grupo.

4.  Os nomes dos computadores que você selecionou na etapa anterior com o botão direito e, em seguida, clique em **alterar associação**.

5.  No **defina a associação de grupo do computador** caixa de diálogo, selecione o teste de grupo que você criou e, em seguida, clique em **Okey**.

## <a name="24-configure-client-updates"></a>2.4. Configurar atualizações do cliente
A Instalação do WSUS configura automaticamente o IIS para que distribua a versão mais recente das Atualizações Automáticas a cada computador cliente que contata o servidor do WSUS. A melhor maneira de configurar as Atualizações Automáticas depende do ambiente de rede.

-   Em um ambiente que usa o serviço de diretório do Active Directory, você pode usar um existente baseado em domínio grupo de política de GPO (objeto) ou criar um novo GPO.

-   Em um ambiente sem o active directory, use o editor de diretiva de Grupo Local para configurar as atualizações automáticas e aponte os computadores cliente ao servidor do WSUS.

> [!IMPORTANT]
> Os procedimentos a seguir pressupõem que a rede execute do Active Directory. Esses procedimentos também pressupõem que você tem familiaridade com Política de Grupo e a use para gerenciar a rede.

Use os procedimentos a seguir para configurar as Atualizações Automáticas para computadores clientes:

-   [Etapa 4: Definir as configurações da Política de Grupo para atualizações automáticas](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3. Configurar grupos de computadores](#23-configure-wsus-computer-groups) neste tópico

### <a name="configure-automatic-updates-in-group-policy"></a>Configurar Atualizações Automáticas na Política de Grupo

Se você tiver definido o active directory em sua rede, você pode configurar um ou vários computadores simultaneamente incluindo-os em um objeto de política de grupo (GPO) e configurando esse GPO com as configurações do WSUS. É recomendável criar um novo GPO que contenha somente as definições do WSUS.

Vincule esse GPO do WSUS para um contêiner do Active Directory que é apropriado para seu ambiente. Em um ambiente simples, você pode vincular um único GPO do WSUS ao domínio. Em um ambiente mais complexo, você pode vincular vários GPOs do WSUS a várias OUs (unidades organizacionais), que permitirão aplicar diferentes definições de política do WSUS a diferentes tipos de computadores.

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>Para habilitar o WSUS por meio de um GPO de domínio

1.  No grupo de política de gerenciamento GPMC (Console), navegue até o GPO no qual você deseja configurar o WSUS e, em seguida, clique em **editar**.

2.  No GPMC, expanda **computer Configuration**, expanda **diretivas**, expanda **modelos administrativos**, expanda **componentes do Windows**e, em seguida, clique em **Windows Update**.

3.  No painel de detalhes, clique duas vezes em **Configurar Atualizações Automáticas**. A política **Configurar Atualizações Automáticas** é aberta.

4.  Clique em **Habilitado** e selecione uma das opções a seguir na definição **Configurar atualização automática**:

    -   **Avisar antes de baixar e de instalar qualquer atualização**. Esta opção notifica um usuário administrativo conectado antes de baixar e instalar as atualizações.

    -   **Baixar automaticamente e notificar antes de instalar**. Esta opção começa automaticamente a baixar as atualizações e notifica um usuário administrativo conectado antes de instalá-las. Por padrão, essa opção é selecionada.

    -   **Baixar automaticamente e agendar a instalação**. Esta opção começa automaticamente a baixar as atualizações e as instala no dia e hora que você especificar.

    -   **Permitir que o administrador local escolha a configuração**. Esta opção permite que administradores locais usem as Atualizações Automáticas do Painel de Controle para escolher uma opção de configuração. Por exemplo, eles podem optar por agendar uma instalação. Os administradores locais não podem desabilitar as Atualizações Automáticas.

5.  Selecione **Habilitar destino do lado do cliente**, selecione **Enabled**e, em seguida, digite o nome do grupo de computadores WSUS ao qual você deseja adicionar este computador no **nome do grupo de destino para este computador**  caixa.

    > [!NOTE]
    > **Habilitar destino do lado do cliente** permite que os computadores cliente se adicionem aos grupos de computadores de destino no servidor do WSUS quando as Atualizações Automáticas são redirecionadas para um servidor do WSUS. Se o status é definido como habilitado, este computador se identificará como um membro de um determinado grupo de computadores quando ele envia informações para o servidor do WSUS, que utiliza para determinar quais atualizações são implantadas no computador. Essa configuração indica ao servidor do WSUS qual grupo o computador cliente usará. Você deve criar o grupo no servidor do WSUS e adicionar computadores membros do domínio ao grupo.

6.  Clique em **OK** para fechar a política **Habilitar destino do lado do cliente** e retornar ao painel de detalhes do Windows Update.

7.  Clique em **OK** para fechar a política **Configurar Atualizações Automáticas** e retornar ao painel de detalhes do Windows Update.

8.  No painel de detalhes do **Windows Update** , clique duas vezes em **Especificar o local do serviço de atualização na intranet da Microsoft**.

9. Clique em **Habilitado** e, em seguida, no servidor nas caixas de texto **Definir o serviço intranet de atualização para detectar atualizações** e **Definir o servidor de estatísticas da intranet** Por exemplo, digite *http://servername* em ambas as caixas (em que *servername* é o nome do servidor do WSUS).

    > [!WARNING]
    > Ao digitar o endereço da intranet do servidor do WSUS, verifique se especificou qual porta será usada. Por padrão, o WSUS usará a porta 8530 para HTTP, e 8531 para HTTPS. Por exemplo, se você estiver usando HTTP, você deverá digitar **http://servername:8530** .

10. Clique em **OK**.

Depois de configurar um computador cliente, levará vários minutos antes do computador apareça na **computadores** página no Console de administração do WSUS. Para computadores clientes que estão configurados com um Objeto de Política de Grupo baseado em domínio, pode levar cerca de 20 minutos para que a Política de Grupo aplique as novas definições de política ao computador cliente. Por padrão, as atualizações de política de grupo em segundo plano a cada 90 minutos, com um deslocamento aleatório de 0 a 30 minutos. Se você quiser atualizar a política de grupo mais cedo, você pode abrir uma janela de prompt de comando no cliente computador e digite gpupdate /force.

para computadores cliente que são configurados usando o editor de diretiva de Grupo Local, o GPO é aplicado imediatamente e a atualização leva cerca de 20 minutos. Se você começar a detecção manualmente, você não precisa aguardar 20 minutos para o computador cliente, entre em contato com o WSUS.

Como a espera para que a detecção comece pode ser um processo demorado, você pode usar o seguinte procedimento para iniciá-la imediatamente.

##### <a name="to-initiate-wsus-detection"></a>Para iniciar uma detecção do WSUS

1.  No computador cliente, abra uma janela de prompt de comando com privilégios elevados.

2.  digite wuauclt.exe. exe /detectnow e, em seguida, pressione ENTER.

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. Proteger o WSUS com o protocolo SSL

Você pode usar o protocolo SSL para ajudar a proteger a implantação do WSUS. O WSUS usa o SSL para autenticar computadores cliente e servidores de WSUS downstream para o servidor do WSUS. Além disso, o WSUS usa SSL para criptografar metadados de atualização.

> [!IMPORTANT]
> Os clientes e servidores downstream que estão configurados para usar a TLS (Segurança da Camada de Transporte) ou HTTPS também devem ser configurados para usar um FQDN (nome de domínio totalmente qualificado) para o servidor do WSUS upstream.

O WSUS usa SSL apenas para metadados, não para arquivos de atualização. Essa é a mesma forma que o Microsoft Update distribui as atualizações. A Microsoft reduz o risco de enviar arquivos de atualização por um canal não criptografado assinando cada atualização. Além disso, um hash é calculado e enviado junto com os metadados para cada atualização. Quando uma atualização é baixada, o WSUS verifica a assinatura digital e o hash. Se a atualização tiver sido alterada, ele não está instalado.

### <a name="limitations-of-wsus-ssl-deployments"></a>Limitações das implantações de SSL do WSUS
Você deve considerar as seguintes limitações ao usar SSL para proteger uma implantação do WSUS:

1.  O uso de SSL aumenta a carga de trabalho do servidor. Você deve esperar uma queda de 10% do desempenho devido ao custo de criptografar todos os metadados que são enviados pela rede.

2.  Se você usar o WSUS com um banco de dados do SQL Server remoto, a conexão entre o servidor do WSUS e o servidor de banco de dados não é protegida por SSL. Se a conexão de banco de dados deve ser protegida, considere as seguintes recomendações:

-   Mova o banco de dados do WSUS para o servidor do WSUS.

-   Mova o servidor de banco de dados remoto e o servidor do WSUS para uma rede privada.

-   Implante o IPsec (Segurança de Protocolo IP) para ajudar a proteger o tráfego de rede. Para obter mais informações sobre IPsec, consulte [Criando e usando políticas IPSec](https://go.microsoft.com/fwlink/?LinkID=203841).

### <a name="configure-ssl-on-the-wsus-server"></a>Configurar o SSL no servidor do WSUS
O WSUS requer duas portas para o SSL: uma porta que usa HTTPS para enviar metadados criptografados e uma porta que usa HTTP para enviar atualizações. Ao configurar o WSUS para usar SSL, considere o seguinte:

-   Você não pode configurar o site inteiro do WSUS para exigir SSL porque todo o tráfego para o site do WSUS precisa ser criptografado. O WSUS criptografa apenas os metadados de atualização. Se um computador tentar se recuperar arquivos de atualização na porta HTTPS, a transferência falhará.

    Você deve exigir o SSL apenas para as seguintes raízes virtuais:

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    Você não deve exigir o SSL para as seguintes raízes virtuais:

    -   **Content**

    -   **Inventário**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   O certificado da AC (autoridade de certificação) deve ser importado para o repositório de AC raiz confiável do computador local ou o repositório de AC raiz confiável do Windows Server Update Service em servidores do WSUS downstream. Se o certificado foi importado apenas para o AC raiz confiável do usuário Local do repositório, o servidor do WSUS downstream não será autenticado no servidor upstream.

    Para obter mais informações sobre como usar certificados SSL no IIS, consulte [exigem Secure Sockets Layer (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846).

-   Você deve importar o certificado para todos os computadores que se comunicarão com o servidor do WSUS. Isso inclui todos os computadores cliente, servidores downstream e computadores que executam o Console de Administração do WSUS. O certificado deve ser importado para o repositório de AC raiz confiável do computador local ou para o repositório de AC raiz confiável do Windows Server Update Service.

-   Você pode usar qualquer porta para o SSL. No entanto, a porta que você configurar para o SSL também determina a porta que o WSUS usa para enviar o tráfego HTTP não criptografado. Considere os exemplos a seguir:

    -   Se você usar a porta padrão do setor 443 para tráfego HTTPS, o WSUS usa a porta padrão do setor 80 para tráfego HTTP não criptografado.

    -   Se você usar qualquer porta diferente de 443 para tráfego HTTPS, o WSUS enviará o tráfego HTTP não criptografado pela porta que numericamente vem antes da porta para HTTPS. Por exemplo, se você usar a porta 8531 para HTTPS, o WSUS usará porta 8530 para HTTP.

-   Você deve reiniciar o *ClientServicingProxy* se o nome do servidor, a configuração de SSL ou o número da porta for alterado.

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>Para configurar o SSL no servidor raiz do WSUS

1.  Faça logon no servidor do WSUS usando uma conta que seja membro do grupo de Administradores do WSUS ou do grupo de Administradores local.

2.  Vá para **iniciar**, digite **CMD**, clique com botão direito **prompt de comando**e, em seguida, clique em **executar como administrador**.

3.  Navegue até a *%ProgramFiles%* **\Update services\tools.\\* * pasta.

4.  Na janela do prompt de comando, digite o seguinte comando:

    **WSUSUTIL. exe configuressl** *certificateName*

    onde:

    *certificateName* é o nome DNS do servidor do WSUS.

### <a name="configure-ssl-on-client-computers"></a>Configurar o SSL em computadores cliente
Ao configurar o SSL em computadores cliente, você deve considerar as seguintes questões:

-   Você deve incluir uma URL para uma porta segura no servidor do WSUS. Como você não pode exigir o SSL no servidor, a única maneira de garantir que os computadores cliente podem usar um canal de segurança é usando uma URL que especifica o HTTPS. Se você usar qualquer porta diferente de 443 para SSL, você deve incluir essa porta na URL também.

-   O certificado em um computador cliente deve ser importado para o repositório de AC raiz confiável do computador Local ou da autoridade de certificação de raiz de confiança do serviço de atualização automática. Se o certificado é importado para o repositório de AC raiz confiável do usuário Local apenas, as atualizações automáticas apresentarão falha na autenticação de servidor.

-   Os computadores cliente devem confiar no certificado que você associar ao servidor do WSUS. Dependendo do tipo de certificado usado, você precisará configurar um serviço para habilitar os computadores cliente para confiarem no certificado vinculado ao servidor do WSUS.

### <a name="configure-ssl-for-downstream-wsus-servers"></a>Configurar o SSL para servidores do WSUS downstream
As instruções a seguir configuram um servidor downstream para sincronizar com um servidor upstream que usa o SSL.

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>Para sincronizar um servidor downstream para um servidor upstream que usa o SSL

1.  Faça logon no computador usando uma conta de usuário que seja membro do grupo de Administradores local ou do grupo de Administradores do WSUS.

2.  Clique em **inicie**, clique em **todos os programas**, clique em **ferramentas administrativas**e, em seguida, clique em **Windows Server Update Services**.

3.  No painel direito, expanda o nome do servidor.

4.  Clique em **Opções**e em **Origem da Atualização e Servidor Proxy**.

5.  Na página **Origem da Atualização** , selecione **Sincronizar de outro servidor do Windows Server Update Services**.

6.  Digite o nome do servidor upstream para o **nome do servidor** caixa de texto. Digite o número da porta usada pelo servidor para conexões SSL para o **número da porta** caixa de texto.

7.  Selecione o **usar SSL ao sincronizar informações de atualização** caixa de seleção e, em seguida, clique em **Okey**.

### <a name="additional-ssl-resources"></a>recursos adicionais do SSL
As etapas necessárias para configurar uma autoridade de certificação, associe o certificado ao site do WSUS e estabelecer uma relação de confiança entre os computadores cliente e o certificado estão além do escopo deste guia. Para obter mais informações e instruções sobre como instalar certificados e configurar esse ambiente, consulte os seguintes tópicos:

-   [Guia passo a passo do Suite B PKI](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [Implementando e administrando modelos de certificado](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [Guia de migração e do Active Directory, atualização de serviços de certificado](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [Configurar o registro automático de certificado](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. Concluir a configuração de IIS
Por padrão, o acesso de leitura anônimo está habilitado para site padrão e todos os novos sites do IIS. Alguns aplicativos, principalmente o Windows SharePoint Services, podem remover o acesso anônimo. Se isso tiver ocorrido, você deve reabilitar o acesso de leitura anônimo antes de instalar e operar o WSUS.

Para habilitar o acesso de leitura anônimo, siga as etapas para a versão aplicável do IIS:

1.  [Habilitar autenticação Anônima (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316), conforme documentado no Guia de Operações do IIS 7.

2.  [Habilitando a autenticação anônima (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391), conforme documentado no Guia de Operações do IIS 6.0.

### <a name="27-configure-a-signing-certificate"></a>2.7. Configurar um certificado de assinatura
O WSUS tem a capacidade de publicar pacotes de atualização personalizada para atualizar produtos Microsoft e não Microsoft. O WSUS pode assinar esses pacotes de atualização personalizada automaticamente para você com um certificado Authenticode. Para habilitar a assinatura de atualização personalizada, você deve instalar um certificado de assinatura de pacote no seu servidor do WSUS. Há várias considerações associadas à assinatura de atualização personalizada.

1.  **Distribuição de certificados**. A chave privada deve ser instalada no servidor do WSUS e a chave pública deve ser explicitamente instalada no repositório de certificados confiáveis em todos os servidores e PCs cliente que devem receber atualizações assinadas personalizadas.

2.  **Expiração** Quando o certificado autoassinado expirar ou se aproximar da expiração, o WSUS registrará os eventos no log de eventos.

3.  **Atualizações/revogação de certificados**. Se você quiser atualizar ou revogar um certificado (ou seja, depois de descobrir que ele expirou), o WSUS não oferecia nenhuma funcionalidade para habilitar isso. Fazer isso se tornou uma tarefa manual que era muito difícil fazer manualmente ou de automatizar com êxito.


