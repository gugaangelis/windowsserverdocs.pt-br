---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurar políticas de autenticação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9345f995af2f256dddcbcbd7d05c4bf6170b563e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189862"
---
# <a name="configure-authentication-policies"></a>Configurar políticas de autenticação

No AD FS no Windows Server 2012 R2, controle de acesso e o mecanismo de autenticação são aprimoradas com vários fatores que incluem dados de usuário, dispositivo, localização e autenticação. Essas melhorias permitem que você, por meio da interface do usuário ou por meio do Windows PowerShell, para gerenciar o risco de conceder permissões de acesso para o AD FS\-protegidos aplicativos por meio de várias\-fatorar o controle de acesso e várias\-autenticação multifator com base em associação de grupo ou identidade de usuário, local de rede, dados de dispositivo que é o local de trabalho\-unidas, e a autenticação do estado quando várias\-autenticação multifator \(MFA\) foi executada.  
  
Para obter mais informações sobre o MFA e multi-thread\-fatorar o controle de acesso nos serviços de Federação do Active Directory \(do AD FS\) no Windows Server 2012 R2, consulte os tópicos a seguir:  


-   [Ingresse no local de trabalho de qualquer dispositivo de SSO contínuo e fatores de autenticação em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gerenciar risco com o Controle de Acesso Condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurar políticas de autenticação por meio do snap de gerenciamento do AD FS\-em  
A associação ao grupo **Administradores** ou equivalente no computador local é o requisito mínimo para concluir esses procedimentos.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
No AD FS no Windows Server 2012 R2, você pode especificar uma política de autenticação em um escopo global que é aplicável a todos os aplicativos e serviços que são protegidos pelo AD FS. Você também pode definir políticas de autenticação para aplicativos e serviços que dependem de objetos de confiança e são protegidos pelo AD FS específicos. Especificando uma política de autenticação para um aplicativo específico por terceira parte confiável de confiança não substitui a política de autenticação global. Se global ou por terceira parte confiável de confiança da política de autenticação requer MFA, a MFA é disparada quando o usuário tenta autenticar para essa terceira parte confiável. A política de autenticação global é um fallback para objetos de confiança para aplicativos e serviços que não têm uma política específica de autenticação configurado. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Para configurar a autenticação primária global no Windows Server 2012 R2 
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  No AD FS snap\-, clique em **políticas de autenticação**.  
  
3.  No **autenticação primária** seção, clique em **editar** lado **configurações globais**. Você pode também com o botão direito\-clique em **políticas de autenticação**e selecione **Editar autenticação primária Global**, ou, sob o **ações** painel, selecione  **Editar autenticação primária Global**.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  No **Editar política de autenticação Global** janela diante a **primário** guia, você pode configurar as configurações a seguir como parte da política de autenticação global:  
  
    -   Métodos de autenticação a ser usado para autenticação primária. Você pode selecionar os métodos de autenticação disponíveis sob o **Extranet** e **Intranet**.  
  
    -   Autenticação de dispositivo por meio de **habilitar autenticação de dispositivo** caixa de seleção. Para obter mais informações, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Para configurar a autenticação primária por terceira parte confiável de confiança  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  No AD FS snap\-, clique em **políticas de autenticação**\\**por confiança da terceira parte**e, em seguida, clique em de terceira parte confiável para o qual você deseja configurar a autenticação políticas.  
  
3.  Diretamente\-clique a terceira parte confiável para o qual você deseja configurar políticas de autenticação e, em seguida, selecione **Editar autenticação primária personalizada**, ou, sob o **ações** painel, Selecione **Editar autenticação primária personalizada**.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  No **editar a política de autenticação para < terceira parte confiável\_terceiros\_confiança\_nome >** janela, no **primário** guia, você pode definir a configuração a seguir como parte do **por terceira parte confiável** política de autenticação:  
  
    -   Se os usuários são solicitados a fornecer suas credenciais cada vez na entrada\-no via o **os usuários precisarão fornecer suas credenciais cada vez na entrada\-em** caixa de seleção.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Para configurar a autenticação multifator globalmente  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  No AD FS snap\-, clique em **políticas de autenticação**.  
  
3.  No **Multi\-autenticação fatores** seção, clique em **editar** próximo a **configurações globais**. Você pode também com o botão direito\-clique em **políticas de autenticação**e selecione **editar vários Global\-autenticação fatores**, ou, no **ações**painel, selecione **Editar Global Multi\-autenticação de fatores**.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  No **Editar política de autenticação Global** janela, sob o **Multi\-fator** guia, você pode configurar as configurações a seguir como parte do global com vários\-fator política de autenticação:  
  
    -   As configurações ou condições para MFA por meio de opções disponíveis sob o **usuários\/grupos**, **dispositivos**, e **locais** seções.  
  
    -   Para habilitar a MFA para qualquer uma dessas configurações, você deve selecionar pelo menos um método de autenticação adicional. **Autenticação de certificado** é a opção de padrão disponíveis. Você também pode configurar outros métodos adicionais de autenticação personalizada, por exemplo, Windows Azure Active Authentication. Para obter mais informações, consulte [guia passo a passo: Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> Você só pode configurar métodos de autenticação adicionais globalmente.  
![políticas de autenticação](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Para configurar várias\-autenticação multifator por terceira parte confiável de confiança  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  No AD FS snap\-, clique em **políticas de autenticação**\\**por confiança da terceira parte**e, em seguida, clique em de terceira parte confiável para o qual você deseja configurar a MFA.  
  
3.  Diretamente\-clique a terceira parte confiável para o qual você deseja configurar a MFA e, em seguida, selecione **Editar personalizado Multi\-autenticação fatores**, ou, nas **ações** painel, Selecione **Editar personalizado Multi\-autenticação fatores**.  
  
4.  No **editar a política de autenticação para < terceira parte confiável\_terceiros\_confiança\_nome >** janela, no **Multi\-fator** guia, você pode configurar as configurações a seguir como parte do por\-política de autenticação de confiança de terceira parte confiável terceiros:  
  
    -   As configurações ou condições para MFA por meio de opções disponíveis sob o **usuários\/grupos**, **dispositivos**, e **locais** seções.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurar políticas de autenticação por meio do Windows PowerShell  
Windows PowerShell permite maior flexibilidade no uso de vários fatores de controle de acesso e o mecanismo de autenticação que estão disponíveis no AD FS no Windows Server 2012 R2 para configurar as políticas de autenticação e autorização regras que são necessárias para implementar o acesso condicional para o AD FS \-recursos protegidos.  
  
Associação de administradores ou equivalente, no computador local é o requisito mínimo para concluir esses procedimentos.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Para configurar um método de autenticação adicionais por meio do Windows PowerShell  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Para verificar se esse comando foi executado com êxito, é possível executar o comando `Get-AdfsGlobalAuthenticationPolicy` .  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Para configurar o MFA por\-terceira parte confiável que se baseia em dados de associação de grupo do usuário  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o nome da sua terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
 
    $MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’”, Value =~ ‘“^(?i) <group_SID>$’”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn’”);” 
    
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Certifique-se de substituir < grupo\_SID > com o valor do identificador de segurança \(SID\) do Active Directory \(AD\) grupo.  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Para configurar a MFA globalmente com base em dados de associação de grupo de usuários  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’", Value == ‘"group_SID’"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Certifique-se de substituir *< grupo\_SID >* com o valor do SID do grupo do AD.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Para configurar a MFA globalmente com base no local do usuário  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork’", Value == ‘"true_or_false’"]  
     = > problema (tipo = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", valor = ' "https://schemas.microsoft.com/claims/multipleauthn'"); "  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Certifique-se de substituir *< true\_ou\_false >* com um `true` ou `false`. O valor depende de sua condição de regra específica que baseia-se a solicitação de acesso é proveniente de extranet ou intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Para configurar a MFA globalmente com base em dados de dispositivo do usuário  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    $MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser’", Value == ‘"true_or_false"']  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");"  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Certifique-se de substituir *< true\_ou\_false >* com um `true` ou `false`. O valor depende de sua condição de regra específica que se baseia no fato do dispositivo no local de trabalho\-ingressou ou não.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Para configurar a MFA globalmente se a solicitação de acesso provir da extranet e de não\-workplace\-dispositivo associado  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Certifique-se de substituir as duas instâncias de *< true\_ou\_false >* com um `true` ou `false`, que depende de suas condições de regra específica. As condições de regra são com base em se o dispositivo está no local de trabalho\-ingressou ou não e se a solicitação de acesso provir da extranet ou intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Para configurar a MFA globalmente se trata de um usuário de extranet que pertence a um determinado grupo de acesso  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Certifique-se de substituir *< grupo\_SID >* com o valor do SID do grupo e *< true\_ou\_false >* com um `true` ou `false`, que depende de sua condição de regra específica que baseia-se a solicitação de acesso provir da extranet ou intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Para conceder acesso a um aplicativo com base nos dados de usuário por meio do Windows PowerShell  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o valor de confiança de terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Certifique-se de substituir *< grupo\_SID >* com o valor do SID do grupo do AD.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Para conceder acesso a um aplicativo que é protegido pelo somente se do AD FS, a identidade do usuário foi validada com MFA  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o valor de confiança de terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Para conceder acesso a um aplicativo que é protegido pelo somente se do AD FS, o acesso a solicitação vier de um local de trabalho\-ingressado no dispositivo que está registrado para o usuário  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o valor de confiança de terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso a um aplicativo que é protegido pelo somente se do AD FS, o acesso a solicitação vier de um local de trabalho\-ingressado no dispositivo que está registrado para um usuário cuja identidade foi validada com MFA  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o valor de confiança de terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Para conceder acesso à extranet a um aplicativo protegido pelo AD FS somente se a solicitação de acesso proveniente de um usuário cuja identidade foi validada com MFA  
  
1.  No servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Certifique-se de substituir *< terceira parte confiável\_terceiros\_confiança >* com o valor de confiança de terceira parte confiável.  
  
2.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
