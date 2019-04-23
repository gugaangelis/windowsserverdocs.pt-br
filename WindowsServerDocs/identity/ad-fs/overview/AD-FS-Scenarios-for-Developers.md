---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Cenários do AD FS para desenvolvedores
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a2a88608f3989522b1ec1c123f29bd679db7e318
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877937"
---
# <a name="ad-fs-scenarios-for-developers"></a>Cenários do AD FS para desenvolvedores

>Aplica-se a: Windows Server 2016

O AD FS no Windows Server 2016 [AD FS 2016] permite que você adicione setor padrão OpenID Connect e OAuth 2.0 com base em autenticação e autorização para aplicativos que você está desenvolvendo e faça com que esses aplicativos autenticar usuários diretamente no AD FS.    
  
AD FS 2016 também dá suporte a WS-Federation, WS-Trust e SAML, protocolos e perfis têm suporte nas versões anteriores.  Se você estiver interessado em diretrizes do desenvolvedor para esses protocolos, consulte o artigo aqui.  Este artigo se concentra em como usar e aproveitar o suporte de protocolo mais recente.  
  
## <a name="why-modern-authentication"></a>Por que a autenticação moderna  
Embora você possa continuar usando o AD FS para logon com WS-Federation, WS-Trust e SAML protocolos assim como você tem antes, com os protocolos mais recentes, você obtém os seguintes benefícios:  
  
* **Simplicidade e consistência**  
    * Use o mesmo conjunto de padrões e APIs para habilitar o logon para:   
        *   vários tipos de aplicativos (servidor, área de trabalho, móvel, navegador)  
        *   várias plataformas (android, iOS e Windows)  
        *   aplicativos dentro da rede corporativa ou hospedado na nuvem  
    * Usar o mesmo conjunto de bibliotecas que você já pode usar para autenticar usuários no AD do Azure  
* **Flexibilidade**  
    * Autorização de usuário padrão, além de habilitar cenários mais complexos, como:      
        * ? 3 pernas logon fluxos em que um usuário autorize um aplicativo ou serviço web para acessar os recursos que residem com outro serviço ou aplicativo web.    
        * ? Fluxos de servidor para servidor em que um serviço de camada intermediária acessa um API de back-end  
        * ? Aplicativos de página única (SPA) baseados em JavaScript  
* **Suporte do setor**  
    * OAuth 2.0 e OpenID Connect aproveitam ampla utilização em toda a indústria, portanto, o conhecimento desses padrões, ajudará você a habilitar a autenticação e autorização fora de um ambiente do Active Directory também  
  
## <a name="how-it-works-the-basics"></a>Como funciona: Os conceitos básicos  
Você pode adicionar autenticação moderna do AD FS para seu aplicativo usando o mesmo conjunto de ferramentas e bibliotecas que você já pode usar para autenticar usuários no AD do Azure.   
  
Em cenários do AD FS, é claro, é o AD FS e não do Azure AD que serve como o provedor de identidade e autorização server.  Caso contrário, os conceitos são exatamente iguais: os usuários forneçam suas credenciais e obter tokens, diretamente ou por meio de um intermediário, para acessar recursos.  
  
O cenário mais básico consiste em um usuário ou o "recurso" proprietário, interagir com um navegador para acessar um aplicativo web:  
  
![O AD FS para desenvolvedores](media/ADFS_DEV_1.png)  
  
O aplicativo web é chamado um "cliente", porque ele inicia a solicitação para o servidor de autorização (AD FS) para um token de acesso ao recurso.  O recurso pode ser hospedado pelo aplicativo web em si ou possa ser acessado como uma API da web em algum lugar na rede ou na internet.   O usuário ou o "proprietário do recurso" autoriza o aplicativo web de cliente para receber esse token de acesso, fornecendo credenciais para o servidor de autorização.    
  
