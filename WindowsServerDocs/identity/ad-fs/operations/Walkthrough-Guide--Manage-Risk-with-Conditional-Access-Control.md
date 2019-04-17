---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: Guia passo a passo - gerenciar risco com controle de acesso condicional
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guia passo a passo: Gerenciar o risco com controle de acesso condicional

>Aplica-se a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Sobre este guia
Este passo a passo fornece instruções para o gerenciamento de risco com um dos fatores (dados do usuário) disponíveis através do mecanismo de controle de acesso condicional no serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2. Para saber mais sobre os mecanismos de autorização e controle de acesso condicional do AD FS em Windows Server 2012 R2, consulte [gerenciar risco com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Este passo a passo consiste as seções a seguir:

-   [Etapa 1: Configurar o ambiente de laboratório](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Etapa 2: Verifique se o mecanismo de controle de acesso padrão do AD FS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Etapa 3: Definir a política de controle de acesso condicional com base nos dados do usuário](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Etapa 4: Verifique se o mecanismo de controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Etapa 1: Configurar o ambiente de laboratório
Para concluir este passo a passo, você precisa de um ambiente que consiste nos seguintes componentes:

-   Um domínio do Active Directory com um usuário de teste e contas de grupo, em execução no Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 com seu esquema atualizada para o Windows Server 2012 R2 ou em um domínio do Active Directory em execução no Windows Server 2012 R2

-   Um servidor de Federação em execução no Windows Server 2012 R2

-   Um servidor web que hospeda o seu aplicativo de amostra

-   Um computador cliente no qual você pode acessar o aplicativo de exemplo

> [!WARNING]
> É altamente recomendável (ambas em ambientes de produção ou teste) que você não use o mesmo computador para ser o servidor de Federação e seu servidor web.

Nesse ambiente, o servidor de Federação emite as declarações são necessárias para que os usuários podem acessar o aplicativo de exemplo. O servidor Web hospeda um aplicativo de amostra que confiará os usuários que apresentam as declarações que os problemas de servidor de Federação.

Para obter instruções sobre como configurar esse ambiente, consulte [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Etapa 2: Verifique se o mecanismo de controle de acesso padrão do AD FS
Nesta etapa, você verificará o mecanismo de controle de acesso do AD FS, onde o usuário será redirecionado para a página de entrada do AD FS, padrão fornece credenciais válidas e é concedido acesso ao aplicativo. Você pode usar o **Robert Hatley** conta AD e o **claimapp** exemplo de aplicativo que você configurou no [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Para verificar o padrão do AD FS acessar o mecanismo de controle

1.  No computador cliente, abra uma janela do navegador e navegue até seu aplicativo de amostra: **https://webserv1.contoso.com/claimapp**.

    Essa ação redireciona automaticamente a solicitação para o servidor de Federação e você será solicitado a fazer logon com um nome de usuário e senha.

2.  Digite as credenciais do **Robert Hatley** conta AD que você criou na [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você terá acesso ao aplicativo.

## <a name="BKMK_3"></a>Etapa 3: Definir a política de controle de acesso condicional com base nos dados do usuário
Nesta etapa você configurar uma política de controle de acesso com base nos dados de associação de grupo do usuário. Em outras palavras, você irá configurar um **regra de autorização de emissão** em seu servidor de federação para uma terceira parte relação de confiança que representa seu aplicativo de amostra - **claimapp**. Pela lógica dessa regra **Robert Hatley** usuário AD será emitido declarações que são necessárias para acessar este aplicativo porque ele pertence a um **Finanças** grupo. Você adicionou o **Robert Hatley** conta para a **Finanças** grupo no [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Você pode concluir essa tarefa usando o Console de gerenciamento do AD FS ou por meio do Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Console de gerenciamento do AD FS

1.  No Console de gerenciamento do AD FS, navegue até **relações de confiança**e depois **confie dependência de terceiros**.

2.  Selecione a terceira confiança de terceiros que representa seu aplicativo de amostra (**claimapp**) e, em seguida, no **ações** painel ou clicando com esta relação de confiança de terceiros confiante, selecione **editar regras de declaração**.

3.  No **editar regras de declaração para claimapp** janela, selecione **regras de autorização de emissão** guia e clique em **Adicionar regra**.

4.  No **emissão autorização reivindicar regra Assistente para adicionar**diante do **página Selecionar modelo de regra**, selecione **permitir ou negar os usuários com base em uma declaração de entrada** reivindicar o modelo de regra e clique em **próxima**.

5.  No **configurar regra** página, faça o seguinte e, em seguida, clique em **concluir**:

    1.  Insira um nome para a regra de declaração, por exemplo **TestRule**.

    2.  Selecione **grupo SID** como **tipo de declaração de entrada**.

    3.  Clique em **procurar**, digite **Finanças** o nome do seu anúncio testar grupo e resolvê-lo para o **valor de solicitação de entrada** campo.

    4.  Selecione o **negar o acesso aos usuários com essa declaração de entrada** opção.

6.  No **editar regras de declaração para claimapp** janela, certifique-se de excluir o **permitir acesso a todos os usuários** regra que foi criada por padrão quando você criou essa terceira confiança de terceiros.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Windows PowerShell

1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Certifique-se de substituir < group_SID > com o valor do SID do seu anúncio **Finanças** grupo.

## <a name="BKMK_4"></a>Etapa 4: Verifique se o mecanismo de controle de acesso condicional
Nesta etapa, você verificará a política de controle de acesso condicional que você configurou na etapa anterior. Você pode usar o procedimento a seguir para verificar se **Robert Hatley** AD usuário pode acessar seu aplicativo de amostra porque ele pertence a **Finanças** grupo e os usuários de anúncio que não pertencem ao **Finanças** grupo não pode acessar o aplicativo de amostra.

1.  No computador cliente, abra uma janela do navegador e navegue até seu aplicativo de amostra: **https://webserv1.contoso.com/claimapp**

    Essa ação redireciona automaticamente a solicitação para o servidor de Federação e você será solicitado a fazer logon com um nome de usuário e senha.

2.  Digite as credenciais do **Robert Hatley** conta AD que você criou na [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você terá acesso ao aplicativo.

3.  Digite as credenciais de outro usuário de anúncio que não pertence ao **Finanças** grupo. (Para obter mais informações sobre como criar contas de usuário no AD, consulte [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    Neste ponto, devido a política de controle de acesso que você configurou na etapa anterior, uma mensagem de 'acesso negado' é exibida para o usuário de anúncio que não pertence ao **Finanças** grupo. O texto da mensagem padrão é **você não está autorizado a acessar este site. Clique aqui para sair e logon novamente ou contate o administrador para permissões.** No entanto, esse texto é totalmente personalizável. Para obter mais informações sobre como personalizar a experiência de entrada, consulte [Personalizando as AD FS Sign-in páginas](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Consulte também
[Gerenciar os riscos com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



