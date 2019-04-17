---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: "Apêndice F - protegendo grupos de administradores de domínio no Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3535714e0df43cb94c7e89c503bc5f875cd49e28
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice f: Protegendo grupos de administradores de domínio no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Apêndice f: Protegendo grupos de administradores de domínio no Active Directory  
Como é o caso com o grupo Administradores de empresa (EA), a associação ao grupo Administradores de domínio (DA) deve ser solicitada somente nos cenários de recuperação de desastres ou de compilação. Não deve haver nenhuma conta de usuário diárias no grupo com exceção de conta de administrador interno para o domínio, se ele tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administradores de domínio são, por padrão, os membros do grupo Administradores local em todos os servidores membros e estações de trabalho em seus respectivos domínios. Este aninhamento padrão não deve ser modificado para fins de recuperação de desastres e suporte. Se a administradores de domínio foram removidos do grupo Administradores local nos servidores membro, o grupo deve ser adicionado ao grupo Administradores em cada servidor membro e estação de trabalho no domínio. Grupo de administradores do domínio de cada domínio deve ser protegido conforme descrito nas instruções passo a passo que seguem.  

Para o grupo Admins. do domínio em cada domínio na floresta:  

1.  Remova todos os membros do grupo, com a possível exceção de conta de administrador interno para o domínio, desde que ela tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Em GPOs vinculados a UOs contendo servidores membros e estações de trabalho em cada domínio, o grupo DA deve ser adicionado à seguintes direitos do usuário em **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  

    -   Negar acesso a este computador pela rede  

    -   Negar logon como um trabalho em lotes  

    -   Negar logon como um serviço  

    -   Negar logon localmente  

    -   Negar logon pelos direitos de usuário de serviços de área de trabalho remota  

3.  Auditoria deve ser configurada para enviar alertas se quaisquer modificações feitas para as propriedades ou associação do grupo Admins. do domínio.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instruções passo a passo para remover todos os membros do grupo Administradores de domínio  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **usuários e Active Directory computadores**.  

2.  Para remover todos os membros do grupo DA, execute as seguintes etapas:  

    1.  Clique duas vezes o **Admins. do domínio** de grupo e clique no **membros** guia.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Selecione um membro do grupo, clique em **remover**, clique em **Sim**e clique em **Okey**.  

3.  Repita a etapa 2 até que todos os membros do grupo DA foram removidos.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instruções passo a passo para proteger Admins. do domínio no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda \ < Forest\ > \\Domains\\\ < Domain\ > e depois **objetos de política de grupo** (onde \ < Forest\ > é o nome da floresta e \ < Domain\ > é o nome do domínio em que você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **objetos de política de grupo**e clique em **nova**.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  No **novo GPO** caixa de diálogo, digite \ < GPO Name\ > e clique em **Okey** (onde \ < GPO Name\ > é o nome desse GPO).  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  No painel de detalhes, clique com botão direito \ < GPO Name\ > e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configure os direitos de usuário para impedir que os membros do grupo Admins. do domínio acesse servidores membros e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que os membros do grupo de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo de fazer logon como um serviço, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Configure os direitos de usuário para impedir que os membros do grupo Admins. do domínio logon localmente para servidores membros e estações de trabalho, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon localmente** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

11. Configure os direitos de usuário para impedir que os membros do grupo Admins. do domínio acesse servidores membros e estações de trabalho por meio de serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **Admins. do domínio**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

12. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e clique em **sair**.  

13. No gerenciamento de política de grupo, vincule o GPO para o servidor membro e estação de trabalho UOs fazendo o seguinte:  

    1.  Navegue até o \ < Forest\ > \Domains\\\ < Domain\ > (onde \ < Forest\ > é o nome da floresta e \ < Domain\ > é o nome do domínio em que você deseja definir a política de grupo).  

    2.  Clique com botão direito na UO que o GPO serão aplicadas a e clique em **vincular um GPO existente**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Crie links para todas as outras unidades organizacionais que contêm estações de trabalho.  

    5.  Crie links para todas as outras unidades organizacionais que contêm servidores membro.  

        > [!IMPORTANT]  
        > Se os servidores de atalhos são usados para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de atalhos estão localizados em uma UO à qual este GPOs não é vinculado.  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique se as configurações de GPO "Negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações GPO (como um "salto servidor"), tente acessar um servidor membro ou estação de trabalho pela rede que é afetada pelas alterações GPO. Para verificar as configurações de GPO, tentar mapear a unidade do sistema usando o **NET USE** comando.  

1.  Faça logon localmente usando uma conta que seja um membro do grupo Admins. do domínio.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e clique em **executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando solicitado para aprovar a elevação, clique em **Sim**.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  No **Prompt de comando** janela, digite **net use \ \ \ < servidor Name\ > \c$**, onde \ < servidor Name\ > é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

###### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **bloco de notas**.  

3.  Em **bloco de notas**, tipo **c: dir**.  

4.  Clique em **arquivo**e clique em **Salvar como**.  

5.  No **arquivo** campo nome, digite **\. bat < Filename\ >** (onde \ < Filename\ > é o nome do novo arquivo em lotes).  

###### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, além do **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  No **Agendador de tarefas** barra de menus, clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **\ < tarefa Name\ >** (onde \ < tarefa Name\ > é o nome da nova tarefa).  

5.  Clique no **ações** guia e clique em **nova**.  

6.  No **ação** campo, selecione **iniciar um programa**.  

7.  Em **programa/script**, clique em **procurar**, localize e selecione o arquivo em lotes criado no **criar um arquivo em lotes** seção e clique em **abrir**.  

8.  Clique em **Okey**.  

9. Clique no **geral** guia.  

10. Em **segurança** opções, clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que é um membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **executar se o usuário fizer logon ou não** e selecione **não armazene senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **Okey**.  

14. Uma caixa de diálogo deve aparecer, credenciais de conta de usuário solicitante para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  Em **faça logon como**, selecione o **essa conta** opção.  

7.  Clique em **procurar**, digite o nome de uma conta que é um membro do grupo Admins. do domínio, clique em **verificar nomes**e clique em **Okey**.  

8.  Em **senha** e **Confirmar senha**, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  Em **faça logon como**, selecione o **LocalSystem** da conta e clique em **Okey**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon localmente"  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, tente fazer logon localmente usando uma conta que seja um membro do grupo Admins. do domínio. Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"    
1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador ao qual você deseja se conectar a e clique em **conectar**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça credenciais de uma conta que seja um membro do grupo Admins. do domínio.  

5.  Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração de domínio seguro](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
