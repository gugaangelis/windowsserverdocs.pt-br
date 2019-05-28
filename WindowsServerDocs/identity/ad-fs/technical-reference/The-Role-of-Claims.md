---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: A função das declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 542d7a24e29b52dd3fa0d7ea6a9b2d27fb620d8d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188496"
---
# <a name="the-role-of-claims"></a>A função das declarações
Nas declarações\-modelo de identidade baseado em declarações desempenham um papel fundamental no processo de federação, eles são o componente de chave pelo qual o resultado de todas as Web\-solicitações com base em autenticação e autorização são determinadas. Esse modelo permite que as organizações projetem, de forma segura, identidade digital e direitos de titularidade, ou *declarações*, através dos limites de segurança e da empresa, de forma padronizada.  
  
## <a name="what-are-claims"></a>O que são declarações?  
Em sua forma mais simples, declarações são simplesmente *instruções* \(por exemplo, nome, identidade, grupo\), tomadas sobre usuários, que são usados principalmente para autorizar o acesso às declarações\-com base em aplicativos localizado em qualquer lugar na Internet. Cada instrução corresponde a um *valor* que é armazenado na declaração.  
  
### <a name="how-claims-are-sourced"></a>Como as declarações são originadas  
O serviço de federação nos serviços de Federação do Active Directory \(do AD FS\) define quais declarações são trocadas entre os parceiros federados. No entanto, antes de fazer isso, ele deve primeiro preencher ou criar a declaração com um valor recuperado ou um valor calculado. Cada valor de declaração representa um valor de um usuário, grupo ou entidade e é originado em uma das duas maneiras:  
  
1.  Quando o valor que compõe a declaração é recuperado de um repositório de atributos, por exemplo, quando um valor de atributo do Departamento de Vendas é recuperado das propriedades de uma conta de usuário do Active Directory. Para obter mais informações, consulte [The Role of Attribute Stores](The-Role-of-Attribute-Stores.md).  
  
2.  Quando o valor de uma declaração de entrada é transformado em outro valor com base na lógica expressa em uma regra. Por exemplo, quando uma declaração de entrada com o valor Admins. do Domínio é transformada em um novo valor de administradores antes de ser enviada como uma declaração de saída. Para mais informações, consulte [A função das regras de declaração](The-Role-of-Claim-Rules.md).  
  
Declarações podem incluir valores como um eletrônico\-endereço, nome UPN de email \(UPN\), associação de grupo e outros atributos da conta.  
  
### <a name="how-claims-flow"></a>Como as declarações funcionam  
Outras partes contam com os valores das declarações para executar tarefas de autorização para Web\-com base em aplicativos que eles hospedam. Essas partes são referidas como *terceiras* no snap do gerenciamento do AD FS\-no. O serviço de Federação é responsável por intermediar a confiança entre muitas partes diferentes. Ele é projetado para processar e fluir a troca confiável de declarações de uma organização que inicialmente cria as declarações, também conhecidas como *provedores de declarações* no snap do gerenciamento do AD FS\-além, para uma terceira parte confiável. Uma terceira parte confiável, então, usa essas declarações para tomar decisões de autorização.  
  
O fluxo de declarações que usa este processo é conhecido como *pipeline de declarações*. Há três etapas no fluxo de declarações através do pipeline de declarações:  
  
1.  As declarações recebidas do provedor de declarações são processadas pelas regras de transformação de aceitação no objeto de confiança no provedor de declarações. Essas regras determinam quais declarações são aceitas do provedor de declarações.  
  
2.  A saída das regras de transformação de aceitação é utilizada como entrada para as regras de autorização de emissão. Essas regras determinam se o usuário tem permissão para acessar a terceira parte confiável.  
  
3.  A saída das regras de transformação de aceitação é utilizada como entrada para as regras de transformação de emissão. Essas regras determinam as declarações que serão enviadas para a terceira parte confiável.  
  
