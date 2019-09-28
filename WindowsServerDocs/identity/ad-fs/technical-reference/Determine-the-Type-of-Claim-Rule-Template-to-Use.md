---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Determinar o tipo de modelo de regra de declaração a ser usado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eaf0e9fca9b24eb4e67caa4237efe044937d5831
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407409"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinar o tipo de modelo de regra de declaração a ser usado


Uma parte importante da criação de uma infraestrutura de Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 é determinar o conjunto completo de regras de declaração — e quais modelos de regra de declaração correspondentes você deve usar para criá-las — para cada parceiro que irá Participe da Federação com sua organização. Você cria regras usando modelos de regra de declaração no gerenciamento de AD FS snap @ no__t-0in.  
  
Cada conjunto de regras de declaração que você configura só pode ser associado uma relação federada de confiança. Isso significa que você não pode criar um conjunto de regras em uma relação de confiança e usá-las em outras relações de confiança no Serviço de Federação. Em vez disso, você pode facilmente criar regras dos modelos de regra de declaração para ajudar a produzir um conjunto desejado de declarações, com a concordância dos parceiros federados e de sua organização.  
  
Para obter mais informações sobre regras e modelos de regras, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Antes de começar a determinar os tipos de modelos de regras de declaração que você deve usar, considere as seguintes perguntas:  
  
-   Quais declarações serão fornecidas por seus provedores de declarações confiáveis?  
  
-   Quais declarações de cada provedor de declarações você confia?  
  
-   Quais declarações são exigidas pelas partes confiantes que confiam neste Serviço de Federação?  
  
-   Que declarações você está disposto a divulgar a cada terceira parte confiável?  
  
-   Quais usuários terão acesso a cada terceira parte confiável?  
  
A resposta a essas perguntas ajudará a planejar um design sólido de regra de declaração. Também ajudará criar uma autorização suave e estratégia de controle de acesso e tornará sua equipe de implantação mais eficiente durante a distribuição.  
  
Na próxima seção você pode aprender sobre o tipo de modelos de regras a selecionar para o seu ambiente com base no que a empresa precisa.  
  
## <a name="claim-rule-template-types"></a>Tipos de modelo de regra de declaração  
A tabela a seguir descreve todos os tipos de modelos de regra de declaração que você pode usar para criar regras usando o snap do AD FS Management @ no__t-0in e os benefícios de usar um tipo de modelo sobre outro.  
  
|Tipos de modelo de regra|Descrição|Vantagens|Desvantagens|  
|----------------------|---------------|--------------|-----------------|  
|Passar ou filtrar uma declaração de entrada|Usado para criar uma regra que passará todos os valores de declaração para um tipo selecionado ou filtrará declarações com base nos valores da declaração para que somente determinados valores de um tipo de declaração selecionado sejam passados.<br /><br />Para obter mais informações, consulte [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Pode ser usado para selecionar declarações específicas a serem aceitas ou emitidas inalteradas|-O tipo e o valor da declaração não podem ser alterados|  
|Transformar uma declaração de entrada|Usado para criar uma regra que pode selecionar uma declaração de entrada e mapeá-la para um tipo de declaração diferente ou mapear seu valor para um novo valor.<br /><br />Para obter mais informações, consulte [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Pode ser usado para normalizar os tipos de declaração ou valores<br />-Pode substituir um sufixo e @ no__t-0mail de uma declaração de entrada|-Substituições de cadeia de caracteres mais complexas exigem uma regra personalizada|  
|Enviar atributos LDAP como declarações|Usado para criar uma regra que selecionará os atributos de um repositório de atributos LDAP para enviar como declarações à terceira parte confiável.<br /><br />Para obter mais informações, consulte [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Pode declarações de origem de qualquer repositório de atributos do AD DS @ no__t-0AD LDS<br />-Várias declarações podem ser emitidas usando uma única regra|-Desempenho – lento como resultado da pesquisa de conta<br />-Não é possível usar um filtro LDAP personalizado para consulta|  
|Enviar associação de grupo como uma declaração|Usado para criar uma regra que pode enviar um tipo de declaração especificado e um valor quando um usuário é membro de um grupo de segurança do Active Directory. Apenas uma única declaração será enviada usando essa regra, com base no grupo selecionado.<br /><br />Para obter mais informações, consulte [quando usar uma associação de grupo Enviar como uma regra de declaração](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Desempenho rápido para emissão de declarações de grupo – sem pesquisa de conta|-O usuário deve ser membro de um grupo de Active Directory local|  
|Enviar declarações usando uma regra personalizada|Usado para criar uma regra personalizada que forneça mais opções avançadas de um modelo de regra padrão. Você escreve regras personalizadas com o idioma da regra de declaração AD FS.<br /><br />Para obter mais informações, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Pode ser usado para declarações de origem de um repositório de atributos SQL<br />-Pode ser usado para especificar um filtro LDAP personalizado<br />-Pode ser usado para emitir um PPID<br />-Pode ser usado com um repositório de atributos personalizado<br />-Pode ser usado para adicionar declarações somente ao conjunto de declarações de entrada<br />-Pode ser usado para enviar declarações com base em mais de uma declaração de entrada|-É mais difícil configurar \- pode ser necessário ter um tempo de backup superior para obter o conhecimento do idioma da regra de declaração|  
|Permitir ou negar usuários com base em uma declaração de entrada|Usado para criar uma regra que vai permitir ou negar o acesso de usuários à terceira parte confiável, com base no tipo e valor de uma declaração de entrada.<br /><br />Para obter mais informações, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifica o processo de autorização|-Requer que apenas um tipo de declaração e um valor de declaração sejam especificados<br />-Não dá suporte à correspondência de padrões para valores de declaração|  
|Permitir todos os usuários|Usado para criar uma regra que permitirá que todos os usuários acessem a terceira parte confiável.<br /><br />Para obter mais informações, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|– Simples de configurar|-Menos seguro do que usar os usuários de permitir ou negar com base em um modelo de declaração de entrada|  
  

