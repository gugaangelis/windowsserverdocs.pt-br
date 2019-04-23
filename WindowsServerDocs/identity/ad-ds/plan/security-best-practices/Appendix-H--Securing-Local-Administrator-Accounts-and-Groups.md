---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Apêndice H - protegendo grupos e contas de Administrador Local
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 71eea3f623968172076708dbea34d5bbf4a07684
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858687"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice h: Protegendo grupos e contas de Administrador Local

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice h: Protegendo grupos e contas de Administrador Local  
Em todas as versões do Windows atualmente em suporte base, a conta de administrador local está desabilitada por padrão, o que torna a conta inutilizável para pass-the-hash e outros ataques de roubo de credenciais. No entanto, em ambientes que contêm sistemas operacionais herdados ou contas de administrador locais foram habilitadas, essas contas podem ser usadas como descrito anteriormente para propagar o comprometimento entre servidores membro e estações de trabalho. Cada conta de administrador local e o grupo devem ser protegidos conforme descrito nas instruções passo a passo que seguem.  

Para obter informações detalhadas sobre as considerações em proteger grupos de administrador interno (BA), consulte [Implementando modelos administrativos com menos privilégios](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de Administrador Local  
Para a conta de administrador local em cada domínio na floresta, você deve configurar as seguintes configurações:  

-   Configurar GPOs para restringir o uso da conta do administrador do domínio em sistemas ingressados em domínio  
    -   Em um ou mais GPOs que você crie e vincule ao servidor de estação de trabalho e membro UOs em cada domínio, adicione a conta de administrador para os seguintes direitos de usuário no **Policies\ de configurações do computador \ Diretivas \ Configurações de segurança Atribuições de direitos do usuário**:  

        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instruções passo a passo para proteger grupos de administradores locais  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurando GPOs para restringir a conta de administrador em sistemas ingressados em domínio  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e então **objetos de diretiva de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja Defina a política de grupo).  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  No **novo GPO** caixa de diálogo, digite **<GPO Name>** e clique em **Okey** (onde <GPO Name> é o nome desse GPO).  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  No painel de detalhes, clique com botão direito **<GPO Name>** e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membros e estações de trabalho na rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Clique em OK.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especifique se você estiver configurando uma conta de administrador local ou uma conta de administrador de domínio como você rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS esses negar direitos, você deveria navegar até a conta de administrador para o domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador local de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especifique se você estiver configurando conta de administrador local ou conta de administrador de domínio como você rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS esses negar direitos, você deveria navegar até a conta de administrador para o domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

9. Configure os direitos de usuário para impedir que a conta de administrador local de fazer logon como um serviço da seguinte maneira:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especifique se você estiver configurando conta de administrador local ou conta de administrador de domínio como você rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS esses negar direitos, você deveria navegar até a conta de administrador para o domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

10. Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membro e estações de trabalho por meio dos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **Okey**. Esse nome de usuário será **administrador**, o padrão quando o Windows está instalado.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Quando você adiciona a conta de administrador para essas configurações, você especifique se você estiver configurando conta de administrador local ou conta de administrador de domínio como você rotular as contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS esses negar direitos, você deveria navegar até a conta de administrador para o domínio TAILSPINTOYS, que será exibido como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

11. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

12. Na **gerenciamento de política de grupo**, vincule o GPO para o servidor membro e a estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até a <Forest>\Domains\\ <Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir a política de grupo).  

    2.  A unidade Organizacional que o GPO será aplicado a e clique com o botão direito **vincular um GPO existente**.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selecione o GPO que você criou e clique em **Okey**.  

        ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Crie links para todas as outras UOs que contêm as estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique as configurações de GPO "Negar acesso a este computador pela rede"  

Em qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO (como um servidor de salto), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações de GPO, tente mapear a unidade do sistema usando o **NET USE** comando.  

1.  Fazer logon localmente para qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador** para abrir o alto prompt de comando.  

4.  Quando solicitado a aprovar a elevação, clique em **Sim**.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  No **Prompt de comando** , digite **net use \\ \\ <Server Name>\c$ /user:<Server Name>\administrator.**, onde <Server Name> é o nome do membro servidor ou estação de trabalho que você está tentando acessar pela rede.  

    > [!NOTE]  
    > As credenciais de administrador locais devem ser do mesmo sistema que você está tentando acessar pela rede.  

6.  Captura de tela a seguir mostra a mensagem de erro deve aparecer.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um trabalho em lotes"  
De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **o bloco de notas**.  

3.  Na **bloco de notas**, digite **dir c:**.  

4.  Clique em **arquivo**e clique em **Salvar como**.  

5.  No **nome do arquivo** , digite  **<Filename>. bat** (onde <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** caixa, digite Agendador de tarefas e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam Windows 8, além de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **<Task Name>** (onde <Task Name> é o nome da nova tarefa).  

5.  Clique o **ações** guia e, em seguida, clique em **New**.  

6.  No **ação** , clique em **iniciar um programa**.  

7.  No **programa/script** , clique em **procurar**, localize e selecione o arquivo de lote criado na **criar um arquivo em lotes** seção e, em seguida, clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. No **opções de segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta de administrador do sistema local, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **Executar estando o usuário conectado ou não** e **não armazenam senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Deve aparecer uma caixa de diálogo, a conta de usuário solicitando credenciais para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Na **fazer logon como** , clique em **esta conta**.  

7.  Clique em **navegue**, digite a conta de administrador do sistema local, clique em **verificar nomes**e clique em **Okey**.  

8.  No **senha** e **Confirmar senha** campos, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler da impressora  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  No **fazer logon como**: campo, selecione **Local Systemaccount**e clique em **Okey**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador que você deseja se conectar a e, em seguida, clique em **Connect**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça as credenciais para o local do sistema **administrador** conta.  

5.  Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos e contas de administrador local seguro](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
