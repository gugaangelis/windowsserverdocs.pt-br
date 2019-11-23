---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Apêndice F-protegendo grupos de administradores de domínio no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4f453aa9f076b0272821849840106dae0c52fbbc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408692"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice F: Proteger grupos de administrador de domínio no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice F: Proteger grupos de administrador de domínio no Active Directory  
Como é o caso do grupo de administradores de empresa (EA), a associação no grupo Administradores de domínio (DA) deve ser necessária somente em cenários de compilação ou recuperação de desastre. Não deve haver contas de usuário cotidianas no grupo DA, com exceção da conta interna de administrador para o domínio, se ele tiver sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Os administradores de domínio são, por padrão, membros dos grupos de administradores locais em todos os servidores membros e estações de trabalho em seus respectivos domínios. Esse aninhamento padrão não deve ser modificado para fins de suporte e recuperação de desastre. Se os administradores de domínio tiverem sido removidos dos grupos de administradores locais nos servidores membros, o grupo deverá ser adicionado ao grupo Administradores em cada servidor membro e estação de trabalho no domínio. O grupo Admins. do domínio de cada domínio deve ser protegido conforme descrito nas instruções passo a passo a seguir.  

Para o grupo Admins. do domínio em cada domínio na floresta:  

1.  Remova todos os membros do grupo, com a possível exceção da conta de administrador interna para o domínio, desde que ele tenha sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Em GPOs vinculados a UOs que contêm servidores membros e estações de trabalho em cada domínio, o grupo DA deve ser adicionado aos seguintes direitos de usuário no **computador \ \** \ \ \ \ \ \ \ \ \ atribuições de direitos:  

    -   Negar o acesso a este computador a partir da rede  

    -   Negar o logon como um trabalho em lotes  

    -   Negar o logon como um serviço  

    -   Negar o logon localmente  

    -   Negar logon por meio de direitos de usuário Serviços de Área de Trabalho Remota  

3.  A auditoria deve ser configurada para enviar alertas se qualquer modificação for feita nas propriedades ou associação do grupo Admins. do domínio.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instruções passo a passo para remover todos os membros do grupo Admins. do domínio  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e em **Active Directory usuários e computadores**.  

2.  Para remover todos os membros do grupo DA, execute as seguintes etapas:  

    1.  Clique duas vezes no grupo **Admins** . do domínio e clique na guia **Membros** .  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selecione um membro do grupo, clique em **remover**, em **Sim**e em **OK**.  

3.  Repita a etapa 2 até que todos os membros do grupo DA tenham sido removidos.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instruções detalhadas para proteger Administradores de domínio no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Gerenciamento de política de grupo**.  

2.  Na árvore de console, expanda \<floresta\>domínios de \\\\\<\>de domínio e, em seguida, **política de grupo objetos** (em que \<floresta\> é o nome da floresta e \<\> de domínio é o nome do domínio no qual você deseja definir o política de grupo).  

3.  Na árvore de console, clique com o botão direito do mouse em **política de grupo objetos**e clique em **novo**.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  Na caixa de diálogo **novo GPO** , digite \<nome do GPO\>e clique em **OK** (em que \<nome do GPO\> é o nome desse GPO).  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  No painel de detalhes, clique com o botão direito do mouse \<nome do GPO\>e clique em **Editar**.  

6.  Navegue até **computador \ \ Diretivas**\ \ políticas e clique em **atribuição de direitos de usuário**.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configure os direitos de usuário para impedir que os membros do grupo Administradores de domínio acessem servidores e estações de trabalho Membros pela rede fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador da rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **Admins**. do domínio, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

8.  Configure os direitos de usuário para impedir que os membros do grupo DA façam logon como um trabalho em lotes fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **Admins**. do domínio, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo de da façam logon como um serviço fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **Admins**. do domínio, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

10. Configure os direitos de usuário para impedir que os membros do grupo Administradores de domínio façam logon localmente em servidores membros e estações de trabalho fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon localmente** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **Admins**. do domínio, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

