---
title: AD FS conceitos do OpenID Connect/OAuth
description: Saiba mais sobre os conceitos de autenticação moderna do AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 26c1635d4218c7d33377b6b8a90bc96ea4ad37b3
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948779"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>AD FS conceitos do OpenID Connect/OAuth
Aplica-se ao AD FS 2016 e posterior
 
## <a name="modern-authentication-actors"></a>Atores de autenticação moderna 

|Ator| Descrição|
|-----|-----|
|Usuário final|Essa é a entidade de segurança (usuários, aplicativos, serviços e grupos) que precisa acessar o recurso.|  
|Remota|Este é seu aplicativo Web, identificado por sua ID de cliente. O cliente é geralmente a parte com a qual o usuário final interage e solicita tokens do servidor de autorização.
|Servidor de autorização/provedor de identidade (IdP)| Este é o servidor de AD FS. Ele é responsável por verificar a identidade das entidades de segurança existentes no diretório de uma organização. Ele emite tokens de segurança (token de acesso de portador, token de ID, token de atualização) após a autenticação bem-sucedida dessas entidades de segurança.
|Servidor de recursos/provedor de recursos/terceira parte confiável| É aí que residem o recurso ou os dados. Ele confia no servidor de autorização para autenticar e autorizar o cliente com segurança e usa tokens de acesso de portador para garantir que o acesso a um recurso possa ser concedido.

O diagrama a seguir fornece a relação mais básica entre os atores:

