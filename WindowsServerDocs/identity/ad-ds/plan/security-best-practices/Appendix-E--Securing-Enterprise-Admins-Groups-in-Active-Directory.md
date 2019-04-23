---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Apêndice E – protegendo grupos de administrador corporativo no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856717"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apêndice E: Protegendo grupos de administrador corporativo no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Apêndice E: Protegendo grupos de administrador corporativo no Active Directory  
O grupo de administradores Enterprise (EA), que está hospedado no domínio raiz da floresta, não deve conter nenhum usuário no dia a dia, com a possível exceção da conta de administrador do domínio raiz, desde que ele é protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administradores de empresa são, por padrão, os membros do grupo Administradores em cada domínio na floresta. Você não deve remover o grupo EA dos grupos de administradores em cada domínio, pois no caso de um cenário de recuperação de desastre de floresta, os direitos EA provavelmente será necessários. Grupo de administradores de empresa da floresta deve ser protegido conforme detalhado em instruções passo a passo a seguir.  

Para o grupo de administradores corporativos na floresta:  

1.  O grupo de administradores de empresa nos GPOs vinculados a OUs que contenham servidores membro e estações de trabalho em cada domínio, deve ser adicionado aos direitos de usuário no **Policies\ de configurações do computador \ Diretivas \ Configurações de segurança Atribuições de direitos do usuário**:  

    -   Negar o acesso a este computador a partir da rede  

    -   Negar o logon como um trabalho em lotes  

    -   Negar o logon como um serviço  

    -   Negar o logon localmente  

    -   Negar o logon por meio dos Serviços de Área de Trabalho Remota  

2.  Configure a auditoria para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo Administradores corporativos.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Instruções passo a passo para a remoção de todos os membros do grupo de administradores de empresa  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Active Directory Users and Computers**.  

2.  Se você não estiver gerenciando o domínio raiz da floresta, na árvore de console, clique com botão direito <Domain>e, em seguida, clique em **alterar domínio** (onde <Domain> é o nome do domínio que você está administrando no momento).  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  No **alterar domínio** caixa de diálogo, clique em **procurar**, selecione o domínio raiz da floresta e clique em **Okey**.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Para remover todos os membros do grupo de EA:  

    1.  Clique duas vezes o **Enterprise Admins** agrupar e, em seguida, clique no **membros** guia.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Selecione um membro do grupo, clique em **remova**, clique em **Yes**e clique em **Okey**.  

5.  Repita a etapa 2 até que todos os membros do grupo EA foram removidos.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instruções passo a passo para proteger os administradores de empresa no Active Directory  

1.  Na **Gerenciador de servidores**, clique em **ferramentas**e clique em **Group Policy Management**.  

2.  Na árvore de console, expanda <Forest>\Domains\\<Domain>e então **objetos de diretiva de grupo** (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio em que você deseja Defina a política de grupo).  

    > [!NOTE]  
    > Em uma floresta que contém vários domínios, um GPO semelhante deve ser criado em cada domínio que exige que o grupo de administradores de empresa sejam protegidos.  

3.  Na árvore de console, clique com botão direito **Group Policy Objects**e clique em **New**.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  No **novo GPO** caixa de diálogo, digite <GPO Name>e clique em **Okey** (onde <GPO Name> é o nome desse GPO).  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  No painel de detalhes, clique com botão direito <GPO Name>e clique em **editar**.  

6.  Navegue até **computador \ Diretivas \ Configurações de segurança \ diretivas**e clique em **atribuição de direitos de usuário**.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configure os direitos de usuário para impedir que membros do grupo Administradores corporativos acessem servidores membro e estações de trabalho na rede, fazendo o seguinte:  

    1.  Clique duas vezes em **negar acesso a este computador pela rede** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

    3.  Tipo de **Enterprise Admins**, clique em **verificar nomes**e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

8.  Configure os direitos de usuário para impedir que membros do grupo Administradores de empresa de fazer logon como um trabalho em lotes, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon como um trabalho em lotes** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e clique em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Tipo de **Enterprise Admins**, clique em **verificar nomes**e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

9. Configure os direitos de usuário para impedir que os membros do grupo de EA de fazer logon como um serviço da seguinte maneira:  

    1.  Clique duas vezes em **Negar logon como um serviço** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e, em seguida, clique em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Tipo de **Enterprise Admins**, clique em **verificar nomes**e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

10. Configure direitos de usuário para impedir que membros do grupo Administradores de empresa de fazer logon localmente para servidores membro e estações de trabalho, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon localmente** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e, em seguida, clique em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Tipo de **Enterprise Admins**, clique em **verificar nomes**e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

11. Configure os direitos de usuário para impedir que membros do grupo Administradores corporativos acessem servidores membro e estações de trabalho por meio dos serviços de área de trabalho remota, fazendo o seguinte:  

    1.  Clique duas vezes em **Negar logon pelos serviços de área de trabalho remota** e selecione **definir estas configurações de política**.  

    2.  Clique em **adicionar usuário ou grupo** e, em seguida, clique em **procurar**.  

        > [!NOTE]  
        > Em uma floresta que contém vários domínios, clique em **locais** e selecione o domínio raiz da floresta.  

    3.  Tipo de **Enterprise Admins**, clique em **verificar nomes**e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Clique em **Okey**, e **Okey** novamente.  

12. Para sair **Editor de gerenciamento de diretiva de grupo**, clique em **arquivo**e clique em **sair**.  

