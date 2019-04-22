---
title: Logoff único para OpenID Connect com o AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825767"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Logoff único para OpenID Connect com o AD FS

## <a name="overview"></a>Visão geral
Aproveitando o suporte Oauth inicial no AD FS no Windows Server 2012 R2, o AD FS 2016 introduziu o suporte para logon OpenId Connect. Com o [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 agora dá suporte ao logoff único para cenários de OpenId Connect. Este artigo fornece uma visão geral de logoff único para o cenário de OpenId Connect e fornece orientação sobre como usá-lo para seus aplicativos de OpenId Connect no AD FS.


## <a name="discovery-doc"></a>Documento de descoberta
OpenID Connect usa um documento JSON chamado "documento de descoberta" para fornecer detalhes sobre a configuração.  Isso inclui os URIs da autenticação, token, userinfo e pontos de extremidade público.  O exemplo a seguir é um exemplo de como o documento de descoberta.

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



Os seguintes valores adicionais estarão disponíveis no documento de descoberta para indicar o suporte para o front-Logout de canal:

- frontchannel_logout_supported: value will be 'true'
- frontchannel_logout_session_supported: value will be 'true'.
- end_session_endpoint: esse é o URI que o cliente pode usar para iniciar o logout no servidor de logout do OAuth.


## <a name="ad-fs-server-configuration"></a>Configuração de servidor do AD FS
A propriedade do AD FS EnableOAuthLogout será habilitada por padrão.  Esta propriedade instrui o servidor do AD FS para navegar para a URL (LogoutURI) com o SID para iniciar o logout no cliente. Se você não tiver [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado, você pode usar o seguinte comando do PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` parâmetro será marcado como obsoleto depois de instalar [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` sempre será true e não terá impacto sobre a funcionalidade de logoff.

>[!NOTE]
>há suporte para a frontchannel_logout **só** após a instalação do [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuração do cliente
Cliente precisa implementar uma url que 'fizer logoff' conectado no usuário. Administrador pode configurar o LogoutUri na configuração do cliente usando os seguintes cmdlets do PowerShell. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

O `LogoutUri` é a url usada pelos AF FS "logoff" o usuário. Para implementar o `LogoutUri`, o cliente precisa para assegurar que ele limpa o estado de autenticação do usuário no aplicativo, por exemplo, removendo a autenticação de tokens que ele tem. O AD FS será navegue para a URL, com o SID como o parâmetro de consulta, a terceira a sinalização / aplicativo faça logoff do usuário. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **O token OAuth com ID de sessão**: AD FS inclui a id de sessão no token OAuth no momento da emissão de token do id_token. Isso será usado posteriormente pelo AD FS para identificar os cookies SSO relevantes sejam limpos para o usuário.
2.  **Usuário inicia o logout no App1**: O usuário pode iniciar um logoff de qualquer aplicativo conectado. Nesse cenário de exemplo, um usuário inicia um logoff do App1.
3.  **Aplicativo envia a solicitação de logoff para o AD FS**: Depois que o usuário inicia o logout, o aplicativo envia uma solicitação GET para end_session_endpoint do AD FS. O aplicativo pode incluir opcionalmente id_token_hint como um parâmetro para esta solicitação. Se id_token_hint estiver presente, o AD FS será usá-lo em conjunto com a ID de sessão para calcular out quais URI o cliente deverá ser redirecionado após logoff (post_logout_redirect_uri).  O post_logout_redirect_uri deve ser um uri válido registrado com o AD FS usando o parâmetro RedirectUris.
4.  **AD FS envia saído para os clientes registrados em**: O AD FS usa o valor do identificador de sessão para localizar os clientes relevantes que o usuário está conectado ao. Os clientes identificados são enviados a solicitação no LogoutUri registrado com o AD FS para iniciar um logoff no lado do cliente.

## <a name="faqs"></a>Perguntas frequentes
**P:** Não vejo os parâmetros frontchannel_logout_supported e frontchannel_logout_session_supported no documento de descoberta.</br>
**R:** Certifique-se de que você tenha [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado em todos os servidores do AD FS. Consulte o logoff único no Server 2016 com [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P:** Configurei o logoff único conforme indicado, mas usuário permanece conectado em outros clientes.</br>
**R:** Certifique-se de que `LogoutUri` está definido para todos os clientes onde o usuário está conectado. Além disso, o AD FS faz uma tentativa de melhor caso para enviar a solicitação de saída em registrado `LogoutUri`. Cliente deve implementar a lógica para manipular a solicitação e tomar medidas para desconectar o usuário do aplicativo.</br>

**P:** Se depois de logoff, um dos clientes voltar para o AD FS com um token de atualização válido, o AD FS emitirá um token de acesso?</br>
**R:** Sim. É responsabilidade do aplicativo cliente para descartar autenticados todos os artefatos depois que uma solicitação de saída foi recebida no registrado `LogoutUri`.


## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