![Atores de autenticação moderna](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>Tipos de aplicativo 
 

|Tipo de Aplicativo|Descrição|Role|
|-----|-----|-----|
|Aplicativo nativo|Às vezes chamado de **cliente público**, destina-se a ser um aplicativo cliente executado em um computador ou dispositivo e com o qual o usuário interage.|Solicita tokens do servidor de autorização (AD FS) para acesso de usuário aos recursos. Envia solicitações HTTP para recursos protegidos, usando os tokens como cabeçalhos HTTP.| 
|Aplicativo de servidor (aplicativo Web)|Um aplicativo Web que é executado em um servidor e geralmente é acessível aos usuários por meio de um navegador. Como ele é capaz de manter seu próprio ' segredo ' ou credencial do cliente, às vezes é chamado de **cliente confidencial**. |Solicita tokens do servidor de autorização (AD FS) para acesso de usuário aos recursos. Antes de solicitar o token, o cliente (aplicativo Web) precisa se autenticar usando seu segredo. | 
|API Web|O recurso final que o usuário está acessando. Considere isso como a nova representação de "terceiras partes confiáveis".|Consome tokens de acesso de portador obtidos pelos clientes| 

## <a name="application-group"></a>Grupo de aplicativos 
 
Todo cliente OAuth (nativo ou aplicativo Web) ou recurso (API Web) configurado com AD FS precisa ser associado a um grupo de aplicativos. Os clientes em um grupo de aplicativos podem ser configurados para acessar os recursos no mesmo grupo. Um grupo de aplicativos pode conter vários clientes e recursos.  

## <a name="security-tokens"></a>Tokens de segurança 
 
A autenticação moderna usa os seguintes tipos de token: 
- **id_token**: um token JWT emitido pelo servidor de autorização (AD FS) e consumido pelo cliente. As declarações no token de ID conterão informações sobre o usuário para que o cliente possa usá-lo.  
- **access_token**: um token JWT emitido pelo servidor de autorização (AD FS) e destinado a ser consumido pelo recurso. O ' AUD ' ou a declaração de público desse token deve corresponder ao identificador do recurso ou API da Web.  
- **refresh_token**: esse token é emitido por AD FS para o cliente usar quando ele precisa atualizar o id_token e access_token. O token é opaco para o cliente e só pode ser consumido por AD FS.  

## <a name="scopes"></a>Escopos 
 
Ao registrar um recurso no AD FS, os escopos podem ser configurados para permitir que AD FS executem ações específicas. Além de configurar o escopo, o valor do escopo também deve ser enviado na solicitação de AD FS para executar a ação. Por exemplo, o administrador precisa configurar o escopo como OpenID durante o registro de recursos e o aplicativo (cliente) precisa enviar o Scope = OpenID na solicitação de autenticação para AD FS para emitir o token de ID. Detalhes sobre os escopos disponíveis em AD FS são fornecidos abaixo 
 
- Aza-se estiver usando [extensões de protocolo OAuth 2,0 para clientes do Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se o parâmetro de escopo contiver o escopo "aza", o servidor emitirá um novo token de atualização primário e o definirá no campo refresh_token da resposta, bem como Configurando o campo refresh_token_expires_in para o tempo de vida do novo token de atualização primário, se um for imposto. 
- OpenID – permite que o aplicativo solicite o uso do protocolo de autorização OpenID Connect. 
- logon_cert-o escopo logon_cert permite que um aplicativo solicite certificados de logon, que podem ser usados para fazer logon interativamente usuários autenticados. O servidor de AD FS omite o parâmetro access_token da resposta e, em vez disso, fornece uma cadeia de certificados CMS codificada em base64 ou uma resposta de PKI completa de CMC. Mais detalhes estão disponíveis [aqui](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e).
- user_impersonation-o escopo de user_impersonation é necessário para solicitar com êxito um token de acesso em nome de AD FS. Para obter detalhes sobre como usar esse escopo, consulte [criar um aplicativo de várias camadas usando obo (em nome de) usando OAuth com AD FS 2016](ad-fs-on-behalf-of-authentication-in-windows-server.md). 
- allatclaims – o escopo allatclaims permite que o aplicativo solicite declarações no token de acesso a serem adicionadas também ao token de ID.   
- vpn_cert-o escopo vpn_cert permite que um aplicativo solicite certificados VPN, que podem ser usados para estabelecer conexões VPN usando a autenticação EAP-TLS. Não há mais suporte para isso. 
- email – permite que o aplicativo solicite a declaração de email para o usuário conectado.  
- Perfil – permite que o aplicativo solicite declarações relacionadas ao perfil para o usuário de conexão.  

## <a name="claims"></a>Declarações 
 
Tokens de segurança (tokens de ID e acesso) emitidos por AD FS contêm declarações ou asserções de informações sobre o assunto que foi autenticado. Os aplicativos podem usar declarações para diversas tarefas, incluindo: 
- Validar o token 
- Identificar o locatário de diretório da entidade 
- Exibir informações do usuário 
- Determine a autorização da entidade que as declarações presentes em qualquer token de segurança específico dependem do tipo de token, do tipo de credencial usado para autenticar o usuário e da configuração do aplicativo.  
 
## <a name="high-level-ad-fs-authentication-flow"></a>Fluxo de autenticação de AD FS de alto nível 

![Fluxo de autenticação AD FS](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS recebe a solicitação de autenticação do cliente.  
 
 2. AD FS valida a ID do cliente na solicitação de autenticação com a ID do cliente obtida durante o registro do cliente e do recurso no AD FS. Se estiver usando o cliente confidencial, AD FS também validará o segredo do cliente fornecido na solicitação de autenticação. AD FS também validar o URI de redirecionamento do cliente. 
 
 3. AD FS identifica o recurso que o cliente deseja acessar por meio do parâmetro de recurso passado na solicitação de autenticação. Se estiver usando a biblioteca de cliente MSAL, o parâmetro de recurso não será enviado. Em vez disso, a URL do recurso é enviada como parte do parâmetro de escopo: *Scope = [URL do recurso]//[valores de escopo, por exemplo, OpenID]* . 

    Se o recurso não for passado usando o parâmetro de escopo ou recurso, o ADFS usará um recurso padrão urn: Microsoft: UserInfo cujas políticas (por exemplo, MFA, emissão ou política de autorização) não podem ser configuradas. 
 
 4. A próxima AD FS valida se o cliente tem as permissões para acessar o recurso. AD FS também valida se os escopos passados na solicitação de autenticação correspondem aos escopos configurados durante o registro do recurso. Se o cliente não tiver as permissões ou se os escopos corretos não forem enviados na solicitação de autenticação, o fluxo de autenticação será encerrado.   
 
 5. Depois que as permissões e os escopos são validados, o AD FS autentica o usuário usando o [método de autenticação](../operations/configure-authentication-policies.md)configurado.   

 6. Se o [método de autenticação adicional](../operations/configure-additional-authentication-methods-for-ad-fs.md) for necessário de acordo com a política de recursos ou a política de autenticação global, o AD FS disparará a autenticação adicional. 

 7. AD FS usa MFA [do Azure](../operations/configure-ad-fs-and-azure-mfa.md) ou [MFA de terceiros](../operations/additional-authentication-methods-ad-fs.md) para realizar a autenticação.   
 
 8. Depois que o usuário é autenticado, AD FS aplica as [regras de declaração](../deployment/configuring-claim-rules.md) (determina as declarações enviadas ao recurso como parte dos tokens de segurança) e as políticas de [controle de acesso](../operations/ad-fs-client-access-policies.md) (determina que o usuário atendeu às condições necessárias para acessar o recurso).   

 9. Em seguida, AD FS gera os tokens de acesso e atualização.. 

 10. AD FS também gera o token de ID. 
 
 11. Se o Scope = allatclaims estiver incluído na solicitação de autenticação, o [token de ID será personalizado](custom-id-tokens-in-ad-fs.md) para incluir declarações no token de acesso com base nas regras de declaração definidas. 
    
 12. Depois que os tokens necessários forem gerados e personalizados, AD FS responderá ao cliente, incluindo os tokens. Somente se a solicitação de autenticação incluir Scope = OpenID, o token de ID será incluído na resposta. O cliente sempre pode obter o token de ID após a autenticação usando o ponto de extremidade do token. 

## <a name="types-of-libraries"></a>Tipos de bibliotecas 
  
Dois tipos de bibliotecas são usados com AD FS: 
- **Bibliotecas de cliente**: os clientes nativos e os aplicativos de servidor usam bibliotecas de cliente para adquirir tokens de acesso para chamar um recurso, como uma API da Web. A biblioteca de autenticação da Microsoft (MSAL) é a biblioteca de cliente mais recente e recomendada ao usar o AD FS 2019. O Biblioteca de Autenticação do Active Directory (ADAL) é recomendado para o AD FS 2016.  

- **Bibliotecas de middleware de servidor**: os aplicativos Web usam bibliotecas de middleware de servidor para entrada do usuário. As APIs da Web usam bibliotecas de middleware de servidor para validar tokens enviados por clientes nativos ou por outros servidores. OWIN (Open Web interface para .NET) é a biblioteca de middleware recomendada. 

## <a name="customize-id-token-additional-claims-in-id-token"></a>Personalizar token de ID (declarações adicionais no token de ID)
 
Em determinados cenários, é possível que o aplicativo Web (cliente) precise de declarações adicionais em um token de ID para ajudar na funcionalidade. Isso pode ser feito usando uma das opções a seguir. 

**Opção 1:** Deve ser usado ao usar um cliente público e um aplicativo Web não tem um recurso que ele está tentando acessar. A opção requer 
1.  response_mode definido como form_post 
2.  O identificador de terceira parte confiável (identificador de API da Web) é o mesmo que o identificador de cliente

![AD FS personalizar a opção de token 1](media/adfs-modern-auth-concepts/option1.png)

**Opção 2:** Deve ser usado quando o aplicativo Web tem um recurso que está tentando acessar e precisa passar declarações adicionais por meio do token de ID. Os clientes públicos e confidenciais podem ser usados. A opção requer 
1.  response_mode definido como form_post 
2.  O KB4019472 é instalado em seus servidores de AD FS 
3.  Escopo allatclaims atribuído ao par cliente – RP. Você pode atribuir o escopo usando o cmdlet do PowerShell Grant-ADFSApplicationPermission (use set-AdfsApplicationPermission se já concedido uma vez), conforme indicado no exemplo a seguir: 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![Opção de token de AD FS personalizar 2](media/adfs-modern-auth-concepts/option2.png)

Para entender melhor como configurar um aplicativo Web no ADFS para adquirir um token de ID personalizado, consulte [Personalizar declarações a serem emitidas no id_token ao usar o OpenID Connect ou o OAuth com o AD FS 2016 ou posterior](Custom-Id-Tokens-in-AD-FS.md).

## <a name="single-log-out"></a>Logoff único

O logoff único resulta na finalização de todas as sessões de cliente usando a ID de sessão. O AD FS 2016 e posterior dá suporte ao logoff único para OpenID Connect/OAuth. Para obter mais detalhes, consulte [log único para OpenID Connect com AD FS](ad-fs-logout-openid-connect.md).


## <a name="ad-fs-endpoints"></a>AD FS pontos de extremidade

|Ponto de extremidade AD FS|Descrição|
|-----|-----|
|/Authorize|AD FS retorna um código de autorização que pode ser usado para obter o token de acesso|
|/token|AD FS retorna um token de acesso que pode ser usado para acessar o recurso (API Web)|
|/userinfo|AD FS retorna declarações sobre o usuário autenticado|
|/devicecode|AD FS retorna o código do dispositivo e o código do usuário|
|/logout|AD FS faz logoff do usuário|
|/keys|AD FS chaves públicas usadas para assinar respostas|
|/.well-known/openid-configuration|AD FS retorna metadados do OAuth/OpenID Connect|
