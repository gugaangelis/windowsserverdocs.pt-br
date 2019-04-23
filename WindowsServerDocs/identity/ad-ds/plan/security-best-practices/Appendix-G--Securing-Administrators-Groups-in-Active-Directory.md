---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: Apêndice G - proteger grupos de administradores no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882707"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apêndice g: Protegendo grupos de administradores no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apêndice g: Protegendo grupos de administradores no Active Directory  
Como é o caso com os grupos de Admins. do EA (Enterprise) e os administradores de domínio (DA), a associação no grupo de administradores (BA) interno deve ser necessária somente em cenários de recuperação de desastre ou de compilação. Não deve haver nenhuma conta de usuário diárias no grupo Administradores, exceto a conta interna administrador para o domínio, se ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Os administradores são, por padrão, os proprietários da maioria dos objetos do AD DS em seus respectivos domínios. Associação neste grupo pode ser necessário em cenários de recuperação de desastre ou de compilação no qual a propriedade ou a capacidade de assumir a propriedade de objetos é necessária. Além disso, DAs e EAs herdar um número de seus direitos e permissões por meio de sua associação padrão no grupo de administradores. Grupo padrão de aninhamento de grupos privilegiados no Active Directory não deve ser modificado e o grupo de administradores de cada domínio deve ser protegido conforme descrito nas instruções passo a passo que seguem.  

Para o grupo de administradores em cada domínio na floresta:  

1.  Remova todos os membros do grupo de administradores, com a possível exceção da conta interna de administrador para o domínio, desde que ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Nos GPOs vinculados a OUs que contenham servidores membro e estações de trabalho em cada domínio, o grupo DA deve ser adicionado aos direitos de usuário no **direitos de usuário do computador \ Diretivas \ Configurações de segurança locais \ Policies\ Atribuição**:  

    -   Negar o acesso a este computador a partir da rede  

    -   Negar o logon como um trabalho em lotes  

    -   Negar o logon como um serviço  

3.  Em controladores de domínio OU em cada domínio na floresta, o grupo de administradores deve receber os seguintes direitos de usuário:  

    -   Acesso a este computador da rede  

    -   Permitir logon localmente  

    -   Permitir logon por meio dos Serviços de Área de Trabalho Remota  

4.  A auditoria deve ser configurada para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo Administradores.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instruções passo a passo para remover todos os membros do grupo de administradores  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Active Directory Users and Computers**.  

2.  Para remover todos os membros do grupo Administradores, execute as seguintes etapas:  

    1.  Clique duas vezes o **administradores** agrupar e clique no **membros** guia.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selecione um membro do grupo, clique em **remova**, clique em **Yes**e clique em **Okey**.  

3.  Repita a etapa 2 até que todos os membros do grupo Administradores foram removidos.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instruções passo a passo para proteger grupos de administradores no Active Directory  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda &lt;floresta&gt;\Domains\\&lt;domínio&gt;e então **objetos de diretiva de grupo** (onde &lt;floresta&gt; é o nome da floresta e &lt;domínio&gt; é o nome do domínio no qual você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde *nome do GPO* é o nome desse GPO).  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  No painel de detalhes, clique com botão direito **<GPO Name>** e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configure os direitos de usuário para impedir que membros do grupo administradores acessem os servidores membro e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que os membros do grupo Administradores de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo Administradores de fazer logon como um serviço da seguinte maneira:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

