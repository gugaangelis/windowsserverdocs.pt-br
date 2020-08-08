---
title: Solução de problemas AD FS-Azure AD
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS e do Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.openlocfilehash: d7941733ff2191e94c6c1e380d4349585a5c98d3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956173"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Solução de problemas AD FS-Azure AD
Com o crescimento da nuvem, muitas empresas passaram a migrar para usar o Azure AD para seus vários aplicativos e serviços.  A Federação com o Azure AD se tornou uma prática padrão com muitas organizações.  Este documento abordará alguns dos aspectos da solução de problemas que surgem com essa Federação.  Vários tópicos do documento geral de solução de problemas ainda pertencem à Federação com o Azure para que este documento se concentre em apenas especificações com o Azure AD e a interação AD FS.

## <a name="redirection-to-ad-fs"></a>Redirecionamento para AD FS

O redirecionamento ocorre quando você entra em um aplicativo, como o Office 365, e você é "Redirecionado" para suas organizações AD FS servidores para entrar.

![Tela de redirecionamento para AD FS](media/ad-fs-tshoot-azure/azure1.png)

### <a name="first-things-to-check"></a>Primeiras coisas a serem verificadas

Se o redirecionamento não estiver ocorrendo, há algumas coisas que você deseja verificar

1. Verifique se o seu locatário do Azure AD está habilitado para Federação entrando no portal do Azure e verificando em Azure AD Connect.

   ![Tela de entrada do usuário no Azure AD Connect](media/ad-fs-tshoot-azure/azure2.png)

2. Certifique-se de que seu domínio personalizado seja verificado clicando no domínio ao lado da Federação no portal do Azure.

   ![Domínio mostrado ao lado da Federação no portal](media/ad-fs-tshoot-azure/azure3.png)

3. Por fim, você deseja verificar o [DNS](ad-fs-tshoot-dns.md) e certificar-se de que seus servidores AD FS ou servidores WAP estejam resolvendo da Internet.  Verifique se isso resolve e se você pode navegar até ele.

