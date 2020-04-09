---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Apêndice H-protegendo contas e grupos de administrador local
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 9c5cb76ff137912893c5bc0322d5b79bee2203fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821459"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice H: Proteger contas e grupos de administrador local

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Apêndice H: Proteger contas e grupos de administrador local  
Em todas as versões do Windows atualmente no suporte base, a conta de administrador local é desabilitada por padrão, o que torna a conta inutilizável para os ataques Pass-the-hash e outros roubos de credenciais. No entanto, em ambientes que contêm sistemas operacionais herdados ou em que as contas de administrador local foram habilitadas, essas contas podem ser usadas conforme descrito anteriormente para propagar o comprometimento entre servidores membros e estações de trabalho. Cada conta de administrador local e grupo deve ser protegido conforme descrito nas instruções passo a passo a seguir.  

Para obter informações detalhadas sobre as considerações sobre como proteger grupos de administradores internos (BA), consulte [implementando modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de administrador local  
Para a conta de administrador local em cada domínio em sua floresta, você deve definir as seguintes configurações:  

-   Configurar GPOs para restringir o uso da conta de administrador do domínio em sistemas ingressados no domínio  
    -   Em um ou mais GPOs que você cria e vincula a estações de trabalho e a UOs de servidor membro em cada domínio, adicione a conta de administrador aos seguintes direitos de usuário em **computador \** \ \ \ \ \ Configurações de direitos:  

        -   Negar o acesso a este computador a partir da rede  

        -   Negar o logon como um trabalho em lotes  

        -   Negar o logon como um serviço  

        -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instruções passo a passo para proteger grupos locais de administradores  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configurando GPOs para restringir a conta de administrador em sistemas ingressados no domínio  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Gerenciamento de política de grupo**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e, em seguida, **política de grupo objetos** (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir o política de grupo).  

3.  Na árvore de console, clique com o botão direito do mouse em **política de grupo objetos**e clique em **novo**.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Na caixa de diálogo **novo GPO** , digite **<GPO Name>** e clique em **OK** (em que <GPO Name> é o nome desse GPO).  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  No painel de detalhes, clique com o botão direito do mouse em **<GPO Name>** e clique em **Editar**.  

6.  Navegue até **computador \ \ Diretivas**\ \ políticas e clique em **atribuição de direitos de usuário**.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membros e estações de trabalho na rede fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador da rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **OK**. Esse nome de usuário será **administrador**, o padrão quando o Windows for instalado.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Clique em OK.  

        > [!IMPORTANT]  
        > Ao adicionar a conta de administrador a essas configurações, você especifica se está configurando uma conta de administrador local ou uma conta de administrador de domínio por meio da identificação das contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS a esses direitos de negação, você navegará para a conta de administrador do domínio TAILSPINTOYS, que apareceria como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

8.  Configure os direitos de usuário para impedir que a conta de administrador local faça logon como um trabalho em lotes fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **OK**. Esse nome de usuário será **administrador**, o padrão quando o Windows for instalado.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Ao adicionar a conta de administrador a essas configurações, você especifica se está configurando a conta de administrador local ou a conta de administrador de domínio por meio da identificação das contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS a esses direitos de negação, você navegará para a conta de administrador do domínio TAILSPINTOYS, que apareceria como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

9. Configure os direitos de usuário para impedir que a conta de administrador local faça logon como um serviço fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **OK**. Esse nome de usuário será **administrador**, o padrão quando o Windows for instalado.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Ao adicionar a conta de administrador a essas configurações, você especifica se está configurando a conta de administrador local ou a conta de administrador de domínio por meio da identificação das contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS a esses direitos de negação, você navegará para a conta de administrador do domínio TAILSPINTOYS, que apareceria como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

10. Configure os direitos de usuário para impedir que a conta de administrador local acesse servidores membros e estações de trabalho via Serviços de Área de Trabalho Remota fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon por meio de serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo**, digite o nome de usuário da conta de administrador local e clique em **OK**. Esse nome de usuário será **administrador**, o padrão quando o Windows for instalado.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Clique em **OK**.  

        > [!IMPORTANT]  
        > Ao adicionar a conta de administrador a essas configurações, você especifica se está configurando a conta de administrador local ou a conta de administrador de domínio por meio da identificação das contas. Por exemplo, para adicionar a conta de administrador do domínio TAILSPINTOYS a esses direitos de negação, você navegará para a conta de administrador do domínio TAILSPINTOYS, que apareceria como TAILSPINTOYS\Administrator. Se você digitar **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado, conforme descrito anteriormente.  

11. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e em **sair**.  

12. No **Gerenciamento de política de grupo**, VINCULE o GPO ao servidor membro e às UOs de estação de trabalho fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir o Política de Grupo).  

    2.  Clique com o botão direito do mouse na UO à qual o GPO será aplicado e clique em **vincular um GPO existente**.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Selecione o GPO que você criou e clique em **OK**.  

        ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Crie links para todas as outras UOs que contêm estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

