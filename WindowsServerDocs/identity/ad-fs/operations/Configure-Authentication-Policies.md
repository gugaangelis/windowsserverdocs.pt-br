---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurar políticas de autenticação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 46eb61db92207a73320f87790a4063076a3cac4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817279"
---
# <a name="configure-authentication-policies"></a>Configurar políticas de autenticação

No AD FS, no Windows Server 2012 R2, o controle de acesso e o mecanismo de autenticação são aprimorados com vários fatores que incluem dados de usuário, dispositivo, local e autenticação. Esses aprimoramentos permitem que você, por meio da interface do usuário ou por meio do Windows PowerShell, gerencie o risco de conceder permissões de acesso para AD FS\-aplicativos protegidos por meio de controle de acesso de vários fatores\-e autenticação multi\-factor que se baseiam na identidade do usuário ou na associação de grupo, no local da rede, nos dados do dispositivo que são\-associados e no estado de\-\(\)  

Para obter mais informações sobre MFA e o controle de acesso de vários\-factor no Serviços de Federação do Active Directory (AD FS) \(AD FS\) no Windows Server 2012 R2, consulte os seguintes tópicos:  


-   [Ingresso no Local de Trabalho em qualquer dispositivo de SSO e autenticação de dois fatores contínua em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gerenciar risco com o Controle de Acesso Condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gerenciar riscos com Autenticação Multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar políticas de autenticação por meio do\-snap de gerenciamento de AD FS no  
A associação ao grupo **Administradores** ou equivalente no computador local é o requisito mínimo para concluir esses procedimentos.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   

No AD FS, no Windows Server 2012 R2, você pode especificar uma política de autenticação em um escopo global que seja aplicável a todos os aplicativos e serviços protegidos pelo AD FS. Você também pode definir políticas de autenticação para aplicativos e serviços específicos que dependem de relações de confiança de terceiros e são protegidos pelo AD FS. A especificação de uma política de autenticação para um aplicativo específico por confiança de terceira parte confiável não substitui a política de autenticação global. Se a política global ou de autenticação de confiança de terceira parte confiável exigir MFA, a MFA será disparada quando o usuário tentar se autenticar nessa terceira parte confiável. A política de autenticação global é um fallback para confianças de terceira parte confiável para aplicativos e serviços que não têm uma política de autenticação configurada específica. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar a autenticação primária globalmente no Windows Server 2012 R2 

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  

2.  Em AD FS snap\-no, clique em **políticas de autenticação**.  

3.  Na seção **autenticação primária** , clique em **Editar** ao lado de **configurações globais**. Você também pode clicar com o botão direito\-do mouse em **políticas de autenticação**e selecionar **Editar Autenticação global primária**ou, no painel **ações** , selecione **Editar Autenticação global primária**.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy1.png)

4.  Na janela **Editar política de autenticação global** , na guia **primário** , você pode definir as seguintes configurações como parte da política de autenticação global:  

    -   Métodos de autenticação a serem usados para autenticação primária. Você pode selecionar os métodos de autenticação disponíveis na **extranet** e na **intranet**.  

    -   Autenticação de dispositivo por meio da caixa de seleção **habilitar autenticação de dispositivo** . Para obter mais informações, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar a autenticação primária por confiança de terceira parte confiável  

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  

2.  Em AD FS snap\-no, clique em **políticas de autenticação**\\**por confiança**de terceira parte confiável e clique na terceira parte confiável para a qual você deseja configurar políticas de autenticação.  

3.  Seja à direita\-clique na relação de confiança de terceira parte confiável para a qual você deseja configurar as políticas de autenticação e selecione **Editar Autenticação primária personalizada**ou, no painel **ações** , selecione **Editar Autenticação primária personalizada**.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Na janela **Editar política de autenticação para < terceira parte\_\_> nome do\_de confiança** , na guia **primária** , você pode definir a seguinte configuração como parte da política de autenticação de confiança de **terceira parte confiável** :  

    -   Se os usuários precisam fornecer suas credenciais a cada vez na\-de entrada por meio dos **usuários, é necessário fornecer suas credenciais a cada vez** na caixa de seleção\-de entrada.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar a autenticação multifator globalmente  

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  

2.  Em AD FS snap\-no, clique em **políticas de autenticação**.  

