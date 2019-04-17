---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: "AD FS cenários para desenvolvedores"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 753b2b235cb1d73ab47588f8f229410c1f81db40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-scenarios-for-developers"></a>AD FS cenários para desenvolvedores

>Aplica-se a: Windows Server 2016

AD FS no Windows Server 2016 [AD FS 2016] permite que você adicione setor conectar padrão de OpenID e OAuth 2.0 com base em autenticação e autorização para aplicativos que você está desenvolvendo e ter esses aplicativos autenticar usuários diretamente contra AD FS.    
  
AD FS 2016 também dá suporte a WS-Federation, WS-Trust e SAML protocolos e perfis têm suporte em versões anteriores.  Se você estiver interessado em diretrizes para desenvolvedores para esses protocolos, consulte o artigo aqui.  Este artigo concentra-se sobre como usar e se beneficiar com o suporte a protocolos mais recente.  
  
## <a name="why-modern-authentication"></a>Por que a autenticação moderna  
Enquanto você pode continuar usando o AD FS para entrar com WS-Federation, WS-Trust e protocolos SAML como você têm antes, com os protocolos mais recentes, você obtém os seguintes benefícios:  
  
* **Simplicidade e consistência**  
    * Use o mesmo conjunto de APIs e padrões para habilitar o logon para:   
        *   vários tipos de aplicativos (servidor, área de trabalho, mobile, navegador)  
        *   várias plataformas (android, iOS, Windows)  
        *   aplicativos dentro da rede corporativa ou hospedados na nuvem  
    * Use o mesmo conjunto de bibliotecas que você já pode usar para autenticar os usuários contra do Azure AD  
* **Flexibilidade**  
    * Além de autorização de usuário padrão, habilite cenários mais complexos, como:      
        * ? 3 pernas logon fluxos em que um usuário autoriza um aplicativo da web ou serviço acesse os recursos que residem com outro serviço ou aplicativo web.    
        * ? Fluxos de servidor a servidor em que um serviço de nível intermediário acessa um back-end API  
        * ? JavaScript com base em aplicativos de página única (SPA)  
* **Suporte do setor**  
    * OAuth 2.0 e conectar-se OpenID aproveite utilização larga da indústria, portanto conhecimento desses padrões ajudará a habilitar a autenticação e autorização fora de um ambiente do Active Directory também  
  
## <a name="how-it-works-the-basics"></a>Como ele funciona: Noções básicas  
Você pode adicionar autenticação moderna do AD FS para seu aplicativo usando o mesmo conjunto de ferramentas e bibliotecas que você já pode usar para autenticar os usuários contra do Azure AD.   
  
Em cenários de AD FS claro, é AD FS e não Azure AD que serve como o provedor de identidade e autorização server.  Caso contrário, os conceitos são exatamente os mesmos: fornecer suas credenciais de usuários e obter tokens, diretamente ou por meio de um intermediário, acesso aos recursos.  
  
O cenário mais básico consiste de um usuário ou "proprietário do recurso", interagir com um navegador para acessar um aplicativo da web:  
  
![AD FS para desenvolvedores](media/ADFS_DEV_1.png)  
  
O aplicativo web é chamado "cliente" porque ele inicia a solicitação para o servidor de autorização (AD FS) para um token de acesso ao recurso.  O recurso pode ser hospedado pelo próprio aplicativo web ou pode ser acessado como uma API da web em algum lugar na rede ou na internet.   O usuário ou "proprietário do recurso" autoriza o aplicativo cliente da web para receber esse token de acesso, fornecendo credenciais para o servidor de autorização.    
  
## <a name="how-it-works-components"></a>Como ele funciona: componentes  
OAuth 2.0 e conectar OpenID cenários de tornar o AD FS usam o mesmo conjunto de ferramentas e bibliotecas que você usa ao Azure AD é o provedor de identidade.  Esses componentes são:  
* Active Directory autenticação biblioteca (ADAL): bibliotecas de cliente que facilitam coletando as credenciais do usuário, criar e enviar solicitações de tokens e recuperar os tokens resultantes.    
* Middleware OWIN (Open Web Interface for .NET): OWIN enquanto é um projeto da comunidade com base, a Microsoft criou um conjunto de servidor lado bibliotecas para proteger aplicativos e web APIs com conectar OpenID e OAuth 2.0  
  
