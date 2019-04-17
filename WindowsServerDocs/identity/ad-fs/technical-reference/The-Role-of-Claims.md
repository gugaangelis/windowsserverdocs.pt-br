---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: "A função de reivindicações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claims"></a>A função de reivindicações
No modelo de identidade baseado em claims\, requerimentos judiciais ou Extrajudiciais desempenham um papel crucial no processo de federação, eles são os componentes principais pelo qual o resultado de todas as solicitações de autenticação e autorização com base em Web\ são determinadas. Esse modelo permite que as organizações projetar com segurança identidade digital e direitos de direitos, ou *requerimentos judiciais ou Extrajudiciais*, em todos os limites de segurança e corporativos em uma maneira padronizada.  
  
## <a name="what-are-claims"></a>Quais são requerimentos judiciais ou Extrajudiciais?  
Em sua forma mais simples, as declarações são simplesmente *instruções* \ (por exemplo, nome, identidade, group\), tomadas sobre usuários, que são usados principalmente para autorizar o acesso a aplicativos com base em claims\ localizados em qualquer lugar na Internet. Cada instrução corresponde a um *valor* que é armazenado em reivindicação.  
  
### <a name="how-claims-are-sourced"></a>Como as declarações são originadas  
O serviço de Federação em serviços de Federação do Active Directory \(AD FS\) define quais declarações são trocadas entre parceiros federados. No entanto, antes que ele pode fazer isso ele deve primeiro popular ou a declaração com um valor recuperado ou um valor calculado de origem. Cada valor de declaração representa um valor de um usuário, grupo ou entidade e originado de duas maneiras:  
  
1.  Quando o valor que compõe a declaração é recuperado de um repositório de atributo, por exemplo, quando um valor de atributo do departamento de vendas é recuperado das propriedades de uma conta de usuário do Active Directory. Para obter mais informações, consulte [as lojas de função do atributo](The-Role-of-Attribute-Stores.md).  
  
2.  Quando o valor de uma declaração de entrada é transformado em outro valor com base na lógica expressada em uma regra. Por exemplo, quando uma declaração de entrada com o valor do grupo Administradores de domínio é transformada em um novo valor de administradores antes que ele é enviado como uma declaração de saída. Para obter mais informações, consulte [a função de declaração regras](The-Role-of-Claim-Rules.md).  
  
Requerimentos judiciais ou Extrajudiciais podem incluir valores como um endereço de email e\, UPN \(UPN\), associação de grupo e outros atributos de conta.  
  
### <a name="how-claims-flow"></a>Como declarações de fluxo  
Outras partes contam com os valores das declarações para executar tarefas de autorização para aplicativos baseados em Web\ que eles hospedam. Essas partes são chamados de *partes confiantes* no AD FS gerenciamento snap\-in. O serviço de Federação é responsável por intermediar confiança entre muitas partes diferentes. Ele foi projetado para processar e fluxo a troca confiável de declarações de uma organização que fontes inicialmente as declarações, também conhecidas como *declarações provedores* no AD FS gerenciamento snap\-in, a um terceiro. Um terceiro, em seguida, usa essas declarações para tomar decisões de autorização.  
  
O fluxo de declarações usando esse processo é conhecido como o *pipeline requerimentos judiciais ou Extrajudiciais*. Há três etapas no fluxo de declarações através do pipeline de declarações:  
  
1.  As declarações são recebidas do provedor de declarações são processadas pelas regras de transformação de aceitação na relação de confiança de provedor requerimentos judiciais ou Extrajudiciais. Essas regras determinar quais declarações são aceitos do provedor de declarações.  
  
2.  A saída das regras de transformação da aceitação é usada como entrada para as regras de autorização de emissão. Essas regras determinar se o usuário tem permissão para acessar o terceiro.  
  
3.  A saída das regras de transformação da aceitação é usada como entrada para as regras de transformação de emissão. Essas regras determinar as declarações que serão enviadas para o terceiro.  
  