3.  Na seção **autenticação do fator de\-múltiplo** , clique em **Editar** ao lado de **configurações globais**. Você também pode clicar com o botão direito\-do mouse em **políticas de autenticação**e selecionar **Editar autenticação global multi\-factor**ou, no painel **ações** , selecione **editar autenticação global de vários fatores de\-** .  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Na janela **Editar política de autenticação global** , na guia **fator de\-vários** , você pode definir as seguintes configurações como parte da política de autenticação do fator de vários\-global:  

    -   Configurações ou condições para MFA por meio de opções disponíveis nas seções **usuários\/grupos**, **dispositivos**e **locais** .  

    -   Para habilitar a MFA para qualquer uma dessas configurações, você deve selecionar pelo menos um método de autenticação adicional. A **autenticação de certificado** é a opção padrão disponível. Você também pode configurar outros métodos de autenticação adicionais personalizados, por exemplo, autenticação ativa do Windows Azure. Para obter mais informações, consulte [Guia de instruções: gerenciar riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  

> [!WARNING]  
> Você só pode configurar métodos de autenticação adicionais globalmente.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar a autenticação de vários fatores de\-por confiança de terceira parte confiável  

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  

2.  Em AD FS snap\-no, clique em **políticas de autenticação**\\**por confiança**de terceira parte confiável e clique na relação de confiança de terceira parte confiável para a qual você deseja configurar a MFA.  

3.  Seja com o botão direito do mouse\-clique no objeto de confiança de terceira parte confiável para o qual você deseja configurar a MFA e, em seguida, selecione **Editar autenticação multi\-factor personalizada**ou, no painel **ações** , selecione **Editar autenticação personalizada de vários fatores de\-** .  

4.  Na janela **Editar política de autenticação para < terceira parte\_\_nome do\_de confiança >** , na guia **fator de\-múltiplo** , você pode definir as seguintes configurações como parte da política de autenticação de confiança de terceira parte confiável de\-:  

    -   Configurações ou condições para MFA por meio de opções disponíveis nas seções **usuários\/grupos**, **dispositivos**e **locais** .  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar políticas de autenticação por meio do Windows PowerShell  
O Windows PowerShell permite maior flexibilidade no uso de vários fatores de controle de acesso e do mecanismo de autenticação que estão disponíveis em AD FS no Windows Server 2012 R2 para configurar políticas de autenticação e regras de autorização que são necessárias para implementar o verdadeiro acesso condicional para seus AD FS \-recursos protegidos.  

A associação ao grupo Administradores ou equivalente no computador local é o requisito mínimo para concluir esses procedimentos.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar um método de autenticação adicional por meio do Windows PowerShell  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Para verificar se esse comando foi executado com êxito, é possível executar o comando `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Para configurar a MFA por\-relação de confiança de terceira parte confiável com base nos dados de associação de grupo de um usuário  

1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o nome de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  


~~~
$MfaClaimRule = "c:[Type == '"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value =~ '"^(?i) <group_SID>$'"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value '"https://schemas.microsoft.com/claims/multipleauthn'");" 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Certifique-se de substituir o grupo de <\_SID > pelo valor do identificador de segurança \(SID\) do grupo Active Directory \(AD\).  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar o MFA globalmente com base nos dados de associação de grupo dos usuários  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == '"group_SID'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Certifique-se de substituir o *grupo de <\_SID >* pelo valor do SID do seu grupo do AD.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar o MFA globalmente com base no local do usuário  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '"true_or_false'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Certifique-se de substituir *< true\_ou\_false >* por `true` ou `false`. O valor depende de sua condição de regra específica que se baseia se a solicitação de acesso é proveniente da extranet ou da intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar o MFA globalmente com base nos dados do dispositivo do usuário  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false"']  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Certifique-se de substituir *< true\_ou\_false >* por `true` ou `false`. O valor depende de sua condição de regra específica que se baseia se o dispositivo é local de trabalho\-associado ou não.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar o MFA globalmente se a solicitação de acesso vier da extranet e de um local de trabalho não\-\-dispositivo ingressado  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Certifique-se de substituir as duas instâncias de *< true\_ou\_false >* por `true` ou `false`, o que depende de suas condições de regra específicas. As condições de regra se baseiam em se o dispositivo é o local de trabalho\-ingressado ou não e se a solicitação de acesso é proveniente da extranet ou da intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar o MFA globalmente se o Access vier de um usuário de extranet que pertence a um determinado grupo  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Certifique-se de substituir o *grupo de <\_sid >* pelo valor do SID do grupo e *< true\_ou\_false >* com `true` ou `false`, o que depende da condição de regra específica que se baseia em se a solicitação de acesso é proveniente da extranet ou da intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acesso a um aplicativo com base nos dados do usuário por meio do Windows PowerShell  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o valor de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `"Authorization`" @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Certifique-se de substituir o *grupo de <\_SID >* pelo valor do SID do seu grupo do AD.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acesso a um aplicativo protegido por AD FS somente se a identidade desse usuário tiver sido validada com MFA  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o valor de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = '"PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acesso a um aplicativo protegido por AD FS somente se a solicitação de acesso vier de um local de trabalho\-dispositivo Unido que está registrado para o usuário  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o valor de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso a um aplicativo protegido por AD FS somente se a solicitação de acesso vier de um local de trabalho\-dispositivo Unido que está registrado para um usuário cuja identidade foi validada com MFA  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o valor de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  

   ```  
   $GroupAuthzRule = '@RuleTemplate = "Authorization"  
   @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso de extranet a um aplicativo protegido por AD FS somente se a solicitação de acesso vier de um usuário cuja identidade tenha sido validada com MFA  

1.  No servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Certifique-se de substituir *< terceira parte\_\_de confiança >* com o valor de sua terceira parte confiável.  

2. Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