As funções desses componentes são mostradas no diagrama a seguir:  
  
![AD FS para desenvolvedores](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Esses cenários de modelagem no AD FS 2016  
  
### <a name="application-groups"></a>Grupos de aplicativos  
Para representar esses cenários na política do AD FS, apresentamos um novo conceito chamado grupos de aplicativos.  Um grupo de aplicativos pode conter qualquer combinação dos seguintes tipos fundamentais do aplicativo e o número:  
  
  
  
Grupo de aplicativos / tipo de aplicativo  |Descrição  |Função    
---------|---------|---------  
Aplicativo nativo     |  Às vezes chamado de cliente público, isso se destina a ser um aplicativo de cliente que é executado em um computador ou dispositivo e com que o usuário interage.       | Tokens de solicitações do servidor de autorização (AD FS) para acesso de usuário aos recursos.  Envia solicitações HTTP para recursos protegidos, usando os tokens como cabeçalhos HTTP.        
Aplicativo de servidor     |   Um aplicativo web que é executado em um servidor e normalmente é acessível aos usuários por meio de um navegador.  Porque ele é capaz de manter sua própria 'segredo do cliente' ou credenciais, ele é chamado, às vezes, um cliente confidencial.      | Tokens de solicitações do servidor de autorização (AD FS) para acesso de usuário aos recursos.  Envia solicitações HTTP para recursos protegidos, usando os tokens como cabeçalhos HTTP.               
API da Web     |  O recurso de fim o usuário está acessando. Pense nisso como a representação da nova de "partes confiantes".| Consome tokens obtidas pelos clientes  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Diferenças do AD FS 2012 R2  
Grupos de aplicativos combinam elementos de confiança e autorização AD FS 2012 R2 separadamente, exposta como partes confiantes, clientes e permissões de aplicativo.  
  
As tabelas a seguir compara os métodos pelos quais objetos de confiança do aplicativo correspondente são criados no AD FS 2012 R2 vs AD FS 2016:  
  
AD FS no Windows Server 2012 R2|No PowerShell|Gerenciamento do AD FS  
---------|---------|---------  
Adicionar cliente nativo|Adicionar AdfsClient|NA  
Adicione o aplicativo de servidor como cliente|Adicionar AdfsClient|NA  
Adicionar API da Web / recursos|Adicionar AdfsRelyingPartyTrust|Criar confiança confiante de terceiros  
  
AD FS 2016|No PowerShell|Gerenciamento do AD FS  
---------|---------|---------  
Adicionar cliente nativo|Adicionar AdfsNativeClientApplication|Adicione o grupo de aplicativo nativo  
Adicione o aplicativo de servidor como cliente|Adicionar AdfsServerApplication|Adicione o grupo de aplicativo de servidor  
Adicionar API da Web / recursos|Adicionar AdfsWebApiApplication|Adicione o grupo de aplicativo de API da Web  
  
### <a name="application-permissions-and-consent"></a>Consentimento e permissões de aplicativo  
Por padrão, os clientes em um grupo de aplicativos têm permissão para acessar os recursos no mesmo grupo.  O administrador não precisa configurar permissões do aplicativo específico.  Grupos de aplicativos também permitem que os administradores especificar os escopos permitidos, como openid ou user_impersonation.  As descrições de cenário abaixo especificam exatamente quais escopos são necessários para cada um deles.  
  
Como o AD FS usa um modelo de consentimento de administrador, os usuários não são solicitados consentimento ao acessar recursos.  Configurando o grupo de aplicativos, o administrador em vigor fornece consentimento em nome de todos os usuários do aplicativo.  
  
## <a name="supported-scenarios"></a>Cenários com suporte  
A seção a seguir descreve os cenários que oferecemos suporte a mais detalhadamente.  
  
### <a name="tokens-used"></a>Tokens usados  
Tornar esses cenários de uso de três tipos de tokens:  
  
* **id_token:** token JWT A usada para representar a identidade do usuário. A declaração 'aud' ou público do id_token corresponde a ID do cliente do aplicativo nativo ou o servidor.  
* **access_token:** token JWT A usado em Oauth e OpenID conectam cenários e destinados a ser consumido pelo recurso.  A declaração 'aud' ou público desse token deve coincidir com o identificador de recurso ou API da Web.  
* **refresh_token:** esse token é enviado no lugar de coleta de credenciais do usuário para fornecer um logon único na experiência.  Esse token foi emitido e consumida pelo AD FS e não pode ser lido por clientes ou recursos.    
  
### <a name="native-client-to-web-api"></a>Cliente nativo a API da Web  
Esse cenário permite que o usuário de um aplicativo cliente nativo para chamar uma API de Web do AD FS 2016 protegido.  
* O aplicativo cliente nativo usa ADAL para enviar a autorização e o token de solicita o AD FS, solicitando credenciais do usuário conforme necessário, em seguida, envia o token resultante como um cabeçalho HTTP na solicitação para a API da Web  
* [Essa parte é para fins de demonstração] A API da web lê as declarações do objeto ClaimsPrincipal que resulta do token de acesso enviado pelo cliente e os envia para o cliente.  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_3.png)  
  