11. Configure os direitos de usuário para impedir que os membros do grupo Admins. do domínio acessem servidores membros e estações de trabalho via Serviços de Área de Trabalho Remota fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon por meio de serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **Adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Digite **Admins**. do domínio, clique em **verificar nomes**e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Clique em **OK**e em **OK** novamente.  

12. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e em **sair**.  

13. No gerenciamento de Política de Grupo, vincule o GPO ao servidor membro e às UOs de estação de trabalho fazendo o seguinte:  

    1.  Navegue até a floresta de \<\>\Domains\\\<domínio\> (em que \<floresta\> é o nome da floresta e \<domínio\> é o nome do domínio no qual você deseja definir o Política de Grupo).  

    2.  Clique com o botão direito do mouse na UO à qual o GPO será aplicado e clique em **vincular um GPO existente**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **OK**.  

        ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Crie links para todas as outras UOs que contêm estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

        > [!IMPORTANT]  
        > Se os servidores de salto forem usados para administrar controladores de domínio e Active Directory, verifique se os servidores de salto estão localizados em uma UO à qual esses GPOs não estão vinculados.  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verificar as configurações de GPO "negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não seja afetada pelas alterações de GPO (como um "servidor de salto"), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações do GPO, tente mapear a unidade do sistema usando o comando **net use** .  

1.  Faça logon localmente usando uma conta que seja membro do grupo Admins. do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **prompt de comando**, clique com o botão direito do mouse em prompt de **comando**e clique em **Executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando for solicitado a aprovar a elevação, clique em **Sim**.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  Na janela do **prompt de comando** , digite **net use \\\\nome do servidor \<\>\c $** , em que \<nome do servidor\> é o nome do servidor membro ou da estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  No **bloco de notas**, digite **dir c:** .  

4.  Clique em **arquivo**e em **salvar como**.  

5.  No campo nome do **arquivo** , digite **\<filename\>. bat** (em que \<filename\> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Na barra de menus **Agendador de tarefas** , clique em **ação**e clique em **criar tarefa**.  

4.  Na caixa de diálogo **criar tarefa** , digite **\<nome da tarefa\>** (em que \<nome da tarefa\> é o nome da nova tarefa).  

5.  Clique na guia **ações** e clique em **novo**.  

6.  No campo **ação** , selecione **Iniciar um programa**.  

7.  Em **programa/script**, clique em **procurar**, localize e selecione o arquivo em lotes criado na seção **criar um arquivo em lotes** e clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. Em opções de **segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que seja membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **OK**.  

12. Selecione **executar se o usuário estiver conectado ou não** e selecione não **armazenar a senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Uma caixa de diálogo deve ser exibida, solicitando credenciais de conta de usuário para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **OK**.  

16. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verificar as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Em **fazer logon como**, selecione a opção **esta conta** .  

7.  Clique em **procurar**, digite o nome de uma conta que seja membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **OK**.  

8.  Em **senha** e **Confirmar senha**, digite a senha da conta selecionada e clique em **OK**.  

9. Clique em **OK** mais três vezes.  

10. Clique com o botão direito do mouse em **spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante à seguinte deverá ser exibida.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

3.  Na caixa de **pesquisa** , digite **Serviços**e clique em **Serviços**.  

4.  Localize e clique duas vezes em **spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Em **fazer logon como**, selecione a conta **sistema local** e clique em **OK**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verificar configurações de GPO "Negar logon local"  

1.  Em qualquer servidor membro ou estação de trabalho afetada pelas alterações do GPO, tente fazer logon localmente usando uma conta que seja membro do grupo Admins. do domínio. Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon por meio do Serviços de Área de Trabalho Remota"    
1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando a **barra** de botões for exibida, clique em **Pesquisar**.  

2.  Na caixa de **pesquisa** , digite **conexão de área de trabalho remota**e clique em **conexão de área de trabalho remota**.  

3.  No campo **computador** , digite o nome do computador ao qual você deseja se conectar e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador.)  

4.  Quando solicitado, forneça credenciais para uma conta que seja membro do grupo Admins. do domínio.  

5.  Uma caixa de diálogo semelhante à seguinte deve aparecer.  

    ![proteger grupos de administradores de domínio](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