## <a name="how-it-works-components"></a>Como ele funciona: componentes  
OAuth 2.0 e OpenID Connect cenários de marca do AD FS usam o mesmo conjunto de ferramentas e bibliotecas que você usar ao Azure AD é o provedor de identidade.  Esses componentes são:  
* Active Directory Authentication Library (ADAL): bibliotecas de cliente que facilitam a coleta de credenciais do usuário, criando e enviando solicitações de token e recuperar os tokens resultantes.    
* Middleware OWIN (Open Web Interface para .NET): Enquanto o OWIN é um projeto da comunidade, a Microsoft criou um conjunto de bibliotecas do lado servidor que para proteger aplicativos web e APIs web com OpenID Connect e OAuth 2.0  
  
As funções desses componentes são mostradas no diagrama a seguir:  
  
![O AD FS para desenvolvedores](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Esses cenários de modelagem no AD FS 2016  
  
### <a name="application-groups"></a>Grupos de aplicativos  
Para representar esses cenários na política do AD FS, apresentamos um novo conceito, chamado de grupos de aplicativos.  Um grupo de aplicativos pode conter qualquer número e a combinação dos seguintes tipos fundamentais do aplicativo:  
  
  
  
Grupo de aplicativos / tipo de aplicativo  |Descrição  |Função    
---------|---------|---------  
Aplicativo nativo     |  Às vezes chamado de um cliente público, isso serve para ser um aplicativo cliente que é executado em um computador ou dispositivo e com que o usuário interage.       | Tokens de solicitações do servidor de autorização (AD FS) para acesso de usuário aos recursos.  Envia solicitações HTTP para recursos protegidos, usando os tokens como cabeçalhos HTTP.        
Aplicativo de servidor     |   Um aplicativo web que é executado em um servidor e é geralmente acessíveis aos usuários por meio de um navegador.  Como ele é capaz de manter seu próprio cliente 'segredo' ou credencial, ele é chamado, às vezes, um cliente confidencial.      | Tokens de solicitações do servidor de autorização (AD FS) para acesso de usuário aos recursos.  Envia solicitações HTTP para recursos protegidos, usando os tokens como cabeçalhos HTTP.               
API da Web     |  O recurso final que o usuário está acessando. Pense nisso como a nova representação do "terceiras partes confiáveis".| Consome os tokens obtidos pelos clientes  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Diferenças do AD FS 2012 R2  
Grupos de aplicativos combinam elementos de confiança e a autorização do AD FS 2012 R2 separadamente, exposto como terceiras partes confiáveis, clientes e permissões do aplicativo.  
  
As tabelas a seguir compara os métodos pelos quais objetos de confiança do aplicativo correspondentes são criados no AD FS 2012 R2 vs AD FS 2016:  
  
AD FS no Windows Server 2012 R2|No PowerShell|Gerenciamento do AD FS  
---------|---------|---------  
Adicionar cliente nativo|Add-AdfsClient|N/D  
Adicionar o aplicativo de servidor como cliente|Add-AdfsClient|N/D  
Adicionar Web API / recursos|Add-AdfsRelyingPartyTrust|Criar confiança de terceira parte confiável  
  
AD FS 2016|No PowerShell|Gerenciamento do AD FS  
---------|---------|---------  
Adicionar cliente nativo|Add-AdfsNativeClientApplication|Adicionar grupo de aplicativos nativo  
Adicionar o aplicativo de servidor como cliente|Add-AdfsServerApplication|Adicionar grupo de aplicativos de servidor  
Adicionar Web API / recursos|Add-AdfsWebApiApplication|Adicionar grupo de aplicativos de API da Web  
  
### <a name="application-permissions-and-consent"></a>Permissões de aplicativo e consentimento  
Por padrão, os clientes em um grupo de aplicativos têm permissão para acessar os recursos no mesmo grupo.  O administrador não precisa configurar permissões de aplicativo específico.  Grupos de aplicativos também permitem que os administradores especifiquem os escopos de permissão, como openid ou user_impersonation.  Descrições de cenários abaixo especificam exatamente quais escopos são necessários para a qual cenário.  
  
Como o AD FS usa um modelo de consentimento do administrador, os usuários não serão solicitados o consentimento ao acessar recursos.  Ao configurar o grupo de aplicativos, o administrador em vigor fornece autorização em nome de todos os usuários do aplicativo.  
  
## <a name="supported-scenarios"></a>Cenários com suporte  
A seção a seguir descreve os cenários há suporte para mais detalhes.  
  
### <a name="tokens-used"></a>Tokens usados  
Esses cenários de uso de três tipos de token:  
  
* **id_token:** Um token JWT usado para representar a identidade do usuário. A declaração "aud" ou o público-alvo do id_token coincide com a ID do cliente do aplicativo nativo ou no servidor.  
* **access_token:** Um token JWT usado no Oauth e OpenID conectam cenários e se destina a ser consumido pelo recurso.  A declaração "aud" ou o público-alvo deste token deve corresponder ao identificador do recurso ou API da Web.  
* **refresh_token:** Esse token é enviado no lugar de coleta de credenciais do usuário para fornecer um logon único na experiência.  Esse token foi emitido e consumido pelo AD FS e não pode ser lido por clientes ou recursos.    
  
### <a name="native-client-to-web-api"></a>Cliente nativo para API da Web  
Esse cenário permite que o usuário de um aplicativo cliente nativo para chamar uma API Web do AD FS 2016 protegido.  
* O aplicativo cliente nativo usa ADAL para enviar a autorização e token de solicitações para o AD FS, no prompt de credenciais do usuário conforme necessário, em seguida, envia o token resultante como um cabeçalho HTTP na solicitação para a API da Web  
* [Esta parte é para fins de demonstração] A API da web lê as declarações do objeto de ClaimsPrincipal que resulta do token de acesso enviado pelo cliente e envia de volta ao cliente.  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_3.png)  
  
