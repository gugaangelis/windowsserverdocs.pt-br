---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS os fluxos do OpenID Connect/OAuth e cenários de aplicativos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e1e0235e50945fadd09fe9dd5ffeaf6d7119e482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385596"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS os fluxos do OpenID Connect/OAuth e cenários de aplicativos
Aplica-se ao AD FS 2016 e posterior


|Cenário|Explicação do cenário usando exemplos|Fluxo/concessão do OAuth 2,0|Tipo de cliente|
|-----|-----|-----|-----|
|Aplicativo de página única</br> | &bull;[Exemplo usando a Adal](../development/Single-Page-Application-with-AD-FS.md)|[Localiza](#implicit-grant-flow)|Public| 
|Aplicativo Web que assina usuários</br> | &bull;[Exemplo usando OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Código de autorização](#authorization-code-grant-flow)|Público, confidencial|  
|API Web de chamadas de aplicativo nativo</br>|&bull;[Exemplo usando MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[Exemplo usando a Adal](../development/native-client-with-ad-fs.md)|[Código de autorização](#authorization-code-grant-flow)|Public|   
|API Web de chamadas de aplicativo Web</br>|&bull;[Exemplo usando MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[Exemplo usando a Adal](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Código de autorização](#authorization-code-grant-flow)|Confidential| 
|A API Web chama outra API da Web em nome de (OBO) o usuário</br>|&bull;[Exemplo usando MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[Exemplo usando a Adal](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[On-behalf-of](#on-behalf-of-flow)|O aplicativo Web atua como confidencial| 
|API Web de chamadas de aplicativo de daemon||[Credenciais do cliente](#client-credentials-grant-flow)|Confidential| 
|O aplicativo Web chama a API Web usando credenciais do usuário||[Credenciais de senha do proprietário do recurso](#resource-owner-password-credentials-grant-flow-not-recommended)|Público, confidencial| 
|API Web de chamadas de aplicativo não-navegador||[Código do dispositivo](#device-code-flow)|Público, confidencial| 

## <a name="implicit-grant-flow"></a>Fluxo de concessão implícita 
 
Para aplicativos de página única (AngularJS, Ember. js, reajam. js e assim por diante), AD FS dá suporte ao fluxo de concessão implícita do OAuth 2,0. O fluxo implícito é descrito na [especificação do OAuth 2,0](https://tools.ietf.org/html/rfc6749#section-4.2). Seu principal benefício é que ele permite que o aplicativo obtenha tokens de AD FS sem executar uma troca de credenciais de servidor de back-end. Isso permite que o aplicativo entre no usuário, mantenha a sessão e obtenha tokens para outras APIs Web no código JavaScript do cliente. Há algumas considerações de segurança importantes a serem levadas em conta ao usar o fluxo implícito especificamente em volta do [cliente](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Se você quiser usar o fluxo implícito e AD FS para adicionar autenticação ao seu aplicativo JavaScript, siga as etapas gerais abaixo.  
  
### <a name="protocol-diagram"></a>Diagrama de protocolo

O diagrama a seguir mostra a aparência de todo o fluxo de entrada implícito e as seções a seguir descrevem cada etapa mais detalhadamente.  

![Entrada implícita](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Token de ID de solicitação e token de acesso 
 
Para conectar inicialmente o usuário em seu aplicativo, você pode enviar uma solicitação de autenticação do OpenID Connect e obter id_token e token de acesso do ponto de extremidade AD FS.  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|Parâmetro|Obrigatório/Opcional|Descrição| 
|-----|-----|-----|
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.| 
|response_type|necessárias|Deve incluir `id_token` para entrar no OpenID Connect. Ele também pode incluir o response_type @ no__t-0. O uso de token aqui permitirá que seu aplicativo receba um token de acesso imediatamente do ponto de extremidade de autorização sem precisar fazer uma segunda solicitação para o ponto de extremidade do token.| 
|redirect_uri|necessárias|O redirect_uri do seu aplicativo, em que as respostas de autenticação podem ser enviadas e recebidas pelo seu aplicativo. Ele deve corresponder exatamente a um dos redirect_uris que você configurou em AD FS.| 
|momentos|necessárias|Um valor incluído na solicitação, gerado pelo aplicativo, que será incluído no id_token resultante como uma declaração. O aplicativo pode, então, verificar esse valor para atenuar os ataques de reprodução de token. O valor normalmente é uma cadeia de caracteres aleatória e exclusiva que pode ser usada para identificar a origem da solicitação. Necessário somente quando um id_token é solicitado.|
|scope|opcionais|Uma lista de escopos separados por espaços. Para o OpenID Connect, ele deve incluir o `openid`escopo.|
|resource|opcionais|A URL da sua API Web.</br>Observação – Se estiver usando a biblioteca de cliente do MSAL, o parâmetro de recurso não será enviado. Em vez disso, a URL do recurso é enviada como parte do parâmetro de escopo:`scope = [resource url]//[scope values e.g., openid]`</br>Se o recurso não for passado aqui ou no escopo, o ADFS usará um recurso padrão urn: Microsoft: UserInfo. as políticas de recurso UserInfo, como MFA, política de emissão ou autorização, não podem ser personalizadas.| 
|response_mode|opcionais| Especifica o método que deve ser usado para enviar o token resultante de volta ao seu aplicativo. Assume o padrão de `fragment`.| 
|state|opcionais|Um valor incluído na solicitação que também será retornado na resposta do token. Pode ser uma cadeia de caracteres de qualquer conteúdo que você desejar. Um valor exclusivo gerado aleatoriamente geralmente é usado para impedir ataques de solicitação entre sites forjado. O estado também é usado para codificar informações sobre o estado do usuário no aplicativo antes que a solicitação de autenticação ocorra, como a página ou a exibição em que eles estavam.| 
|prompt|opcionais|Indica o tipo de interação do usuário que é necessário. Os únicos valores válidos no momento são logon e nenhum.</br>- `prompt=login` forçará o usuário a inserir suas credenciais nessa solicitação, negando o logon único. </br>- `prompt=none` é o oposto: ele garantirá que o usuário não seja apresentado a nenhum prompt interativo. Se a solicitação não puder ser concluída silenciosamente por meio de logon único, AD FS retornará um erro interaction_required.| 
|login_hint|opcionais|Pode ser usado para preencher previamente o campo nome de usuário/endereço de email da página de entrada do usuário, se você souber seu nome de usuário antes do tempo. Com frequência, os aplicativos usarão esse parâmetro durante a reautenticação, já tendo extraído o nome de usuário de uma entrada `upn`anterior usando `id_token`a declaração de.| 
|domain_hint|opcionais|Se for incluído, ele ignorará o processo de descoberta baseado em domínio que o usuário passa na página de entrada, levando a uma experiência de usuário um pouco mais simplificada.| 

Neste ponto, o usuário será solicitado a inserir suas credenciais e concluir a autenticação. Depois que o usuário for autenticado, o ponto de extremidade de autorização de AD FS retornará uma resposta ao seu aplicativo no redirect_uri indicado, usando o método especificado no parâmetro response_mode.  
 
### <a name="successful-response"></a>Resposta bem-sucedida 
 
Uma resposta bem-sucedida usando `response_mode=fragment and response_type=id_token+token` é semelhante ao seguinte  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|Parâmetro|Descrição| 
|-----|-----|
|access_token|Incluído se response_type incluir @ no__t-0.|
|token_type|Incluído se response_type incluir @ no__t-0. Sempre será portador.| 
|expires_in| Incluído se response_type incluir @ no__t-0. Indica o número de segundos que o token é válido, para fins de cache.| 
|scope| Indica os escopos para os quais o access_token será válido.|  
|id_token|Incluído se response_type incluir @ no__t-0. Um JWT (token Web JSON) assinado. O aplicativo pode decodificar os segmentos desse token para solicitar informações sobre o usuário que se conectou. O aplicativo pode armazenar em cache os valores e exibi-los, mas não deve depender deles para qualquer autorização ou limites de segurança.| 
|state|Se um parâmetro de estado for incluído na solicitação, o mesmo valor deverá aparecer na resposta. O aplicativo deve verificar se os valores de estado na solicitação e na resposta são idênticos.|

### <a name="refresh-tokens"></a>Tokens de atualização 
A concessão implícita não fornece tokens de atualização.  `id_tokens` E `access_tokens` expirarão após um curto período de tempo, de modo que seu aplicativo deve estar preparado para atualizar esses tokens periodicamente. Para atualizar qualquer tipo de token, você pode executar a mesma solicitação de iframe oculto acima usando o `prompt=none` parâmetro para controlar o comportamento da plataforma de identidade. Se você quiser receber um `new id_token`, certifique-se de usar. `response_type=id_token` 

## <a name="authorization-code-grant-flow"></a>Fluxo de concessão de código de autorização 
 
A concessão de código de autorização OAuth 2,0 pode ser usada em aplicativos Web para obter acesso a recursos protegidos, como APIs Web. O fluxo de código de autorização do OAuth 2,0 é descrito na [seção 4,1 da especificação do OAuth 2,0](https://tools.ietf.org/html/rfc6749). Ele é usado para executar autenticação e autorização na maioria dos tipos de aplicativos, incluindo aplicativos Web e aplicativos instalados nativamente. O Flow permite que os aplicativos adquiram com segurança access_tokens que podem ser usados para acessar recursos que confiam AD FS.  
 
### <a name="protocol-diagram"></a>Diagrama de protocolo 
 
Em um alto nível, o fluxo de autenticação para um aplicativo nativo parece um pouco assim:

![Fluxo de concessão de código de autorização](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Solicitar um código de autorização 
 
O fluxo do código de autorização começa com o cliente direcionando o usuário para o ponto de extremidade/Authorize. Nessa solicitação, o cliente indica as permissões que precisa adquirir do usuário: 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|Parâmetro|Obrigatório/Opcional|Descrição|
|-----|-----|-----| 
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.|  
|response_type|necessárias| Deve incluir código para o fluxo do código de autorização.| 
|redirect_uri|necessárias|O `redirect_uri` do seu aplicativo, onde as respostas de autenticação podem ser enviadas e recebidas pelo seu aplicativo. Ele deve corresponder exatamente a um dos redirect_uris que você registrou no AD FS para o cliente.|  
|resource|opcionais|A URL da sua API Web.</br>Observação – Se estiver usando a biblioteca de cliente do MSAL, o parâmetro de recurso não será enviado. Em vez disso, a URL do recurso é enviada como parte do parâmetro de escopo:`scope = [resource url]//[scope values e.g., openid]`</br>Se o recurso não for passado aqui ou no escopo, o ADFS usará um recurso padrão urn: Microsoft: UserInfo. as políticas de recurso UserInfo, como MFA, política de emissão ou autorização, não podem ser personalizadas.| 
|scope|opcionais|Uma lista de escopos separados por espaços.|
|response_mode|opcionais|Especifica o método que deve ser usado para enviar o token resultante de volta ao seu aplicativo. Pode ser um dos seguintes: </br>-consulta </br>-fragmento </br>- form_post</br>`query` fornece o código como um parâmetro de cadeia de caracteres de consulta em seu URI de redirecionamento. Se você estiver solicitando o código, poderá usar Query, Fragment ou form_post.  `form_post` @ no__t-1executes uma POSTAgem contendo o código para o URI de redirecionamento.|
|state|opcionais|Um valor incluído na solicitação que também será retornado na resposta do token. Pode ser uma cadeia de caracteres de qualquer conteúdo que você desejar. Um valor exclusivo gerado aleatoriamente geralmente é usado para impedir ataques de solicitação entre sites forjado. O valor também pode codificar informações sobre o estado do usuário no aplicativo antes que a solicitação de autenticação ocorra, como a página ou a exibição em que eles estavam.|
|prompt|opcionais|Indica o tipo de interação do usuário que é necessário. Os únicos valores válidos no momento são logon e nenhum.</br>- `prompt=login` forçará o usuário a inserir suas credenciais nessa solicitação, negando o logon único. </br>- `prompt=none` é o oposto: ele garantirá que o usuário não seja apresentado a nenhum prompt interativo. Se a solicitação não puder ser concluída silenciosamente por meio de logon único, AD FS retornará um erro interaction_required.|
|login_hint|opcionais|Pode ser usado para preencher previamente o campo nome de usuário/endereço de email da página de entrada do usuário, se você souber seu nome de usuário antes do tempo. Com frequência, os aplicativos usarão esse parâmetro durante a reautenticação, já tendo extraído o nome de usuário de uma entrada `upn`anterior usando `id_token`a declaração de.|
|domain_hint|opcionais|Se for incluído, ele ignorará o processo de descoberta baseado em domínio que o usuário passa na página de entrada, levando a uma experiência de usuário um pouco mais simplificada.|
|code_challenge_method|opcionais|O método usado para codificar o code_verifier para o parâmetro code_challenge. Pode ser um dos seguintes valores: </br>-Plain </br>- S256 </br>Se for excluído, o code_challenge será considerado em texto sem formatação se @ no__t-0 @ no__t-1is for incluído. AD FS dá suporte a Plain e S256. Para obter mais informações, consulte a [RFC PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|opcionais| Usado para proteger as concessões de código de autorização por meio da chave de prova de troca de código (PKCE) de um cliente nativo. Necessário se `code_challenge_method` estiver incluído. Para obter mais informações, consulte a [RFC do PKCE](https://tools.ietf.org/html/rfc7636)|

Neste ponto, o usuário será solicitado a inserir suas credenciais e concluir a autenticação. Depois que o usuário for autenticado, o AD FS retornará uma resposta ao seu aplicativo no indicado `redirect_uri`, usando o método especificado `response_mode` no parâmetro.  
 
### <a name="successful-response"></a>Resposta bem-sucedida 
 
Uma resposta bem-sucedida usando response_mode = Query é semelhante a: 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Parâmetro|Descrição|
|-----|-----|
|code|O `authorization_code` que o aplicativo solicitou. O aplicativo pode usar o código de autorização para solicitar um token de acesso para o recurso de destino. Authorization_codes são de curta duração, normalmente elas expiram após cerca de 10 minutos.|
|state|Se um `state` parâmetro for incluído na solicitação, o mesmo valor deverá aparecer na resposta. O aplicativo deve verificar se os valores de estado na solicitação e na resposta são idênticos.|

### <a name="request-an-access-token"></a>Solicitar um token de acesso 
 
Agora que você adquiriu um `authorization_code` e recebeu permissão pelo usuário, pode resgatar o código de um `access_token` para o recurso desejado. Faça isso enviando uma solicitação POST para o ponto de extremidade/token:  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|Parâmetro|Obrigatório/opcional|Descrição|
|-----|-----|-----| 
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.| 
|grant_type|necessárias|Deve ser `authorization_code` para o fluxo do código de autorização.| 
|code|necessárias|O `authorization_code` que você adquiriu no primeiro segmento do fluxo.| 
|redirect_uri|necessárias|O mesmo `redirect_uri` valor que foi usado para adquirir o `authorization_code`.| 
|client_secret|necessário para aplicativos Web|O segredo do aplicativo que você criou durante o registro do aplicativo no AD FS. Você não deve usar o segredo do aplicativo em um aplicativo nativo porque o client_secrets não pode ser armazenado de forma confiável em dispositivos. Ele é necessário para aplicativos Web e APIs Web, que têm a capacidade de armazenar o client_secret com segurança no lado do servidor. O segredo do cliente deve ser codificado por URL antes de ser enviado. Esses aplicativos também podem usar uma autenticação baseada em chave assinando um JWT e adicionando isso como parâmetro client_assertion.| 
|code_verifier|opcionais|O mesmo `code_verifier` que foi usado para obter o authorization_code. Obrigatório se PKCE foi usado na solicitação de concessão de código de autorização. Para obter mais informações, consulte a [RFC PKCE](https://tools.ietf.org/html/rfc7636).</br>Observação – aplica-se ao AD FS 2019 e posterior| 

### <a name="successful-response"></a>Resposta bem-sucedida 
 
Uma resposta de token bem-sucedida terá A seguinte aparência: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|Parâmetro|Descrição| 
|-----|-----|
|access_token|O token de acesso solicitado. O aplicativo pode usar esse token para se autenticar no recurso protegido (API Web).| 
|token_type|Indica o valor do tipo de token. O único tipo ao qual AD FS dá suporte é portador.
|expires_in|Por quanto tempo o token de acesso é válido (em segundos).
|refresh_token|Um token de atualização OAuth 2,0. O aplicativo pode usar esse token para adquirir tokens de acesso adicionais depois que o token de acesso atual expirar. Refresh_tokens são de vida longa e podem ser usados para manter o acesso a recursos por longos períodos de tempo.| 
|refresh_token_expires_in|Por quanto tempo o token de atualização é válido (em segundos).| 
|id_token|Um JWT (token Web JSON). O aplicativo pode decodificar os segmentos desse token para solicitar informações sobre o usuário que se conectou. O aplicativo pode armazenar em cache os valores e exibi-los, mas não deve confiar neles para qualquer autorização ou limites de segurança.|

### <a name="use-the-access-token"></a>Usar o token de acesso 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Atualizar o token de acesso 
 
Access_tokens são de curta duração e você deve atualizá-los depois que eles expirarem para continuar acessando os recursos. Você pode fazer isso enviando outra solicitação POST para o @ no__t-0 @ no__t-1endpoint, desta vez fornecendo o refresh_token em vez do código. Os tokens de atualização são válidos para todas as permissões para as quais o cliente já recebeu o token de acesso. 
 
Os tokens de atualização não têm tempos de vida especificados. Normalmente, os tempos de vida de tokens de atualização são relativamente longos. No entanto, em alguns casos, os tokens de atualização expiram, são revogados ou não têm privilégios suficientes para a ação desejada. Seu aplicativo precisa esperar e tratar os erros retornados pelo ponto de extremidade de emissão de token corretamente.  
 
Embora os tokens de atualização não sejam revogados quando usados para adquirir novos tokens de acesso, você deve descartar o token de atualização antigo. A especificação do OAuth 2,0 diz: "O servidor de autorização pode emitir um novo token de atualização; nesse caso, o cliente deve descartar o token de atualização antigo e substituí-lo pelo novo token de atualização. O servidor de autorização pode revogar o token de atualização antigo depois de emitir um novo token de atualização para o cliente. " 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|Parâmetro|Obrigatório/Opcional|Descrição| 
|-----|-----|-----|
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.| 
|grant_type|necessárias|Deve ser `refresh_token` para esse trecho do fluxo do código de autorização.| 
|resource|opcionais|A URL da sua API Web.</br>Observação – Se estiver usando a biblioteca de cliente do MSAL, o parâmetro de recurso não será enviado. Em vez disso, a URL do recurso é enviada como parte do parâmetro de escopo:`scope = [resource url]//[scope values e.g., openid]`</br>Se o recurso não for passado aqui ou no escopo, o ADFS usará um recurso padrão urn: Microsoft: UserInfo. as políticas de recurso UserInfo, como MFA, política de emissão ou autorização, não podem ser personalizadas.|
|scope|opcionais|Uma lista de escopos separados por espaços.| 
|refresh_token|necessárias|O refresh_token que você adquiriu no segundo segmento do fluxo.| 
|client_secret|necessário para aplicativos Web| O segredo do aplicativo que você criou no portal de registro de aplicativo para seu aplicativo. Ele não deve ser usado em um aplicativo nativo, pois o client_secrets não pode ser armazenado de forma confiável em dispositivos. Ele é necessário para aplicativos Web e APIs Web, que têm a capacidade de armazenar o client_secret com segurança no lado do servidor. Esses aplicativos também podem usar uma autenticação baseada em chave assinando um JWT e adicionando isso como parâmetro client_assertion.|

### <a name="successful-response"></a>Resposta bem-sucedida 
Uma resposta de token bem-sucedida terá A seguinte aparência: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|Parâmetro|Descrição| 
|-----|-----|
|access_token|O token de acesso solicitado. O aplicativo pode usar esse token para se autenticar no recurso protegido, como uma API da Web.| 
|token_type|Indica o valor do tipo de token. O único tipo ao qual AD FS dá suporte é portador|
|expires_in|Por quanto tempo o token de acesso é válido (em segundos).|
|scope|Os escopos para os quais o access_token é válido.| 
|refresh_token|Um token de atualização OAuth 2,0. O aplicativo pode usar esse token para adquirir tokens de acesso adicionais depois que o token de acesso atual expirar. Refresh_tokens são de vida longa e podem ser usados para manter o acesso a recursos por longos períodos de tempo.| 
|refresh_token_expires_in|Por quanto tempo o token de atualização é válido (em segundos).| 
|id_token|Um JWT (token Web JSON). O aplicativo pode decodificar os segmentos desse token para solicitar informações sobre o usuário que se conectou. O aplicativo pode armazenar em cache os valores e exibi-los, mas não deve confiar neles para qualquer autorização ou limites de segurança.|

## <a name="on-behalf-of-flow"></a>Fluxo em nome de 
 
O fluxo em nome de do OAuth 2,0 (OBO) atende ao caso de uso em que um aplicativo invoca uma API de serviço/Web, que, por sua vez, precisa chamar outra API de serviço/Web. A ideia é propagar a identidade do usuário delegado e as permissões por meio da cadeia de solicitação. Para que o serviço de camada intermediária faça solicitações autenticadas para o serviço downstream, ele precisa proteger um token de acesso do AD FS em nome do usuário.  
 
### <a name="protocol-diagram"></a>Diagrama de protocolo 
Suponha que o usuário tenha sido autenticado em um aplicativo usando o fluxo de concessão de código de autorização OAuth 2,0 descrito acima. Neste ponto, o aplicativo tem um token de acesso para a API A (token A) com as declarações do usuário e o consentimento para acessar a API da Web de camada intermediária (API A). Verifique se o cliente solicita o escopo user_impersonation no token. Agora, a API A precisa fazer uma solicitação autenticada para a API da Web downstream (API B). 

As etapas a seguir constituem o fluxo OBO e são explicadas com a ajuda do diagrama a seguir. 

![Fluxo em nome de](media/adfs-scenarios-for-developers/obo.png)

  1. O aplicativo cliente faz uma solicitação para a API A com o token A.  
  Observação: Ao configurar o fluxo obo no AD FS certifique- `user_impersonation` se de que o escopo está `user_impersonation` selecionado e o escopo de solicitação do cliente na solicitação. 
  2. A API A é autenticada no ponto de extremidade de emissão de token AD FS e solicita um token para acessar a API B. Observação: Ao configurar esse fluxo no AD FS Verifique se a API A também está registrada como um aplicativo de servidor com clientID com o mesmo valor que a ID de recurso na API A. Para obter mais detalhes, consulte em nome do exemplo adicionar link.  
  3. O ponto de extremidade de emissão de token AD FS valida as credenciais da API A com o token A e emite o token de acesso para a API B (token B). 
  4. O token B é definido no cabeçalho Authorization da solicitação para a API B. 
  5. Os dados do recurso protegido são retornados pela API B. 

### <a name="service-to-service-access-token-request"></a>Solicitação de token de acesso de serviço a serviço 
 
Para solicitar um token de acesso, faça um HTTP POST para o ponto de extremidade do token AD FS com os parâmetros a seguir.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primeiro caso: Solicitação de token de acesso com um segredo compartilhado 
 
Ao usar um segredo compartilhado, uma solicitação de token de acesso de serviço a serviço contém os seguintes parâmetros: 


|Parâmetro|Obrigatório/Opcional|Descrição|
|-----|-----|-----| 
|grant_type|necessárias|O tipo de solicitação de token. Para uma solicitação usando um JWT, o valor deve ser urn: IETF: params: OAuth: Grant-Type: JWT-portador.|  
|client_id|necessárias|A ID do cliente que você configura ao registrar sua primeira API Web como um aplicativo de servidor (aplicativo de camada intermediária). Isso deve ser o mesmo que a ID do recurso usada no primeiro segmento, ou seja, a URL da primeira API da Web.| 
|client_secret|necessárias|O segredo do aplicativo que você criou durante o registro de aplicativo do servidor no AD FS.| 
|assertion|necessárias|O valor do token usado na solicitação.|  
|requested_token_use|necessárias|Especifica como a solicitação deve ser processada. No fluxo OBO, o valor deve ser definido como on_behalf_of| 
|resource|necessárias|A ID de recurso fornecida ao registrar a primeira API Web como o aplicativo de servidor (aplicativo de camada intermediária). A ID do recurso deve ser a URL do segundo aplicativo de camada intermediária da API Web chamará em nome do cliente.|
|scope|opcionais|Uma lista de escopos separados por espaço para a solicitação de token.| 

#### <a name="example"></a>Exemplo 
 
O seguinte `HTTP POST` solicita um token de acesso e um token de atualização 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: Solicitação de token de acesso com um certificado 
 
Uma solicitação de token de acesso de serviço a serviço com um certificado contém os seguintes parâmetros: 


|Parâmetro|obrigatório/opcional|Descrição|
|-----|-----|-----| 
|grant_type|necessárias|O tipo de solicitação de token. Para uma solicitação usando um JWT, o valor deve ser urn: IETF: params: OAuth: Grant-Type: JWT-portador. |
|client_id|necessárias|A ID do cliente que você configura ao registrar sua primeira API Web como um aplicativo de servidor (aplicativo de camada intermediária). Isso deve ser o mesmo que a ID do recurso usada no primeiro segmento, ou seja, a URL da primeira API da Web.|  
|client_assertion_type|necessárias|O valor deve ser urn: IETF: params: OAuth: cliente-Assertion-Type: JWT-portador.| 
|client_assertion|necessárias|Uma asserção (um token Web JSON) que você precisa para criar e assinar com o certificado que você registrou como credenciais para seu aplicativo.|  
|assertion|necessárias|O valor do token usado na solicitação.| 
|requested_token_use|necessárias|Especifica como a solicitação deve ser processada. No fluxo OBO, o valor deve ser definido como on_behalf_of| 
|resource|necessárias|A ID de recurso fornecida ao registrar a primeira API Web como o aplicativo de servidor (aplicativo de camada intermediária). A ID do recurso deve ser a URL do segundo aplicativo de camada intermediária da API Web chamará em nome do cliente.|
|scope|opcionais|Uma lista de escopos separados por espaço para a solicitação de token.|


Observe que os parâmetros são quase iguais aos do caso da solicitação por segredo compartilhado, exceto pelo fato de que o parâmetro client_secret é substituído por dois parâmetros: client_assertion_type e client_assertion. 

#### <a name="example"></a>Exemplo 
O HTTP POST a seguir solicita um token de acesso para a API Web com um certificado.

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>Resposta de token de acesso de serviço a serviço 
 
Uma resposta de êxito é uma resposta JSON OAuth 2,0 com os parâmetros a seguir. 


|Parâmetro|Descrição|
|-----|-----| 
|token_type|Indica o valor do tipo de token. O único tipo ao qual AD FS dá suporte é portador. | 
|scope|O escopo de acesso concedido no token.| 
|expires_in|O período de tempo, em segundos, que o token de acesso é válido.| 
|access_token|O token de acesso solicitado. O serviço de chamada pode usar esse token para se autenticar no serviço de recebimento.| 
|id_token|Um JWT (token Web JSON). O aplicativo pode decodificar os segmentos desse token para solicitar informações sobre o usuário que se conectou. O aplicativo pode armazenar em cache os valores e exibi-los, mas não deve confiar neles para qualquer autorização ou limites de segurança.| 
|refresh_token|O token de atualização para o token de acesso solicitado. O serviço de chamada pode usar esse token para solicitar outro token de acesso depois que o token de acesso atual expirar.|
|Refresh_token_expires_in|O período de tempo, em segundos, que o token de atualização é válido. 

### <a name="success-response-example"></a>Exemplo de resposta de êxito 
 
O exemplo a seguir mostra uma resposta de êxito a uma solicitação de um token de acesso para a API da Web. 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
Use o token de acesso para acessar o recurso protegido agora o serviço de camada intermediária pode usar o token adquirido acima para fazer solicitações autenticadas para a API da Web downstream, definindo o token no cabeçalho Authorization.  

#### <a name="example"></a>Exemplo 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Fluxo de concessão de credenciais de cliente 
 
Você pode usar a concessão de credenciais de cliente OAuth 2,0 especificada na [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4)para acessar recursos hospedados na Web usando a identidade de um aplicativo. Esse tipo de concessão é comumente usado para interações de servidor para servidor que devem ser executadas em segundo plano, sem interação imediata com um usuário. Esses tipos de aplicativos são geralmente chamados de daemons ou contas de serviço. 

O fluxo de concessão de credenciais de cliente do OAuth 2,0 permite que um serviço Web (cliente confidencial) Use suas próprias credenciais, em vez de representar um usuário, para autenticar ao chamar outro serviço Web. Nesse cenário, o cliente é normalmente um serviço Web de camada intermediária, um serviço de daemon ou um site da Web. Para um nível mais alto de garantia, o AD FS também permite que o serviço de chamada use um certificado (em vez de um segredo compartilhado) como uma credencial. 

### <a name="protocol-diagram"></a>Diagrama de protocolo 

O diagrama a seguir mostra o fluxo de concessão de credenciais de cliente. 

![Credenciais do cliente](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Solicitar um token 
 
Para obter um token usando a concessão de credenciais de cliente, envie `POST` uma solicitação para o ponto de extremidade de AD FS/token:  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primeiro caso: Solicitação de token de acesso com um segredo compartilhado 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|Parâmetro|Obrigatório/Opcional|Descrição|
|-----|-----|-----| 
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.| 
|scope|opcionais|Uma lista separada por espaços de escopos aos quais você deseja que o usuário concorde.| 
|client_secret|necessárias|O segredo do cliente que você gerou para seu aplicativo no portal de registro de aplicativo. O segredo do cliente deve ser codificado por URL antes de ser enviado.| 
|grant_type|necessárias|Deve ser definido como `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: Solicitação de token de acesso com um certificado 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|Parâmetro|Obrigatório/Opcional|Descrição| 
|-----|-----|-----|
|client_assertion_type|necessárias|O valor deve ser definido como urn: IETF: params: OAuth: cliente-Assertion-Type: JWT-portador.| 
|client_assertion|necessárias|Uma asserção (um token Web JSON) que você precisa para criar e assinar com o certificado que você registrou como credenciais para seu aplicativo.|  
|grant_type|necessárias|Deve ser definido como `client_credentials`.|
|client_id|opcionais|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo. Isso faz parte do client_assertion, portanto, não é necessário transmiti-lo aqui.| 
|scope|opcionais|Uma lista separada por espaços de escopos aos quais você deseja que o usuário concorde.| 

### <a name="use-a-token"></a>Usar um token 
 
Agora que você adquiriu um token, use o token para fazer solicitações para o recurso. Quando o token expirar, repita a solicitação para o ponto de extremidade/token para adquirir um novo token de acesso.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Fluxo de concessão de credenciais de senha do proprietário do recurso (não recomendado) 
 
A concessão de credenciais de senha do proprietário do recurso (ROPC) permite que um aplicativo Conecte o usuário manipulando sua senha diretamente. O fluxo ROPC requer um alto grau de confiança e exposição do usuário, e você só deve usar esse fluxo quando outros, mais seguros, fluxos não podem ser usados.  
 
### <a name="protocol-diagram"></a>Diagrama de protocolo 
 
O diagrama a seguir mostra o fluxo ROPC.

![Fluxo de ROPC](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Solicitação de autorização 
O fluxo ROPC é uma única solicitação — ele envia a identificação do cliente e as credenciais do usuário para o IDP e, em seguida, recebe tokens em retorno. O cliente deve solicitar o endereço de email (UPN) do usuário e a senha antes de fazer isso. Imediatamente após uma solicitação bem-sucedida, o cliente deve liberar com segurança as credenciais do usuário da memória. Ele nunca deve salvá-los.  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|Parâmetro|Obrigatório/Opcional|Descrição| 
|-----|-----|-----|
|client_id|necessárias|ID do cliente| 
|grant_type|necessárias|Deve ser definido como senha.| 
|username|necessárias|O endereço de email do usuário.| 
|password|necessárias|A senha do usuário.| 
|scope|opcionais|Uma lista de escopos separados por espaços.|

### <a name="successful-authentication-response"></a>Resposta de autenticação bem-sucedida 
O exemplo a seguir mostra uma resposta de token bem-sucedida: 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|Parâmetro|Descrição| 
|-----|-----|
|token_type|Sempre definido como portador.| 
|scope|Se um token de acesso for retornado, esse parâmetro listará os escopos para os quais o token de acesso é válido.| 
|expires_in|Número de segundos para o qual o token de acesso incluído é válido.| 
|access_token|Emitido para os escopos que foram solicitados.| 
|id_token|Um JWT (token Web JSON). O aplicativo pode decodificar os segmentos desse token para solicitar informações sobre o usuário que se conectou. O aplicativo pode armazenar em cache os valores e exibi-los, mas não deve confiar neles para qualquer autorização ou limites de segurança.| 
|refresh_token_expires_in|Número de segundos para o qual o token de atualização incluído é válido.| 
|refresh_token|Emitido se o parâmetro de escopo original incluía offline_access.|

Você pode usar o token de atualização para adquirir novos tokens de acesso e atualizar tokens usando o mesmo fluxo descrito na seção de fluxo de concessão de código de autenticação acima.   

## <a name="device-code-flow"></a>Fluxo de código do dispositivo 
 
A concessão de código de dispositivo permite que os usuários entrem em dispositivos com restrições de entrada, como uma TV inteligente, um dispositivo IoT ou uma impressora. Para habilitar esse fluxo, o dispositivo tem o usuário visita uma página da Web em seu navegador em outro dispositivo para entrar. Depois que o usuário entra, o dispositivo é capaz de obter tokens de acesso e atualizar tokens conforme necessário. 
 
### <a name="protocol-diagram"></a>Diagrama de protocolo 
 
Todo o fluxo de código do dispositivo é semelhante ao próximo diagrama. Descrevemos cada uma das etapas mais adiante neste artigo. 
 
![Fluxo de código do dispositivo](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Solicitação de autorização do dispositivo 
O cliente deve primeiro verificar o servidor de autenticação para obter um código de usuário e de dispositivo usado para iniciar a autenticação. O cliente coleta essa solicitação do ponto de extremidade/devicecode. Nessa solicitação, o cliente também deve incluir as permissões que ele precisa para adquirir do usuário. A partir do momento em que essa solicitação é enviada, o usuário tem apenas 15 minutos para entrar (o valor normal para expires_in), portanto, faça essa solicitação apenas quando o usuário indicou que está pronto para entrar. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Parâmetro|Condição|Descrição|
|-----|-----|-----| 
|client_id|necessárias|A ID do aplicativo (cliente) que o AD FS atribuído ao seu aplicativo.| 
|scope|opcionais|Uma lista de escopos separados por espaços.|

### <a name="device-authorization-response"></a>Resposta de autorização do dispositivo 
Uma resposta bem-sucedida será um objeto JSON que contém as informações necessárias para permitir que o usuário entre. 


|Parâmetro|Descrição|
|-----|-----| 
|device_code|Uma cadeia de caracteres longa usada para verificar a sessão entre o cliente e o servidor de autorização. O cliente usa esse parâmetro para solicitar o token de acesso do servidor de autorização.| 
|user_code|Uma cadeia de caracteres curta mostrada para o usuário que é usado para identificar a sessão em um dispositivo secundário.| 
|verification_uri|O URI ao qual o usuário deve ir com o user_code para entrar.| 
|verification_uri_complete|O URI ao qual o usuário deve ir com o user_code para entrar. Isso é preenchedo com user_code para que o usuário não precise inserir user_code| 
|expires_in|O número de segundos antes que device_code e user_code expirem.| 
|interval|O número de segundos que o cliente deve aguardar entre solicitações de sondagem.| 
|mensagem|Uma cadeia de caracteres legível por humanos com instruções para o usuário. Isso pode ser localizado com a inclusão de um parâmetro de consulta na solicitação do formulário? MKT = XX-XX, preenchendo o código de cultura do idioma apropriado.  

### <a name="authenticating-the-user"></a>Autenticando o usuário 
Depois de receber o user_code e o verification_uri, o cliente os exibe para o usuário, instruindo-os a entrar usando seu telefone celular ou navegador de PC. Além disso, o cliente pode usar um código QR ou um mecanismo semelhante para exibir o verfication_uri_complete, que executará a etapa de inserir o user_code para o usuário. Enquanto o usuário está Autenticando no verification_uri, o cliente deve sondar o ponto de extremidade/token para o token solicitado usando o device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Parâmetro|necessárias|Descrição|
|-----|-----|-----| 
|grant_type|necessárias|Deve ser urn: IETF: params: OAuth: Grant-Type: device_code| 
|client_id|necessárias|Deve corresponder ao client_id usado na solicitação inicial.| 
|code|necessárias|O device_code retornado na solicitação de autorização do dispositivo.|

### <a name="successful-authentication-response"></a>Resposta de autenticação bem-sucedida 
Uma resposta de token bem-sucedida terá A seguinte aparência:  


|Parâmetro|Descrição|
|-----|-----| 
|token_type|Sempre "portador".| 
|scope|Se um token de acesso for retornado, isso listará os escopos para os quais o token de acesso é válido.| 
|expires_in|Número de segundos antes que o token de acesso incluído seja válido para.| 
|access_token|Emitido para os escopos que foram solicitados.| 
|id_token|Emitido se o parâmetro de escopo original incluía o escopo de OpenID.| 
|refresh_token|Emitido se o parâmetro de escopo original incluía offline_access.| 
|refresh_token_expires_in|Número de segundos antes que o token de atualização incluído seja válido para.| 


## <a name="related-content"></a>Conteúdo relacionado  
Consulte [AD FS desenvolvimento](../AD-FS-Development.md) para obter a lista completa de artigos detalhados, que fornecem instruções passo a passo sobre como usar os fluxos relacionados. 
