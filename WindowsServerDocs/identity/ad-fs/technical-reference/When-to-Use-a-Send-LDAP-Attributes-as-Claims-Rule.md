---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: Quando usar uma regra Enviar Atributos LDAP como Declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4ba491f9fa0bee4a92cba8667ff0cf28cf7e8e52
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958714"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>Quando usar uma regra Enviar Atributos LDAP como Declarações
Você pode usar essa regra em Serviços de Federação do Active Directory (AD FS) \( AD FS \) quando desejar emitir declarações de saída que contenham valores reais de atributo LDAP do protocolo de acesso de diretório Lightweight \( \) , que existem em um repositório de atributos e, em seguida, associar um tipo de declaração a cada um dos atributos LDAP. Para obter mais informações sobre repositórios de atributos, consulte [a função de repositórios de atributos](The-Role-of-Attribute-Stores.md).

Quando você usa essa regra, emite uma declaração para cada atributo LDAP especificado e que corresponde à lógica da regra, conforme descrito na tabela a seguir.

|Opção de regras|Lógica de regras|
|---------------|--------------|
|Mapeamento de atributos LDAP para tipos de declaração de saída|Se o repositório de atributos é igual ao *repositório de atributos especificado* e o atributo LDAP é igual ao *valor especificado*, mapeie o valor do atributo LDAP para o tipo de *declaração de saída especificada* e emita a declaração.|

As seções a seguir fornecem uma introdução básica às regras de declaração. Elas também fornecem detalhes sobre quando usar uma regra Enviar Atributos LDAP como Declarações.

## <a name="about-claim-rules"></a>Sobre as regras de declaração
Uma regra de declaração representa uma instância da lógica de negócios que usará uma declaração de entrada, aplicará uma condição a ela \( se x depois y \) e produzir uma declaração de saída com base nos parâmetros de condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:

-   No snap in de gerenciamento de AD FS \- , as regras de declaração só podem ser criadas usando modelos de regra de declaração

-   As regras de declaração processam declarações de entrada diretamente de um provedor de declarações \( , como Active Directory ou outro serviço de Federação \) ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.

-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.

-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.

Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).

## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>Mapeamento de atributos LDAP para tipos de declaração de saída
Ao usar o modelo enviar atributos LDAP como regra de declarações, você pode selecionar atributos de um repositório de atributos LDAP, como Active Directory ou Active Directory Domain Services \( AD DS \) para enviar seus valores como declarações para a terceira parte confiável. Essencialmente, isso mapeia atributos LDAP específicos de um repositório de atributos definido para um conjunto de declarações de saída que pode ser usado para autorização.

Usando esse modelo, você pode adicionar vários atributos, que serão enviados como várias declarações, de uma única regra. Por exemplo, você pode usar esse modelo de regra para criar uma regra que pesquisará valores de atributo para os usuários autenticados por meio dos atributos do Active Directory **company** e **department** e enviará esses valores como duas declarações de saída diferentes.

Você também pode usar essa regra para enviar todas as associações de grupo do usuário. Se você desejar enviar apenas as associações de grupo individuais, use o modelo de regra Enviar Associação de Grupo como uma Declaração. Para obter mais informações, consulte [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).

## <a name="how-to-create-this-rule"></a>Como criar essa regra
Você pode criar essa regra usando o idioma da regra de declaração ou usando o modelo enviar atributos LDAP como regra de declarações no snap-in de gerenciamento de AD FS \- . Essa regra fornece as seguintes opções de configuração:

-   Especificar um nome de regra de declaração

-   Selecionar um repositório de atributos do qual extrair atributos LDAP

-   Mapeamento de atributos LDAP para tipos de declaração de saída

Para obter mais informações sobre como criar essa regra, consulte [criar uma regra para enviar atributos LDAP como declarações](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807115(v=ws.11)).

## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração
Se a consulta para Active Directory, AD DS ou Active Directory Lightweight Directory Services \( AD LDS \) deve comparar com um atributo LDAP diferente de **samAccountname**, você deve usar uma regra personalizada em vez disso. Se não houver nenhuma declaração de Nome da Conta do Windows no conjunto de entrada, você também deve usar uma regra personalizada para especificar a declaração a ser usada para consultar o AD DS ou AD LDS.