Para mais informações, consulte [A função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Como as declarações são emitidas  
Quando você escreve regras de declaração, a origem das declarações de entrada para as regras de declaração varia dependendo se você está escrevendo regras em um objeto de confiança do provedor de declarações ou em um objeto de confiança de terceira parte confiável. Quando você escreve regras para um objeto de confiança do provedor de declarações, as declarações de entrada são as reivindicações enviadas do provedor de declarações confiável para o Serviço de Federação. Quando você escreve regras para um objeto de confiança de terceira parte confiável, as declarações de entrada são as declarações de saída das regras de declaração do objeto de confiança do provedor de declarações. Para obter mais informações sobre declarações de entrada e saída, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md) e [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Quais são os tipos de declaração?  
Um tipo de declaração fornece o contexto para o valor da declaração. Ele normalmente é expresso como um Uniform Resource Identifier \(URI\). O AD FS pode dar suporte a qualquer tipo de declaração e ele é configurado por padrão com os tipos de declaração na tabela a seguir.  
  
|Nome|Descrição|URI|  
|--------|---------------|-------|  
|E\-endereço de email|A e\-endereço do usuário de email|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|Nome fornecido|O nome fornecido do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|Nome|O nome exclusivo do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|O nome UPN \(UPN\) do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|Nome comum|O nome comum do usuário|http:\/\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x E\-endereço de email|A e\-email endereço do usuário ao interoperar com AD FS 1.1 ou ADFS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Grupo|Um grupo do qual o usuário é membro|http:\/\/schemas.xmlsoap.org\/claims\/Group|  
|AD FS 1.x UPN|O UPN do usuário ao interagir com AD FS 1.1 ou ADFS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/UPN|  
|Função|Uma função que o usuário tenha|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|Sobrenome|O sobrenome do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|O identificador privado do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificador de nome|O identificador de nome SAML do usuário|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|Método de autenticação|O método usado para autenticar o usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|SID do grupo somente negar|Negar\-apenas SID do usuário do grupo|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|SID primário somente negar|Negar\-apenas o SID primário do usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|SID de grupo primário somente negar|Negar\-apenas SID de grupo primário do usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID de grupo|O SID de grupo do usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|SID de grupo primário|O SID de grupo primário do usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID primário|O SID primário do usuário|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nome da conta Windows|O nome da conta de domínio do usuário na forma de \<domínio\>\\\<usuário\>|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>O que são descrições de declaração?  
As descrições de declaração representam uma lista de tipos de declarações dá suporte do AD FS e que podem ser publicados nos metadados da federação. Os tipos de declaração mencionados na tabela anterior são configurados como descrições de declaração no snap gerenciamento do AD FS\-no.  
  
A coleção de descrições de declaração que será publicada nos metadados da federação é armazenada no banco de dados de configuração do AD FS. Essas descrições de declaração são usadas por vários componentes do Serviço de Federação.  
  
Cada descrição de declaração inclui um tipo de declaração URI, nome, estado de publicação e descrição. Você pode gerenciar a coleção de descrições de declaração usando o **descrições de declaração** nó no snap do gerenciamento do AD FS\-no. Você pode modificar o estado da publicação de uma descrição de declaração usando o snap-\-no. As seguintes configurações estão disponíveis:  
  
-   **Publicar esta declaração nos metadados da federação como um tipo de declaração que este serviço de Federação pode aceitar** \(Publicar como aceito\)— indica os tipos de declaração que serão aceitos de outros provedores de declarações por essa federação Serviço.  
  
-   **Publicar esta declaração nos metadados de federação como um tipo de declaração que este serviço de Federação pode aceitar** \(Publicar como enviado\)— indica os tipos de declaração que são oferecidos por este serviço de Federação. Estes são os tipos de declaração que o Serviço de Federação publica para outros como sendo aqueles que está disposto a enviar. Os tipos de declaração reais enviados pelo provedor de declarações muitas vezes são um subconjunto dessa lista.  
  
Para obter mais informações sobre como definir o estado da publicação de um tipo de declaração, consulte [Add a Claim Description](https://technet.microsoft.com/library/dd807051.aspx) no guia de implantação do AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Quando gerar metadados de federação  
Os Metadados de Federação incluem todas as descrições de declaração marcadas para publicação.  
  
### <a name="when-claims-rules-are-processed"></a>Quando as regras de declaração são processadas  
Quando você mantém informações de configuração sobre descrições de declaração, é mais fácil configurar as regras sobre declarações. Para mais informações sobre regras de declaração que podem ser usadas ​​na organização do provedor de declarações, consulte [A função das regras de declaração](The-Role-of-Claim-Rules.md).  
  

