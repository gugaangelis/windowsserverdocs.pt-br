---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: "Apêndice H - protegendo grupos e contas de Administrador Local"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice h: Protegendo grupos e contas de Administrador Local

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice h: Protegendo grupos e contas de Administrador Local  
Em todas as versões do Windows no momento de suporte base, a conta de administrador local é desabilitada por padrão, o que torna a conta inutilizável para pass-the-hash e outros ataques de roubo de credenciais. No entanto, em ambientes que contêm os sistemas operacionais herdados ou em que as contas de administrador locais tiverem sido habilitadas, essas contas podem ser usadas conforme descrito anteriormente para propagar comprometimento em estações de trabalho e servidores membro. Cada conta de administrador local e o grupo devem ser protegidas conforme descrito nas instruções passo a passo que seguem.  

Para obter informações detalhadas sobre considerações na proteção de grupos de administrador interno (BA), consulte [Implementando modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de Administrador Local  
Para a conta de administrador local em cada domínio na floresta, você deve configurar as seguintes configurações:  

-   Configurar GPOs para restringir o uso da conta de administrador do domínio em sistemas ingressados em domínio  
    -   Em um ou mais GPOs que você crie e vincule a estação de trabalho e membro do servidor UOs em cada domínio, adicione a conta de administrador para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  

        -   Negar acesso a este computador pela rede  

        -   Negar logon como um trabalho em lotes  

        -   Negar logon como um serviço  

        -   Negar logon pelos serviços de área de trabalho remota  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instruções passo a passo para proteger os grupos de administradores locais  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurando GPOs para restringir a conta de administrador em sistemas ingressados em domínio  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e depois **objetos de política de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **objetos de política de grupo**e clique em **nova**.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  No **novo GPO** caixa de diálogo, digite ** <GPO Name> **e clique em **Okey** (onde <GPO Name> é o nome desse GPO).  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  No painel de detalhes, clique com botão direito ** <GPO Name> **e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membros e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome do usuário da conta do administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Clique em Okey.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especificar se você estiver configurando uma conta de administrador local ou uma conta de administrador do domínio como rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS para estes negar direitos, você pode navegar para a conta de administrador do domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador local de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome do usuário da conta do administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Clique em **Okey**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especificar se você estiver configurando conta administrador local ou conta de administrador de domínio como rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS para estes negar direitos, você pode navegar para a conta de administrador do domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

9. Configure os direitos de usuário para impedir que a conta de administrador local de fazer logon como um serviço, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome do usuário da conta do administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Clique em **Okey**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especificar se você estiver configurando conta administrador local ou conta de administrador de domínio como rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS para estes negar direitos, você pode navegar para a conta de administrador do domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

10. Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membros e estações de trabalho por meio de serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome do usuário da conta do administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Clique em **Okey**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especificar se você estiver configurando conta administrador local ou conta de administrador de domínio como rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS para estes negar direitos, você pode navegar para a conta de administrador do domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você irá restringir a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

11. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e clique em **sair**.  

12. Em **Group Policy Management**, vincular o GPO para o servidor membro e estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

    2.  Clique com botão direito na UO que o GPO serão aplicadas a e clique em **vincular um GPO existente**.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selecione o GPO que você criou e clique em **Okey**.  

        ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Crie links para todas as outras unidades organizacionais que contêm estações de trabalho.  

    5.  Crie links para todas as outras unidades organizacionais que contêm servidores membro.  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique se as configurações de GPO "Negar acesso a este computador pela rede"  

De qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações GPO (como um servidor de atalhos), tente acessar um servidor membro ou estação de trabalho pela rede que é afetada pelas alterações GPO. Para verificar as configurações de GPO, tentar mapear a unidade do sistema usando o **NET USE** comando.  

1.  Faça logon localmente a qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações GPO.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e clique em **executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando solicitado para aprovar a elevação, clique em **Sim**.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  No **Prompt de comando** janela, digite **net use \ \ \<Server Name>\c$ /user:<Server Name>\Administrator**, onde <Server Name> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

    > [!NOTE]  
    > As credenciais de administrador locais devem ser do mesmo sistema que está tentando acessar pela rede.  

6.  Captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um trabalho em lotes"  
De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  Em **bloco de notas**, tipo **c: dir**.  

4.  Clique em **arquivo**e clique em **Salvar como**.  

5.  No **nome do arquivo** , digite ** <Filename>. bat** (onde <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** caixa, digite o Agendador de tarefas e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, além do **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite ** <Task Name> ** (onde <Task Name> é o nome da nova tarefa).  

5.  Clique no **ações** guia e clique em **nova**.  

6.  No **ação** campo, clique em **iniciar um programa**.  

7.  No **programa/script** campo, clique em **procurar**, localize e selecione o arquivo em lotes criado no **criar um arquivo em lotes** seção e clique em **abrir**.  

8.  Clique em **Okey**.  

9. Clique no **geral** guia.  

10. No **opções de segurança** campo, clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta de administrador local do sistema, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **executar se o usuário fizer logon ou não** e **não armazene senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **Okey**.  

14. Uma caixa de diálogo deve aparecer, credenciais de conta de usuário solicitante para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  Em **faça logon como** campo, clique em **essa conta**.  

7.  Clique em **procurar**, digite a conta de administrador local do sistema, clique em **verificar nomes**e clique em **Okey**.  

8.  No **senha** e **Confirmar senha** campos, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  No **faça logon como**: campo, selecione **Systemaccount Local**e clique em **Okey**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador ao qual você deseja se conectar a e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça as credenciais para o sistema local **administrador** conta.  

5.  Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![contas de administrador local seguro e grupos](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