1.  O aplicativo cliente nativo inicia o fluxo com uma chamada para a biblioteca ADAL.  Isso dispara um navegador com base em HTTP GET para o AD FS autorizar o ponto de extremidade:  
  
**Solicitação de autorização:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código"  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do aplicativo nativo no grupo de aplicativos do cliente  
redirect_uri|URI de redirecionamento do aplicativo nativo no grupo de aplicativos  
  
**Resposta de solicitação de autorização:**  
Se o usuário não tiver entrado antes, o usuário é solicitado a fornecer credenciais.    
O AD FS responde, retornando um código de autorização como o parâmetro "code" no componente de consulta do redirect_uri.  Por exemplo:  Local do HTTP/1.1 302 encontrado:  **http://redirect_uri:80/?code=&lt; código&gt;.**  
  
2.  O native client, em seguida, envia o código, juntamente com os parâmetros a seguir, para o ponto de extremidade de token do AD FS:  
  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code" 
code|código de autorização de 1  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do aplicativo nativo no grupo de aplicativos do cliente  
redirect_uri|URI de redirecionamento do aplicativo nativo no grupo de aplicativos  
  
**Resposta de solicitação de token:**  
O AD FS responde com um HTTP 200, com o access_token, refresh_token e id_token no corpo.  
  
3.  O aplicativo nativo, em seguida, envia a parte access_token da resposta acima como o cabeçalho de autorização na solicitação HTTP à API da web.  
  
### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
Solicitações do cliente subsequentes dentro de 1 hora (por padrão) o access_token ainda serão válidos no cache e uma nova solicitação não disparará a qualquer tráfego para o AD FS.  O access_token automaticamente será buscada no cache por ADAL.  
  
Depois que o token de acesso expira, o ADAL automaticamente enviará uma solicitação de token com base de atualização para a extremidade de token do AD FS (ignorando a solicitação de autorização automaticamente).  
**Solicitação de token de atualização:**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Parâmetro|Valor|
---------|---------
grant_type|"refresh_token"|
recurso|RP ID (identificador) da API Web no grupo de aplicativos|
client_id|Id do aplicativo nativo no grupo de aplicativos do cliente
refresh_token|o token de atualização emitido pelo AD FS em resposta à solicitação de token inicial

  
  
