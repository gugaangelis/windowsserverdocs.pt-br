---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: "Apêndice D - proteção de contas de administrador interno no Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice d: proteção de contas de administrador interno no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice d: proteção de contas de administrador interno no Active Directory  
Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é um membro do grupo Admins. do domínio e administradores no domínio padrão e se o domínio for domínio raiz da floresta, a conta também é um membro do grupo Administradores corporativos.

Uso da conta de administrador do domínio deve ser usado somente para atividades de compilação inicial e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador pode ser usada para efetuar reparos que podem ser usadas sem outras contas, você não deve alterar a associação padrão da conta do administrador em qualquer domínio na floresta. Em vez disso, você deve proteger a conta de administrador em cada domínio na floresta, conforme descrito na seção a seguir e detalhados nas instruções passo a passo que seguem. 

> [!NOTE]
> Este guia costumava desabilitam a conta. Isso foi removido como o white paper de recuperação de floresta utiliza a padrão da conta de administrador. O motivo é que isso é a única conta que permite que o logon sem um servidor de Catálogo Global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador interno  
Para a conta de administrador interno em cada domínio na floresta, você deve configurar as seguintes configurações:  

-   Habilitar o **conta é confidencial e não pode ser delegada** sinalizador na conta.  

-   Habilitar o **cartão inteligente é necessário para logon interativo** sinalizador na conta.  

-   Configure GPOs para restringir o uso da conta de administrador no domínio sistemas:  

    -   Em um ou mais GPOs que você crie e vincule a estação de trabalho e membro do servidor UOs em cada domínio, adicionar conta de administrador do domínio cada para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  

        -   Negar acesso a este computador pela rede  

        -   Negar logon como um trabalho em lotes  

        -   Negar logon como um serviço  

        -   Negar logon pelos serviços de área de trabalho remota  

> [!NOTE]  
> Quando você adicionar contas para essa configuração, você deve especificar se você estiver configurando contas administrador locais ou contas de administrador do domínio. Por exemplo adicionar a conta de administrador do domínio NWTRADERS para estes negar direitos, você precisará digitar a conta como NWTRADERS\Administrador ou navegue até a conta de administrador para o domínio NWTRADERS. Se você digitar "Administrador" nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado.
>   
> É recomendável restringir contas administrador locais em servidores membros e estações de trabalho da mesma maneira como contas de administrador baseado em domínio. Portanto, geralmente, você deve adicionar a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais para essas configurações de direitos de usuário. Captura de tela a seguir mostra um exemplo de configuração esses direitos de usuário bloquear contas administrador locais e conta de administrador do domínio executem logons que não deve ser necessário para essas contas.  


![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar GPOs para restringir a contas de administrador em controladores de domínio  
    -   Em cada domínio na floresta, o GPO de controladores de domínio padrão ou uma política vinculados aos controladores de domínio, unidade Organizacional deverá ser modificado para adicionar a conta de administrador do domínio cada para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:   
        -   Negar acesso a este computador pela rede  

        -   Negar logon como um trabalho em lotes  

        -   Negar logon como um serviço  

        -   Negar logon pelos serviços de área de trabalho remota  

> [!NOTE]  
> Essas configurações garantirá que conta de administrador interno do domínio não pode ser usada para se conectar a um controlador de domínio, embora a conta, se habilitada, pode logon localmente em controladores de domínio. Porque essa conta só deve ser habilitada e usada em cenários de recuperação de desastres, antecipa-se que o acesso físico a pelo menos um controlador de domínio estará disponível ou que outras contas com permissões para acessar remotamente controladores de domínio podem ser usadas.  

-   Configurar a auditoria de contas de administrador  

    Quando você tê protegido conta de administrador do domínio cada e desabilitado-lo, você deve configurar a auditoria para monitorar as alterações feitas na conta. Se a conta está habilitada, é redefinir sua senha ou quaisquer outras modificações feitas na conta, alertas devem ser enviados para os usuários ou equipes responsáveis para administração do Active Directory, além de equipes de resposta a incidentes em sua organização.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instruções passo a passo para proteger as contas de administrador interno no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **usuários e Active Directory computadores**.  

2.  Para evitar ataques que aproveitam delegação para usar as credenciais da conta em outros sistemas, execute as seguintes etapas:  

    1.  Clique com botão direito do **administrador** da conta e clique em **propriedades**.  

    2.  Clique no **conta** guia.  

    3.  Em **opções de conta**, selecione **conta é confidencial e não pode ser delegada** sinalizar conforme indicado na seguinte captura de tela e clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar o **cartão inteligente é necessário para logon interativo** sinalizar na conta, execute as seguintes etapas:  

    1.  Clique com botão direito do **administrador** da conta e selecione **propriedades**.  

    2.  Clique no **conta** guia.  

    3.  Em **conta** opções, marque o **cartão inteligente é necessário para logon interativo** sinalizar conforme indicado na seguinte captura de tela e clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurando GPOs restringir contas de administrador no nível do domínio  

> [!WARNING]  
> Esse GPO nunca deve ser vinculado no nível do domínio, pois isso pode tornar a conta de administrador interno inutilizável, mesmo em cenários de recuperação de desastres.  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e depois **objetos de política de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja criar a política de grupo).  