4. Você também pode usar o cmdlt do PowerShell `Get-AzureADDomain` para obter essas informações também.

   ![Tela de cmdlet do PowerShell](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Você está recebendo um erro de método de autenticação desconhecido
Você pode encontrar um erro de "método de autenticação desconhecido" informando que AuthnContext não tem suporte no nível AD FS ou STS quando você é redirecionado do Azure.

Isso é mais comum quando o Azure AD redireciona para o AD FS ou STS usando um parâmetro que impõe um método de autenticação.

Para impor um método de autenticação, use um dos seguintes métodos:
- Para o WS-Federation, use uma cadeia de caracteres de consulta WAUTH para forçar um método de autenticação preferencial.

- Para o SAML 2.0, use o seguinte:
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Quando o método de autenticação imposto for enviado com um valor incorreto ou se esse método de autenticação não tiver suporte no AD FS ou STS, você receberá uma mensagem de erro antes de ser autenticado.

|Método de autenticação desejado|URI de wauth|
|-----|-----|
|Autenticação por nome de usuário e senha|urn: Oasis: names: TC: SAML: 1.0: am: Password|
|Autenticação de cliente SSL|urn: IETF: RFC: 2246|
|Autenticação integrada do Windows|urn: Federação: autenticação: Windows|

Classes de contexto de autenticação SAML com suporte

|Método de autenticação|URI de classe de contexto de autenticação|
|-----|-----|
|Nome e senha do usuário|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|Transporte protegido por senha|urn: Oasis: names: TC: SAML: 2.0: AC: classes: PasswordProtectedTransport|
|Cliente de segurança de camada de transporte (TLS)|urn: Oasis: names: TC: SAML: 2.0: AC: classes: TLSClient
|Certificado X.509|urn: Oasis: names: TC: SAML: 2.0: AC: classes: X509
|Autenticação integrada do Windows|urn: Federação: autenticação: Windows|
|Kerberos|urn: Oasis: names: TC: SAML: 2.0: AC: classes: Kerberos|

Para certificar-se de que o método de autenticação tem suporte no nível de AD FS, verifique o seguinte.

#### <a name="ad-fs-20"></a>AD FS 2.0

Em **/adfs/ls/web.config**, certifique-se de que a entrada para o tipo de autenticação esteja presente.

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

Em **Gerenciamento de AD FS**, clique em **políticas de autenticação** no snap-in de AD FS.

Na seção **autenticação primária** , clique em Editar ao lado de configurações globais. Você também pode clicar com o botão direito do mouse em políticas de autenticação e selecionar Editar Autenticação global primária. Ou, no painel Ações, selecione Editar Autenticação global primária.

Na janela Editar política de autenticação global, na guia primário, você pode definir as configurações como parte da política de autenticação global. Por exemplo, para autenticação primária, você pode selecionar os métodos de autenticação disponíveis em extranet e intranet.

* * Verifique se a caixa de seleção método de autenticação necessário está marcada.

#### <a name="ad-fs-2016"></a>AD FS 2016

Em **Gerenciamento de AD FS**, clique em **métodos de autenticação** e **serviço** no snap-in de AD FS.

Na seção **autenticação primária** , clique em Editar.

Na janela **Editar métodos de autenticação** , na guia primário, você pode definir as configurações como parte da política de autenticação.

![Janela Editar métodos de autenticação](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Tokens emitidos por AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>O Azure AD gera um erro após a emissão do token
Depois que AD FS emitir um token, o Azure AD poderá gerar um erro. Nessa situação, verifique os seguintes problemas:
- As declarações emitidas por AD FS no token devem corresponder aos respectivos atributos do usuário no Azure AD.
- o token para o Azure AD deve conter as seguintes declarações necessárias:
    - WSFED
        - UPN: o valor dessa declaração deve corresponder ao UPN dos usuários no Azure AD.
        - Imutávelid: o valor dessa declaração deve corresponder ao sourceAnchor ou à imutávelid do usuário no Azure AD.

Para obter o valor de atributo de usuário no Azure AD, execute a seguinte linha de comando:`Get-AzureADUser –UserPrincipalName <UPN>`

![Tela de cmdlet do PowerShell](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2,0:
       - Idpemail.: o valor dessa declaração deve corresponder ao nome principal do usuário dos usuários no Azure AD.
       - NAMEid: o valor dessa declaração deve corresponder ao sourceAnchor ou à imutávelid do usuário no Azure AD.

Para obter mais informações, consulte [usar um provedor de identidade SAML 2,0 para implementar o logon único](/previous-versions/azure/azure-services/dn641269(v=azure.100)).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Incompatibilidade de certificado de assinatura de token entre AD FS e o Azure AD.

AD FS usa o certificado de autenticação de tokens para assinar o token que é enviado para o usuário ou aplicativo. A relação de confiança entre o AD FS e o Azure AD é uma relação de confiança federada baseada nesse certificado de autenticação de tokens.

No entanto, se o certificado de autenticação de tokens no lado do AD FS for alterado devido à substituição automática de certificado ou por alguma intervenção, os detalhes do novo certificado deverão ser atualizados no lado do Azure AD para o domínio federado. Quando o certificado de autenticação de tokens primário no AD FS for diferente dos anúncios do Azure, o token emitido pelo AD FS não será confiável pelo Azure AD. Portanto, o usuário federado não tem permissão para fazer logon.

Para corrigir isso, você pode usar a estrutura de tópicos de etapas em [renovar certificados de Federação para o Office 365 e Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Outras coisas comuns a serem verificadas
Veja a seguir uma lista rápida de itens para verificar se você está tendo problemas com AD FS e a interação do Azure AD.
- credenciais obsoletas ou armazenadas em cache no Gerenciador de credenciais do Windows
- O algoritmo de hash seguro configurado no confiável da terceira parte confiável para o Office 365 é definido como SHA1

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
