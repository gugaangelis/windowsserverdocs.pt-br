---
title: "Logoff único para OpenID se conectar com o AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Logoff único para OpenID se conectar com o AD FS

## <a name="overview"></a>Visão geral
Com base no suporte Oauth inicial no AD FS no Windows Server 2012 R2, o AD FS 2016 introduziu o suporte para se conectar OpenId logon. Com [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 agora dá suporte a única logoff para cenários OpenId se conectar. Este artigo fornece uma visão geral do logoff único cenário OpenId se conectar e fornece orientações sobre como usá-la para seus aplicativos OpenId conectar no AD FS.


## <a name="discovery-doc"></a>Documento de descoberta
OpenID Connect usa um documento JSON chamado "document descoberta" para fornecer detalhes sobre a configuração.  Isso inclui URIs da autenticação, token, userinfo e pontos de extremidade do público.  A seguir está um exemplo do documento de descoberta.

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



Os seguintes valores adicionais estará disponíveis em um documento de descoberta para indicar o suporte para frontal logoff de canal:

- frontchannel_logout_supported: valor será 'true'
- frontchannel_logout_session_supported: valor será 'true'.
- end_session_endpoint: Este é o logoff de OAuth URI que o cliente pode usar para iniciar o logoff no servidor.


## <a name="ad-fs-server-configuration"></a>Configuração do servidor do AD FS
A propriedade AD FS EnableOAuthLogout será habilitada por padrão.  Essa propriedade informa ao servidor do AD FS para navegar para a URL (LogoutURI) com o SID para iniciar o logoff no cliente. Se você não tiver [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalados, você pode usar o seguinte comando do PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` Parâmetro será marcado como obsoleto após a instalação [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` Sempre será true e não terá impacto sobre a funcionalidade de logoff.

>[!NOTE]
>Há suporte para frontchannel_logout **somente** após a instalação do [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuração do cliente
Cliente precisa implementar uma url que 'logoff' o fez logon no usuário. Administrador pode configurar o LogoutUri na configuração do cliente usando os seguintes cmdlets do PowerShell. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

O `LogoutUri`é a url usados pelo AF FS "logoff" do usuário. Para implementar o `LogoutUri`, o cliente precisa garantir que ele limpa o estado de autenticação do usuário no aplicativo, por exemplo, removendo a autenticação tokens que ele tem. AD FS irá navegar para a URL, com o SID como o parâmetro de consulta, indicando o terceiro / aplicativo para fazer logoff do usuário. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Token de OAuth com ID da sessão**: AD FS inclui a id da sessão no token do OAuth no momento da emissão de token id_token. Isso será usado posteriormente pelo AD FS para identificar os cookies SSO relevantes para limpo para o usuário.
2.  **Inicia o usuário de logoff em App1**: O usuário pode iniciar um logoff em qualquer um dos aplicativos conectados. Neste cenário de exemplo, um usuário inicia um logoff do apl1.
3.  **Aplicativo envia a solicitação de logoff para AD FS**: depois que o usuário inicia o logoff, o aplicativo envia uma solicitação GET para end_session_endpoint do AD FS. O aplicativo pode, opcionalmente, incluir id_token_hint como um parâmetro para essa solicitação. Se id_token_hint estiver presente, o AD FS usará-lo em conjunto com a ID da sessão para calcular as quais URI o cliente deve ser redirecionado para após logout (post_logout_redirect_uri).  O post_logout_redirect_uri deve ser um uri válido registrado com o AD FS usando o parâmetro RedirectUris.
4.  **AD FS envia saído para clientes conectados**: usa AD FS o valor de identificador de sessão para encontrar os clientes relevantes o usuário fizer logon no. Os clientes identificados são enviados solicitação sobre o LogoutUri registrado com o AD FS para iniciar um logoff no lado do cliente.

## <a name="faqs"></a>Perguntas frequentes
**P:** que não posso ver os parâmetros frontchannel_logout_supported e frontchannel_logout_session_supported o documento de descoberta.</br>
**R:** Verifique se você tem [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado em todos os servidores do AD FS. Consulte logoff único no servidor 2016 com [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P:** configurei logoff único conforme indicado, mas deixa de usuário conectado em outros clientes.</br>
**R:** Certifique-se de que `LogoutUri`está definida para todos os clientes onde o usuário está conectado. Além disso, o AD FS faz uma tentativa de melhor para enviar a solicitação de saída em registrado `LogoutUri`. Cliente deve implementar a lógica para lidar com a solicitação e executar uma ação de saída do usuário do aplicativo.</br>

**P:** se após logout, um dos clientes volta para o AD FS com um token de atualização válido, AD FS emitirá um token de acesso?</br>
**R:** Sim. É responsabilidade do aplicativo cliente para soltar artefatos tudo autenticados depois que uma solicitação de saída foi recebida em registrado `LogoutUri`.


## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  
