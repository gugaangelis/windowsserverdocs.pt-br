---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: Guia de instruções-gerenciar riscos com o controle de acesso condicional
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aefcd597a580de526a758c6d026c6c91d02d10c8
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371658"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guia Passo a passo: gerenciar riscos com controle de acesso condicional




## <a name="about-this-guide"></a>Sobre este guia
Este tutorial fornece instruções para gerenciar o risco com um dos fatores (dados do usuário) disponíveis por meio do mecanismo de controle de acesso condicional no Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2. Para obter mais informações sobre o controle de acesso condicional e os mecanismos de autorização no AD FS no Windows Server 2012 R2, consulte [gerenciar riscos com o controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Este passo a passo consiste nas seguintes seções:

-   [Etapa 1: Configurando o ambiente de laboratório](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Etapa 2: verificar o mecanismo de controle de acesso de AD FS padrão](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Etapa 3: configurar a política de controle de acesso condicional com base nos dados do usuário](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Etapa 4: verificar o mecanismo de controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Etapa 1: Configurando o ambiente de laboratório
Para concluir este passo a passo, é necessário um ambiente que consiste nos seguintes componentes:

-   Um domínio Active Directory com um usuário de teste e contas de grupo, em execução no Windows Server 2008, no Windows Server 2008 R2 ou no Windows Server 2012 com seu esquema atualizado para o Windows Server 2012 R2 ou um domínio de Active Directory em execução no Windows Server 2012 R2

-   Um servidor de Federação em execução no Windows Server 2012 R2

-   Um servidor Web que hospede o aplicativo de exemplo

-   Um computador cliente do qual é possível acessar o aplicativo de exemplo

> [!WARNING]
> É altamente recomendável (em ambientes de produção ou de teste) que você não use o mesmo computador para ser o servidor de federação e o servidor Web.

Nesse ambiente, o servidor de federação emite as declarações que são necessárias para que os usuários possam acessar o aplicativo de exemplo. O servidor Web hospeda um aplicativo de exemplo que confiará nos usuários que apresentarem as declarações que o servidor de federação emitir.

Para obter instruções sobre como configurar esse ambiente, consulte [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Etapa 2: verificar o mecanismo de controle de acesso de AD FS padrão
Nesta etapa, você verificará o mecanismo de controle de acesso do AD FS padrão, no qual o usuário é redirecionado para a página de entrada do AD FS, fornece credenciais válidas e recebe o acesso ao aplicativo. Você pode usar a conta do AD de **Robert Hatley** e o aplicativo de exemplo **ClaimApp** que você configurou em [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Para verificar o mecanismo de controle de acesso do AD FS padrão

1.  No computador cliente, abra uma janela do navegador e navegue até o aplicativo de exemplo: **https://webserv1.contoso.com/claimapp** .

    Essa ação automaticamente redireciona a solicitação ao servidor de federação, e você será solicitado a entrar com um nome de usuário e uma senha.

2.  Digite as credenciais da conta de **Robert Hatley** do AD que você criou em [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você receberá acesso ao aplicativo.

## <a name="BKMK_3"></a>Etapa 3: configurar a política de controle de acesso condicional com base nos dados do usuário
Nesta etapa, você configurará uma política de controle de acesso com base nos dados de associação de grupo do usuário. Em outras palavras, você configurará uma **Regra de Autorização de Emissão** no servidor de federação para um objeto de confiança de terceira parte confiável que representa o aplicativo de exemplo – **claimapp**. Pela lógica dessa regra, o usuário de **Robert Hatley** ad receberá declarações que são necessárias para acessar esse aplicativo porque ele pertence a um grupo de **Finanças** . Você adicionou a conta **Robert Hatley** ao grupo **Finanças** em [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

É possível concluir essa tarefa usando o Console de gerenciamento do AD FS ou o Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Console de Gerenciamento do AD FS

1.  No Console de Gerenciamento do AD FS, navegue até **Relações de Confiança** e depois **Objetos de Confiança da Terceira Parte Confiável**.

2.  Selecione o objeto de confiança de terceira parte confiável que representa o aplicativo de exemplo (**claimapp**) e, no painel **Ações** ou clicando com o botão direito do mouse nesse objeto de confiança de terceira parte confiável, selecione **Editar Regras de Declaração**.

3.  Na janela **Editar Regras de Declaração para claimapp**, selecione a guia **Regra de Autorização de Emissão** e clique em **Adicionar Regra**.

4.  No **Assistente de Adição de Regra de Declarações de Autorização de Emissão**, na **página Selecionar Modelo de Regra**, selecione o modelo de regra de declaração **Permitir ou Negar Usuários Com Base em uma Declaração de Entrada** e clique em **Avançar**.

5.  Na página **Configurar Regra**, execute todos os itens a seguir e clique em **Concluir**:

    1.  Digite um nome para a regra de declaração, por exemplo, **TestRule**.

    2.  Selecione **SID de Grupo** como **Tipo de declaração de entrada**.

    3.  Clique em **Procurar**, digite **Finanças** para o nome do seu grupo de teste do AD e resolva-o para o campo **Valor de declaração de entrada**.

    4.  Selecione a opção **Negar o acesso a usuários com esta declaração de entrada**.

6.  Na janela **Editar Regras de Declaração para claimapp**, exclua a regra **Permitir o Acesso a Todos os Usuários**, que foi criada por padrão quando você criou esse objeto de confiança da terceira parte confiável.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Para configurar a política de controle de acesso condicional com base nos dados do usuário por meio do Windows PowerShell

1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:


~~~
`$rp = Get-AdfsRelyingPartyTrust -Name claimapp`
~~~


2. Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:


~~~
`$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`
~~~

> [!NOTE]
> Não se esqueça de substituir <SID_do_grupo> pelo valor da SID do seu grupo **Finanças** do AD.

## <a name="BKMK_4"></a>Etapa 4: verificar o mecanismo de controle de acesso condicional
Nesta etapa, você verificará a política de controle de acesso condicional configurada na etapa anterior. É possível usar o procedimento a seguir para verificar se o usuário do AD **Eduardo Gomes** pode acessar o aplicativo de exemplo, uma vez que ele pertence ao grupo **Finanças**, bem como se os usuários do AD que não pertencem ao grupo **Finanças** não podem acessar o aplicativo de exemplo.

1.  No computador cliente, abra uma janela do navegador e navegue até o aplicativo de exemplo: **https://webserv1.contoso.com/claimapp**

    Essa ação automaticamente redireciona a solicitação ao servidor de federação, e você será solicitado a entrar com um nome de usuário e uma senha.

2.  Digite as credenciais da conta de **Robert Hatley** do AD que você criou em [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Você receberá acesso ao aplicativo.

3.  Digite as credenciais de outro usuário do AD que NÃO pertence ao grupo **Finanças**. (Para obter mais informações sobre como criar contas de usuário no AD, consulte [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    Neste ponto, devido à política de controle de acesso que você configurou na etapa anterior, uma mensagem de ' acesso negado ' é exibida para esse usuário do AD que não pertence ao grupo de **Finanças** . O texto da mensagem padrão **não está autorizado a acessar este site. Clique aqui para sair e entrar novamente ou contate o administrador para obter permissões.** No entanto, esse texto é totalmente personalizável. Para obter mais informações sobre como personalizar a experiência de entrada, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Veja também
[Gerenciar o risco com o controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



