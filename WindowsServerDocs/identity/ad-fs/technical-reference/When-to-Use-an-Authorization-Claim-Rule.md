---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: "Quando usar uma regra de declaração de autorização"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-an-authorization-claim-rule"></a>Quando usar uma regra de declaração de autorização
Você pode usar essa regra em serviços de Federação do Active Directory \(AD FS\) quando você precisa levar um tipo de declaração de entrada e, em seguida, aplicar uma ação que irá determinar se um usuário será permitido ou negado acesso com base no valor que você especificar na regra. Quando você usa essa regra, você passa por ou transforma declarações que correspondem a lógica de regra a seguir, com base em uma das opções que você configurar na regra:  
  
|Opção de regra|Lógica de regra|  
|---------------|--------------|  
|Permitir que todos os usuários|Se for igual a entrada tipo de declaração *qualquer reivindicação tipo* e o valor é igual a *qualquer valor*, em seguida, o problema reivindicar com o valor é igual a *permitir*|  
|Permitir o acesso aos usuários com essa declaração de entrada|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *especificado reivindicação valor*, em seguida, o problema reivindicar com o valor é igual a *permitir*|  
|Negar acesso aos usuários com essa declaração de entrada|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *especificado reivindicação valor*, em seguida, o problema reivindicar com o valor é igual a *negar*|  
  
As seções a seguir fornecem uma introdução básica para reivindicar regras e fornecer mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma declaração de entrada, aplicar uma condição a ele \ (se x y\ then) e produzir uma declaração de saída com base nos parâmetros condição. A lista a seguir descreve dicas que você deve conhecer reivindicar regras antes de ler ainda mais importante neste tópico:  
  
-   No AD FS gerenciamento snap\-in, regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
  
-   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência nas regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
  
-   Modelos de regra de declaração sempre exigirá que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="permit-all-users"></a>Permitir que todos os usuários  
Quando você usa o modelo de regra permitir que todos os usuários, todos os usuários terão acesso à parte confiável. No entanto, você pode usar regras de autorização adicional para restringir o acesso. Se uma regra permite que um usuário para acessar o terceiro e outra regra nega o acesso do usuário para a parte confiante, o resultado de negação substituirá o resultado de permissão e o usuário tem acesso negado.  
  
Os usuários que têm o acesso permitidos para a parte do serviço de federação podem ainda ser negados serviço pela terceira parte.  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>Permitir o acesso aos usuários com essa declaração de entrada  
Quando você usa a permitir ou negar a usuários com base em um modelo de regra reivindicar recebidas para criar uma regra e definir a condição para permitir, você pode permitir acesso do usuário específicas à parte confiável com base no tipo de valor de uma declaração de entrada. Por exemplo, você pode usar esse modelo de regra para criar uma regra que permitirá que somente os usuários que têm um grupo reivindicar com um valor de administradores de domínio. Se uma regra permite que um usuário para acessar o terceiro e outra regra nega o acesso do usuário para a parte confiante, o resultado de negação substituirá o resultado de permissão e o usuário tem acesso negado.  
  
Os usuários que têm permissão para acessar a terceira parte do serviço de federação podem ainda ser negados serviço pela terceira parte. Se você quiser permitir que todos os usuários acessem o terceiro, use o modelo de regra permitir que todos os usuários.  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>Negar acesso aos usuários com essa declaração de entrada  
Quando você usa a permitir ou negar a usuários com base em um modelo de regra reivindicar recebidas para criar uma regra e definir a condição de negação, você pode negar o acesso do usuário para a parte confiante com base no tipo de valor de uma declaração de entrada. Por exemplo, você pode usar esse modelo de regra para criar uma regra que negará a declaração de todos os usuários que têm um grupo com um valor de usuários de domínio.  
  
Se você quiser usar a condição de negação, mas também permitem acesso à parte confiável para usuários específicos, você deve adicionar mais tarde explicitamente as regras de autorização com a condição de permissão para permitir que os usuários acessem o terceiro.  
  
Se um usuário tem acesso negado quando o mecanismo de emissão requerimentos judiciais ou Extrajudiciais processa o conjunto de regras, outras regras de processamento é desligado e AD FS retorna um erro "Acesso negado" para a solicitação do usuário.  
  
## <a name="authorizing-users"></a>Autorizar usuários  
No AD FS, regras de autorização são usadas para emitir uma permissão ou negação reivindicação que irá determinar se um usuário ou um grupo de usuários \ (dependendo do used\ de tipo de declaração) terão permissão para acessar os recursos baseados em Web\ em um determinado terceiro ou não. Regras de autorização só podem ser definidas na terceira relações de confiança de terceiros.  
  
### <a name="authorization-rule-sets"></a>Conjuntos de regras de autorização  
Conjuntos de regra de autorização diferente existem dependendo do tipo de permissão ou negação operações que você precisa configurar. Esses conjuntos de regras incluem:  
  
-   **Regras de autorização de emissão**: essas regras determinam se um usuário pode receber reclamações por um terceiro e, portanto, o acesso à parte confiável.  
  
-   **Regras de autorização de delegação**: essas regras determinam se um usuário pode atuar como outro usuário à parte confiável. Quando um usuário está atuando como outro usuário, declarações sobre o usuário solicitante ainda são colocadas no token.  
  