13. Na **gerenciamento de política de grupo**, vincule o GPO para o servidor membro e a estação de trabalho UOs, fazendo o seguinte:  

    1.  Navegue até a <Forest>\Domains\\ <Domain> (onde <Forest> é o nome da floresta e <Domain> é o nome do domínio no qual você deseja definir a política de grupo).  

    2.  A unidade Organizacional que o GPO será aplicado a e clique com o botão direito **vincular um GPO existente**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Selecione o GPO que você acabou de criar e clique em **Okey**.  

        ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Crie links para todas as outras UOs que contêm as estações de trabalho.  

    5.  Crie links para todas as outras UOs que contêm servidores membro.  

    6.  Em uma floresta que contém vários domínios, um GPO semelhante deve ser criado em cada domínio que exige que o grupo de administradores de empresa sejam protegidos.  

> [!IMPORTANT]  
> Se os servidores de salto são usadas para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de salto estão localizados em uma unidade Organizacional ao qual este GPOs não está vinculado.  

### <a name="verification-steps"></a>Etapas de verificação  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Verifique as configurações de GPO "Negar acesso a este computador pela rede"  
Em qualquer servidor membro ou estação de trabalho que não é afetada pelas alterações de GPO (como "servidor de salto"), tente acessar um servidor membro ou estação de trabalho na rede que é afetada pelas alterações de GPO. Para verificar as configurações de GPO, tente mapear a unidade do sistema usando o **NET USE** comando executando as seguintes etapas:  

1.  Fazer logon localmente usando uma conta que seja um membro do grupo EA.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **prompt de comando**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador** para abrir o alto prompt de comando.  

4.  Quando solicitado a aprovar a elevação, clique em **Sim**.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  No **Prompt de comando** , digite **net use \\ \\ \<nome do servidor\>\c$**, onde \<nome do servidor\> é o nome do servidor membro ou estação de trabalho que você está tentando acessar pela rede.  

6.  A captura de tela a seguir mostra a mensagem de erro deve aparecer.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um trabalho em lotes"  

De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

##### <a name="create-a-batch-file"></a>Criar um arquivo em lotes  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **bloco de notas**e clique em **o bloco de notas**.  

3.  Na **bloco de notas**, digite **dir c:**.  

4.  Clique em **arquivo**e clique em **Salvar como**.  

5.  No **arquivo** caixa de nome, digite  **<Filename>. bat** (onde <Filename> é o nome do novo arquivo em lotes).  

##### <a name="schedule-a-task"></a>Agendar uma tarefa  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **Agendador de tarefas**e clique em **Agendador de tarefas**.  

    > [!NOTE]  
    > Em computadores que executam Windows 8, além de **pesquisa** , digite **agendar tarefas**e clique em **agendar tarefas**.  

3.  Clique em **ação**e clique em **criar tarefa**.  

4.  No **criar tarefa** caixa de diálogo, digite **<Task Name>** (onde <Task Name> é o nome da nova tarefa).  

5.  Clique o **ações** guia e, em seguida, clique em **New**.  

6.  No **ação** campo, selecione **iniciar um programa**.  

7.  Sob **programa/script**, clique em **procurar**, localize e selecione o arquivo de lote criado na **criar um arquivo em lotes** seção e, em seguida, clique em **abrir**.  

8.  Clique em **OK**.  

9. Clique na guia **Geral**.  

10. No **opções de segurança** , clique em **Alterar usuário ou grupo**.  

11. Digite o nome de uma conta que seja membro do grupo de atributos estendidos, clique em **verificar nomes**e clique em **Okey**.  

12. Selecione **Executar estando o usuário conectado ou não** e selecione **não armazenam senha**. A tarefa só terá acesso aos recursos do computador local.  

13. Clique em **OK**.  

14. Deve aparecer uma caixa de diálogo, a conta de usuário solicitando credenciais para executar a tarefa.  

15. Depois de inserir as credenciais, clique em **Okey**.  

16. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Verifique as configurações de GPO "Negar logon como um serviço"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como**, selecione **esta conta**.  

7.  Clique em **navegue**, digite o nome de uma conta que seja membro do grupo de atributos estendidos, clique em **verificar nomes**e clique em **Okey**.  

8.  Sob **senha:** e **Confirmar senha**, digite a senha da conta selecionada e clique em **Okey**.  

9. Clique em **Okey** mais três vezes.  

10. Clique com botão direito do **Spooler de impressão** de serviço e selecione **reiniciar**.  

11. Quando o serviço for reiniciado, deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Reverter alterações para o serviço de Spooler da impressora  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, faça logon localmente.  

2.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

3.  No **pesquisa** , digite **services**e clique em **serviços**.  

4.  Localize e clique duas vezes em **Spooler de impressão**.  

5.  Clique na guia **Fazer Logon**.  

6.  Sob **fazer logon como**, selecione o **sistema Local** da conta e, em seguida, clique em **Okey**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Verifique as configurações de GPO "Negar logon localmente"  

1.  De qualquer servidor membro ou estação de trabalho afetados pelas alterações de GPO, tente fazer logon localmente usando uma conta que seja um membro do grupo EA. Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Verifique as configurações de GPO "Negar logon pelos serviços de área de trabalho remota"  

1.  Com o mouse, mova o ponteiro para o canto superior direito ou inferior direito da tela. Quando o **botões** barra for exibida, clique em **pesquisa**.  

2.  No **pesquisa** , digite **conexão de área de trabalho remota**e, em seguida, clique em **Conexão de área de trabalho remota**.  

3.  No **computador** , digite o nome do computador que você deseja se conectar a e, em seguida, clique em **Connect**. (Você também pode digitar o endereço IP em vez do nome do computador).  

4.  Quando solicitado, forneça credenciais para uma conta que seja um membro do grupo EA.  

5.  Deve aparecer uma caixa de diálogo semelhante à seguinte.  

    ![proteger o grupos de administradores enterprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
