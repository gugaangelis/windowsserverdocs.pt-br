---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Apêndice D-protegendo contas de administrador internas no Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3e0060e4b732fe77de4371c7b84b77da21de9e20
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821659"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice D: Proteger contas de administrador internas no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice D: Proteger contas de administrador internas no Active Directory  
Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é, por padrão, um membro dos grupos Administradores de domínio e administradores no domínio, e se o domínio for o domínio raiz da floresta, a conta também será um membro do grupo Administradores de empresa.

O uso da conta de administrador de um domínio deve ser reservado somente para atividades de compilação iniciais e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador possa ser usada para afetar os reparos no caso de que nenhuma outra conta possa ser usada, você não deve alterar a associação padrão da conta de administrador em nenhum domínio na floresta. Em vez disso, você deve proteger a conta de administrador em cada domínio na floresta, conforme descrito na seção a seguir e detalhado nas instruções passo a passo a seguir. 

> [!NOTE]
> Este guia costumava ser usado para recomendar a desabilitação da conta. Isso foi removido, pois a white paper de recuperação de floresta usa a conta de administrador padrão. O motivo é que essa é a única conta que permite o logon sem um servidor de catálogo global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador internas  
Para a conta de administrador interno em cada domínio em sua floresta, você deve definir as seguintes configurações:  

-   Habilitar a **conta é confidencial e o sinalizador não pode ser delegado** na conta.  

-   Habilitar o **cartão inteligente é necessário para** o sinalizador de logon interativo na conta.  

-   Configure GPOs para restringir o uso da conta de administrador em sistemas ingressados no domínio:  

    -   Em um ou mais GPOs que você cria e vincula a estações de trabalho e UOs de servidor membro em cada domínio, adicione a conta de administrador de cada domínio aos seguintes direitos de usuário no **computador \** \ \ \ \ Configurações de direitos:  

        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Ao adicionar contas a essa configuração, você deve especificar se está configurando contas de administrador local ou contas de administrador de domínio. Por exemplo, para adicionar a conta de administrador do domínio NWTRADERS a esses direitos de negação, você deve digitar a conta como Nwtraders\administrador ou navegar até a conta de administrador do domínio NWTRADERS. Se você digitar "administrador" nessas configurações de direitos de usuário na Editor de Objeto de Política de Grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado.
>   
> É recomendável restringir contas de administrador local em estações de trabalho e servidores membros da mesma maneira que as contas de administrador baseadas em domínio. Portanto, você geralmente deve adicionar a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais a essas configurações de direitos de usuário. A captura de tela a seguir mostra um exemplo de configuração desses direitos de usuário para bloquear as contas de administrador local e a conta de administrador de um domínio de executar logons que não devem ser necessários para essas contas.  


![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar GPOs para restringir contas de administrador em controladores de domínio  
    -   Em cada domínio na floresta, o GPO controladores de domínio padrão ou uma política vinculada à UO Controladores de domínio deve ser modificada para adicionar a conta de administrador de cada domínio aos seguintes direitos de usuário no **computador \** \ \ \ \ \ Configurações de direitos:   
        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Essas configurações garantirão que a conta de administrador interno do domínio não possa ser usada para se conectar a um controlador de domínio, embora a conta, se habilitada, possa fazer logon localmente nos controladores de domínio. Como essa conta só deve ser habilitada e usada em cenários de recuperação de desastres, é esperado que o acesso físico a pelo menos um controlador de domínio esteja disponível ou que outras contas com permissões para acessar controladores de domínio remotamente possam ser usadas.  

-   Configurar a auditoria de contas de administrador  

    Quando você tiver protegido a conta de administrador de cada domínio e desabilitá-la, deverá configurar a auditoria para monitorar as alterações feitas na conta. Se a conta estiver habilitada, sua senha for redefinida ou outras modificações forem feitas na conta, os alertas deverão ser enviados aos usuários ou às equipes responsáveis pela administração do Active Directory, além das equipes de resposta a incidentes em sua organização.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instruções passo a passo para proteger contas de administrador internas no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e em **Active Directory usuários e computadores**.  

2.  Para evitar ataques que aproveitam a delegação para usar as credenciais da conta em outros sistemas, execute as seguintes etapas:  

    1.  Clique com o botão direito do mouse na conta de **administrador** e clique em **Propriedades**.  

    2.  Clique na guia **conta** .  

    3.  Em **Opções de conta**, selecione a **conta é confidencial e não pode ser** demarcada como indicado na captura de tela a seguir e clique em **OK**.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar o **cartão inteligente é necessário para** o sinalizador de logon interativo na conta, execute as seguintes etapas:  

    1.  Clique com o botão direito do mouse na conta de **administrador** e selecione **Propriedades**.  

    2.  Clique na guia **conta** .  

    3.  Em opções de **conta** , selecione o sinalizador **cartão inteligente necessário para logon interativo** , conforme indicado na captura de tela a seguir, e clique em **OK**.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurando GPOs para restringir contas de administrador no nível de domínio  

> [!WARNING]  
> Esse GPO nunca deve ser vinculado no nível de domínio porque pode tornar a conta de administrador interna inutilizável, mesmo em cenários de recuperação de desastres.  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Gerenciamento de política de grupo**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e, em seguida, **política de grupo objetos** (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja criar o política de grupo).  

3.  Na árvore de console, clique com o botão direito do mouse em **política de grupo objetos**e clique em **novo**.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Na caixa de diálogo **novo GPO** , digite <GPO Name>e clique em **OK** (em que <GPO Name> é o nome desse GPO), conforme indicado na captura de tela a seguir.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  No painel de detalhes, clique com o botão direito do mouse em <GPO Name>e clique em **Editar**.  

6.  Navegue até **computador \ \ Diretivas**\ \ políticas e clique em **atribuição de direitos de usuário**.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configure os direitos de usuário para impedir que a conta de administrador acesse servidores membros e estações de trabalho na rede fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador da rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **administrador**, clique em **verificar nomes**e clique em **OK**. Verifique se a conta é exibida no formato <DomainName>\Username, conforme indicado na captura de tela a seguir.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador faça logon como um trabalho em lotes fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **administrador**, clique em **verificar nomes**e clique em **OK**. Verifique se a conta é exibida no formato <DomainName>\Username, conforme indicado na captura de tela a seguir.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

9. Configure os direitos de usuário para impedir que a conta de administrador faça logon como um serviço fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **administrador**, clique em **verificar nomes**e clique em **OK**. Verifique se a conta é exibida no formato <DomainName>\Username, conforme indicado na captura de tela a seguir.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

10. Configure os direitos de usuário para impedir que a conta do BA acesse servidores membros e estações de trabalho via Serviços de Área de Trabalho Remota fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon por meio de serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **administrador**, clique em **verificar nomes**e clique em **OK**. Verifique se a conta é exibida no formato <DomainName>\Username, conforme indicado na captura de tela a seguir.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

11. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e em **sair**.  

12. No **Gerenciamento de política de grupo**, VINCULE o GPO ao servidor membro e às UOs de estação de trabalho fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir o Política de Grupo).  

    2.  Clique com o botão direito do mouse na UO à qual o GPO será aplicado e clique em **vincular um GPO existente**.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selecione o GPO que você criou e clique em **OK**.  

        ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Crie links para todas as outras UOs que contêm estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