**Resposta de solicitação de token de atualização:**  
Se o token de atualização estiver dentro de < SSO_period >, a solicitação resultará em um novo token de acesso. O usuário não é solicitado para credenciais.  Para obter mais informações sobre as configurações, consulte SSO [AD FS configurações de logon único](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Se o token de atualização tiver expirado, a solicitação resulta em um 401 de HTTP com o erro "invalid_grant" e "error_description" "MSIS9615: O token de atualização recebido no parâmetro refresh_token expirou". Nesse caso, o ADAL automaticamente envia uma nova solicitação de autorização que se parece com #1 acima.    
  
### <a name="web-browser-to-web-app"></a>Navegador da Web para aplicativo Web   
Nesse cenário, um usuário com um navegador precisa acessar recursos hospedados por um aplicativo web.    
Há dois cenários que fazem isso.  
  
#### <a name="oauth-confidential-client"></a>Cliente confidencial do OAuth  
Esse cenário é semelhante ao acima em que há uma solicitação de autorização, seguida por um código de troca de token.  O aplicativo web (modelado como um aplicativo de servidor no AD FS) inicia a solicitação de autorização por meio do navegador e troca o código para o token (conectando-se diretamente ao AD FS)  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_4.png)  
  
1.  Inicia o aplicativo Web a uma autorização de solicitação por meio do navegador, que envia um HTTP GET para o AD FS autoriza o ponto de extremidade  
**Solicitação de autorização**:  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código"  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo nativo no grupo de aplicativos  
redirect_uri|URI de redirecionamento do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
Resposta de solicitação de autorização:  
Se o usuário não tiver entrado antes, o usuário é solicitado a fornecer credenciais.  
O AD FS responde, retornando um código de autorização como o parâmetro "code" no componente de consulta do redirect_uri, por exemplo: Local do HTTP/1.1 302 encontrado: https://webapp.contoso.com/?code=&lt; código&gt;.  
  
2.  Como resultado de 302 acima, o navegador inicia um HTTP GET para o aplicativo web, por exemplo: Obtenha http://redirect_uri:80/?code=&lt; código&gt;.   
  
3.  Neste ponto o aplicativo web, ter recebido o código inicia uma solicitação para o AD FS token ponto de extremidade, enviando o seguinte  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code"  
code|código de autorização do 2 acima  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|URI de redirecionamento do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
client_secret|Segredo do aplicativo web (aplicativo de servidor) no grupo de aplicativos. **Observação: Credencial do cliente não precisa ser um client_secret.  AD FS dá suporte a capacidade de usar certificados ou autenticação integrada do Windows também.**  
  
**Resposta de solicitação de token:**  
O AD FS responde com um HTTP 200, com o access_token, refresh_token e id_token no corpo.  
declarações  
4.  A web, aplicativo e em seguida, o consome a parte access_token da resposta acima (no caso em que o próprio aplicativo web hospeda o recurso), ou caso contrário, envia como o cabeçalho de autorização na solicitação HTTP para a API da web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
Enquanto o token de acesso ainda serão válido por 1 hora (por padrão) no cache do cliente, você pode achar que a segunda solicitação funcione como no cenário acima - cliente nativo que todo o tráfego para o AD FS não irá disparar uma nova solicitação como o token de acesso será automaticamente ser buscadas no cache por ADAL.  No entanto, é possível que o aplicativo web pode enviar solicitações de token, o primeiro por meio do link de URL distinta, como em nossa amostra e autorização distinta.  
  
Nesse caso, é o cookie do SSO de navegador do AD FS que permite que o AD FS emitir um novo código de autorização sem avisar o usuário forneça credenciais. Em seguida, chama o aplicativo web para o AD FS para o novo código de autorização para um novo token de acesso do exchange.  O usuário não é solicitado para credenciais.  
  
