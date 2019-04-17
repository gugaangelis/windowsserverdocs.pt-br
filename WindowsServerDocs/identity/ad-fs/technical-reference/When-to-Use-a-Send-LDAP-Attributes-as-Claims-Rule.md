---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: "Quando usar um atributos LDAP enviar como declarações de regra"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quando usar um atributos LDAP enviar como declarações de regra
Você pode usar essa regra em serviços de Federação do Active Directory \(AD FS\) quando quiser emitir declarações de saída que contêm valores de atributo Lightweight Directory Access Protocol \(LDAP\) real que existem em um repositório de atributo e, em seguida, associar um tipo de declaração cada um dos atributos LDAP. Para obter mais informações sobre repositórios de atributo, consulte [função The do atributo armazena](The-Role-of-Attribute-Stores.md).  
  
Quando você usa essa regra, você emitir uma declaração para cada atributo LDAP que você especificar e que corresponde a lógica de regra, conforme descrito na tabela a seguir.  
  
|Opção de regra|Lógica de regra|  
|---------------|--------------|  
|Mapeamento de atributos LDAP para tipos de declaração de saída|Se for igual a loja de atributo *store atributo especificado* e atributo LDAP é igual *especificado valor*, mapeie o valor do atributo LDAP para o *declaração de saída especificada* digite e execute a declaração.|  
  
As seções a seguir fornecem uma introdução básica de declaração regras. Eles também fornecem detalhes sobre quando usar os atributos de LDAP enviar como regra requerimentos judiciais ou Extrajudiciais.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma declaração de entrada, aplicar uma condição a ele \ (se x y\ then) e produzir uma declaração de saída com base nos parâmetros condição. A lista a seguir descreve dicas que você deve conhecer reivindicar regras antes de ler ainda mais importante neste tópico:  
  
-   No AD FS gerenciamento snap\-in, regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
  
-   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência nas regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
  
-   Modelos de regra de declaração sempre exigirá que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mapeamento de atributos LDAP para tipos de declaração de saída  
Quando você usa os atributos de LDAP enviar como modelo de regra de requerimentos judiciais ou Extrajudiciais, você pode selecionar atributos de um repositório de atributo LDAP, como Active Directory ou Active Directory Domain Services \(AD DS\) para enviar seus valores como declarações para a parte confiante. Isso é mapeado essencialmente atributos LDAP específicos de um repositório de atributo que definem a um conjunto de declarações de saída que pode ser usado para autorização.  
  
Usando esse modelo, você pode adicionar vários atributos, que serão enviados como várias declarações, de uma única regra. Por exemplo, você pode usar esse modelo de regra para criar uma regra que ficarão valores de atributo para usuários autenticados do **empresa** e **departamento** Active Directory atributos e, em seguida, enviar esses valores como duas declarações de saída diferentes.  
  