1.  O aplicativo cliente nativo inicia o fluxo com uma chamada para a biblioteca ADAL.  Isso dispara um navegador com base em HTTP GET para o AD FS autorizar o ponto de extremidade:  
  
**Solicitação de autorização:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código"  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do aplicativo nativo no grupo de aplicativos de cliente  
redirect_uri|Redirecionar o URI do aplicativo nativo no grupo de aplicativos  
  
**Resposta da solicitação de autorização:**  
Se o usuário não tiver entrado no antes, o usuário é solicitado a fornecer as credenciais.    
AD FS responde retornando um código de autorização como o parâmetro "código" no componente de consulta do redirect_uri.  Por exemplo: HTTP/1.1 302 encontrado local: **http://redirect_uri:80 /? código =&lt;código&gt;.**  
  
2.  O cliente nativo envia o código, juntamente com os parâmetros a seguir, para o ponto de extremidade de token do AD FS:  
  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code" 
código|código de autorização de 1  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do aplicativo nativo no grupo de aplicativos de cliente  
redirect_uri|Redirecionar o URI do aplicativo nativo no grupo de aplicativos  
  
**Resposta da solicitação de token:**  
AD FS responde com um HTTP 200 com id_token no corpo, access_token e refresh_token.  
  
3.  O aplicativo nativo, em seguida, envia a parte access_token da resposta acima do cabeçalho de autorização na solicitação HTTP para a API da web.  
  
### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
Subsequente cliente solicita dentro de 1 hora (por padrão) a access_token ainda serão válidas no cache, e uma nova solicitação não ativará todo o tráfego para AD FS.  O access_token automaticamente serão obtidos do cache por ADAL.  
  
Depois que o token de acesso expira, ADAL automaticamente enviará uma solicitação baseado em token de atualização para o AD FS token ponto de extremidade (ignorando a solicitação de autorização automaticamente).  
**Atualize a solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Parâmetro|Valor|
---------|---------
grant_type|"refresh_token"|
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos|
client_id|Id do aplicativo nativo no grupo de aplicativos de cliente
refresh_token|o token de atualização emitido pelo AD FS em resposta à solicitação de token inicial

  
  