3.  Na árvore de console, clique com botão direito **objetos de política de grupo**e clique em **nova**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde <GPO Name> é o nome desse GPO) conforme indicado na seguinte captura de tela.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  No painel de detalhes, clique com botão direito <GPO Name>e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configure os direitos de usuário para impedir que a conta de administrador acesse servidores membros e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administrador**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida em <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administrador**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida em <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que a conta de administrador fazer logon como um serviço, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administrador**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida em <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Configure os direitos de usuário para impedir que a conta BA acessem servidores membros e estações de trabalho por meio de serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administrador**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida em <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

11. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e clique em **sair**.  

12. Em **Group Policy Management**, vincular o GPO para o servidor membro e estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

    2.  Clique com botão direito na UO que o GPO serão aplicadas a e clique em **vincular um GPO existente**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selecione o GPO que você criou e clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Crie links para todas as outras unidades organizacionais que contêm estações de trabalho.  

    5.  Crie links para todas as outras unidades organizacionais que contêm servidores membro.  

> [!IMPORTANT]  
> Quando você adiciona a conta de administrador para essas configurações, você especificar se você estiver configurando uma conta de administrador local ou uma conta de administrador do domínio como rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS para estes negar direitos, você pode navegar para a conta de administrador do domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar "Administrador" nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

#### <a name="verification-steps"></a>Etapas de verificação  
As etapas de verificação descritas aqui são específicas para o Windows 8 e Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Verifique se "cartão inteligente é necessário para logon interativo" opção de conta  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, tente fazer logon interativamente no domínio usando a conta de administrador interno do domínio. Depois de tentar fazer logon, deve aparecer uma caixa de diálogo semelhante ao seguinte.  

![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Verifique se "Conta está desabilitada" opção de conta  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, tente fazer logon interativamente no domínio usando a conta de administrador interno do domínio. Depois de tentar fazer logon, deve aparecer uma caixa de diálogo semelhante ao seguinte.  

![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique se as configurações de GPO "Negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações GPO (como um servidor de atalhos), tente acessar um servidor membro ou estação de trabalho pela rede que é afetada pelas alterações GPO. Para verificar as configurações de GPO, tentar mapear a unidade do sistema usando o **NET USE** comando seguindo as etapas a seguir:  

1.  Faça logon no domínio usando a conta de administrador interno do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e clique em **executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando solicitado para aprovar a elevação, clique em **Sim**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  No **Prompt de comando** janela, digite **net use \ \ \<Server Name>\c$**, onde <Server Name> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  Captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  Em **bloco de notas**, tipo **c: dir**.  

4.  Clique em **arquivo** e clique em **Salvar como**.  

5.  No **Filename** , digite ** <Filename>. bat** (onde <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de pesquisa, digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Em **Agendador de tarefas**, clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite ** <Task Name> ** (onde ** <Task Name> ** é o nome da nova tarefa).  

5.  Clique no **ações** guia e clique em **nova**.  

6.  Em **ação:**, selecione **iniciar um programa**.  

7.  Em **programa/script:**, clique em **procurar**, localize e selecione o arquivo em lotes criado na seção "Criar um arquivo em lotes" e clique em **abrir**.  

8.  Clique em **Okey**.  

9. Clique no **geral** guia.  

10. Em **segurança** opções, clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta BA com o nível de domínio, clique **verificar nomes**e clique em **Okey**.  

12. Selecione **executar se o usuário fizer logon ou não** e **não armazene senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **Okey**.  

14. Uma caixa de diálogo deve aparecer, credenciais de conta de usuário solicitante para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  Em **faça logon como:**, selecione **essa conta**.  

7.  Clique em **procurar**, digite o nome da conta BA no nível do domínio, clique em **verificar nomes**e clique em **Okey**.  

8.  Em **senha:** e **Confirmar senha:**, digite a senha da conta de administrador e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito do **serviço de Spooler de impressão** e selecione **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  Em **faça logon como:**, selecione o **LocalSystem** da conta e clique em **Okey**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador ao qual você deseja se conectar a e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça as credenciais para o nome da conta BA no nível do domínio.  

5.  Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
