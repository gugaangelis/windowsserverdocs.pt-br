---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: "Apêndice G - protegendo grupos de administradores no Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cf89f7bf31ce848de384778b0213d0ddc822158
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apêndice g: Protegendo grupos de administradores no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Apêndice g: Protegendo grupos de administradores no Active Directory  
Como é o caso com os grupos Administradores de empresa (EA) e administradores de domínio (DA), a associação ao grupo Administradores (BA) interno deve ser solicitada somente nos cenários de recuperação de desastres ou de compilação. Não deve haver nenhuma conta de usuário diárias no grupo Administradores, exceto a conta de administrador interno para o domínio, se ele tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Os administradores estão, por padrão, os proprietários da maioria dos objetos do AD DS em seus respectivos domínios. A associação ao grupo esse grupo pode ser necessário nos cenários de recuperação de desastres ou de compilação no qual a propriedade ou a capacidade de apropriar-se de objetos é necessária. Além disso, DAs e EAs herdam um número de seus direitos e permissões em virtude de sua participação padrão no grupo Administradores. Grupo padrão aninhamento de grupos privilegiados no Active Directory não deve ser modificado e grupo de administradores de cada domínio deve ser protegido, conforme descrito nas instruções passo a passo que seguem.  

Para o grupo de administradores em cada domínio na floresta:  

1.  Remover todos os membros do grupo Administradores, com a possível exceção de conta de administrador interno para o domínio, desde que ela tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Em GPOs vinculados a UOs contendo servidores membros e estações de trabalho em cada domínio, o grupo DA deve ser adicionado à seguintes direitos do usuário em **atribuição de direitos do computador \ Diretivas \ Configurações de segurança locais Policies\ usuário**:  

    -   Negar acesso a este computador pela rede  

    -   Negar logon como um trabalho em lotes  

    -   Negar logon como um serviço  

3.  Em controladores de domínio OU em cada domínio na floresta, o grupo Administradores deve ser concedido seguintes direitos do usuário:  

    -   Acessar este computador pela rede  

    -   Permitir logon localmente  

    -   Permitir logon pelos serviços de área de trabalho remota  

4.  Auditoria deve ser configurada para enviar alertas se quaisquer modificações feitas para as propriedades ou os membros do grupo Administradores.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instruções passo a passo para remover todos os membros do grupo Administradores  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **usuários e Active Directory computadores**.  

2.  Para remover todos os membros do grupo Administradores, execute as seguintes etapas:  

    1.  Clique duas vezes o **administradores** de grupo e clique no **membros** guia.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Selecione um membro do grupo, clique em **remover**, clique em **Sim**e clique em **Okey**.  

3.  Repita a etapa 2 até que todos os membros do grupo Administradores foram removidos.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instruções passo a passo para proteger os grupos de administradores no Active Directory  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e depois **objetos de política de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **objetos de política de grupo**e clique em **nova**.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde *nome do GPO* é o nome desse GPO).  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  No painel de detalhes, clique com botão direito ** <GPO Name> **e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configure os direitos de usuário para impedir que os membros do grupo administradores acessem servidores membros e estações de trabalho pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que os membros do grupo Administradores de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo Administradores de fazer logon como um serviço, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e clique em **sair**.  

11. Em **Group Policy Management**, vincular o GPO para o servidor membro e estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

    2.  Clique com botão direito na UO que o GPO serão aplicadas a e clique em **vincular um GPO existente**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Crie links para todas as outras unidades organizacionais que contêm estações de trabalho.  

    5.  Crie links para todas as outras unidades organizacionais que contêm servidores membro.  

        > [!IMPORTANT]  
        > Se os servidores de atalhos são usados para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de atalhos estão localizados em uma UO à qual este GPOs não é vinculado.  

        > [!NOTE]  
        > Quando você implementa restrições no grupo Administradores em GPOs, o Windows aplica as configurações aos membros do grupo Administradores local de um computador além do grupo de administradores do domínio. Portanto, você deve usar cuidado ao implementar restrições no grupo Administradores. Embora proibir rede, em lotes e logons de serviço para membros do grupo Administradores é aconselhado onde é possível implementar, não restringem logons locais ou logons pelos serviços de área de trabalho remota. Bloquear esses tipos de logon pode bloquear legítima administração de um computador por membros do grupo Administradores local.  
        >   
        > A captura de tela a seguir mostra as definições de configuração que bloqueiam o uso indevido das interno local e domínio da conta do administrador, além de uso indevido dos grupos de administradores local ou de domínio internos. Observe que o **Negar logon pelos serviços de área de trabalho remota** direito de usuário não inclui o grupo de administradores, pois incluindo-a nessa configuração também bloqueia esses logons para contas que são membros do grupo de administradores do computador local. Se os serviços em computadores configurados para serem executados no contexto de qualquer um dos grupos privilegiados descritos nesta seção, implementar essas configurações pode causar serviços e aplicativos falhar. Portanto, como ocorre com todas as recomendações nesta seção, você deve testar as configurações de aplicabilidade em seu ambiente.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instruções passo a passo para conceder direitos ao grupo Administradores  

