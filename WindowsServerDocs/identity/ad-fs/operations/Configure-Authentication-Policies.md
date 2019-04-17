---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: "Configurar as políticas de autenticação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>Configurar as políticas de autenticação

>Aplica-se a: Windows Server 2012 R2

No AD FS, no Windows Server 2012 R2, redes de controle de acesso e o mecanismo de autenticação são aprimorada com diversos fatores que incluem dados de usuário, dispositivo, localização e autenticação. Essas melhorias permitem que você, por meio da interface do usuário ou por meio do Windows PowerShell para gerenciar o risco de conceder permissões de acesso aos aplicativos protegidos FS\ AD por meio de controle de acesso do fator multi\ e autenticação de fator multi\ que se baseiam em associação de identidade ou grupo de usuário, local de rede, os dados do dispositivo é ingressado workplace\, e o estado de autenticação quando a autenticação de fator multi\ \(MFA\) foi realizado.  
  
Para saber mais sobre o controle de acesso MFA e fator de multi\ em \(AD FS\) serviços de Federação do Active Directory no Windows Server 2012 R2, consulte os tópicos a seguir:  


-   [Junte-se a empresa de qualquer dispositivo para SSO e perfeito segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gerenciar os riscos com controle de acesso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar as políticas de autenticação via o AD FS snap\-in Gerenciamento  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esses procedimentos.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
No AD FS, no Windows Server 2012 R2, você pode especificar uma política de autenticação em um escopo global que se aplica a todos os aplicativos e serviços que são protegidos pelo AD FS. Você também pode definir políticas de autenticação para aplicativos específicos e serviços que dependem de relações de confiança de terceiros e são protegidos pelo AD FS. Especificando uma política de autenticação para um aplicativo específico por confiar confiança de terceiros não substitui a política de autenticação global. Se global ou por confiar confiança de terceiros a política de autenticação exige MFA, MFA é disparada quando o usuário tentar autenticar nesta terceira relação de confiança de terceiros. A política de autenticação global é um fallback para terceira relações de confiança de terceiros para aplicativos e serviços que não têm uma política de autenticação configurado específico. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar a autenticação principal globalmente no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  No AD FS snap\-in, clique em **políticas de autenticação**.  
  
3.  No **autenticação primária** seção, clique em **editar** próximo ao **configurações globais**. Você também pode clicar right\ **políticas de autenticação**e selecione **Editar Global autenticação primária**, ou, no **ações** painel, selecione **Editar Global autenticação primária**.  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  No **Editar política de autenticação Global** janela diante do **principal** guia, você pode configurar as seguintes configurações como parte da política de autenticação global:  
  
    -   Métodos de autenticação a ser usado para autenticação primária. Você pode selecionar os métodos de autenticação disponíveis sob o **Extranet** e **Intranet**.  
  
    -   Autenticação de dispositivo por meio do **habilitar a autenticação de dispositivo** caixa de seleção. Para obter mais informações, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar a autenticação principal por confiar confiança de terceiros  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  No AD FS snap\-in, clique em **políticas de autenticação**\\**por confiar terceiros confiança**e, em seguida, clique na terceira confiança de terceiros para o qual você deseja configurar políticas de autenticação.  
  
3.  Right\-clicar o terceiro confiável para o qual você deseja configurar as políticas de autenticação e, em seguida, selecione **Editar Custom Authentication principal**, ou, no **ações** painel, selecione **Editar Custom Authentication principal**.  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  No **Editar política de autenticação para < relying\_party\_trust\_name >** janela, no **principal** guia, você pode definir as seguintes configurações como parte do **por confiar terceiros confiar** política de autenticação:  
  
    -   Se os usuários são necessários para fornecer suas credenciais de cada vez no momento do sign\ via o **usuários serão solicitados a fornecer suas credenciais de cada vez no momento do sign\** caixa de seleção.  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar a autenticação multifator globalmente  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  No AD FS snap\-in, clique em **políticas de autenticação**.  
  
3.  No **autenticação de fator Multi\** seção, clique em **editar** próximo ao **configurações globais**. Você também pode clicar right\ **políticas de autenticação**e selecione **Editar Global autenticação de fator Multi\**, ou, no **ações** painel, selecione **Editar Global autenticação de fator Multi\**.  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  No **Editar política de autenticação Global** janela, no **Multi\ fator** guia, você pode configurar as seguintes configurações como parte da política de autenticação de fator de multi\ global:  
  
    -   Configurações ou condições mfa por meio de opções disponíveis sob o **/grupos de usuários \**, **dispositivos**, e **locais** seções.  
  
    -   Para habilitar a MFA para qualquer uma dessas configurações, você deve selecionar pelo menos um método de autenticação adicional. **Autenticação de certificado** é a opção disponível padrão. Você também pode configurar outros métodos de autenticação adicional personalizado, por exemplo, Windows Azure Active autenticação. Para obter mais informações, consulte [guia passo a passo: gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> Você só pode configurar métodos de autenticação adicional globalmente.  
![políticas de autorização](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar a autenticação de fator multi\ por confiar confiança de terceiros  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  No AD FS snap\-in, clique em **políticas de autenticação**\\**por confiar terceiros confiança**e, em seguida, clique na terceira confiança de terceiros para o qual você deseja configurar MFA.  
  
3.  Right\-clicar o terceiro confiável para o qual você deseja configurar MFA e, em seguida, selecione **autenticação de fator Multi\ Editar personalizado**, ou, no **ações** painel, selecione **autenticação de fator Multi\ Editar personalizado**.  
  
4.  No **Editar política de autenticação para < relying\_party\_trust\_name >** janela, no **Multi\ fator** guia, você pode configurar as seguintes configurações como parte da parte dependência per\ confiar em política de autenticação:  
  
    -   Configurações ou condições mfa por meio de opções disponíveis sob o **/grupos de usuários \**, **dispositivos**, e **locais** seções.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar as políticas de autenticação por meio do Windows PowerShell  
Windows PowerShell proporciona maior flexibilidade usando vários fatores do controle de acesso e o mecanismo de autenticação que estão disponíveis no AD FS no Windows Server 2012 R2 para configurar políticas de autenticação e autorização regras que são necessárias para implementar true acesso condicional para seus recursos de \-secured AD FS.  
  
A associação ao grupo Administradores, ou equivalente, no computador local é o requisito mínimo para concluir esses procedimentos.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar um método de autenticação adicionais por meio do Windows PowerShell  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Para verificar se esse comando foi executado com êxito, você pode executar o `Get-AdfsGlobalAuthenticationPolicy`comando.  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Para configurar a dependência de per\ MFA confiança de terceiros que se baseia em dados de associação de grupo do usuário  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Certifique-se para substituir *< relying\_party\_trust >* com o nome de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  
 
    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valor = ~ '" $^(?i) < group_SID >'"] = > problema (tipo = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor '" https://schemas.microsoft.com/claims/multipleauthn'");" 
    
    Conjunto AdfsRelyingPartyTrust – TargetRelyingParty $rp – AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Certifique-se para substituir < group\_SID > com o valor do identificador de segurança \(SID\) do seu grupo \(AD\) do Active Directory.  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar MFA globalmente com base em dados de associação de grupo de usuários  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", valor = = '" group_SID'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
      
    Conjunto-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Certifique-se para substituir *< group\_SID >* com o valor do SID do seu grupo de anúncios.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar MFA globalmente com base na localização do usuário  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", valor = = '" true_or_false'"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Conjunto-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Certifique-se para substituir *< true\_or\_false >* com um `true` ou `false`. O valor depende de sua condição de regra específica com base em se a solicitação de acesso é proveniente de extranet ou a intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar MFA globalmente com base em dados de dispositivo do usuário  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    $MfaClaimRule = "c: [tipo = = '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", valor = = '" true_or_false"']  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Conjunto-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Certifique-se para substituir *< true\_or\_false >* com um `true` ou `false`. O valor depende de sua condição de regra específica com base em se o dispositivo é ingressado workplace\ ou não.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar MFA globalmente quando a solicitação de acesso é proveniente de extranet e de um dispositivo non\ workplace\ ingressados  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Certifique-se para substituir as duas instâncias do *< true\_or\_false >* com um `true` ou `false`, que depende das condições de regra específica. As condições de regra são baseadas em se o dispositivo é ingressado workplace\ ou não e se a solicitação de acesso é proveniente de extranet ou intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar MFA globalmente quando acesso é proveniente de um usuário extranet que pertence a um determinado grupo  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    Set-AdfsAdditionalAuthenticationRule "c: [tipo = = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", valor = = `"group_SID`"] & & c2: [tipo = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valor = = `"true_or_false`"] = > problema (tipo = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", valor ='"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Certifique-se para substituir *< group\_SID >* com o valor do grupo SID e *< true\_or\_false >* com um `true` ou `false`, que depende de sua condição de regra específica com base em se a solicitação de acesso é proveniente de extranet ou intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acesso a um aplicativo com base nos dados do usuário por meio do Windows PowerShell  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Certifique-se para substituir *< relying\_party\_trust >* com o valor de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Certifique-se para substituir *< group\_SID >* com o valor do SID do seu grupo de anúncios.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acesso a um aplicativo que é protegido por somente se o AD FS identidade do usuário foi validado com MFA  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Certifique-se para substituir *< relying\_party\_trust >* com o valor de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acesso a um aplicativo que está protegido pelo AD FS somente se a solicitação de acesso vem de um dispositivo ingressado workplace\ que está registrado para o usuário  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Certifique-se para substituir *< relying\_party\_trust >* com o valor de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [tipo = = `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", valor = ~ `"^(?i)true$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valor = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso a um aplicativo que está protegido pelo AD FS somente se a solicitação de acesso vem de um dispositivo ingressado workplace\ que está registrado para um usuário cuja identidade foi validada com MFA  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Certifique-se para substituir *< relying\_party\_trust >* com o valor de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso extranet a um aplicativo protegido pelo AD FS somente quando a solicitação de acesso é proveniente de um usuário cuja identidade foi validada com MFA  
  
1.  Em seu servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Certifique-se para substituir *< relying\_party\_trust >* com o valor de sua confiança confiante de terceiros.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    C1: [tipo = = `"https://schemas.microsoft.com/claims/authnmethodsreferences`", valor = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] & &  
    C2: [tipo = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", valor = ~ `"^(?i)false$`"] = > problema (tipo = `"https://schemas.microsoft.com/authorization/claims/permit`", valor = `"PermitUsersWithClaim`"); "  

## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