11. Na **gerenciamento de política de grupo**, vincule o GPO para o servidor membro e a estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até a &lt;floresta&gt;> \Domains\\&lt;domínio&gt; (onde &lt;floresta&gt; é o nome da floresta e &lt;domínio&gt; é o nome do domínio no qual você deseja definir a política de grupo).  

    2.  A unidade Organizacional que o GPO será aplicado a e clique com o botão direito **vincular um GPO existente**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Crie links para todas as outras UOs que contêm as estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

        > [!IMPORTANT]  
        > Se os servidores de salto são usadas para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de salto estão localizados em uma unidade Organizacional ao qual este GPOs não está vinculado.  

        > [!NOTE]  
        > Quando você implementa restrições no grupo de administradores em GPOs, o Windows aplicam as configurações para os membros do grupo de administradores local do computador além do grupo de administradores do domínio. Portanto, você deve usar o cuidado ao implementar restrições no grupo de administradores. Embora proibindo de rede, lote e logons de serviço para os membros do grupo Administradores é aconselhável onde quer que seja viável para implementação, não restringir logons locais ou logons por meio dos serviços de área de trabalho remota. Esses tipos de logon de bloqueio pode bloquear administração legítima de um computador por membros do grupo Administradores local.  
        >   
        > A captura de tela a seguir mostra as definições de configuração que bloqueiam o uso indevido do interna local e domínio da conta do administrador, além de uso indevido dos grupos internos de administradores de domínio ou local. Observe que o **Negar logon pelos serviços de área de trabalho remota** direito de usuário não inclui o grupo de administradores, porque os incluí-lo nesta configuração também bloqueia esses logons para contas que são membros do computador local Grupo de administradores. Se serviços em computadores são configurados para executar no contexto de qualquer um dos grupos privilegiados descritos nesta seção, implementar essas configurações pode causar falhas em aplicativos e serviços. Portanto, assim como acontece com todas as recomendações nesta seção, você deve testar as configurações de aplicabilidade no seu ambiente.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instruções passo a passo para conceder direitos ao grupo de administradores  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e então **objetos de diretiva de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja Defina a política de grupo).  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde <GPO Name> é o nome desse GPO).  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  No painel de detalhes, clique com botão direito **<GPO Name>** e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configure os direitos de usuário para permitir que os membros do grupo Administradores para acessar os controladores de domínio pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para permitir que os membros do grupo administradores fazer logon localmente, fazendo o seguinte:  

    1.  Clique duas vezes em **permitir logon localmente** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **administradores**, clique em verificar **nomes**e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para permitir que os membros do grupo administradores fazer logon por meio dos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **permitir logon pelos serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

11. Na **gerenciamento de política de grupo**, vincular o GPO à UO de controladores de domínio, fazendo o seguinte:  

    1.  Navegue até a <Forest>\Domains\\ <Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir a política de grupo).  

    2.  Os controladores de domínio UO e clique com o botão direito **vincular um GPO existente**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique as configurações de GPO "Negar acesso a este computador pela rede"  
Em qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO (como "servidor de salto"), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações de GPO, tente mapear a unidade do sistema usando o **NET USE** comando.  

1.  Faça logon localmente usando uma conta que seja membro do grupo Administradores.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador** para abrir o alto prompt de comando.  

4.  Quando solicitado a aprovar a elevação, clique em **Sim**.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  No **Prompt de comando** , digite **net use \\ \\ \<nome do servidor\>\c$**, onde \<nome do servidor\> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro deve aparecer.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

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

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de pesquisa, digite agendar tarefas e clique em tarefas de agendamento.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **<Task Name>** (onde <Task Name> é o nome da nova tarefa).  

5.  Clique o **ações** guia e, em seguida, clique em **New**.  

6.  No **ação** campo, selecione **iniciar um programa**.  

7.  No **programa/script** , clique em **procurar**, localize e selecione o arquivo de lote criado na **criar um arquivo em lotes** seção e, em seguida, clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. No **opções de segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que seja membro do grupo Administradores, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **Executar estando o usuário não está conectado onor** e **não armazenam senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Deve aparecer uma caixa de diálogo, a conta de usuário solicitando credenciais para executar a tarefa.  

15. Depois de inserir a senha, clique em **Okey**.  

16. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  No **fazer logon como** campo, selecione **esta conta**.  

7.  Clique em **navegue**, digite o nome de uma conta que seja membro do grupo Administradores, clique em **verificar nomes**e clique em **Okey**.  

8.  No **senha** e **Confirmar senha** campos, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![grupos de administração seguras](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler da impressora  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  No **fazer logon como** , clique em **sistema Local** da conta e, em seguida, clique em **Okey**.  
