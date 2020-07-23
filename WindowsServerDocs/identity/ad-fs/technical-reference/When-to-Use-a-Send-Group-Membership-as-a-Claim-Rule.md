---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Quando usar uma associação a um grupo de envio como uma regra de declaração
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 43d9e8767c0a179a23d015484b09a0228829870b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966478"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando usar uma associação a um grupo de envio como uma regra de declaração
Você pode usar essa regra em Serviços de Federação do Active Directory (AD FS) \( AD FS \) quando desejar emitir um novo valor de declaração de saída somente para os usuários que são membros de um grupo de segurança de Active Directory especificado. Quando você usa essa regra, emite uma declaração única para apenas o grupo especificado e que corresponde à lógica da regra, conforme descrito na tabela a seguir.  
  
|Opção de regras|Lógica de regras|  
|---------------|--------------|  
|Valor de declaração de saída|Se a associação de grupo de um usuário for igual ao *grupo especificado* e o tipo de declaração de saída for igual ao *tipo de declaração especificado*, substitua o valor de nome de grupo existente pelo *valor de declaração de saída especificado* e emita a declaração.|  
  
As seções a seguir fornecem uma introdução básica às regras de declaração. Elas também fornecem detalhes sobre quando usar a associação a um grupo de envio como uma regra de declaração.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância da lógica de negócios que usará uma declaração de entrada, aplicará uma condição a ela \( se x depois y \) e produzir uma declaração de saída com base nos parâmetros de condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:  
  
-   No snap in de gerenciamento de AD FS \- , as regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   As regras de declaração processam declarações de entrada diretamente de um provedor de declarações \( , como Active Directory ou outro serviço de Federação \) ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.  
  
-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.  
  
-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valor de declaração de saída  
Usando o modelo de regras Enviar Associação a um Grupo como uma Declaração, você pode emitir uma declaração que é condicionada a se um usuário é um membro de um grupo especificado.  
  
Em outras palavras, esse modelo de regra emite uma declaração somente quando o usuário tem o SID da ID de segurança do grupo \( \) que corresponde ao grupo de Active Directory que o administrador especifica. Todos os usuários que se autenticam no Active Directory Domain Services \( AD DS \) terão declarações de Sid de grupo de entrada para cada grupo ao qual pertencem. Por padrão, as regras de transformação de aceitação na relação de confiança do provedor de declarações do Active Directory passam por essas declarações da SID do grupo. Usar esses SIDs de grupo como base para emitir declarações é muito mais rápido do que Pesquisar os grupos do usuário em AD DS.  
  
Quando você usa essa regra, somente uma única declaração é enviada, com base no grupo do Active Directory selecionado. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de "Admin" se o usuário for um membro do grupo de segurança Admins. do Domínio.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Os administradores devem usar este tipo de regra nas regras de transformação de aceitação de uma relação de confiança do provedor de declarações somente quando as SIDs do grupo estão sendo recebidas do provedor de declarações, o que é muito raro que qualquer provedor de declarações exceto o AD DS ou o Active Directory.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você cria essa regra usando o idioma da regra de declaração ou usando o modelo enviar Associação de grupo LDAP como uma regra de declaração no snap-in de gerenciamento de AD FS \- . Essa regra fornece as seguintes opções de configuração:  
  
-   Especificar um nome de regra de declaração  
  
-   Selecionar um grupo de usuários usando o seletor de objetos  
  
-   Selecionar um tipo de saída  
  
-   Selecione um formato de ID de nome \( de saída que esteja disponível somente quando a ID de nome for escolhida no campo tipo de declaração de saída\)  
  
-   Especificar um valor de declaração de saída  
  
Para obter mais informações sobre como criar essa regra, consulte [criar uma regra para enviar a associação de grupo como uma declaração](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913569(v=ws.11)).  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Se você quiser emitir declarações com base em uma SID de entrada que não uma SID de grupo, use o modelo de regra Transformar uma Declaração de Entrada. Se o administrador deseja recuperar os nomes de todos os grupos dos quais o usuário é membro, use o modelo de regras Enviar Atributos LDAP como Declarações em vez disso com o atributo **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemplo: como emitir declarações de grupo com base na associação de grupo do usuário  
A regra a seguir emite declarações de grupo para um usuário com base em uma SID de grupo de entrada:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar atributos LDAP como declarações](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807115(v=ws.11))  
  
