---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Quando usar uma regra de declaração personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e566de4df2895dfa2ed1104f85c1429881ff5bbf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188293"
---
# <a name="when-to-use-a-custom-claim-rule"></a>Quando usar uma regra de declaração personalizada
Você escreve uma regra de declaração personalizadas nos serviços de Federação do Active Directory \(do AD FS\) usando a linguagem de regra de declaração, que é a estrutura que o mecanismo de emissão de declarações usa para gerar programaticamente, transformar, passar e filtrar declarações. Usando uma regra personalizada, você pode criar regras com lógica mais complexa do que um modelo de regra padrão. Considere o uso de uma regra personalizada quando quiser:  
  
-   Enviar declarações com base nos valores que são extraídas de uma linguagem de consulta estruturada \(SQL\) repositório de atributos.  
  
-   Enviar declarações com base nos valores que são extraídas de um Lightweight Directory Access Protocol \(LDAP\) usando um filtro LDAP personalizado de repositório de atributos.  
  
-   Enviar declarações com base nos valores extraídos de um repositório de atributos personalizados.  
  
-   Enviar declarações somente quando duas ou mais declarações de entrada estão presentes.  
  
-   Enviar declarações somente quando uma declaração de uma entrada corresponder a um padrão complexo.  
  
-   Enviar declarações com alterações complexas em um valo de declaração de entrada.  
  
-   Criar declarações para uso apenas em regras posteriores, sem realmente enviar as declarações.  
  
-   Construir uma declaração de saída do conteúdo de mais de uma declaração de entrada.  
  
Você também pode usar uma regra personalizada quando o valor da declaração de saída se basear no valor da declaração e entrada, mas também deve incluir conteúdo adicional.  
  
A linguagem da regra de declarações é baseada em regras. Ele tem uma parte de condição e uma parte da execução. Você pode usar a sintaxe de linguagem da regra de declaração para enumerar, adicionar, excluir ou modificar declarações para atender às necessidades da sua organização. Para obter mais informações sobre como cada uma dessas partes funciona, consulte [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
As seções a seguir fornecem uma introdução básica às regras de declaração. Elas também fornecem detalhes sobre quando usar uma regra de declaração personalizada.  
  
## <a name="about-claim-rules"></a>Sobre regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que usa uma declaração de entrada, aplicar uma condição a ela \(if x, y, em seguida,\) e produzir uma declaração de saída com base nos parâmetros da condição.  
  
> [!IMPORTANT]  
> -   No snap do gerenciamento do AD FS\-, declaração de regras podem ser criadas apenas usando modelos de regra de declaração  
> -   Processo de regras de declaração entrada declarações diretamente de um provedor de declarações \(como o Active Directory ou outro serviço de Federação\) ou da saída da aceitação regras de transformação em uma relação de confiança do provedor de declarações.  
> -   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar ainda mais as declarações geradas por regras anteriores dentro de determinado conjunto de regras.  
> -   Modelos de regras de declaração sempre exigirão um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração, usando uma única regra.  
  
Para obter mais informações sobre regras de declaração e conjuntos de regras de declaração, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obter mais informações como os conjuntos de regras de declaração são processados, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Criar esta regra criando primeiro a sintaxe que você precisa para sua operação usando a linguagem da regra de declaração e colá-lo, em seguida, o resultado no texto da caixa que é fornecido na enviar declarações usando um modelo de regra personalizada nas propriedades de um provedor de declarações tr a ust ou uma terceira confiar no snap do gerenciamento do AD FS\-no.  
  
Essa regra fornece as seguintes opções:  
  
-   Especificar um nome de regra de declaração  
  
-   Digite um ou mais condições opcionais e uma instrução de emissão usando o AD FS de linguagem da regra de declaração  
  
Para obter mais instruções para criar uma regra personalizada usando esse modelo, consulte [criar uma regra para enviar declarações usando uma regra personalizada](https://technet.microsoft.com/library/dd807049.aspx) no guia de implantação do AD FS.  
  
Para uma melhor compreensão sobre como funciona a linguagem de regra de declaração, exibir a sintaxe de linguagem de regra de declaração de outras regras que já existem no snap\-em clicando o **exibir linguagem da regra** guia nas propriedades dessa regra. O uso das informações nesta seção e das informações de sintaxe dessa guia pode fornecer informações sobre como criar suas próprias regras personalizadas.  
  
Para obter mais informações sobre como usar a linguagem de regra de declaração, consulte [The Role of a linguagem da regra de declaração](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemplo: Como combinar nomes e sobrenomes com base nos valores de atributo de nome do usuário  
A sintaxe de regra a seguir combina nomes e sobrenomes de valores de atributo em determinado repositório de atributos. O mecanismo de políticas forma um produto cartesiano de correspondências para cada condição. Por exemplo, a saída para o nome {"Frank", "Alan"} e sobrenomes {"Miller", "Shen"} é {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Exemplo: Como emitir uma declaração de gerenciador com base em se os usuários têm relatórios diretos ou não  
A regra a seguir emite uma declaração de gerenciador apenas se o usuário tiver subordinados diretos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Exemplo: Como emitir uma declaração PPID com base em um atributo LDAP  
A regra a seguir emite um identificador de pessoal privado \(PPID\) declaração com base nas **windowsaccountname** e **originalissuer** atributos de usuários em um atributo LDAP armazenamento:  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Atributos comuns que podem ser usados para identificar exclusivamente o usuário para essa consulta incluem o seguinte:  
  
-   **SID de usuário**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

