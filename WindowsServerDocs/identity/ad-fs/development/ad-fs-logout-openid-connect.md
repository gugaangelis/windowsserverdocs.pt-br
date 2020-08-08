---
title: Logoff único para OpenID Connect com o AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.openlocfilehash: 1ab6735e09d912bac5b1a319a3793ee6e0c70fa2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964933"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Logoff único para OpenID Connect com o AD FS

## <a name="overview"></a>Visão geral
Com base no suporte de OAuth inicial no AD FS no Windows Server 2012 R2, AD FS 2016 introduziu o suporte para logon do OpenId Connect. Com o [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), o AD FS 2016 agora dá suporte ao logoff único para cenários do OpenID Connect. Este artigo fornece uma visão geral do cenário de logoff único para OpenId Connect e fornece orientação sobre como usá-lo para seus aplicativos OpenId Connect no AD FS.


## <a name="discovery-doc"></a>Documento de descoberta
O OpenID Connect usa um documento JSON chamado "documento de descoberta" para fornecer detalhes sobre a configuração.  Isso inclui URIs de autenticação, token, UserInfo e pontos de extremidade públicos.  Veja a seguir um exemplo do documento de descoberta.

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



Os seguintes valores adicionais estarão disponíveis no documento de descoberta para indicar suporte para o logout do canal frontal:

- frontchannel_logout_supported: o valor será ' true '
- frontchannel_logout_session_supported: o valor será ' true '.
- end_session_endpoint: esse é o URI de logout do OAuth que o cliente pode usar para iniciar o logout no servidor.


## <a name="ad-fs-server-configuration"></a>Configuração do AD FS Server
A propriedade AD FS EnableOAuthLogout será habilitada por padrão.  Essa propriedade informa ao servidor de AD FS para procurar a URL (LogoutURI) com o SID para iniciar o logout no cliente.
Se você não tiver o [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado, poderá usar o seguinte comando do PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout`o parâmetro será marcado como obsoleto após a instalação de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout`será sempre verdadeiro e não terá impacto sobre a funcionalidade de logout.

>[!NOTE]
>**só** há suporte para frontchannel_logout após instalação VCRedist de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuração do cliente
O cliente precisa implementar uma URL que ' faz logoff ' do usuário conectado. O administrador pode configurar o LogoutUri na configuração do cliente usando os seguintes cmdlets do PowerShell.


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

O `LogoutUri` é a URL usada pelo AF FS para "fazer logoff" do usuário. Para implementar o `LogoutUri` , o cliente precisa garantir que ele limpe o estado de autenticação do usuário no aplicativo, por exemplo, descartando os tokens de autenticação que ele tem. AD FS navegará até essa URL, com o SID como o parâmetro de consulta, sinalizando a terceira parte confiável/aplicativo para fazer logoff do usuário.

![Diagrama do usuário fazer logoff do ADFS](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)

1.  **Token OAuth com ID de sessão**: AD FS inclui a ID de sessão no token OAuth no momento da emissão de token id_token. Isso será usado posteriormente por AD FS para identificar os cookies de SSO relevantes a serem limpos para o usuário.
2.  O **usuário inicia o logout no App1**: o usuário pode iniciar um logoff de qualquer um dos aplicativos conectados. Neste cenário de exemplo, um usuário inicia um logout do App1.
3.  O **aplicativo envia a solicitação de logout para AD FS**: depois que o usuário inicia o logout, o aplicativo envia uma solicitação GET para end_session_endpoint de AD FS. O aplicativo pode, opcionalmente, incluir id_token_hint como um parâmetro para essa solicitação. Se id_token_hint estiver presente, AD FS o usará em conjunto com a ID de sessão para descobrir a qual URI o cliente deve ser redirecionado após o logout (post_logout_redirect_uri).  O post_logout_redirect_uri deve ser um URI válido registrado com AD FS usando o parâmetro RedirectUris.
4.  **AD FS envia a saída para clientes conectados**: AD FS usa o valor do identificador de sessão para localizar os clientes relevantes aos quais o usuário está conectado. Os clientes identificados são solicitações enviadas no LogoutUri registrado com AD FS para iniciar um logoff no lado do cliente.

## <a name="faqs"></a>Perguntas frequentes
**P:** Não vejo os parâmetros frontchannel_logout_supported e frontchannel_logout_session_supported no documento de descoberta.</br>
**R:** Verifique se você tem o [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado em todos os servidores de AD FS. Consulte o log único no servidor 2016 com [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P:** Configurei o logoff único como direcionado, mas o usuário permanece conectado em outros clientes.</br>
**R:** Verifique se `LogoutUri` o está definido para todos os clientes em que o usuário está conectado. Além disso, AD FS faz uma tentativa melhor de enviar a solicitação de saída no registrado `LogoutUri` . O cliente deve implementar a lógica para lidar com a solicitação e tomar medidas para desconectar o usuário do aplicativo.</br>

**P:** Se, após o logout, um dos clientes voltar para AD FS com um token de atualização válido, AD FS emitirá um token de acesso?</br>
**R:** Sim. É responsabilidade do aplicativo cliente descartar todos os artefatos autenticados depois que uma solicitação de saída foi recebida no registrado `LogoutUri` .


## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)
