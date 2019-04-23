---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: Guia passo a passo – gerencie riscos com controle de acesso condicional
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826647"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guia Passo a passo: Gerenciar riscos com Controle de Acesso Condicional

>Aplica-se a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Sobre este guia
Este passo a passo fornece instruções sobre como gerenciar risco com um dos fatores (dados do usuário) disponíveis através do mecanismo de controle de acesso condicional no Active Directory Federation Services (AD FS) no Windows Server 2012 R2. Para obter mais informações sobre mecanismos de autorização e controle de acesso condicional no AD FS no Windows Server 2012 R2, consulte [gerenciar riscos com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Este passo a passo consiste nas seguintes seções:

-   [Etapa 1: Configurando o ambiente de laboratório](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Etapa 2: Verifique se o mecanismo de controle de acesso do AD FS padrão](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Etapa 3: Configurar política de controle de acesso condicional com base nos dados de usuário](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Etapa 4: Verifique se o mecanismo de controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Etapa 1: Configuração do ambiente de laboratório
Para concluir este passo a passo, é necessário um ambiente que consiste nos seguintes componentes:

-   Um domínio do Active Directory com um usuário de teste e contas de grupo, em execução no Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 com seu esquema atualizado para o Windows Server 2012 R2 ou um domínio do Active Directory em execução no Windows Server 2012 R2

-   Um servidor de Federação em execução no Windows Server 2012 R2

-   Um servidor Web que hospede o aplicativo de exemplo

-   Um computador cliente do qual é possível acessar o aplicativo de exemplo

> [!WARNING]
> É altamente recomendável (em ambientes de produção ou de teste) que você não use o mesmo computador para ser o servidor de federação e o servidor Web.

Nesse ambiente, o servidor de federação emite as declarações que são necessárias para que os usuários possam acessar o aplicativo de exemplo. O servidor Web hospeda um aplicativo de exemplo que confiará nos usuários que apresentarem as declarações que o servidor de federação emitir.

Para obter instruções sobre como configurar esse ambiente, consulte [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Etapa 2: verificar o mecanismo de controle de acesso do AD FS padrão
Nesta etapa, você verificará o mecanismo de controle de acesso do AD FS padrão, no qual o usuário é redirecionado para a página de entrada do AD FS, fornece credenciais válidas e recebe o acesso ao aplicativo. Você pode usar o **Robert Hatley** conta do AD e o **claimapp** que você configurou no aplicativo de exemplo [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Para verificar o padrão de mecanismo de controle de acesso do AD FS

1.  No computador cliente, abra uma janela do navegador e navegue até o aplicativo de exemplo: **https://webserv1.contoso.com/claimapp**.

    Essa ação automaticamente redireciona a solicitação ao servidor de federação, e você será solicitado a entrar com um nome de usuário e uma senha.

2.  Digite as credenciais do **Robert Hatley** que você criou na conta do AD [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você receberá acesso ao aplicativo.

## <a name="BKMK_3"></a>Etapa 3: configurar política de controle de acesso condicional com base nos dados do usuário
Nesta etapa, você configurará uma política de controle de acesso com base nos dados de associação de grupo do usuário. Em outras palavras, você configurará uma **Regra de Autorização de Emissão** no servidor de federação para um objeto de confiança de terceira parte confiável que representa o aplicativo de exemplo – **claimapp**. Pela lógica dessa regra, **Robert Hatley** usuário do AD receberá emissões de declarações que são necessárias para acessar este aplicativo porque ele pertence a um **Finanças** grupo. Você adicionou o **Robert Hatley** da conta para o **Finanças** grupo [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

É possível concluir essa tarefa usando o Console de gerenciamento do AD FS ou o Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Console de Gerenciamento do AD FS

1.  No Console de Gerenciamento do AD FS, navegue até **Relações de Confiança**e depois **Objetos de Confiança da Terceira Parte Confiável**.

2.  Selecione o objeto de confiança de terceira parte confiável que representa o aplicativo de exemplo (**claimapp**) e, no painel **Ações** ou clicando com o botão direito do mouse nesse objeto de confiança de terceira parte confiável, selecione **Editar Regras de Declaração**.

3.  Na janela **Editar Regras de Declaração para claimapp** , selecione a guia **Regra de Autorização de Emissão** e clique em **Adicionar Regra**.

4.  No **Assistente de Adição de Regra de Declarações de Autorização de Emissão**, na **página Selecionar Modelo de Regra**, selecione o modelo de regra de declaração **Permitir ou Negar Usuários Com Base em uma Declaração de Entrada** e clique em **Avançar**.

5.  Na página **Configurar Regra** , execute todos os itens a seguir e clique em **Concluir**:

    1.  Digite um nome para a regra de declaração, por exemplo, **TestRule**.

    2.  Selecione **SID de Grupo** como **Tipo de declaração de entrada**.

    3.  Clique em **Procurar**, digite **Finanças** para o nome do seu grupo de teste do AD e resolva-o para o campo **Valor de declaração de entrada** .

    4.  Selecione a opção **Negar o acesso a usuários com esta declaração de entrada** .

6.  Na janela **Editar Regras de Declaração para claimapp** , exclua a regra **Permitir o Acesso a Todos os Usuários** , que foi criada por padrão quando você criou esse objeto de confiança da terceira parte confiável.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Windows PowerShell

1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Não se esqueça de substituir <SID_do_grupo> pelo valor da SID do seu grupo **Finanças** do AD.

## <a name="BKMK_4"></a>Etapa 4: verificar o mecanismo de controle de acesso condicional
Nesta etapa, você verificará a política de controle de acesso condicional configurada na etapa anterior. É possível usar o procedimento a seguir para verificar se o usuário do AD **Eduardo Gomes** pode acessar o aplicativo de exemplo, uma vez que ele pertence ao grupo **Finanças** , bem como se os usuários do AD que não pertencem ao grupo **Finanças** não podem acessar o aplicativo de exemplo.

1.  No computador cliente, abra uma janela do navegador e navegue até o aplicativo de exemplo: **https://webserv1.contoso.com/claimapp**

    Essa ação automaticamente redireciona a solicitação ao servidor de federação, e você será solicitado a entrar com um nome de usuário e uma senha.

2.  Digite as credenciais do **Robert Hatley** que você criou na conta do AD [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você receberá acesso ao aplicativo.

3.  Digite as credenciais de outro usuário do AD que NÃO pertence ao grupo **Finanças**. (Para obter mais informações sobre como criar contas de usuário no AD, consulte [ https://technet.microsoft.com/library/cc7833232.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    Neste ponto, por causa da política de controle de acesso que você configurou na etapa anterior, uma mensagem de "acesso negado" é exibida para o usuário do AD que não pertence à **Finanças** grupo. O texto da mensagem padrão é **você não está autorizado a acessar este site. Clique aqui para sair e entrar novamente ou contate o administrador para permissões.** No entanto, esse texto é totalmente personalizável. Para obter mais informações sobre como personalizar a experiência de entrada, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Consulte também
[Gerencie riscos com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