> [!IMPORTANT]  
> Ao adicionar a conta de administrador a essas configurações, você especifica se está configurando uma conta de administrador local ou uma conta de administrador de domínio por meio da identificação das contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS a esses direitos de negação, você navegará para a conta de administrador do domínio TAILSPINTOYS, que apareceria como TAILSPINTOYS\Administrator. Se você digitar "administrador" nessas configurações de direitos de usuário na Editor de Objeto de Política de Grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

#### <a name="verification-steps"></a>Etapas de Verificação  
As etapas de verificação descritas aqui são específicas para o Windows 8 e o Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Opção de conta "cartão inteligente necessário para logon interativo"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações de GPO, tente fazer logon interativamente no domínio usando a conta de administrador interna do domínio. Depois de tentar fazer logon, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Opção de conta verificar "a conta está desabilitada"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações de GPO, tente fazer logon interativamente no domínio usando a conta de administrador interna do domínio. Depois de tentar fazer logon, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificar as configurações de GPO "negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não seja afetada pelas alterações de GPO (como um servidor de salto), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações do GPO, tente mapear a unidade do sistema usando o comando **net use** executando as seguintes etapas:  

1.  Faça logon no domínio usando a conta de administrador interna do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **prompt de comando**, clique com o botão direito do mouse em prompt de **comando**e clique em **Executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando for solicitado a aprovar a elevação, clique em **Sim**.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Na janela do **prompt de comando** , digite **net use \\\\nome do servidor \<\>\c $** , em que \<nome do servidor\> é o nome do servidor membro ou da estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  No **bloco de notas**, digite **dir c:** .  

4.  Clique em **arquivo** e em **salvar como**.  

5.  No campo **nome de arquivo** , digite **<Filename>. bat** (em que <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de pesquisa, digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Em **Agendador de tarefas**, clique em **ação**e clique em **criar tarefa**.  

4.  Na caixa de diálogo **criar tarefa** , digite **<Task Name>** (em que **<Task Name>** é o nome da nova tarefa).  

5.  Clique na guia **ações** e clique em **novo**.  

6.  Em **ação:** , selecione **Iniciar um programa**.  

7.  Em **programa/script:** , clique em **procurar**, localize e selecione o arquivo em lotes criado na seção "criar um arquivo em lotes" e clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. Em opções de **segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta do BA no nível do domínio, clique em **verificar nomes**e clique em **OK**.  

12. Selecione **executar se o usuário estiver conectado ou não** e não **armazenar a senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Uma caixa de diálogo deve ser exibida, solicitando credenciais de conta de usuário para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **OK**.  

16. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  Em **fazer logon como:** , selecione **esta conta**.  

7.  Clique em **procurar**, digite o nome da conta do Ba no nível do domínio, clique em **verificar nomes**e clique em **OK**.  

8.  Em **senha:** e **Confirmar senha:** , digite a senha da conta de administrador e clique em **OK**.  

9. Clique em **OK** mais três vezes.  

10. Clique com o botão direito do mouse no **serviço spooler de impressão** e selecione **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  Em **fazer logon como:** , selecione a conta **sistema local** e clique em **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon por meio do Serviços de Área de Trabalho Remota"

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **conexão de área de trabalho remota**e clique em **conexão de área de trabalho remota**.  

3.  No campo **computador** , digite o nome do computador ao qual você deseja se conectar e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador.)  

4.  Quando solicitado, forneça credenciais para o nome da conta do BA no nível do domínio.  

5.  Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![Protegendo contas de administrador internas](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