Caso contrário, se o aplicativo web é inteligente o suficiente para saber que se o usuário já está autenticado, a solicitação de autorização pode ser ignorada e qualquer um dos:  
* o token de acesso armazenada em cache, se não expirado, é recuperado e usado, ou   
* uma solicitação de baseado em token de solicitação pode ser enviada ao ponto de extremidade de token do AD FS, conforme descrito abaixo  
  
**Solicitação de token de atualização:**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Parâmetro|Valor  
---------|---------  
grant_type|"refresh_token"  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
refresh_token|Atualizar o token emitido pelo AD FS em resposta à solicitação de token inicial  
client_secret|Segredo do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
**Resposta de solicitação de token de atualização:**  
Se o token de atualização estiver dentro de < SSO_period >, a solicitação resultará em um novo token de acesso. O usuário não é solicitado para credenciais. Para obter mais informações sobre as configurações, consulte SSO [AD FS configurações de logon único](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Se o token de atualização tiver expirado, a solicitação resulta em um 401 de HTTP com o erro "invalid_grant" e "error_description" "MSIS9615: O token de atualização recebido no parâmetro refresh_token expirou". Nesse caso, o ADAL automaticamente envia uma nova solicitação de autorização que se parece com #1 acima.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect: Fluxo híbrido  
Esse cenário é semelhante ao acima em que existe uma solicitação de autorização que é iniciada pelo aplicativo web por meio de redirecionamento do navegador e um código para troca de token do aplicativo web para o AD FS.  A diferença nesse cenário é que o AD FS emite um id_token como parte da resposta de solicitação de autorização inicial.  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_5.png)  
  
1.  Inicia o aplicativo Web a uma autorização de solicitação por meio do navegador, que envia um HTTP GET para o AD FS autoriza o ponto de extremidade  
  
**Solicitação de autorização:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código + id_token"  
response_mode|"form_post"  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|URI de redirecionamento do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
**Resposta de solicitação de autorização:**  
Se o usuário não tiver entrado antes, o usuário é solicitado a fornecer credenciais.  
AD FS responde com um HTTP 200 e o formulário que contém o abaixo como elementos ocultos:  
* código: o código de autorização  
* id_token: um token JWT que contêm declarações que descrevem a autenticação do usuário  
2.  O formulário lançará automaticamente para o redirect_uri do aplicativo web, enviando o código e o id_token para o aplicativo web.  
  
3.  Neste ponto o aplicativo web, ter recebido o código inicia uma solicitação para o AD FS token ponto de extremidade, enviando o seguinte  
  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code"  
code|código de autorização acima  
recurso|RP ID (identificador) da API Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|URI de redirecionamento do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
client_secret|Segredo do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
**Resposta de solicitação de token:**  
O AD FS responde com um HTTP 200, com o access_token, refresh_token e id_token no corpo.  
  
4.  A web, aplicativo e em seguida, o consome a parte access_token da resposta acima (no caso em que o próprio aplicativo web hospeda o recurso), ou caso contrário, envia como o cabeçalho de autorização na solicitação HTTP para a API da web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
O comportamento de logon único é da mesma maneira que o fluxo de cliente confidencial do Oauth 2.0 acima.  
  
### <a name="on-behalf-of"></a>Em nome de  
Nesse cenário, um aplicativo web usa o token de acesso original de um usuário para solicitar e obter outro token de acesso para outra API da Web, que, em seguida, acessará o aplicativo web como o usuário final.  Isso é chamado de um fluxo "em nome de".  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_6.png)  
  
As etapas 1 e 2 trabalho assim como as etapas 3 e 4 no fluxo anterior.  
Na etapa 3, o principal requisito é que o parâmetro client_id, a ID do cliente do aplicativo Web 2, deve corresponder à RP ID de API da Web A.  Em outras palavras, o público-alvo do token de acesso que estão sendo trocado para o novo token deve corresponder a ID do cliente da entidade que está solicitando o novo token.  

## <a name="related-content"></a>Conteúdo relacionado  
Ver [desenvolvimento do AD FS](../AD-FS-Development.md) para obter uma lista de artigos passo a passo, que fornecem instruções passo a passo sobre como usar os fluxos relacionados. 