1.  Em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e depois **objetos de política de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

3.  Na árvore de console, clique com botão direito **objetos de política de grupo**e clique em **nova**.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde <GPO Name> é o nome desse GPO).  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  No painel de detalhes, clique com botão direito ** <GPO Name> **e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configure os direitos de usuário para permitir que os membros do grupo Administradores em controladores de domínio de acesso pela rede, fazendo o seguinte:  

    1.  Clique duas vezes em **acesso a este computador pela rede** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para permitir que os membros do grupo administradores fazer logon localmente, fazendo o seguinte:  

    1.  Clique duas vezes em **permitir logon localmente** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administradores**, clique em verificar **nomes**e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para permitir que os membros do grupo Administradores de logon pelos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **permitir logon pelos serviços de área de trabalho remota** e selecione **definir essas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo **administradores**, clique em **verificar nomes**e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Para sair **Editor de gerenciamento de política de grupo**, clique em **arquivo**e clique em **sair**.  

11. Em **Group Policy Management**, vincular o GPO para os controladores de domínio, unidade Organizacional, fazendo o seguinte:  

    1.  Navegue até o <Forest>\Domains\\<Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja definir a política de grupo).  

    2.  Clique com botão direito os controladores de domínio, unidade Organizacional e clique em **vincular um GPO existente**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Etapas de verificação  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique se as configurações de GPO "Negar acesso a este computador pela rede"  
De qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações GPO (como um "salto servidor"), tente acessar um servidor membro ou estação de trabalho pela rede que é afetada pelas alterações GPO. Para verificar as configurações de GPO, tentar mapear a unidade do sistema usando o **NET USE** comando.  

1.  Faça logon localmente usando uma conta que seja um membro do grupo Administradores.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e clique em **executar como administrador** para abrir um prompt de comando com privilégios elevados.  

4.  Quando solicitado para aprovar a elevação, clique em **Sim**.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  No **Prompt de comando** janela, digite **net use \ \ \<Server Name>\c$**, onde <Server Name> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro que deve aparecer.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

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

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam o Windows 8, na caixa de pesquisa, digite agendar tarefas e clique em Agendar tarefas.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite ** <Task Name> ** (onde <Task Name> é o nome da nova tarefa).  

5.  Clique no **ações** guia e clique em **nova**.  

6.  No **ação** campo, selecione **iniciar um programa**.  

7.  No **programa/script** campo, clique em **procurar**, localize e selecione o arquivo em lotes criado no **criar um arquivo em lotes** seção e clique em **abrir**.  

8.  Clique em **Okey**.  

9. Clique no **geral** guia.  

10. No **opções de segurança** campo, clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que é um membro do grupo Administradores, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **executar se o usuário não está conectado onor** e **não armazene senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **Okey**.  

14. Uma caixa de diálogo deve aparecer, credenciais de conta de usuário solicitante para executar a tarefa.  

15. Depois de inserir a senha, clique em **Okey**.  

16. Uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique se as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  No **faça logon como** campo, selecione **essa conta**.  

7.  Clique em **procurar**, digite o nome de uma conta que é um membro do grupo Administradores, clique em **verificar nomes**e clique em **Okey**.  

8.  No **senha** e **Confirmar senha** campos, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito **Spooler de impressão** e clique em **reiniciar**.  

11. Quando o serviço for reiniciado, uma caixa de diálogo semelhante ao seguinte deve aparecer.  

    ![grupos de administração segura](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler de impressora  

1.  De qualquer servidor membro ou estação de trabalho afetadas pelas alterações GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **serviços**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique no **logon** guia.  

6.  No **faça logon como** campo, clique em **LocalSystem** da conta e clique em **Okey**.  
