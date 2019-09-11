---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: Quando usar uma regra de declaração de autorização
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3189056de8feff65d37b1846059d871c99ee9ede
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869224"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>Quando usar uma regra de declaração de autorização
Você pode usar essa regra em serviços de Federação do Active Directory (AD FS) \(AD FS\) quando precisar pegar um tipo de declaração de entrada e aplicar uma ação que determinará se um usuário terá o acesso permitido ou negado com base no valor que você Especifique na regra. Quando você usa essa regra, você passa ou transforma quaisquer declarações que correspondem à lógica da regra a seguir, com base em uma das opções configuradas na regra:  
  
|Opção de regras|Lógica de regras|  
|---------------|--------------|  
|Permitir todos os usuários|Se o tipo de declaração de entrada for igual a *qualquer tipo de declaração* e o valor for igual a *qualquer valor*, emita declarações com valores iguais a *Permitir*|  
|Permitir o acesso a usuários com esta declaração de entrada|Se o tipo de declaração de entrada for igual a *tipo de declaração específico* e o valor for igual a *valor de declaração específico*, emita declarações com valores iguais a *Permitir*|  
|Negar o acesso a usuários com esta declaração de entrada|Se o tipo de declaração de entrada for igual a *tipo de declaração específico* e o valor for igual a *valor de declaração específico*, emita declarações com valores iguais a *Negar*|  
  
As seções a seguir fornecem uma introdução básica às regras de declaração e mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre regras de declaração  
Uma regra de declaração representa uma instância da lógica de negócios que usará uma declaração de entrada, aplicará \(uma condição a ela\) se x depois y e produzir uma declaração de saída com base nos parâmetros de condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:  
  
-   No snap\-in de gerenciamento de AD FS, as regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   As regras de declaração processam declarações de entrada diretamente de \(um provedor de declarações, como\) Active Directory ou outro serviço de Federação ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.  
  
-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.  
  
-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Permitir todos os usuários  
Quando você usa o modelo de regra Permitir todos os usuários, todos os usuários terão acesso à terceira parte confiável. No entanto, você pode usar regras de autorização adicionais para restringir o acesso. Se uma regra permitir que um usuário acesse a terceira parte confiável, e outra regra negar o acesso do usuário à terceira parte confiável, o resultado da negação substituirá o resultado da permissão e o usuário terá o acesso negado.  
  
Usuários com acesso à terceira parte confiável do Serviço de Federação ainda podem ter o serviço negado pela terceira parte confiável.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Permitir o acesso a usuários com esta declaração de entrada  
Quando você usa a permissão ou negação de usuários com base em um modelo de regra de declaração de entrada para criar uma regra e definir a condição como permitir, você pode permitir o acesso do usuário específico à terceira parte confiável com base no tipo e no valor de uma declaração de entrada. Por exemplo, você pode usar esse modelo de regra para criar uma regra que permitirá somente usuários que tenham um grupo de declaração com um valor de Admins do domínio. Se uma regra permitir que um usuário acesse a terceira parte confiável, e outra regra negar o acesso do usuário à terceira parte confiável, o resultado da negação substituirá o resultado da permissão e o usuário terá o acesso negado.  
  
Usuários com permissão de acesso à terceira parte confiável do Serviço de Federação ainda podem ter o serviço negado pela terceira parte confiável. Se você quiser permitir que todos os usuários tenham acesso à terceira parte confiável, use o modelo de regra Permitir todos os usuários.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Negar o acesso a usuários com esta declaração de entrada  
Quando você usa os usuários permitir ou negar com base em um modelo de regra de declaração de entrada para criar uma regra e definir a condição como negar, você pode negar o acesso do usuário à terceira parte confiável com base no tipo e no valor de uma declaração de entrada. Por exemplo, você pode usar esse modelo de regra para criar uma regra que rejeite todos os usuários que tenham uma declaração de grupo com um valor Usuários do domínio.  
  
Se você quiser usar a condição negar, mas ainda permitir acesso à parte confiável para usuários específicos, você deve, mais tarde, adicionar regras de autorização explicitamente, com a condição de permitir que esses usuários acessem a terceira parte confiável.  
  
Se um usuário tiver acesso negado quando o mecanismo de emissão de declarações processar o conjunto de regras, o processamento de regras adicionais será encerrado e AD FS retornará um erro de "acesso negado" à solicitação do usuário.  
  
## <a name="authorizing-users"></a>Autorizando usuários  
No AD FS, as regras de autorização são usadas para emitir uma declaração de permissão ou negação que determinará se um usuário ou \(um grupo de usuários dependendo do tipo\) de declaração usado terá permissão para\-acessar recursos baseados na Web em uma determinada dependência parte ou não. As regras de autorização só podem ser definidas em objetos de confiança da terceira parte confiável.  
  
### <a name="authorization-rule-sets"></a>Conjuntos de regras de autorização  
Existem conjuntos de regras de autorização diferentes, dependendo do tipo de operação de permissão ou negação que você precisa configurar. Esses conjuntos de regra incluem:  
  
-   **Regras de autorização de emissão**: Essas regras determinam se um usuário pode receber declarações em nome de uma parte confiável e, portanto, ter acesso à terceira parte confiável.  
  
-   **Regras de autorização de delegação**: Essas regras determinam se um usuário pode agir como outro usuário para a terceira parte confiável. Quando um usuário está agindo como outro usuário, declarações sobre o usuário solicitante ainda são colocadas no token.  
  
