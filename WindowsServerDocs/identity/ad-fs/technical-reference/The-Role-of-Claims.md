---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: A função das declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: eb41b8168024a231282716e5edd0bc59554d7da6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937819"
---
# <a name="the-role-of-claims"></a>A função das declarações

No modelo de \- identidade baseado em declarações, as declarações desempenham uma função dinâmica no processo de Federação, que são o principal componente pelo qual o resultado de todas as \- solicitações de autorização e autenticação baseadas na Web são determinados. Esse modelo permite que as organizações projetem, de forma segura, identidade digital e direitos de titularidade, ou *declarações*, através dos limites de segurança e da empresa, de forma padronizada.

## <a name="what-are-claims"></a>O que são declarações?

Em sua forma mais simples, as declarações são simplesmente *instruções* , \( por exemplo, nome, identidade, grupo \) , feitas sobre os usuários, que são usados principalmente para autorizar o acesso a \- aplicativos baseados em declarações localizados em qualquer lugar da Internet. Cada instrução corresponde a um *valor* que é armazenado na declaração.

### <a name="how-claims-are-sourced"></a>Como as declarações são originadas

O Serviço de Federação no Serviços de Federação do Active Directory (AD FS) \( AD FS \) define quais declarações são trocadas entre parceiros federados. No entanto, antes de fazer isso, ele deve primeiro preencher ou criar a declaração com um valor recuperado ou um valor calculado. Cada valor de declaração representa um valor de um usuário, grupo ou entidade e é originado em uma das duas maneiras:

1.  Quando o valor que compõe a declaração é recuperado de um repositório de atributos, por exemplo, quando um valor de atributo do Departamento de Vendas é recuperado das propriedades de uma conta de usuário do Active Directory. Para mais informações, consulte [A função dos repositórios de atributo](The-Role-of-Attribute-Stores.md).

2.  Quando o valor de uma declaração de entrada é transformado em outro valor com base na lógica expressa em uma regra. Por exemplo, quando uma declaração de entrada com o valor Admins. do Domínio é transformada em um novo valor de administradores antes de ser enviada como uma declaração de saída. Para mais informações, consulte [A função das regras de declaração](The-Role-of-Claim-Rules.md).

As declarações podem incluir valores como endereço de \- email, UPN de nome principal \( \) de usuário, associação de grupo e outros atributos de conta.

### <a name="how-claims-flow"></a>Como as declarações funcionam

Outras partes dependem dos valores das declarações para executar tarefas de autorização para \- aplicativos baseados na Web que eles hospedam. Essas partes são conhecidas como *partes confiantes* no snap-in de gerenciamento de AD FS \- . O serviço de Federação é responsável por intermediar a confiança entre muitas partes diferentes. Ele foi projetado para processar e fluir a troca confiável de declarações de uma organização que inicialmente origina as declarações, também conhecidas como *provedores de declarações* no snap-in de gerenciamento de AD FS \- , para uma terceira parte confiável. Uma terceira parte confiável, então, usa essas declarações para tomar decisões de autorização.

O fluxo de declarações que usa este processo é conhecido como *pipeline de declarações*. Há três etapas no fluxo de declarações através do pipeline de declarações:

1.  As declarações recebidas do provedor de declarações são processadas pelas regras de transformação de aceitação no objeto de confiança no provedor de declarações. Essas regras determinam quais declarações são aceitas do provedor de declarações.

2.  A saída das regras de transformação de aceitação é utilizada como entrada para as regras de autorização de emissão. Essas regras determinam se o usuário tem permissão para acessar a terceira parte confiável.

3.  A saída das regras de transformação de aceitação é utilizada como entrada para as regras de transformação de emissão. Essas regras determinam as declarações que serão enviadas para a terceira parte confiável.

Para mais informações, consulte [A função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md)

### <a name="how-claims-are-issued"></a>Como as declarações são emitidas

Quando você escreve regras de declaração, a origem das declarações de entrada para as regras de declaração varia dependendo se você está escrevendo regras em um objeto de confiança do provedor de declarações ou em um objeto de confiança de terceira parte confiável. Quando você escreve regras para um objeto de confiança do provedor de declarações, as declarações de entrada são as reivindicações enviadas do provedor de declarações confiável para o Serviço de Federação. Quando você escreve regras para um objeto de confiança de terceira parte confiável, as declarações de entrada são as declarações de saída das regras de declaração do objeto de confiança do provedor de declarações. Para mais informações sobre declarações de entrada e declarações de saída, consulte [A função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md) e [A função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md).

## <a name="what-are-claim-types"></a>Quais são os tipos de declaração?

Um tipo de declaração fornece o contexto para o valor da declaração. Geralmente é expresso como um URI de \( Uniform Resource Identifier \) . O AD FS pode dar suporte a qualquer tipo de declaração e está configurado com os tipos de declaração na tabela a seguir por padrão.

