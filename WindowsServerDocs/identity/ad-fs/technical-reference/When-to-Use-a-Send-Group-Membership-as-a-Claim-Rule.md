---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: "Quando usar uma associação de grupo de envio como uma regra de declaração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Quando usar uma associação de grupo de envio como uma regra de declaração
Você pode usar essa regra em serviços de Federação do Active Directory \(AD FS\) quando você desejar emitir um novo valor de declaração saída somente para os usuários que são membros de um grupo de segurança Activ diretório especificado. Quando você usa essa regra, emitir uma única declaração para apenas o grupo que você especificar e que corresponde a lógica de regra, conforme descrito na tabela a seguir.  
  
|Opção de regra|Lógica de regra|  
|---------------|--------------|  
|Valor de declaração de saída|Se a associação de grupo do usuário é igual à *grupo especificado* e tipo de declaração de saída é igual *especificado o tipo de declaração*, substitua o valor de nome de grupo existente com o *valor de solicitação de saída especificado* e execute a declaração.|  
  
As seções a seguir fornecem uma introdução básica de declaração regras. Eles também fornecem detalhes sobre quando usar a associação do grupo Enviar como uma regra de declaração.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma declaração de entrada, aplicar uma condição a ele \ (se x y\ then) e produzir uma declaração de saída com base nos parâmetros condição. A lista a seguir descreve dicas que você deve conhecer reivindicar regras antes de ler ainda mais importante neste tópico:  
  
-   No AD FS gerenciamento snap\-in, regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
  
-   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência nas regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
  
-   Modelos de regra de declaração sempre exigirá que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valor de declaração de saída  
Usando a associação do grupo Enviar como um modelo de regra de declaração, você pode emitir uma reivindicação é vinculada se um usuário é um membro de um grupo que você especificar.  
  
Em outras palavras, problemas de modelo essa regra a reivindicação somente quando o usuário tem a segurança de grupo ID \(SID\) que corresponde ao grupo do Active Directory que especifica o administrador. Todos os usuários que autenticar usando os serviços de domínio do Active Directory \(AD DS\) terá declarações do SID para cada grupo que eles pertencem ao grupo de entrada. Por padrão, as regras de transformação de aceitação no Active Directory requerimentos judiciais ou Extrajudiciais provedor de confiança passam por meio desse grupo SID declarações. Usando esses grupo SIDs como base para emitir declarações é muito mais rápido do que consultar grupos do usuário no AD DS.  
  
Quando você usa essa regra, apenas uma única declaração é enviada, com base no grupo do Active Directory que você selecionar. Por exemplo, você pode usar esse modelo de regra para criar uma regra que enviará uma declaração de grupo com um valor de "Admin" se o usuário é um membro do grupo de segurança Administradores de domínio.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Os administradores devem usar esse tipo de regra as regras de transformação de aceitação de uma relação de confiança do provedor de declarações somente ao grupo SIDs estão sendo recebidos do provedor de declarações, que é muito comum que os provedores de declarações exceto do Active Directory ou AD DS.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Criar essa regra usando a linguagem de regra de declaração ou usando a associação do grupo Enviar LDAP como uma reivindicação modelo no AD FS snap\-in Gerenciamento de regras. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especifique um nome de regra de declaração  
  
-   Selecione o grupo de um usuário usando o seletor de objetos  
  
-   Selecione um tipo de declaração de saída  
  
-   Selecionar um formato de ID de nome saído \ (que está disponível somente quando o ID de nome é escolhido da saída field\ de tipo de declaração)  
  
-   Especificar um valor de declaração de saída  
  
Para obter mais informações sobre como criar essa regra, consulte [criar uma regra para enviar a associação de grupo como uma declaração](https://technet.microsoft.com/en-us/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Se você quiser emitir declarações com base em uma SID de entrada que não seja um SID de grupo, use a transformação um modelo de regra de declaração de entrada. Se o administrador deseja recuperar os nomes de todos os grupos que o usuário for membro de, use os atributos de LDAP enviar como modelo de regra de declarações em vez disso, com o **tokenGroups** atributo.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Exemplo: Como emitir declarações de grupo com base na associação de grupo do usuário  
A seguinte regra emite declarações de grupo para um usuário com base em um grupo de entrada SID:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar atributos LDAP como declarações](https://technet.microsoft.com/library/dd807115.aspx)  
  