#### <a name="verification-steps"></a>Etapas de Verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificar as configurações de GPO "negar acesso a este computador pela rede"  

De qualquer servidor membro ou estação de trabalho que não seja afetada pelas alterações de GPO (como um servidor de salto), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações do GPO, tente mapear a unidade do sistema usando o comando **net use** .  

1.  Faça logon localmente em qualquer servidor membro ou estação de trabalho que não seja afetada pelas alterações do GPO.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **prompt de comando**, clique com o botão direito do mouse em prompt de **comando**e clique em **Executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando for solicitado a aprovar a elevação, clique em **Sim**.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Na janela do **prompt de comando** , digite **net use \\\\<Server Name>\c $/User:<Server Name>\Administrador**, em que <Server Name> é o nome do servidor membro ou da estação de trabalho que você está tentando acessar pela rede.  

    > [!NOTE]  
    > As credenciais de administrador local devem ser do mesmo sistema que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um trabalho em lotes"  
De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  No **bloco de notas**, digite **dir c:** .  

4.  Clique em **arquivo**e em **salvar como**.  

5.  Na caixa **nome do arquivo** , digite **<Filename>. bat** (em que <Filename> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite Agendador de tarefas e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  Na caixa de diálogo **criar tarefa** , digite **<Task Name>** (em que <Task Name> é o nome da nova tarefa).  

5.  Clique na guia **ações** e clique em **novo**.  

6.  No campo **ação** , clique em **Iniciar um programa**.  

7.  No campo **programa/script** , clique em **procurar**, localize e selecione o arquivo em lotes criado na seção **criar um arquivo em lotes** e clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. No campo **Opções de segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome da conta de administrador local do sistema, clique em **verificar nomes**e clique em **OK**.  

12. Selecione **executar se o usuário estiver conectado ou não** e não **armazenar a senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Uma caixa de diálogo deve ser exibida, solicitando credenciais de conta de usuário para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **OK**.  

16. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  Em **fazer logon como** campo, clique **nessa conta**.  

7.  Clique em **procurar**, digite a conta de administrador local do sistema, clique em **verificar nomes**e clique em **OK**.  

8.  Nos campos **senha** e **Confirmar senha** , digite a senha da conta selecionada e clique em **OK**.  

9. Clique em **OK** mais três vezes.  

10. Clique com o botão direito do mouse em **spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  No campo **fazer logon como**:, selecione **local SystemAccount**e clique em **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon por meio do Serviços de Área de Trabalho Remota"  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **conexão de área de trabalho remota**e clique em **conexão de área de trabalho remota**.  

3.  No campo **computador** , digite o nome do computador ao qual você deseja se conectar e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador.)  

4.  Quando solicitado, forneça credenciais para a conta de **administrador** local do sistema.  

5.  Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger contas e grupos do administrador local](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
