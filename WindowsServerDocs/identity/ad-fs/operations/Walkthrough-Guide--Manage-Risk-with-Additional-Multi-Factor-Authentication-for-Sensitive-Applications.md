---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "Guia passo a passo - gerenciar risco com autenticação multifator adicional para aplicativos confidenciais"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guia passo a passo: Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais

>Aplica-se a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Sobre este guia
Este passo a passo fornece instruções para configurar a autenticação multifator (MFA) em serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2 com base nos dados de associação de grupo do usuário.

Para saber mais sobre os mecanismos de autenticação e MFA no AD FS, consulte [gerenciar risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Este passo a passo consiste as seções a seguir:

-   [Etapa 1: Configurar o ambiente de laboratório](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Etapa 2: Verifique se o mecanismo de autenticação do AD FS padrão](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Etapa 3: Definir MFA em seu servidor de Federação](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Etapa 4: Verifique se o mecanismo MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Etapa 1: Configurar o ambiente de laboratório
Para concluir este passo a passo, você precisa de um ambiente que consiste nos seguintes componentes:

-   Um domínio do Active Directory com um usuário de teste e contas de grupo, em execução no Windows Server 2012 R2 ou em um domínio do Active Directory em execução no Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 com seu esquema atualizada para o Windows Server 2012 R2

-   Um servidor de Federação em execução no Windows Server 2012 R2

-   Um servidor web que hospeda o seu aplicativo de amostra

-   Um computador cliente no qual você pode acessar o aplicativo de exemplo

> [!WARNING]
> É altamente recomendável (ambas em ambientes de teste e produção) que você não use o mesmo computador para ser o servidor de Federação e seu servidor web.

Nesse ambiente, o servidor de Federação emite as declarações são necessárias para que os usuários podem acessar o aplicativo de exemplo. O servidor Web hospeda um aplicativo de amostra que confiará os usuários que apresentam as declarações que os problemas de servidor de Federação.

Para obter instruções sobre como configurar esse ambiente, consulte [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Etapa 2: Verifique se o mecanismo de autenticação do AD FS padrão
Nesta etapa, você verificará o mecanismo de controle de acesso padrão do AD FS (**autenticação de formulários** para extranet e **autenticação do Windows** para intranet), onde o usuário é redirecionado para a página de entrada do AD FS, fornece credenciais válidas e é concedido acesso ao aplicativo. Você pode usar o **Robert Hatley** conta AD e o **claimapp** exemplo de aplicativo que você configurou no [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  No computador cliente, abra uma janela do navegador e navegue até seu aplicativo de amostra: **https://webserv1.contoso.com/claimapp**.

    Essa ação redireciona automaticamente a solicitação para o servidor de Federação e você será solicitado a fazer logon com um nome de usuário e senha.

2.  Digite as credenciais do **Robert Hatley** conta AD que você criou na [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você terá acesso ao aplicativo.

## <a name="BKMK_3"></a>Etapa 3: Definir MFA em seu servidor de Federação
Há duas partes para configurar o MFA do AD FS em Windows Server 2012 R2:

-   [Selecione um método de autenticação adicionais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configurar a política MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Selecione um método de autenticação adicionais
Para configurar a MFA, você deve selecionar um método de autenticação adicional. Neste passo a passo, para o método de autenticação adicional, você pode escolher entre as seguintes opções:

-   Selecione [autenticação de certificado](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) método que está disponível no AD FS no Windows Server 2012 R2 por padrão

-   Configurar e selecione [autenticação multifator do Windows Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Autenticação de certificado
Execute qualquer um dos procedimentos a seguir para selecionar a autenticação de certificado como o método de autenticação adicional:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Para configurar a autenticação de certificado como um método de autenticação adicionais por meio do Console de gerenciamento do AD FS

1.  Em seu servidor de federação, no Console de gerenciamento do AD FS, navegue até o **políticas de autenticação** nó e, em **autenticação multifator** seção, clique no **editar** próximo ao link o **configurações globais** subseção.

2.  No **Editar política de autenticação Global** janela, selecione **autenticação de certificado** como um método de autenticação adicional e depois clique em **Okey**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Para configurar a autenticação de certificado como um método de autenticação adicionais por meio do Windows PowerShell

1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Para verificar se esse comando foi executado com êxito, você pode executar o `Get-AdfsGlobalAuthenticationPolicy`comando.

#### <a name="BKMK_8"></a>Windows Azure a autenticação multifator
Concluir os procedimentos a seguir para baixar e configurar e selecione **Windows Azure a autenticação multifator** como autenticação adicional em seu servidor de Federação:

1.  [Crie um provedor de autenticação multifator por meio do Windows Portal do Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Baixe o Windows Azure a autenticação multifator Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Instalar o servidor de autenticação multifator Azure do Windows em seu servidor de Federação](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurar o Windows Azure a autenticação multifator como um método de autenticação adicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Crie um provedor de autenticação multifator por meio do Windows Portal do Azure

1.  Logon do Windows Azure Portal como administrador.

2.  À esquerda, selecione Active Directory.

3.  Na página do Active Directory, na parte superior, selecione **provedores de autenticação multifator**.  Na parte inferior, clique em **nova**.

4.  Em **do Active Directory -> Serviços de aplicativo**, selecione **provedor de autenticação multifator**e selecione **criação rápida**.

5.  Em **serviços de aplicativo**, selecione **Active provedores Auth**e selecione **criação rápida**.

6.  Preencha os seguintes campos e selecione **criar**.

    1.  **Nome** -o nome do provedor de autenticação multifator.

    2.  **Modelo de uso** -o modelo de uso do provedor de autenticação multifator.

        -   **Por autenticação** -pelo modelo de compra que cobra por autenticação. Normalmente usado para cenários que usam autenticação multifator do Windows Azure em um aplicativo voltado para o consumidor.

        -   **Por usuário habilitado** -pelo modelo de compra que cobra por usuário habilitado.  Normalmente usado para cenários de voltado para o funcionário como o Office 365.

        Para obter informações adicionais sobre modelos de uso, consulte [Windows Azure detalhes de preços](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Diretório** -locatário do Azure Active Directory do Windows que o provedor de autenticação multifator está associado. Isso é opcional, pois o provedor não precisa estar vinculada ao Windows Azure Active Directory quando Protegendo aplicativos locais.

7.  Depois de clicar em criar, o provedor de autenticação multifator será criado e você verá uma mensagem informando: criado com êxito o provedor de autenticação multifator.  Clique em **Okey**.

Em seguida, você deve baixar o servidor de autenticação multifator do Windows Azure. Você pode fazer isso iniciando o Portal de autenticação multifator do Windows Azure por meio do portal do Windows Azure.

##### <a name="BKMK_b"></a>Baixe o Windows Azure a autenticação multifator Server

1.  Fazer logon no Portal do Windows Azure como administrador e clique no provedor de autenticação multifator você criou no procedimento acima. Clique no **gerenciar** botão.

    Isso inicia o **Windows Azure a autenticação multifator** portal.

2.  No **Windows Azure a autenticação multifator** portal, clique em **Downloads**e clique em **baixar** para baixar uma cópia do servidor de autenticação multifator do Windows Azure.

Depois que você baixou o executável do servidor de autenticação multifator Windows Azure, você deve instalá-lo em seu servidor de Federação.

##### <a name="BKMK_c"></a>Instalar o servidor de autenticação multifator Azure do Windows em seu servidor de Federação

1.  Baixe e clique duas vezes no executável para o servidor de autenticação multifator do Windows Azure.  Isso iniciará a instalação.

2.  Na tela do contrato de licença, leia o contrato, selecione **concordo** e clique em **próxima**.

3.  Certifique-se de que a pasta de destino está correta e clique em **próxima**.

4.  Depois de concluir a instalação, clique em **concluir**.

Agora você está pronto para iniciar o servidor de autenticação multifator do Windows Azure que você instalou no seu servidor de Federação e configurá-lo como um método de autenticação adicional.

##### <a name="BKMK_d"></a>Configurar o Windows Azure a autenticação multifator como um método de autenticação adicional

1.  Iniciar **Windows Azure a autenticação multifator** de onde você instalado em seu servidor de Federação e na página de boas-vindas, verifique o **ignorar usando o Assistente de configuração de autenticação** caixa de seleção e clique em **próxima**.

2.  Para ativar o servidor de autenticação multifator, volte para a página no portal de gerenciamento de autenticação multifator em que você baixou o servidor de autenticação multifator e clique no **gerar credenciais de ativação** botão. Na interface do usuário do servidor de autenticação multifator, insira as credenciais que foram geradas e clique em **ativar**.

3.  Em seguida, o **servidor de autenticação multifator** interface do usuário solicita que você executar o **Assistente de configuração de vários servidores**.  Selecione **não**.

    > [!IMPORTANT]
    > Você pode ignorar a concluir o **Assistente de configuração de vários servidores** o ambiente de laboratório com o servidor de Federação apenas um que é usada para concluir este passo a passo. No entanto, se o ambiente contiver vários servidores de federação, você deve instalar o servidor de autenticação multifator e conclua o **Assistente de configuração de vários servidores** em cada servidor de federação para habilitar a replicação entre os servidores de vários fatores que executam em seus servidores de Federação.

4.  No **servidor de autenticação multifator** interface do usuário, selecione o **usuários** ícone, clique em **importar do Active Directory**, selecione o **Robert Hatley** conta para provisioná-lo no Windows Azure a autenticação multifator e, em seguida, clique em **importar**.

5.  No **usuários** lista, selecione o **Robert Hatley** conta, clique em **editar**e no **usuário edite** janela, forneça um número de telefone celular dessa conta, certifique-se a **Enabled** caixa de seleção está marcada e clique em **aplicar**.

6.  No **usuários** lista, selecione o **Robert Hatley** da conta e clique em **teste**. No **usuário de teste** janela, forneça as credenciais para o **Robert Hatley** conta. Quando o telefone de célula toca, pressione '#' para concluir a verificação da conta.

7.  No **servidor de autenticação multifator** interface do usuário, selecione o **AD FS** ícone, certifique-se de que **registro de usuário permitir**, **permitem que os usuários selecionem método** (incluindo **chamada telefônica** e **SMS**), **Use perguntas de segurança de fallback** e **Habilitar log** caixas de seleção estiverem marcadas, clique em **instalar AD FS adaptador**e conclua o **autenticação multifator AD FS adaptador** Assistente de instalação.

    > [!NOTE]
    > O **autenticação multifator AD FS adaptador** Assistente de instalação cria um grupo de segurança chamado **PhoneFactor Admins** no Active Directory e, em seguida, adiciona a conta de serviço do AD FS do seu serviço de Federação a esse grupo.
    > 
    > É recomendável que você verifique no controlador de domínio que o **PhoneFactor Admins** grupo realmente é criado e o AD FS conta de serviço é um membro desse grupo.
    > 
    > Se necessário, adicione a conta de serviço do AD FS do **PhoneFactor Admins** manualmente de grupo no controlador de domínio.

    Para obter detalhes adicionais sobre como instalar o AD FS adaptador, clique no link de Ajuda no canto superior direito do servidor de autenticação multifator.

8.  Para registrar o adaptador em seu serviço de federação, em seu servidor de federação, iniciar a janela de comando do Windows PowerShell e execute o seguinte comando:`\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. O adaptador agora está registrado como **WindowsAzureMultiFactorAuthentication**.  Você deve reiniciar o serviço do AD FS do registro que entrem em vigor.

9. Para configurar a autenticação multifator do Windows Azure como o método de autenticação adicional, no Console de gerenciamento do AD FS, navegue até o **políticas de autenticação** nó e, em **autenticação multifator** seção, clique no **editar** próximo ao link o **configurações globais** subseção. No **Editar política de autenticação Global** janela, selecione **autenticação multifator** como um método de autenticação adicional e depois clique em **Okey**.

    > [!NOTE]
    > Você pode personalizar o nome e descrição de como o método de autenticação multifator do Windows Azure, bem como qualquer configurados método de autenticação de terceiros, como ele aparece na sua interface do usuário do AD FS executando o **conjunto AdfsAuthenticationProviderWebContent** cmdlet. Para obter mais informações, consulte [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configurar a política MFA
Para possibilitar MFA, você deve configurar a política MFA em seu servidor de Federação. Para este tutorial, de acordo com nossa política MFA, **Robert Hatley** conta é necessária para passar por MFA porque ele pertence a **Finanças** grupo que você configurou no [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Você pode configurar a política MFA por meio do Console de gerenciamento do AD FS ou usando o Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Para configurar a política MFA com base nos dados de associação de grupo do usuário para claimapp por meio do Console de gerenciamento do AD FS

1.  Em seu servidor de federação, no Console de gerenciamento do AD FS, navegue até **políticas de autenticação**\\**por confiar terceiros confiança** nó e selecione a terceira confiança de terceiros que representa seu aplicativo de amostra (**claimapp**).

2.  No **ações** página ou clicando em **claimapp**, selecione **Editar personalizado a autenticação multifator**.

3.  No **Editar dependência parte confiável para claimapp** janela, clique no **adicionar** próximo ao botão o **/grupos de usuários** lista. Digite **Finanças** o nome do seu grupo de anúncio que você criou na [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)e clique em **verificar nomes**e quando o nome for resolvido, clique em **Okey**.

4.  Clique em **Okey** no **Editar dependência parte confiável para claimapp** janela.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Para configurar a política MFA com base nos dados de associação de grupo do usuário para claimapp por meio do Windows PowerShell

1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Certifique-se de substituir < group_SID > com o valor do SID do seu grupo AD **Finanças**.

## <a name="BKMK_4"></a>Etapa 4: Verifique se o mecanismo MFA
Nesta etapa, você verificará a funcionalidade MFA que você configurou na etapa anterior. Você pode usar o procedimento a seguir para verificar se **Robert Hatley** AD usuário pode acessar seu aplicativo de amostra e esse tempo é necessário para passar por MFA porque ele pertence a **Finanças** grupo.

1.  No computador cliente, abra uma janela do navegador e navegue até seu aplicativo de amostra: **https://webserv1.contoso.com/claimapp**.

    Essa ação redireciona automaticamente a solicitação para o servidor de Federação e você será solicitado a fazer logon com um nome de usuário e senha.

2.  Digite as credenciais do **Robert Hatley** conta AD.

    Neste ponto, devido a política MFA que você configurou, o usuário precisará passar por autenticação adicional. O texto da mensagem padrão é **por motivos de segurança, nós requerem informações adicionais para verificar sua conta.** No entanto, esse texto é totalmente personalizável. Para obter mais informações sobre como personalizar a experiência de entrada, consulte [Personalizando as AD FS Sign-in páginas](https://technet.microsoft.com/library/dn280950.aspx).

    Se você configurou a autenticação de certificado como o método de autenticação adicional, o texto da mensagem padrão é **selecionar um certificado que você deseja usar para autenticação. Se você cancelar a operação, feche o navegador e tente novamente.**

    Se você configurou o Windows Azure a autenticação multifator como o método de autenticação adicional, o texto da mensagem padrão é **uma chamada será colocada ao seu telefone para concluir a autenticação.** Para obter mais informações sobre como fazer logon com autenticação multifator do Windows Azure e usar várias opções para o método preferencial de verificação, consulte [visão geral de autenticação do Windows Azure multifator](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Consulte também
[Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