-   **Regras de autorização de representação**: Essas regras determinam se um usuário pode representar totalmente outro usuário para a terceira parte confiável. Representar outro usuário dessa maneira é um recurso muito poderoso, porque a terceira parte confiável não saberá que o usuário está sendo representado.  
  
Para obter mais detalhes sobre como o processo de regra de autorização se encaixa no pipeline de emissão de declarações, consulte A função do mecanismo de emissão de declarações.  
  
### <a name="supported-claim-types"></a>Tipos de declaração com suporte  
AD FS define dois tipos de declaração que são usados para determinar se um usuário é permitido ou negado. Esses \(URIs\) de identificadores de recursos uniformes de tipo de declaração são os seguintes:  
  
1.  **Permitir**: http:\/\/\/schemas.Microsoft.comdedeclarações\/de autorização permitido\/  
  
2.  **Deny**: http:\/\/negação\/dedeclarações\/de autorização de schemas.Microsoft.com\/  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você pode criar ambas as regras de autorização usando o idioma da regra de declaração ou usando o modelo de regra **permitir todos os usuários** ou os **usuários permitir ou negar com base em um** modelo de regra\-de declaração de entrada no snap-in de gerenciamento de AD FS. O modelo de regra Permitir todos os usuários não oferece opções de configuração. No entanto, o modelo de regra Permitir ou negar usuários com base em um modelo de regra de declaração de entrada fornece as seguintes opções de configuração:  
  
-   Especificar um nome de regra de declaração  
  
-   Especificar um tipo de declaração de entrada  
  
-   Digitar um valor de declaração de entrada  
  
-   Permitir o acesso a usuários com esta declaração de entrada  
  
-   Negar o acesso a usuários com esta declaração de entrada  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para permitir que todos os usuários](https://technet.microsoft.com/library/ee913577.aspx) ou [criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](https://technet.microsoft.com/library/ee913594.aspx) no guia de implantação de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Se uma declaração dever ser enviada apenas quando o valor da declaração corresponder a um padrão personalizado, você deverá usar uma regra personalizada. Para obter mais informações, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Exemplo de como criar uma regra de autorização com base em várias declarações  
Ao usar a sintaxe de linguagem de regra de declaração para autorizar declarações, uma declaração também pode ser emitida com base na presença de várias declarações nas declarações originais do usuário. A regra a seguir só emitirá uma declaração de autorização se o usuário for um membro do grupo Editores e foi autenticado usando a autenticação do Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Exemplo de como criar regras de autorização regras que delegarão quem pode criar ou remover relações de confiança de proxy do servidor de federação  
Para que um Serviço de Federação possa usar um proxy de servidor de federação para redirecionar solicitações de cliente, uma relação de confiança deve ser estabelecida entre o Serviço de Federação e o computador proxy do servidor de federação. Por padrão, é estabelecida uma relação de confiança de proxy quando qualquer uma das seguintes credenciais é fornecida com êxito no assistente de configuração de Proxy do Servidor de Federação do AD FS:  
  
-   A conta de serviço usada pelo Serviço de Federação que o proxy vai proteger  
  
-   Uma conta de domínio do Active Directory que seja membro do grupo de administradores locais em todos os servidores de federação em um farm de servidores de federação  
  
Quando você deseja especificar qual usuário ou usuários podem criar uma relação de confiança de proxy para determinado Serviço de Federação, você pode usar qualquer um dos seguintes métodos de delegação. Essa lista de métodos está em ordem de prioridade, com base nas recomendações da equipe do produto AD FS dos métodos de delegação mais seguros e menos problemáticos. É necessário usar apenas um desses métodos, dependendo das necessidades da sua organização:  
  
1.  Crie um grupo de segurança de domínio \(em Active Directory por exemplo\), FSProxyTrustCreators, adicione esse grupo ao grupo local de administradores em cada um dos servidores de Federação no farm e, em seguida, adicione somente as contas de usuário às quais você deseja para delegar esse direito ao novo grupo. Essa é a opção preferencial.  
  
2.  Adicione a conta de domínio do usuário ao grupo de administradores em cada um dos servidores de Federação no farm.  
  
3.  Se por alguma razão você não pode usar nenhum desses métodos, também pode criar uma regra de autorização para essa finalidade. Embora não seja recomendado, devido a possíveis complicações que poderão ocorrer se essa regra não estiver escrita corretamente, você pode usar uma regra de autorização personalizada para delegar quais domínio do Active Directory as contas de usuário também podem criar ou até mesmo remover relações de confiança entre todos os proxies do servidor de federação associados a determinado Serviço de Federação.  
  
    Se você escolher o método 3, poderá usar a seguinte sintaxe de regra para emitir uma declaração de autorização que permitirá a \(um usuário especificado, nesse\\caso,\) a contoso Frank para criar relações de confiança para um ou mais proxies de servidor de Federação para o Serviço de Federação. Você deve aplicar essa regra usando o comando do Windows **PowerShell\-Set adfsproperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Posteriormente, se você quiser remover o usuário para que ele não possa mais criar relações de confiança de proxy, poderá reverter para a regra de autorização de confiança de proxy padrão para remover o direito de usuário de criar relações de confiança do proxy para o Serviço de Federação. Você também deve aplicar essa regra usando o comando do Windows **PowerShell\-Set adfsproperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Para obter mais informações sobre como usar o idioma da regra de declaração, consulte [a função do idioma da regra de declaração](The-Role-of-the-Claim-Rule-Language.md).  
  