-   **Regras de autorização de representação**: essas regras determinam se um usuário totalmente pode representar outro usuário à parte confiável. Se passando por outro usuário é uma funcionalidade muito poderosa, pois o terceiro não saberá que o usuário está sendo representado.  
  
Para obter mais detalhes sobre como o processo de regra de autorização se encaixa o pipeline de emissão requerimentos judiciais ou Extrajudiciais, consulte a função do mecanismo de emissão de declarações.  
  
### <a name="supported-claim-types"></a>Tipos de declaração com suporte  
AD FSdefines dois tipos de declaração que são usados para determinar se um usuário é permitido ou negado. Esses identificadores de recurso uniforme \(URIs\) são do tipo de solicitação:  
  
1.  **Permitir**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **Negar**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você pode criar ambas as regras de autorização usando a linguagem de regra de declaração ou usando o **permitir que todos os usuários** modelo de regra ou o **permitir ou negar os usuários com base em uma declaração de entrada** modelo de regra no AD FS gerenciamento snap\-in. O modelo de regra permitir que todos os usuários não fornece opções de configuração. No entanto, a permitir ou negar os usuários com base em um modelo de regra de declaração de entrada oferece as seguintes opções de configuração:  
  
-   Especifique um nome de regra de declaração  
  
-   Especificar um tipo de declaração de entrada  
  
-   Digite um valor de declaração de entrada  
  
-   Permitir o acesso aos usuários com essa declaração de entrada  
  
-   Negar acesso aos usuários com essa declaração de entrada  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para permitir que todos os usuários](https://technet.microsoft.com/library/ee913577.aspx) ou [criar uma regra para permitir ou negar a usuários com base em uma declaração de entrada](https://technet.microsoft.com/library/ee913594.aspx) no guia de implantação do AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Se uma reivindicação deve ser enviada apenas quando o valor da declaração corresponde a um padrão personalizado, você deve usar uma regra personalizada. Para obter mais informações, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>Exemplo de como criar uma regra de autorização com base em várias declarações  
Ao usar a sintaxe da linguagem reivindicação regra para autorizar requerimentos judiciais ou Extrajudiciais, uma reivindicação também pode ser emitida com base na presença de várias declarações em declarações original do usuário. A seguinte regra emite uma declaração de autorização somente se o usuário é um membro dos editores de grupo e foi autenticado usando a autenticação do Windows:  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>Exemplo de como criar autorização regras que delega quem pode criar ou remover relações de confiança do federação servidor proxy  
Antes de um serviço de Federação pode usar um proxy de servidor de federação para redirecionar solicitações de cliente, uma relação de confiança deve primeiro ser estabelecida entre o serviço de Federação e o computador de proxy do servidor de Federação. Por padrão, é estabelecida uma relação de confiança do proxy quando qualquer uma das credenciais seguintes é fornecido com êxito no AD FS federação servidor Proxy Configuration Wizard:  
  
-   A conta de serviço, usada pelo serviço de federação, que protege o proxy  
  
-   Uma conta de domínio do Active Directory é um membro do grupo Administradores local em todos os servidores de Federação um farm de servidores de Federação  
  
Quando você quiser especificar quais usuários ou os usuários podem criar uma relação de confiança de proxy para um determinado serviço de federação, você pode usar qualquer um dos seguintes métodos de delegação. Esta lista de métodos é em ordem de prioridade, com base nas recomendações da equipe de produto do AD FS dos métodos mais seguras e menos problemáticas da delegação. É necessário usar somente um desses métodos, dependendo das necessidades da sua organização:  
  
1.  Criar um grupo de segurança de domínio no Active Directory \ (por exemplo, FSProxyTrustCreators\), adicione esse grupo ao grupo local Administradores em cada um dos servidores de federação no farm e, em seguida, adicione apenas as contas de usuário ao qual você deseja delegar esse direito ao novo grupo. Este é o método preferencial.  
  
2.  Adicione conta de domínio do usuário ao grupo Administradores em cada um dos servidores de federação no farm.  
  
3.  Se por algum motivo, você não pode usar qualquer um desses métodos, você também pode criar uma regra de autorização para essa finalidade. Embora isso não é recomendado, por causa de possíveis complicações que podem ocorrer se essa regra não está escrita corretamente, você pode usar uma regra de autorização personalizada para delegar quais Active Directory contas de usuário de domínio podem também criar ou remover até mesmo as relações de confiança entre todos os os proxies de servidor de federação que estão associados um determinado serviço de Federação.  
  
    Se você escolher método 3, você pode usar a seguinte sintaxe de regra para emitir uma declaração de autorização que permitirá que um usuário específico \ (nesse caso, contoso\\frankm\) para criar relações de confiança para um ou mais federação proxies de servidor para o serviço de Federação. Você deve aplicar essa regra usando o comando do Windows PowerShell **Set \ ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    Mais tarde, se você quiser remover o usuário para que o usuário não pode criar relações de confiança do proxy, você pode reverter para a regra de autorização de confiança de proxy padrão para remover o direito de usuário criar relações de confiança de proxy para o serviço de Federação. Você também deve aplicar essa regra usando o comando do Windows PowerShell **Set \ ADFSProperties AddProxyAuthorizationRules**.  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
Para obter mais informações sobre como usar a linguagem de regra de declaração, veja [a função da linguagem de regra reivindicar](The-Role-of-the-Claim-Rule-Language.md).  
  