|Nome|Descrição|URI|
|--------|---------------|-------|
|\-Endereço de email|O \- endereço de email e do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ solicitações de identidade \/ \/ EmailAddress|
|Nome|O nome fornecido do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declarações de identidade \/ \/ fornecidas|
|Nome|O nome exclusivo do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ \/ nome das declarações de identidade \/|
|UPN|O \( UPN do nome principal \) do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declaração de identidade \/ \/ UPN|
|Nome comum|O nome comum do usuário|http: \/ \/schemas.xmlSOAP.org \/ Claims \/ Common|
|Endereço de email do AD FS 1. x \-|O \- endereço de email e do usuário ao interoperar com AD FS 1,1 ou ADFS 1,0|http: \/ \/schemas.xml\/ EmailAddress de declarações SOAP.org \/|
|Agrupar|Um grupo do qual o usuário é membro|http: \/ \/schemas.xml\/ grupo de declarações SOAP.org \/|
|AD FS 1.x UPN|O UPN do usuário ao interagir com AD FS 1.1 ou ADFS 1.0|http: \/ \/schemas.xml\/ UPN de declarações SOAP.org \/|
|Função|Uma função que o usuário tenha|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ \/ função de declarações de identidade \/|
|Sobrenome|O sobrenome do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declarações de identidade \/ \/ sobrenome|
|PPID|O identificador privado do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declarações de identidade \/ \/ privatepersonalidentifier|
|Identificador do Nome|O identificador de nome SAML do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declarações de identidade \/ \/ nameidentifier|
|Método de autenticação|O método usado para autenticar o usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ Identity \/ Claims \/ AuthenticationMethod|
|SID do grupo somente negar|O \- Sid de grupo somente negação do usuário|http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ declarações de identidade \/ \/ DenyOnlySid|
|SID primário somente negar|O \- Sid principal somente negar do usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ declarações de identidade \/ \/ denyonlyprimarysid|
|SID de grupo primário somente negar|O \- SID do grupo primário somente negação do usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ declarações de identidade \/ \/ denyonlyprimarygroupsid|
|SID de grupo|O SID de grupo do usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ Identity \/ Claims \/ GroupId|
|SID de grupo primário|O SID de grupo primário do usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ declarações de identidade \/ \/ primarygroupsid|
|SID primário|O SID primário do usuário|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ declarações de identidade \/ \/ PrimarySid|
|Nome da conta Windows|O nome da conta de domínio do usuário na forma de\<domain\>\\\<user\>|http: \/ \/ schemas.microsoft.com \/ WS \/ 2008 \/ 06 \/ declarações de identidade \/ \/ windowsaccountname|

## <a name="what-are-claim-descriptions"></a>O que são descrições de declaração?

Descrições de declaração representam uma lista de tipos de declarações que AD FS dá suporte e que podem ser publicadas em metadados de Federação. Os tipos de declaração mencionados na tabela anterior são configurados como descrições de declarações no snap-in de gerenciamento de AD FS \- .

A coleção de descrições de declaração que será publicada nos metadados da federação é armazenada no banco de dados de configuração do AD FS. Essas descrições de declaração são usadas por vários componentes do Serviço de Federação.

Cada descrição de declaração inclui um tipo de declaração URI, nome, estado de publicação e descrição. Você pode gerenciar a coleção de descrição de declaração usando o nó **descrições de declaração** no snap-in gerenciamento de AD FS \- . Você pode modificar o estado de publicação de uma descrição de declaração usando o snap- \- in. As seguintes configurações estão disponíveis:

- **Publicar esta declaração em metadados de Federação como um tipo de declaração que esse serviço de Federação pode aceitar** \( Publicar como aceito \) – indica os tipos de declaração que serão aceitos de outros provedores de declarações por este serviço de Federação.

- **Publicar esta declaração em metadados de Federação como um tipo de declaração que este serviço de Federação pode enviar** \( Publicar como enviado \) – indica os tipos de declaração que são oferecidos por essa serviço de Federação. Estes são os tipos de declaração que o Serviço de Federação publica para outros como sendo aqueles que está disposto a enviar. Os tipos de declaração reais enviados pelo provedor de declarações muitas vezes são um subconjunto dessa lista.

Para obter mais informações sobre como definir o estado de publicação de um tipo de declaração, consulte [Adicionar uma descrição de declaração](../operations/add-a-claim-description.md) no guia de implantação de AD FS.

### <a name="when-generating-federation-metadata"></a>Quando gerar metadados de federação

Os Metadados de Federação incluem todas as descrições de declaração marcadas para publicação.

### <a name="when-claims-rules-are-processed"></a>Quando as regras de declaração são processadas

Quando você mantém informações de configuração sobre descrições de declaração, é mais fácil configurar as regras sobre declarações. Para mais informações sobre regras de declaração que podem ser usadas ​​na organização do provedor de declarações, consulte [A função das regras de declaração](The-Role-of-Claim-Rules.md).
