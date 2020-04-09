---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Apêndice E-protegendo grupos de administradores corporativos no Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5294be945ce4a93ffeb1c27cffa8a82470920e7b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821629"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apêndice E: Proteger grupos de administrador corporativo no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apêndice E: Proteger grupos de administrador corporativo no Active Directory  
O grupo Enterprise Admins (EA), que está hospedado no domínio raiz da floresta, não deve conter nenhum usuário diariamente, com a possível exceção da conta de administrador do domínio raiz, desde que ele seja protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Os administradores corporativos são, por padrão, membros do grupo Administradores em cada domínio na floresta. Você não deve remover o grupo EA dos grupos de administradores em cada domínio porque, no caso de um cenário de recuperação de desastre de floresta, os direitos de EA provavelmente serão necessários. O grupo de administradores corporativos da floresta deve ser protegido conforme detalhado nas instruções passo a passo a seguir.  

Para o grupo de administradores de empresa na floresta:  

1.  Em GPOs vinculados a UOs que contêm servidores membros e estações de trabalho em cada domínio, o grupo Administradores de empresa deve ser adicionado aos direitos de usuário a seguir no **computador \** \ \ \ \ \ \ \ \ \ \ Configurações de direitos:  

    -   Negar o acesso a este computador a partir da rede  

    -   Negar o logon como um trabalho em lotes  

    -   Negar o logon como um serviço  

    -   Negar o logon localmente  

    -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

2.  Configure a auditoria para enviar alertas se qualquer modificação for feita nas propriedades ou associação do grupo de administradores de empresa.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Instruções passo a passo para remover todos os membros do grupo de administradores de empresa  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e em **Active Directory usuários e computadores**.  

2.  Se você não estiver gerenciando o domínio raiz da floresta, na árvore de console, clique com o botão direito do mouse em <Domain>e clique em **alterar domínio** (em que <Domain> é o nome do domínio que você está administrando no momento).  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  Na caixa de diálogo **alterar domínio** , clique em **procurar**, selecione o domínio raiz da floresta e clique em **OK**.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Para remover todos os membros do grupo EA:  

    1.  Clique duas vezes no grupo **Administradores de empresa** e, em seguida, clique na guia **Membros** .  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selecione um membro do grupo, clique em **remover**, em **Sim**e em **OK**.  

5.  Repita a etapa 2 até que todos os membros do grupo EA tenham sido removidos.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instruções detalhadas para proteger administradores corporativos no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Gerenciamento de política de grupo**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e, em seguida, **política de grupo objetos** (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir o política de grupo).  

    > [!NOTE]  
    > Em uma floresta que contém vários domínios, um GPO semelhante deve ser criado em cada domínio que exige que o grupo Administradores de empresa seja protegido.  

3.  Na árvore de console, clique com o botão direito do mouse em **política de grupo objetos**e clique em **novo**.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  Na caixa de diálogo **novo GPO** , digite <GPO Name>e clique em **OK** (em que <GPO Name> é o nome desse GPO).  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  No painel de detalhes, clique com o botão direito do mouse em <GPO Name>e clique em **Editar**.  

6.  Navegue até **computador \ \ Diretivas**\ \ políticas e clique em **atribuição de direitos de usuário**.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configure os direitos de usuário para impedir que os membros do grupo Administradores de empresa acessem servidores membros e estações de trabalho na rede fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador da rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **administradores corporativos**, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

8.  Configure os direitos de usuário para impedir que os membros do grupo Administradores de empresa façam logon como um trabalho em lotes fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Digite **administradores corporativos**, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo EA façam logon como um serviço fazendo o seguinte:  

    1.  Clique duas vezes em **negar log como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Digite **administradores corporativos**, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

10. Configure os direitos de usuário para impedir que os membros do grupo Administradores de empresa façam logon localmente em servidores membros e estações de trabalho fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon localmente** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Digite **administradores corporativos**, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

11. Configure os direitos de usuário para impedir que os membros do grupo Administradores de empresa acessem servidores membros e estações de trabalho via Serviços de Área de Trabalho Remota fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon por meio de serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Digite **administradores corporativos**, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

12. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e em **sair**.  

13. No **Gerenciamento de política de grupo**, VINCULE o GPO ao servidor membro e às UOs de estação de trabalho fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (em que <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir o Política de Grupo).  

    2.  Clique com o botão direito do mouse na UO à qual o GPO será aplicado e clique em **vincular um GPO existente**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **OK**.  

        ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Crie links para todas as outras UOs que contêm estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

    6.  Em uma floresta que contém vários domínios, um GPO semelhante deve ser criado em cada domínio que exige que o grupo Administradores de empresa seja protegido.  

> [!IMPORTANT]  
> Se os servidores de salto forem usados para administrar controladores de domínio e Active Directory, verifique se os servidores de salto estão localizados em uma UO à qual esses GPOs não estão vinculados.  

### <a name="verification-steps"></a>Etapas de Verificação  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificar as configurações de GPO "negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não seja afetada pelas alterações de GPO (como um "servidor de salto"), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações do GPO, tente mapear a unidade do sistema usando o comando **net use** executando as seguintes etapas:  

1.  Faça logon localmente usando uma conta que seja membro do grupo EA.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **prompt de comando**, clique com o botão direito do mouse em prompt de **comando**e clique em **Executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando for solicitado a aprovar a elevação, clique em **Sim**.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  Na janela do **prompt de comando** , digite **net use \\\\nome do servidor \<\>\c $** , em que \<nome do servidor\> é o nome do servidor membro ou da estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

##### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  No **bloco de notas**, digite **dir c:** .  

4.  Clique em **arquivo**e em **salvar como**.  

5.  Na caixa nome do **arquivo** , digite **<Filename>. bat** (em que <Filename> é o nome do novo arquivo em lotes).  

##### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  Na caixa de diálogo **criar tarefa** , digite **<Task Name>** (em que <Task Name> é o nome da nova tarefa).  

5.  Clique na guia **ações** e clique em **novo**.  

6.  No campo **ação** , selecione **Iniciar um programa**.  

7.  Em **programa/script**, clique em **procurar**, localize e selecione o arquivo em lotes criado na seção **criar um arquivo em lotes** e clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. No campo **Opções de segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que seja membro do grupo EAs, clique em **verificar nomes**e clique em **OK**.  

12. Selecione **executar se o usuário estiver conectado ou não** e selecione não **armazenar a senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Uma caixa de diálogo deve ser exibida, solicitando credenciais de conta de usuário para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **OK**.  

16. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  Em **fazer logon como**, selecione **esta conta**.  

7.  Clique em **procurar**, digite o nome de uma conta que seja membro do grupo EAS, clique em **verificar nomes**e clique em **OK**.  

8.  Em **senha:** e **confirme a senha**, digite a senha da conta selecionada e clique em **OK**.  

9. Clique em **OK** mais três vezes.  

10. Clique com o botão direito do mouse no serviço **spooler de impressão** e selecione **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Logon**.  

6.  Em **fazer logon como**, selecione a conta **sistema local** e clique em **OK**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificar configurações de GPO "Negar logon local"  

1.  Em qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, tente fazer logon localmente usando uma conta que seja membro do grupo EA. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon por meio do Serviços de Área de Trabalho Remota"  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **conexão de área de trabalho remota**e clique em **conexão de área de trabalho remota**.  

3.  No campo **computador** , digite o nome do computador ao qual você deseja se conectar e, em seguida, clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador.)  

4.  Quando solicitado, forneça as credenciais para uma conta que seja membro do grupo EA.  

5.  Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores corporativos](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