Para obter mais informações, consulte [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Como as declarações são emitidas  
Quando você escreve reivindicação regras, a origem de declarações de entrada para as regras de declaração varia de acordo com se você estiver escrevendo regras em uma relação de confiança do provedor de declarações ou uma terceira confiança de terceiros. Quando você escreve regras de declaração para uma relação de confiança do provedor de declarações, declarações de entrada são as declarações enviadas do provedor de declarações confiável para o serviço de Federação. Quando você escreve regras para uma terceira confiança de terceiros, as declarações de entrada são as declarações são produzidos pelas regras de declaração da relação de confiança de provedor requerimentos judiciais ou Extrajudiciais aplicável. Para obter mais informações sobre declarações de entrada e saída, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md) e [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>Quais são os tipos de declaração?  
Um tipo de declaração fornece contexto para o valor da declaração. Ele geralmente é expresso como um Uniform Resource Identifier \(URI\). AD FS pode dar suporte a qualquer tipo de declaração e ele está configurado com os tipos de declaração na tabela a seguir por padrão.  
  
|Nome|Descrição|URI|  
|--------|---------------|-------|  
|Endereço de email E\|O endereço de email e\ do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/EmailAddress|  
|Nome determinado|O nome determinado do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenName|  
|Nome|O nome exclusivo do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Name|  
|UPN|A entidade de segurança do usuário nomear \(UPN\) do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/UPN|  
|Nome comum|O nome comum do usuário|http:///\/schemas.xmlsoap.org\/claims\/CommonName|  
|Endereço do AD FS 1. x E\-Mail|O endereço de email e\ do usuário quando interoperar com o AD FS 1.1 ou ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Grupo|Um grupo que o usuário é um membro do|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|UPN do AD FS 1. x|O UPN do usuário quando interoperar com o AD FS 1.1 ou ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|Função|Uma função que o usuário tenha|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/Role|  
|Sobrenome|O sobrenome do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|O identificador privado do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Nome de identificador|O identificador SAML do nome do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|Método de autenticação|O método usado para autenticar o usuário|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/AuthenticationMethod|  
|Negar apenas SID de grupo|O grupo somente deny\ SID do usuário|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|Negar apenas SID principal|O SID somente deny\ principal do usuário|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|Negar somente grupo primário SID|O somente deny\ grupo primário SID do usuário|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|Grupo SID|O grupo SID do usuário|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/GroupSID|  
|Grupo primário SID|O grupo primário SID do usuário|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID principal|O SID do usuário principal|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nome de conta do Windows|O nome da conta de domínio do usuário na forma de<domain>\\<user>|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>Quais são as descrições de declaração?  
Descrições de declaração representam uma lista dos tipos de declarações que é compatível com o AD FS e que pode ser publicado nos metadados de Federação. Os tipos de declaração mencionados na tabela anterior são configurados como descrições de declarações no AD FS gerenciamento snap\-in.  
  
A coleção de descrições de declaração que será publicado para metadados de Federação é armazenada no banco de dados de configuração do AD FS. Esses reivindicar descrições são usadas por vários componentes do serviço de Federação.  
  
A descrição de cada declaração inclui um tipo de declaração URI, o nome, o estado de publicação e a descrição. Você pode gerenciar a coleção de descrição reivindicação usando o **reivindicar descrições** nó no AD FS gerenciamento snap\-in. Você pode modificar o estado de publicação de uma descrição de declaração usando o snap\-in. As configurações a seguir estão disponíveis:  
  
-   **Publicar dessa declaração nos metadados de federação como um tipo de declaração que esse serviço de Federação pode aceitar** \(Publish as Accepted\) – indica os tipos de declaração serão aceito de outros provedores de requerimentos judiciais ou Extrajudiciais por este serviço de Federação.  
  
-   **Publicar dessa declaração nos metadados de federação como um tipo de declaração que esse serviço de Federação pode enviar** \(Publish as Sent\) – indica os tipos de declaração são oferecidos por este serviço de Federação. Estes são os tipos de declaração que o serviço de Federação publica a outras pessoas como aqueles que está prestes a enviar. Os tipos de declaração real enviados pelo provedor de declarações geralmente são um subconjunto nessa lista.  
  
Para obter mais informações sobre como definir o estado de publicação de um tipo de declaração, veja [adicionar uma descrição reivindicar](https://technet.microsoft.com/library/dd807051.aspx) no guia de implantação do AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Quando a geração de metadados de Federação  
Metadados de Federação incluem todas as descrições de declaração são marcadas para publicação.  
  
### <a name="when-claims-rules-are-processed"></a>Quando as regras de declarações são processadas  
Quando você mantém informações de configuração sobre descrições de requerimentos judiciais ou Extrajudiciais, é mais fácil para você configurar as regras sobre declarações. Para saber mais sobre as regras de declaração que podem ser usados na organização provedor requerimentos judiciais ou Extrajudiciais, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md).  
  

