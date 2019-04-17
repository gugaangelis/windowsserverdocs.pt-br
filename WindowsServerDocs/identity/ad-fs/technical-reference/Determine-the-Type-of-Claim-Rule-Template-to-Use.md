---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: "Determinar o tipo de modelo de regra de declaração para usar"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinar o tipo de modelo de regra de declaração para usar

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Uma parte importante da criação de uma infraestrutura de \(AD FS\) de serviços de Federação do Active Directory é determinar o conjunto completo de regras de declaração — e quais correspondente reivindicar modelos de regra, você deve usar para criá-los — para cada parceiro participarão federação com sua organização. Você pode criar regras usando modelos de regra de declaração no AD FS gerenciamento snap\-in.  
  
Cada conjunto de regras de declaração que você configurar somente pode ser associado uma relação de confiança federada. Isso significa que você não pode criar um conjunto de regras em uma relação de confiança e usá-los para outras relações de confiança em seu serviço de Federação. Em vez disso, você pode facilmente criar regras de declaração modelos de regra para ajudar mais rapidamente a produzir um conjunto desejado de declarações que são acordados entre cada parceiro federado e sua organização.  
  
Para obter mais informações sobre as regras e modelos de regra, consulte [a função de declaração regras](The-Role-of-Claim-Rules.md).  
  
Antes de começar a determinar os tipos de modelos de regra de declaração, que você deve usar, considere as seguintes perguntas:  
  
-   O que diz será fornecido por seus provedores de declarações confiável?  
  
-   Declarações do qual você confia de cada provedor de declarações?  
  
-   Quais declarações são necessárias para as partes confiantes que confia esse serviço de Federação?  
  
-   Requerimentos judiciais ou Extrajudiciais você estão disposto a divulgar para cada terceiro?  
  
-   Quais usuários devem ter acesso a cada terceiro?  
  
Responder a essas perguntas ajudarão a planejar um sólido solicitará a criação de regra. Ele também será ajudá-lo a criar uma autorização suave e estratégia de controle de acesso e tornar sua equipe de implantação mais eficiente durante a distribuição.  
  
A próxima seção, que você pode aprender sobre o tipo dos modelos de regra para selecionar para seu ambiente com base em sua empresa precisa.  
  
## <a name="claim-rule-template-types"></a>Tipos de modelo de regra de declaração  
A tabela a seguir descreve todos os tipos de modelos de regra reivindicação que você pode usar para criar regras usando o AD FS snap\-in Gerenciamento e as vantagens de usar um modelo digite sobre outro.  
  
|Tipo de modelo de regra|Descrição|Vantagens|Desvantagens|  
|----------------------|---------------|--------------|-----------------|  
|Passagem ou filtrar uma declaração de entrada|Usado para criar uma regra que irá passar por todos os valores de declaração para um tipo de declaração selecionado ou filtrar com base nos valores de declaração para que apenas determinados valores de declaração para um tipo de declaração selecionado passará por meio de declarações.<br /><br />Para obter mais informações, consulte [quando usar uma passagem ou uma regra de declaração de filtro](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Pode ser usado para selecionar requerimentos judiciais ou Extrajudiciais específicos para ser aceito ou emitidos inalterada|-Declaração de tipo e o valor não podem ser alterados|  
|Transformar uma declaração de entrada|Usado para criar uma regra que pode marcar uma declaração de entrada e mapeá-lo para um tipo de declaração diferente ou seu valor de declaração são mapeadas para um novo valor de declaração.<br /><br />Para obter mais informações, consulte [quando usar uma regra de declaração de transformação](When-to-Use-a-Transform-Claim-Rule.md).|-Pode ser usado para normalizar os tipos de declaração ou valores<br />-Pode substituir um sufixo e\ email de uma declaração de entrada|-Substituições de cadeia de caracteres mais complexas exigem uma regra personalizada|  
|Enviar atributos LDAP como requerimentos judiciais ou Extrajudiciais|Usado para criar uma regra que selecionará os atributos de um repositório de atributo LDAP enviar como declarações para a parte confiante.<br /><br />Para obter mais informações, consulte [quando usar um atributos de LDAP enviar como requerimentos judiciais ou Extrajudiciais regra](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Possa ser fonte declarações de qualquer loja de atributo DS\ AD/AD LDS<br />-Várias declarações podem ser emitidas uma única regra|-Lento desempenho – como resultado de pesquisa da conta<br />-Um filtro personalizado do LDAP não podem ser usados para consultar|  
|Enviar a associação ao grupo como uma reivindicação|Usado para criar uma regra que pode enviar um tipo de declaração especificado e um valor quando um usuário é um membro de um grupo de segurança do Active Directory. Somente uma única declaração será enviada usando essa regra, com base no grupo que você selecionar.<br /><br />Para obter mais informações, consulte [quando usar uma associação de grupo Enviar como uma regra de declaração](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rápido desempenho para a emissão de declarações de grupo – nenhuma pesquisa da conta|-O usuário deve ser um membro de um grupo local do Active Directory|  
|Enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada|Usado para criar uma regra personalizada que fornecerá opções mais avançadas de um modelo de regra padrão. Você escreve regras personalizadas com o AD FS reivindicar idioma de regra.<br /><br />Para obter mais informações, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).|-Pode ser usado para declarações de um repositório de atributo do SQL de origem<br />-Pode ser usado para especificar um filtro personalizado do LDAP<br />-Pode ser usado para emitir um PPID<br />-Pode ser usado com um repositório de atributo personalizado<br />-Pode ser usado para adicionar declarações somente para o conjunto de declarações de entrada<br />-Pode ser usado para enviar solicitações com base em mais de uma declaração de entrada|-Mais difícil configurar \-alguns rampa o tempo pode ser necessário para inicialmente obter informações importantes sobre a linguagem de regra de declaração|  
|Permitir ou negar aos usuários com base em uma declaração de entrada|Usado para criar uma regra que irá permitir ou negar o acesso por usuários para a parte confiante, com base no tipo de valor de uma declaração de entrada.<br /><br />Para obter mais informações, consulte [quando usar uma regra de declaração de autorização](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifica o processo de autorização|-Exige que somente uma declaração tipo e um valor de solicitação de ser especificados<br />-Não oferece suporte a padrões correspondentes para valores de declaração|  
|Permitir que todos os usuários|Usado para criar uma regra que permitirá que todos os usuários acessem o terceiro.<br /><br />Para obter mais informações, consulte [quando usar uma regra de declaração de autorização](When-to-Use-an-Authorization-Claim-Rule.md).|-Simples de configurar|-Menos segura do que usar a permitir ou negar a usuários com base em um modelo de declaração de entrada|  
  

