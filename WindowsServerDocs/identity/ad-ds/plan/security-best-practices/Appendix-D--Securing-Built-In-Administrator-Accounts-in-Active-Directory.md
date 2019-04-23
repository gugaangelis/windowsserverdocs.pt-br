---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Apêndice D - Protegendo contas de administrador internas no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 51e503f55ee0fca1f1a53339de555fd213c69296
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870677"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice D: Protegendo contas de administrador internas no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Apêndice D: Protegendo contas de administrador internas no Active Directory  
Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é um membro dos grupos Admins. do domínio e administradores do domínio padrão e se o domínio for o domínio raiz da floresta, a conta também é membro do grupo Administradores corporativos.

Uso da conta de administrador do domínio deve ser reservado apenas para atividades de compilação inicial e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador pode ser usada para efetuar reparos que nenhuma outra conta pode ser usada, você não deve alterar a associação padrão da conta de administrador em qualquer domínio na floresta. Em vez disso, você deve proteger a conta de administrador em cada domínio na floresta, conforme descrito na seção a seguir e detalhada em instruções passo a passo a seguir. 

> [!NOTE]
> Este guia usado para recomendar a desabilitar a conta. Isso foi removido como o white paper da recuperação de floresta faz uso da conta de administrador padrão. O motivo é que esta é a única conta que permite que o logon sem um servidor de Catálogo Global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador interno  
Para a conta interna de administrador em cada domínio na floresta, você deve configurar as seguintes configurações:  

-   Habilitar o **conta é sigilosa e não pode ser delegada** sinalizador na conta.  

-   Habilitar o **cartão inteligente é necessário para logon interativo** sinalizador na conta.  

-   Configure GPOs para restringir o uso da conta de administrador em sistemas ingressados em domínio:  

    -   Em um ou mais GPOs que você crie e vincule ao servidor UOs em cada domínio de estação de trabalho e o membro, adicionar a conta de administrador de cada domínio para os seguintes direitos de usuário no **configurações do computador \ Diretivas \ Configurações de segurança Atribuições de direitos de atribuição**:  

        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Quando você adiciona contas para essa configuração, você deve especificar se você estiver configurando as contas de administrador locais ou contas de administrador de domínio. Por exemplo adicionar a conta de administrador do domínio NWTRADERS esses negar direitos, você deve digitar a conta como NWTRADERS\Administrador ou navegue até a conta de administrador para o domínio NWTRADERS. Se você digitar "Administrador" essas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado.
>   
> É recomendável restringir contas de administrador locais nos servidores de membro e estações de trabalho da mesma maneira como as contas de administrador baseado em domínio. Portanto, você deve adicionar geralmente a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais a essas configurações de direitos de usuário. Captura de tela a seguir mostra um exemplo de como configurar esses direitos de usuário para bloquear contas de administrador locais e a conta de administrador do domínio de executar logons que não deve ser necessário para essas contas.  


![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurar GPOs para impedir que contas de administrador em controladores de domínio  
    -   Em cada domínio na floresta, o GPO de controladores de domínio padrão ou uma política vinculado aos controladores de domínio OU deve ser modificado para adicionar a conta de administrador de cada domínio para os seguintes direitos de usuário no **Configuration\Policies\Windows do computador Atribuições de direitos de políticas locais \ atribuição**:   
        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Essas configurações garantirão que a conta interna de administrador do domínio não pode ser usada para se conectar a um controlador de domínio, embora a conta, se habilitado, pode fazer logon localmente para controladores de domínio. Porque essa conta só deve estar habilitada e usada em cenários de recuperação de desastres, espera-se que o acesso físico a pelo menos um controlador de domínio estarão disponível ou que outras contas com permissões para acessar os controladores de domínio remotamente podem ser usado.  

-   Configurar a auditoria de contas de administrador  

    Quando você tiver protegido cada conta de administrador do e desabilitados, você deve configurar a auditoria para monitorar as alterações na conta. Se a conta está habilitada, sua senha for redefinida ou quaisquer outras modificações são feitas para a conta, alertas devem ser enviados aos usuários ou equipes responsáveis pela administração do Active Directory, além das equipes de resposta a incidentes da sua organização.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instruções passo a passo para proteger contas de administrador internas no Active Directory  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Active Directory Users and Computers**.  

2.  Para impedir ataques que aproveitam a delegação para usar as credenciais da conta em outros sistemas, execute as seguintes etapas:  

    1.  Clique com botão direito do **Administrator** conta e clique em **propriedades**.  

    2.  Clique o **conta** guia.  

    3.  Sob **opções de conta**, selecione **conta é sigilosa e não pode ser delegada** sinalizador conforme indicado na seguinte captura de tela e, em seguida, clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Para habilitar o **cartão inteligente é necessário para logon interativo** sinalizador na conta, execute as seguintes etapas:  

    1.  Clique com botão direito do **administrador** conta e selecione **propriedades**.  

    2.  Clique o **conta** guia.  

    3.  Sob **conta** opções, selecionadas a **cartão inteligente é necessário para logon interativo** sinalizador conforme indicado na seguinte captura de tela e, em seguida, clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configurando GPOs para restringir as contas de administrador no nível do domínio  

> [!WARNING]  
> Esse GPO nunca deve ser vinculado no nível do domínio, pois isso pode tornar a conta interna administrador inutilizável, até mesmo em cenários de recuperação de desastres.  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e então **objetos de diretiva de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja Crie a política de grupo).  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde <GPO Name> é o nome desse GPO) conforme indicado na seguinte captura de tela.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  No painel de detalhes, clique com botão direito <GPO Name>e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configure os direitos de usuário para impedir que a conta de administrador acesse servidores membros e estações de trabalho na rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Administrator**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida no <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Administrator**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida no <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que a conta de administrador de fazer logon como um serviço da seguinte maneira:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Administrator**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida no <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Configure os direitos de usuário para impedir que a conta de BA acesse servidores membro e estações de trabalho por meio dos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Administrator**, clique em **verificar nomes**e clique em **Okey**. Verifique se a conta é exibida no <DomainName>\Username formato conforme indicado na seguinte captura de tela.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

11. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

12. Na **gerenciamento de política de grupo**, vincule o GPO para o servidor membro e a estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até a <Forest>\Domains\\ <Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir a política de grupo).  

    2.  A unidade Organizacional que o GPO será aplicado a e clique com o botão direito **vincular um GPO existente**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Selecione o GPO que você criou e clique em **Okey**.  

        ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Crie links para todas as outras UOs que contêm as estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

> [!IMPORTANT]  
> Quando você adiciona a conta de administrador para essas configurações, você especifique se você estiver configurando uma conta de administrador local ou uma conta de administrador de domínio como você rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS esses negar direitos, você deveria navegar até a conta de administrador para o domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar "Administrador" essas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

#### <a name="verification-steps"></a>Etapas de verificação  
As etapas de verificação descritas aqui são específicas para o Windows 8 e Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Verifique se "o cartão inteligente é necessário para logon interativo" opção de conta  

1.  Em qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, tente fazer logon interativamente no domínio usando a conta interna de administrador do domínio. Depois de tentar fazer logon, uma caixa de diálogo semelhante à mostrada a seguir deve aparecer.  

![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Verifique se "Conta desabilitada" opção de conta  

1.  Em qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, tente fazer logon interativamente no domínio usando a conta interna de administrador do domínio. Depois de tentar fazer logon, uma caixa de diálogo semelhante à mostrada a seguir deve aparecer.  

![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique as configurações de GPO "Negar acesso a este computador pela rede"  
Em qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO (como um servidor de salto), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações de GPO, tente mapear a unidade do sistema usando o **NET USE** comando executando as seguintes etapas:  

1.  Faça logon no domínio usando a conta interna de administrador do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador** para abrir o alto prompt de comando.  

4.  Quando solicitado a aprovar a elevação, clique em **Sim**.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  No **Prompt de comando** , digite **net use \\ \\ \<nome do servidor\>\c$**, onde \<nome do servidor\> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  Captura de tela a seguir mostra a mensagem de erro deve aparecer.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **o bloco de notas**.  

3.  Na **bloco de notas**, digite **dir c:**.  

4.  Clique em **arquivo** e clique em **Salvar como**.  

5.  No **Filename** , digite  **<Filename>. bat** (onde <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de pesquisa, digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Na **Agendador de tarefas**, clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **<Task Name>** (onde **<Task Name>** é o nome da nova tarefa).  

5.  Clique o **ações** guia e, em seguida, clique em **New**.  

6.  Sob **ação:**, selecione **iniciar um programa**.  

7.  Sob **programa/script:**, clique em **procurar**, localize e selecione o arquivo de lote criado na seção "Criar um arquivo em lotes" e clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. Sob **segurança** opções, clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta do BA no nível de domínio, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **Executar estando o usuário conectado ou não** e **não armazenam senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Deve aparecer uma caixa de diálogo, a conta de usuário solicitando credenciais para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como:**, selecione **esta conta**.  

7.  Clique em **navegue**, digite o nome da conta do BA no nível do domínio, clique em **verificar nomes**e clique em **Okey**.  

8.  Sob **senha:** e **Confirmar senha:**, digite a senha da conta de administrador e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito do **serviço de Spooler de impressão** e selecione **reiniciar**.  

11. Quando o serviço for reiniciado, deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler da impressora  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como:**, selecione o **sistema Local** da conta e, em seguida, clique em **Okey**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador que você deseja se conectar a e, em seguida, clique em **Connect**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça as credenciais para o nome da conta do BA no nível do domínio.  

5.  Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![Protegendo contas de administrador interno](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