**Atualize a resposta da solicitação de token:**  
Se o token de atualização estiver dentro de < SSO_period >, a solicitação resultará em um novo token de acesso. O usuário não é solicitado credenciais.  Para obter mais informações sobre, consulte Configurações SSO [AD FS único sinal em configurações](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Se o token de atualização tiver expirado, a solicitação resulta em um 401 HTTP com o erro "invalid_grant" e "error_description" "MSIS9615: O token de atualização recebido no parâmetro refresh_token expirou". Nesse caso, ADAL envia automaticamente uma nova solicitação de autorização que parece muito com #1 acima.    
  
### <a name="web-browser-to-web-app"></a>Navegador da Web ao aplicativo Web   
Nesse cenário, um usuário com um navegador precisa acessar recursos hospedados por um aplicativo web.    
Existem dois cenários que fazer isso.  
  
#### <a name="oauth-confidential-client"></a>Cliente confidencial OAuth  
Este cenário é idêntico ao especificado acima em que há uma solicitação de autorização, seguida por um código para troca de token.  O aplicativo da web (modelado como um aplicativo de servidor no AD FS) inicia a solicitação de autorização por meio do navegador e troca o código para o token (conectando-se diretamente para o AD FS)  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_4.png)  
  
1.  Inicia o aplicativo Web solicitar uma autorização por meio do navegador, que envia um HTTP GET para o AD FS autoriza o ponto de extremidade  
**Solicitação de autorização**:  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código"  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo nativo no grupo de aplicativos  
redirect_uri|Redirecionar o URI de aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
Resposta da solicitação de autorização:  
Se o usuário não tiver entrado no antes, o usuário é solicitado a fornecer as credenciais.  
AD FS responde retornando um código de autorização como o parâmetro "código" no componente de consulta do redirect_uri, por exemplo: HTTP/1.1 302 encontrado local: https://webapp.contoso.com/?code=&lt;código&gt;.  
  
2.  Como resultado do 302 acima, o navegador inicia um HTTP GET para o aplicativo da web, por exemplo: GET http://redirect_uri:80 /? código =&lt;código&gt;.   
  
3.  Neste ponto o aplicativo da web, ter recebido o código inicia uma solicitação para o AD FS token ponto de extremidade, enviando o seguinte  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code"  
código|código de autorização do 2 acima  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|Redirecionar o URI de aplicativo web (aplicativo de servidor) no grupo de aplicativos  
client_secret|Segredo do aplicativo da web (aplicativo de servidor) no grupo de aplicativos. **Observação: Credenciais do cliente não precisam ser um client_secret.  AD FS dá suporte a capacidade de usar certificados ou autenticação integrada do Windows também.**  
  
**Resposta da solicitação de token:**  
AD FS responde com um HTTP 200 com id_token no corpo, access_token e refresh_token.  
requerimentos judiciais ou Extrajudiciais  
4.  Na web, aplicativo e nenhuma consome a parte access_token da resposta acima (no caso em que o próprio aplicativo web hospeda o recurso), ou de outra forma envia como o cabeçalho de autorização na solicitação HTTP para a API da web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
Enquanto o token de acesso ainda serão válido por 1 hora (por padrão) no cache do cliente, talvez você ache que a segunda solicitação funciona como o cenário de cliente nativo acima - que todo o tráfego para o AD FS não irá disparar uma nova solicitação conforme o token de acesso será automaticamente buscado no cache por ADAL.  No entanto, é possível que o aplicativo web pode envia autorização distinta e solicitações de token, vincula o primeiro via URL distinta, como em nosso exemplo.  
  
Nesse caso, é o cookie SSO de navegador do AD FS que permite que o AD FS emitir um novo código de autorização sem avisar ao usuário suas credenciais. O aplicativo da web, em seguida, chama para AD FS para trocar o novo código de autorização para um novo token de acesso.  O usuário não é solicitado credenciais.  
  
Caso contrário, se o aplicativo web estiver inteligentes o suficiente para saber que se o usuário já está autenticado, a solicitação de autorização pode ser ignorada e:  
* o token de acesso em cache, se não expirou, é recuperado e usado, ou   
* uma solicitação de baseado em token solicitação pode ser enviada ao ponto de extremidade de token do AD FS, conforme descrito abaixo  
  
**Atualize a solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Parâmetro|Valor  
---------|---------  
grant_type|"refresh_token"  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
refresh_token|Atualizar o token emitido pelo AD FS em resposta à solicitação de token inicial  
client_secret|Segredo do aplicativo da web (aplicativo de servidor) no grupo de aplicativos  
  
