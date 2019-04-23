---
title: Solucionando problemas do AD FS - Azure AD
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS e Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6f85c447ac0816c46e07145dbe9a491a29e17c0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846467"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Solucionando problemas do AD FS - Azure AD
Com o crescimento da nuvem, muitas empresas têm se movendo para usar o Azure AD para seus vários aplicativos e serviços.  Federação com o AD do Azure se tornou uma prática padrão com muitas organizações.  Este documento abordará alguns dos aspectos de solução de problemas que podem surgir com essa federação.  Vários dos tópicos do documento de solução de problemas gerais ainda pertencem à Federação com o Azure, portanto, este documento se concentra em informações específicas apenas com o Azure AD e interação com o AD FS.

## <a name="redirection-to-ad-fs"></a>Redirecionamento para o AD FS
O redirecionamento ocorre quando você entrar para um aplicativo como o Office 365 e são "redirecionado" para as organizações de servidores do AD FS para entrar.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Primeiras coisas a verificar
Se o redirecionamento não está ocorrendo que há algumas coisas que você deseja verificar

   1. Certifique-se de que seu locatário do AD do Azure é habilitado para federação entrar no portal do Azure e verificando no Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

   2.  Certifique-se de que seu domínio personalizado é verificado, clicando no domínio ao lado de federação no portal do Azure.
![](media/ad-fs-tshoot-azure/azure3.png)

   3. Por fim, você deseja verificar [DNS](ad-fs-tshoot-dns.md) e certifique-se de que seus servidores do AD FS ou servidores WAP estão resolvendo da internet.  Verifique se que isso seja resolvido e que você seja capaz de navegar até ele.
   4. Você também pode usar o cmdlet do PowerShell `Get-AzureADDomain` para obter essas informações também.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Você está recebendo um erro de método de autenticação desconhecido
Você pode encontrar um erro de "Método de autenticação desconhecido" indicando que AuthnContext não é suportado no nível do AD FS ou STS, quando você é redirecionado do Azure. 

Isso é mais comum ao Azure AD redireciona para o AD FS ou STS usando um parâmetro que impõe um método de autenticação. 

Para impor um método de autenticação, use um dos seguintes métodos:
- Para WS-Federation, use uma cadeia de caracteres de consulta WAUTH para forçar um método de autenticação preferido.

- Para SAML 2.0, use o seguinte:
```
<saml:AuthnContext>
<saml:AuthnContextClassRef>
urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
</saml:AuthnContextClassRef>
</saml:AuthnContext>
```
Quando o método de autenticação imposta é enviado com um valor incorreto, ou se não há suporte para esse método de autenticação no AD FS ou STS, você receberá uma mensagem de erro antes de ser autenticado.

|Método de autenticação que queria|wauth URI|
|-----|-----|
|Autenticação por nome de usuário e senha|urn:oasis:names:tc:SAML:1.0:am:password|
|Autenticação de cliente SSL|urn:ietf:rfc:2246|
|Autenticação integrada do Windows|urn:federation:authentication:windows|

Classes de contexto de autenticação SAML com suporte

|Método de autenticação|Classe de contexto de autenticação URI|
|-----|-----| 
|Nome de usuário e senha|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|Transporte protegido por senha|urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport|
|Cliente do Transport Layer Security (TLS)|urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient
|Certificado X.509|urn:oasis:names:tc:SAML:2.0:ac:classes:X509
|Autenticação integrada do Windows|urn:federation:authentication:windows|
|Kerberos|urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos|

Para certificar-se de que o método de autenticação é suportado no nível do AD FS, verifique o seguinte.

#### <a name="ad-fs-20"></a>AD FS 2.0 

Sob **/adfs/ls/web.config**, certifique-se de que a entrada para o tipo de autenticação está presente.

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

Sob **gerenciamento do AD FS**, clique em **políticas de autenticação** no AD FS, snap-in.

No **autenticação primária** seção, clique em Editar ao lado de configurações globais. Você pode também com o botão direito as políticas de autenticação e, em seguida, selecione Editar autenticação primária Global. Ou, no painel de ações, selecione Editar autenticação primária Global.

Na janela Editar política de autenticação Global, na guia principal, você pode configurar as configurações como parte da política de autenticação global. Por exemplo, para autenticação primária, você pode selecionar os métodos de autenticação disponíveis na Intranet e Extranet.

* * Certifique-se de que a caixa de seleção do método de autenticação necessária é selecionada. 

#### <a name="ad-fs-2016"></a>AD FS 2016

Sob **gerenciamento do AD FS**, clique em **Service** e **métodos de autenticação** no AD FS, snap-in.

No **autenticação primária** seção, clique em Editar.

No **editar métodos de autenticação** janela, na guia principal, você pode definir as configurações como parte da política de autenticação.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Tokens emitidos pelo AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD gera o erro após a emissão de token
Depois que o AD FS emite um token, o Azure AD poderão gerar um erro. Nessa situação, verifique os seguintes problemas:
- As declarações que são emitidas pelo AD FS no token devem corresponder os respectivos atributos do usuário no Azure AD.
- o token do AD do Azure deve conter as seguintes declarações necessárias:
    - WSFED: 
        - UPN: O valor dessa declaração deve corresponder o UPN dos usuários no AD do Azure.
        - ImmutableID: O valor dessa declaração deve corresponder o sourceAnchor ou ImmutableID do usuário no Azure AD.

Para obter o valor do atributo de usuário no Azure AD, execute a seguinte linha de comando: `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0:
       - IDPEmail: O valor dessa declaração deve corresponder ao nome principal do usuário dos usuários no AD do Azure.
       - NAMEID: O valor dessa declaração deve corresponder o sourceAnchor ou ImmutableID do usuário no Azure AD.

Para obter mais informações, consulte [usar um provedor de identidade SAML 2.0 para implementar o logon único](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Incompatibilidade de certificado autenticação de token entre o AD FS e Azure AD.

AD FS usa o certificado de autenticação de tokens para assinar o token que é enviado para o usuário ou aplicativo. A relação de confiança entre o AD FS e Azure AD é uma confiança federada que tem com base nesse certificado de autenticação de token.

No entanto, se o certificado de autenticação de token no lado do AD FS for alterado devido a sobreposição de certificado automática ou por alguma intervenção, os detalhes do novo certificado devem ser atualizados no lado do Azure AD para o domínio federado. Quando o certificado de autenticação de token primário no AD FS é diferente de anúncios do Azure, o token emitido pelo AD FS não é confiável pelo Azure AD. Portanto, o usuário federado não é permitido fazer logon.

Para corrigir isso, você pode usar as etapas descrevem [renovar certificados de federação para o Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Outras tarefas comuns para verificar
O exemplo a seguir é uma lista rápida das tarefas para verificar se você estiver tendo problemas com a interação com o AD FS e o Azure AD.
- obsoletas ou em cache as credenciais no Gerenciador de credenciais do Windows
- Algoritmo de Hash seguro que está configurado na terceira parte confiável para o Office 365 é definido como SHA1

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)