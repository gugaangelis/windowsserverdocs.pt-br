---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Apêndice F - protegendo grupos de administradores de domínio do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1f35503d1f02d616255c067fbc1750a0cab974cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880137"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice F: Protegendo grupos de administradores de domínio do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice F: Protegendo grupos de administradores de domínio do Active Directory  
Como é o caso com o grupo de administradores Enterprise (EA), a associação no grupo Admins. do domínio (DA) deve ser necessária somente em cenários de recuperação de desastre ou de compilação. Não deve haver nenhuma conta de usuário diárias no grupo de acelerador de desenvolvimento com a exceção a conta interna de administrador para o domínio, se ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Os administradores de domínio são, por padrão, os membros dos grupos Administradores locais em todos os servidores membro e estações de trabalho em seus respectivos domínios. O aninhamento esse padrão não deve ser modificado para fins de recuperação de desastres e capacidade de suporte. Se Admins. do domínio foram removidos dos grupos Administradores locais nos servidores membro, o grupo deve ser adicionado ao grupo de administradores em cada servidor membro e a estação de trabalho no domínio. Grupo de Admins. do domínio de cada domínio deve ser protegido conforme descrito nas instruções passo a passo que seguem.  

Para o grupo Admins. do domínio em cada domínio na floresta:  

1.  Remover todos os membros do grupo, com a possível exceção da conta interna de administrador para o domínio, desde que ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Nos GPOs vinculados a OUs que contenham servidores membro e estações de trabalho em cada domínio, o grupo DA deve ser adicionado aos direitos de usuário no **direitos de atribuição de configurações do computador \ Diretivas \ Configurações de segurança As atribuições de**:  

    -   Negar o acesso a este computador a partir da rede  

    -   Negar o logon como um trabalho em lotes  

    -   Negar o logon como um serviço  

    -   Negar o logon localmente  

    -   Negar logon por meio de direitos de usuário dos serviços de área de trabalho remota  

3.  A auditoria deve ser configurada para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo Admins. do domínio.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instruções passo a passo para a remoção de todos os membros do grupo de administradores de domínio  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Active Directory Users and Computers**.  

2.  Para remover todos os membros do grupo de acelerador de desenvolvimento, execute as seguintes etapas:  

    1.  Clique duas vezes o **Admins. do domínio** agrupar e clique no **membros** guia.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selecione um membro do grupo, clique em **remova**, clique em **Yes**e clique em **Okey**.  

3.  Repita a etapa 2 até que todos os membros do grupo DA foram removidos.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instruções passo a passo para proteger a Admins. do domínio no Active Directory  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda \<floresta\>\\domínios\\\<domínio\>e então **objetos de diretiva de grupo** (onde \<floresta\> é o nome da floresta e \<domínio\> é o nome do domínio no qual você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  No **novo GPO** caixa de diálogo, digite \<nome do GPO\>e clique em **Okey** (onde \<nome do GPO\> é o nome desse GPO).  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  No painel de detalhes, clique com botão direito \<nome do GPO\>e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configure os direitos de usuário para impedir que membros do grupo Admins. do domínio acessem os servidores membros e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que membros do grupo de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que membros do grupo de fazer logon como um serviço da seguinte maneira:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Configure os direitos de usuário para impedir que os membros do grupo Admins. do domínio de logon localmente para servidores membro e estações de trabalho fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon localmente** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

11. Configure os direitos de usuário para impedir que membros do grupo Admins. do domínio acessem os servidores membro e estações de trabalho por meio dos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

12. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

13. Em gerenciamento de diretiva de grupo, vincule o GPO para o servidor membro e a estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até a \<floresta\>\Domains\\\<domínio\> (onde \<floresta\> é o nome da floresta e \<domínio\> é o nome do o domínio no qual você deseja definir a política de grupo).  

    2.  A unidade Organizacional que o GPO será aplicado a e clique com o botão direito **vincular um GPO existente**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Crie links para todas as outras UOs que contêm as estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

        > [!IMPORTANT]  
        > Se os servidores de salto são usadas para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de salto estão localizados em uma unidade Organizacional ao qual este GPOs não está vinculado.  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique as configurações de GPO "Negar acesso a este computador pela rede"  
Em qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO (como "servidor de salto"), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações de GPO, tente mapear a unidade do sistema usando o **NET USE** comando.  

1.  Fazer logon localmente usando uma conta que seja um membro do grupo Admins. do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador** para abrir o alto prompt de comando.  

4.  Quando solicitado a aprovar a elevação, clique em **Sim**.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  No **Prompt de comando** , digite **net use \\ \\ \<nome do servidor\>\c$**, onde \<nome do servidor\> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro deve aparecer.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **o bloco de notas**.  

3.  Na **bloco de notas**, digite **dir c:**.  

4.  Clique em **arquivo**e clique em **Salvar como**.  

5.  No **arquivo** campo nome, digite  **\<Filename\>. bat** (onde \<Filename\> é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam Windows 8, além de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  No **Agendador de tarefas** barra de menus, clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **\<nome da tarefa\>** (onde \<nome da tarefa\> é o nome da nova tarefa).  

5.  Clique o **ações** guia e, em seguida, clique em **New**.  

6.  No **ação** campo, selecione **iniciar um programa**.  

7.  Sob **programa/script**, clique em **procurar**, localize e selecione o arquivo de lote criado na **criar um arquivo em lotes** seção e, em seguida, clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. Sob **segurança** opções, clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que seja membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **Executar estando o usuário conectado ou não** e selecione **não armazenam senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Deve aparecer uma caixa de diálogo, a conta de usuário solicitando credenciais para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como**, selecione o **esta conta** opção.  

7.  Clique em **navegue**, digite o nome de uma conta que seja membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **Okey**.  

8.  Sob **senha** e **Confirmar senha**, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler da impressora  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como**, selecione o **sistema Local** da conta e, em seguida, clique em **Okey**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verifique as configurações de GPO "Negar logon localmente"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, tente fazer logon localmente usando uma conta que seja um membro do grupo Admins. do domínio. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"    
1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador que você deseja se conectar a e, em seguida, clique em **Connect**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça credenciais para uma conta que seja um membro do grupo Admins. do domínio.  

5.  Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administradores de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