Os exemplos a seguir são fornecidos para ajudá-lo a entender algumas das várias maneiras que você pode construir uma regra personalizada usando a linguagem da regra de declaração para consultar e extrair dados em um repositório de atributos.

### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>Exemplo: como consultar um repositório de atributos do AD LDS e retornar um valor especificado
Os parâmetros devem ser separados por ponto e vírgula. O primeiro parâmetro é o filtro LDAP. Os parâmetros subsequentes são os atributos para retornar em quaisquer objetos correspondentes.

O exemplo a seguir mostra como pesquisar um usuário pelo atributo **sAMAccountName** e emitir uma \- declaração de endereço de email, usando o valor do atributo mail do usuário:

```
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));

```

O exemplo a seguir mostra como pesquisar um usuário pelo atributo de **email** e o título do problema e as declarações de nome de exibição, usando os valores dos atributos **title** e **DisplayName** do usuário:

```
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);
```

O exemplo a seguir mostra como pesquisar um usuário por email e título e, em seguida, emitir uma declaração de nome de exibição usando o atributo **DisplayName** do usuário:

```
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);
```

### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>Exemplo: como consultar um repositório de atributos do Active Directory e retornar um valor especificado
A consulta Active Directory deve incluir o nome do usuário \( com o nome de domínio \) como o parâmetro final para que o repositório de atributos Active Directory possa consultar o domínio correto. Caso contrário, a mesma sintaxe tem suporte.

O exemplo a seguir mostra como pesquisar um usuário pelo atributo **sAMAccountName** em seu domínio e, em seguida, retornar o atributo **mail**:

```
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);
```

### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>Exemplo: como consultar um repositório de atributos do Active Directory com base no valor de uma declaração de entrada

```
c:[Type == "http://test/name"]

   => issue(store = "Enterprise AD Attribute Store",

         types = ("http://test/email"),

         query = ";mail;{0}",

         param = c.Value)

```

A consulta anterior é composta pelas três partes a seguir:

-   O filtro LDAP: você especifica essa parte da consulta para recuperar os objetos para os quais deseja consultar os atributos. Para obter informações gerais sobre consultas LDAP válidas, consulte a RFC 2254. Quando você está consultando um repositório de atributos Active Directory e não especifica um filtro LDAP, a consulta samAccountName \= {0} é assumida e o armazenamento de atributo Active Directory espera um parâmetro que pode alimentar o valor para {0} . Caso contrário, a consulta resultará em um erro. Para um repositório de atributos LDAP que não o Active Directory, você não pode omitir a parte do filtro LDAP da consulta ou a consulta resultará em um erro.

-   Especificação de atributo — nesta segunda parte da consulta, você especifica os atributos \( que são \- separados por vírgula se você usar vários valores \) de atributo que deseja para fora dos objetos filtrados. O número de atributos especificados deve corresponder ao número de tipos de declaração que você define na consulta.

-   Domínio do Active Directory: você especifica a última parte da consulta somente quando o repositório de atributos é o Active Directory. \(Não é necessário quando você consulta outros repositórios de atributos. \) Essa parte da consulta é usada para especificar a conta de usuário no formato nome de domínio \\ . O repositório de atributos do Active Directory usa a parte do domínio para determinar o controlador de domínio apropriado ao qual se conectar e executar a consulta e solicitar os atributos.

### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>Exemplo: como usar duas regras personalizadas para extrair o gerente e o \- email de um atributo no Active Directory
As duas regras personalizadas a seguir, quando usadas em conjunto na ordem mostrada abaixo, consultam Active Directory para o atributo **Manager** da regra de conta de usuário \( 1 \) e, em seguida, usam esse atributo para consultar a conta de usuário do gerente para a regra de atributo de **email** \( 2 \) . Por fim, o atributo de **email** é emitido como uma declaração "ManagerEmail". Em resumo, a regra 1 consulta Active Directory e passa o resultado da consulta para a regra 2, que, em seguida, extrai os valores do gerente e de \- email.

Por exemplo, quando essas regras terminam de executar, é emitida uma declaração que contém o \- endereço de email do gerente para um usuário no domínio corp.fabrikam.com.

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
> Essas regras só funcionarão se o gerente do usuário estiver no mesmo domínio que o usuário \( Corp.fabrikam.com neste exemplo \) .

## <a name="additional-references"></a>Referências adicionais
[Criar uma regra para enviar atributos LDAP como declarações](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807115(v=ws.11))