**Atualize a resposta da solicitação de token:**  
Se o token de atualização estiver dentro de < SSO_period >, a solicitação resultará em um novo token de acesso. O usuário não é solicitado credenciais. Para obter mais informações sobre, consulte Configurações SSO [AD FS único sinal em configurações](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Se o token de atualização tiver expirado, a solicitação resulta em um 401 HTTP com o erro "invalid_grant" e "error_description" "MSIS9615: O token de atualização recebido no parâmetro refresh_token expirou". Nesse caso, ADAL envia automaticamente uma nova solicitação de autorização que parece muito com #1 acima.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID conectar: Fluxo de híbrido  
Esse cenário é semelhante ao acima em que há uma solicitação de autorização é iniciada pelo aplicativo web por meio de redirecionamento do navegador e um código para troca de token do aplicativo da web para o AD FS.  A diferença nesse cenário é que o AD FS emite um id_token como parte da resposta da solicitação de autorização inicial.  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_5.png)  
  
1.  Inicia o aplicativo Web solicitar uma autorização por meio do navegador, que envia um HTTP GET para o AD FS autoriza o ponto de extremidade  
  
**Solicitação de autorização:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parâmetro|Valor  
---------|---------  
response_type|"código + id_token"  
response_mode|"form_post"  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|Redirecionar o URI de aplicativo web (aplicativo de servidor) no grupo de aplicativos  
  
**Resposta da solicitação de autorização:**  
Se o usuário não tiver entrado no antes, o usuário é solicitado a fornecer as credenciais.  
AD FS responde com um HTTP 200 e uma forma que contém o abaixo como oculto elementos:  
* código: o código de autorização  
* id_token: um token JWT contendo declarações descrevendo a autenticação do usuário  
2.  O formulário automaticamente postagens redirect_uri do aplicativo da web, enviar o código e o id_token para o aplicativo da web.  
  
3.  Neste ponto o aplicativo da web, ter recebido o código inicia uma solicitação para o AD FS token ponto de extremidade, enviando o seguinte  
  
**Solicitação de token:**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Parâmetro|Valor  
---------|---------  
grant_type|"authorization_code"  
código|código de autorização acima  
recurso|ID de RP (Identifier) da API de Web no grupo de aplicativos  
client_id|Id do cliente do aplicativo web (aplicativo de servidor) no grupo de aplicativos  
redirect_uri|Redirecionar o URI de aplicativo web (aplicativo de servidor) no grupo de aplicativos  
client_secret|Segredo do aplicativo da web (aplicativo de servidor) no grupo de aplicativos  
  
**Resposta da solicitação de token:**  
AD FS responde com um HTTP 200 com id_token no corpo, access_token e refresh_token.  
  
4.  Na web, aplicativo e nenhuma consome a parte access_token da resposta acima (no caso em que o próprio aplicativo web hospeda o recurso), ou de outra forma envia como o cabeçalho de autorização na solicitação HTTP para a API da web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamento de logon único  
O comportamento de logon único é o mesmo para o fluxo de cliente confidenciais do Oauth 2.0 acima.  
  
### <a name="on-behalf-of"></a>Em nome de  
Nesse cenário, um aplicativo web usa o token de acesso original de um usuário solicitar e obter o token de acesso do outro para outra API da Web, que o aplicativo da web, em seguida, terá acesso como o usuário final.  Isso é chamado de um fluxo "em nome de".  
  
![Descrição do fluxo de protocolo](media/ADFS_DEV_6.png)  
  
As etapas 1 e 2 trabalho assim como as etapas 3 e 4 no fluxo anterior.  
Na etapa 3, o requisito de chave é que o parâmetro client_id, a ID do cliente do aplicativo Web 2, deve coincidir com a API de ID da Web de RP A. Em outras palavras, o público o token de acesso durante a troca para o novo token deve coincidir com a ID do cliente da entidade que solicitou o novo token.  

## <a name="related-content"></a>Conteúdo relacionado  
Consulte [AD FS desenvolvimento](../AD-FS-Development.md) para obter a lista completa de artigos de Noções básicas, que fornecem instruções passo a passo sobre como usar os fluxos relacionados. 
