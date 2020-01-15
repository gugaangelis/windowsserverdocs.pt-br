---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Quando usar uma regra de declaração personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c784c4b6dbfee7034dd9302dc87fc74b896763f5
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950140"
---
# <a name="when-to-use-a-custom-claim-rule"></a>Quando usar uma regra de declaração personalizada
Você escreve uma regra de declaração personalizada em Serviços de Federação do Active Directory (AD FS) \(AD FS\) usando o idioma da regra de declaração, que é a estrutura que o mecanismo de emissão de declarações usa para gerar, transformar, passar e filtrar declarações programaticamente. Usando uma regra personalizada, você pode criar regras com lógica mais complexa do que um modelo de regra padrão. Considere o uso de uma regra personalizada quando quiser:  
  
-   Envie declarações com base em valores extraídos de um linguagem SQL \(\) armazenamento de atributos do SQL.  
  
-   Envie declarações com base em valores extraídos de um protocolo de acesso de diretório simples \(armazenamento de atributos de\) LDAP usando um filtro LDAP personalizado.  
  
-   Enviar declarações com base nos valores extraídos de um repositório de atributos personalizados.  
  
-   Enviar declarações somente quando duas ou mais declarações de entrada estão presentes.  
  
-   Enviar declarações somente quando uma declaração de uma entrada corresponder a um padrão complexo.  
  
-   Enviar declarações com alterações complexas em um valo de declaração de entrada.  
  
-   Criar declarações para uso apenas em regras posteriores, sem realmente enviar as declarações.  
  
-   Construir uma declaração de saída do conteúdo de mais de uma declaração de entrada.  
  
Você também pode usar uma regra personalizada quando o valor da declaração de saída se basear no valor da declaração e entrada, mas também deve incluir conteúdo adicional.  
  
A linguagem da regra de declarações é baseada em regras. Ele tem uma parte de condição e uma parte da execução. Você pode usar a sintaxe de linguagem da regra de declaração para enumerar, adicionar, excluir ou modificar declarações para atender às necessidades da sua organização. Para obter mais informações sobre como cada uma dessas partes funciona, consulte [a função do idioma da regra de declaração](The-Role-of-the-Claim-Rule-Language.md).  
  
As seções a seguir fornecem uma introdução básica às regras de declaração. Elas também fornecem detalhes sobre quando usar uma regra de declaração personalizada.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância da lógica de negócios que usa uma declaração de entrada, aplicar uma condição a ela \(se x, em seguida, y\) e produzir uma declaração de saída com base nos parâmetros de condição.  
  
> [!IMPORTANT]  
> -   No snap\-de gerenciamento de AD FS no, as regras de declaração podem ser criadas somente usando modelos de regra de declaração  
> -   As regras de declaração processam declarações de entrada diretamente de um provedor de declarações \(como Active Directory ou outro Serviço de Federação\) ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.  
> -   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar ainda mais as declarações geradas por regras anteriores dentro de determinado conjunto de regras.  
> -   Modelos de regras de declaração sempre exigirão um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração, usando uma única regra.  
  
Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Crie essa regra criando primeiro a sintaxe necessária para a operação usando o idioma da regra de declaração e, em seguida, colando o resultado na caixa de texto fornecida no modelo enviar uma declaração usando uma regra personalizada nas propriedades de uma relação de confiança do provedor de declarações ou de uma terceira parte confiável no snap\-de gerenciamento de AD FS no.  
  
Essa regra fornece as seguintes opções:  
  
-   Especificar um nome de regra de declaração  
  
-   Digite uma ou mais condições opcionais e uma instrução de emissão usando o idioma da regra de declaração AD FS  
  
Para obter mais instruções sobre como criar uma regra personalizada usando esse modelo, consulte [criar uma regra para enviar declarações usando uma regra personalizada](https://technet.microsoft.com/library/dd807049.aspx) no guia de implantação do AD FS.  
  
Para um melhor entendimento de como funciona a linguagem de regra de declaração, exiba a sintaxe de linguagem de regra de declaração de outras regras que já existem no\-de snap no clicando na guia **Exibir idioma da regra** nas propriedades dessa regra. O uso das informações nesta seção e das informações de sintaxe dessa guia pode fornecer informações sobre como criar suas próprias regras personalizadas.  
  
Para obter mais informações sobre como usar o idioma da regra de declaração, consulte [a função do idioma da regra de declaração](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemplo: como combinar nomes e sobrenomes com base em valores de atributo de nome de um usuário  
A sintaxe de regra a seguir combina nomes e sobrenomes de valores de atributo em determinado repositório de atributos. O mecanismo de políticas forma um produto cartesiano de correspondências para cada condição. Por exemplo, a saída para o nome {"Frank", "Alan"} e sobrenomes {"Miller", "Shen"} é {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Exemplo: como emitir uma declaração de gerenciador com base em se os usuários têm subordinados diretos ou não  
A regra a seguir emite uma declaração de gerenciador apenas se o usuário tiver subordinados diretos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Exemplo: como emitir uma declaração PPID com base em um atributo LDAP  
A regra a seguir emite um identificador pessoal particular \(declaração de\) do PPID com base nos atributos **windowsaccountname** e **OriginalIssuer** de usuários em um repositório de atributos LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Atributos comuns que podem ser usados para identificar exclusivamente o usuário para essa consulta incluem o seguinte:  
  
-   **SID de usuário**  
  
-   **windowsaccountname**  
  
-   **sAMAccountName**  
  

