---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quando usar uma associação a um grupo de envio como uma regra de declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea537aa61cd7bfbe05ed1dd151eddd4a0bfc5ca7
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188301"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando usar uma associação a um grupo de envio como uma regra de declaração
Você pode usar essa regra nos serviços de Federação do Active Directory \(do AD FS\) quando desejar emitir um novo valor de declaração de saída somente para os usuários que são membros de um grupo de segurança do Active Directory especificado. Quando você usa essa regra, emite uma declaração única para apenas o grupo especificado e que corresponde à lógica da regra, conforme descrito na tabela a seguir.  
  
|Opção de regras|Lógica de regras|  
|---------------|--------------|  
|Valor de declaração de saída|Se a associação a um grupo do usuário for igual ao *grupo especificado* e o tipo de declaração de saída for igual ao *tipo de declaração especificado*, substitua o valor de nome de grupo existente pelo *valor da declaração de saída especificado* e emita a declaração.|  
  
As seções a seguir fornecem uma introdução básica às regras de declaração. Elas também fornecem detalhes sobre quando usar a associação a um grupo de envio como uma regra de declaração.  
  
## <a name="about-claim-rules"></a>Sobre regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que irá levar uma declaração de entrada, aplicar uma condição a ela \(se x e y\) e produzir uma declaração de saída com base nos parâmetros da condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:  
  
-   No snap do gerenciamento do AD FS\-, declaração de regras só podem ser criadas usando modelos de regra de declaração  
  
-   Processo de regras de declaração entrada declarações diretamente de um provedor de declarações \(como o Active Directory ou outro serviço de Federação\) ou da saída da aceitação regras de transformação em uma relação de confiança do provedor de declarações.  
  
-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.  
  
-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre regras de declaração e conjuntos de regras de declaração, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obter mais informações como os conjuntos de regras de declaração são processados, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valor de declaração de saída  
Usando o modelo de regras Enviar Associação a um Grupo como uma Declaração, você pode emitir uma declaração que é condicionada a se um usuário é um membro de um grupo especificado.  
  
Em outras palavras, esse modelo de regra emite uma declaração somente quando o usuário tem a ID de segurança do grupo \(SID\) que corresponde ao grupo do Active Directory que o administrador especifica. Todos os usuários autenticados no Active Directory Domain Services \(AD DS\) terão declarações de SID para cada grupo que pertencem ao grupo de entrada. Por padrão, as regras de transformação de aceitação na relação de confiança do provedor de declarações do Active Directory passam por essas declarações da SID do grupo. Usar essas SIDs do grupo como uma base para emitir declarações é muito mais rápido do que procurar grupos do usuário no AD DS.  
  
Quando você usa essa regra, somente uma única declaração é enviada, com base no grupo do Active Directory selecionado. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de "Admin" se o usuário for um membro do grupo de segurança Admins. do Domínio.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Os administradores devem usar este tipo de regra nas regras de transformação de aceitação de uma relação de confiança do provedor de declarações somente quando as SIDs do grupo estão sendo recebidas do provedor de declarações, o que é muito raro que qualquer provedor de declarações exceto o AD DS ou o Active Directory.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você criar essa regra usando a linguagem de regra de declaração ou usando a associação de grupo LDAP enviar como um modelo de regra de declaração no gerenciamento do AD FS snap\-no. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especificar um nome de regra de declaração  
  
-   Selecionar um grupo de usuário utilizando o seletor de objetos  
  
-   Selecionar um tipo de saída  
  
-   Selecione um formato de nome de identificação a saída \(que está disponível apenas quando a ID de nome é escolhido do campo de tipo de declaração de saída\)  
  
-   Especificar um valor de declaração de saída  
  
Para obter mais informações sobre como criar essa regra, consulte [criar uma regra enviar associação do grupo como uma declaração](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Se você quiser emitir declarações com base em uma SID de entrada que não uma SID de grupo, use o modelo de regra Transformar uma Declaração de Entrada. Se o administrador deseja recuperar os nomes de todos os grupos dos quais o usuário é membro, use o modelo de regras Enviar Atributos LDAP como Declarações em vez disso com o atributo **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemplo: Como emitir declarações de grupo com base na associação a um grupo do usuário  
A regra a seguir emite declarações de grupo para um usuário com base em uma SID de grupo de entrada:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar atributos LDAP como declarações](https://technet.microsoft.com/library/dd807115.aspx)  
  