Você também pode usar essa regra para enviar os membros do grupo Todos do usuário. Se você quiser enviar somente membros do grupo individuais, use a associação do grupo Enviar como um modelo de regra de declaração. Para obter mais informações, consulte [quando usar uma associação de grupo Enviar como uma regra de declaração](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você pode criar essa regra usando a linguagem de regra de declaração ou usando os atributos de LDAP enviar como declarações de modelo no AD FS snap\-in Gerenciamento de regras. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especifique um nome de regra de declaração  
  
-   Selecione um repositório de atributo dos quais extrair atributos LDAP  
  
-   Mapeamento de atributos LDAP para tipos de declaração de saída  
  
Para obter mais informações sobre como criar essa regra, consulte [criar uma regra para enviar LDAP atributos como declarações](https://technet.microsoft.com/library/dd807115.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Se a consulta a ser \(AD LDS\) do Active Directory, o AD DS ou o Active Directory Lightweight Directory Services deve comparar com um atributo LDAP diferente **samAccountname**, você deve usar uma regra personalizada em vez disso. Se não houver nenhuma declaração de nome de conta do Windows no conjunto de entrada, você também deve usar uma regra personalizada para especificar a declaração para ser usados para consultar o AD DS ou AD LDS.  
  
Os exemplos a seguir são fornecidos para ajudá-lo a entender algumas das várias maneiras que você pode agora construir uma regra personalizada usando a linguagem de regra de declaração a consulta e extraia dados em um repositório de atributo.  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>Exemplo: Como um repositório de atributo do AD LDS de consulta e retornar um valor especificado  
Parâmetros devem ser separados por um ponto e vírgula. O primeiro parâmetro é o filtro LDAP. Parâmetros subsequentes são os atributos para retornar em todos os objetos correspondentes.  
  
O exemplo a seguir mostra como procurar um usuário pelo **sAMAccountName** atributo e emitir uma declaração de endereço de email e\, usando o valor do atributo de email do usuário:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
O exemplo a seguir mostra como procurar um usuário pelo **mail** atributo e emitir declarações do título e o nome de exibição, usando os valores do usuário **título** e **displayname** atributos:  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
O exemplo a seguir mostra como procurar um usuário por email e o título e, em seguida, execute uma declaração de nome de exibição usando o usuário **displayname** atributo:  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>Exemplo: Como consultar um repositório de atributo do Active Directory e retornar um valor especificado  
A consulta do Active Directory deve incluir o nome do usuário \ (com o domínio name\) como o parâmetro final para que o atributo do Active Directory armazenar pode consultar o domínio correto. Caso contrário, a mesma sintaxe é compatível.  
  
O exemplo a seguir mostra como procurar um usuário pelo **sAMAccountName** o atributo no seu domínio e, em seguida, retornar o **mail** atributo:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Exemplo: Como consultar um repositório de atributo do Active Directory com base no valor de uma declaração de entrada  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
A consulta anterior é composta por três partes:  
  
-   O filtro LDAP — você especificar essa parte da consulta para recuperar os objetos para os quais você deseja consultar os atributos. Para obter informações gerais sobre consultas LDAP válidas, consulte RFC 2254. Quando você está consultando um repositório de atributo do Active Directory e você não especificar um filtro LDAP, o samAccountName\ = {0} consulta é considerada e armazenamento de atributo do Active Directory espera um parâmetro que pode alimentar o valor de {0}. Caso contrário, a consulta resultará em erro. Para um repositório de atributo LDAP que não sejam do Active Directory, você não pode omitir a parte de filtro LDAP da consulta ou a consulta resultará em erro.  
  
-   Especificação de atributo — nesta segunda parte da consulta, você especifica os atributos \ (que são separados comma\ se você usar vários atributo values\) que você quer fora os objetos filtrados. O número de atributos que você especificar deve corresponder ao número de tipos de declaração que você define na consulta.  
  
-   Domínio do Active Directory — você especificar a última parte da consulta somente quando o repositório de atributo é do Active Directory. \ (Não é necessário quando você consulta outras lojas de atributo. \) essa parte da consulta é usada para especificar a conta de usuário no domain\\name formulário. Armazenamento de atributo do Active Directory usa a parte de domínio para determinar o controlador de domínio apropriado para se conectar a e execute a consulta e solicitar os atributos.  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>Exemplo: Como usar duas regras personalizadas para extrair o e\-mensagens do Gerenciador de um atributo no Active Directory  
As seguintes duas regras personalizadas, quando usados juntos na ordem mostrada abaixo, consultar o Active Directory para o **Gerenciador** atributo do usuário \(Rule 1\) da conta e, em seguida, usar esse atributo para consultar a conta de usuário do Gerenciador para o **mail** \(Rule 2\) de atributo. Por fim, o **mail** atributo é emitido como a declaração de "ManagerEmail". Em resumo, regra 1 consulta o Active Directory e passa o resultado da consulta para a regra 2, que extrai, em seguida, o Gerenciador de valores e\ mail.  
  
Por exemplo, quando essas regras término da execução, uma declaração é emitida que contém o endereço de email e\ do Gerenciador de um usuário no domínio corp.fabrikam.com.  
  
**Regra 1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**Regra 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> Essas regras funcionam somente se o Gerenciador do usuário está no mesmo domínio que o usuário \ (corp.fabrikam.com nesse example\).  
  
## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar atributos LDAP como declarações](https://technet.microsoft.com/library/dd807115.aspx)  
  

