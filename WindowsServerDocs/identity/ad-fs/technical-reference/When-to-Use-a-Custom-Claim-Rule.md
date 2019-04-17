---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: "Quando usar uma regra de declaração personalizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>Quando usar uma regra de declaração personalizada
Escrever uma regra de declaração personalizada em serviços de Federação do Active Directory \(AD FS\) usando a linguagem de regra reivindicação, que é a estrutura que o mecanismo de emissão requerimentos judiciais ou Extrajudiciais usa para gerar programaticamente, transform, passar e filtrar requerimentos judiciais ou Extrajudiciais. Usando uma regra personalizada, você pode criar regras com lógica mais complexa do que um modelo de regra padrão. Considere usar uma regra personalizada quando quiser:  
  
-   Envie com base nos valores que são extraídos de um repositório de atributo de linguagem de consulta estruturada \(SQL\) requerimentos judiciais ou Extrajudiciais.  
  
-   Envie declarações com base nos valores que são extraídos de um repositório de atributo Lightweight Directory Access Protocol \(LDAP\) usando um filtro personalizado do LDAP.  
  
-   Envie declarações com base nos valores que são extraídos de um repositório de atributo personalizado.  
  
-   Envie requerimentos judiciais ou Extrajudiciais somente quando dois ou mais declarações de entrada estão presentes.  
  
-   Envie requerimentos judiciais ou Extrajudiciais somente quando uma entrada reivindicar correspondências de valor de um padrão complexo.  
  
-   Envie o valor de solicitação de declarações com alterações complexas para uma entrada.  
  
-   Crie declarações para uso somente em regras posteriores, sem realmente enviar as declarações.  
  
-   Construa uma declaração de saída do conteúdo dos mais de uma declaração de entrada.  
  
Você também pode usar uma regra personalizada quando o valor da declaração da reivindicação saída deve ser baseado no valor da declaração de entrada, mas ele também deve incluir conteúdo adicional.  
  
O idioma de regra requerimentos judiciais ou Extrajudiciais é regra com base. Ele tem uma parte de condição e uma parte da execução. Você pode usar a sintaxe de linguagem de regra de declaração para enumerar, adicionar, excluir ou modificar declarações para atender às necessidades da sua organização. Para saber mais sobre como cada uma dessas partes funciona, consulte [a função da linguagem de regra de declaração ](The-Role-of-the-Claim-Rule-Language.md).  
  
As seções a seguir fornecem uma introdução básica de declaração regras. Eles também fornecem detalhes sobre quando usar uma regra de declaração personalizada.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância da lógica de negócios que leva uma declaração de entrada, aplicar uma condição a ele \ (se x, em seguida, y\) e produzir uma declaração de saída com base nos parâmetros condição.  
  
> [!IMPORTANT]  
> -   No AD FS gerenciamento snap\-in, reivindicar regras podem ser criadas apenas com modelos de regra de declaração  
> -   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
> -   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência sobre as regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
> -   Modelos de regra de declaração sempre exigem que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você pode criar essa regra criando primeiro a sintaxe que você precisa para sua operação usando a linguagem de regra de declaração e, em seguida, colando o resultado na caixa de texto que é fornecida na enviar um requerimentos judiciais ou Extrajudiciais usando um modelo de regra personalizada nas propriedades de um provedor de declarações confiar ou um terceiro confiar no AD FS gerenciamento snap\-in.  
  
Esse modelo de regra fornece as seguintes opções:  
  
-   Especifique um nome de regra de declaração  
  
-   Digite uma ou mais condições opcionais e uma instrução de emissão usando o AD FS reivindicar idioma de regra  
  
Para obter mais instruções para criar uma regra personalizada usando esse modelo, consulte [criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada](https://technet.microsoft.com/library/dd807049.aspx) no guia de implantação do AD FS.  
  
Para melhor compreensão de como funciona a linguagem de regra de declaração, exibir a sintaxe da linguagem reivindicação regra de outras regras que já existem no snap\-in clicando o **idioma de regra de modo de exibição** guia nas propriedades para essa regra. Usar as informações nesta seção e a sintaxe dessa guia pode fornecer informações sobre como construir regras personalizadas.  
  
Para obter mais informações sobre como usar a linguagem de regra de declaração, veja [a função da linguagem de regra reivindicar](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Exemplo: Como combinar nomes e sobrenomes com base nos valores de atributo de nome do usuário  
A seguinte sintaxe de regra combina nomes e sobrenomes dos valores de atributo em um repositório de determinado atributo. O mecanismo da política de forma um produto cartesiano de correspondências para cada condição. Por exemplo, a saída de nome {"Frank", "Alan"} e sobrenomes {"Miller", "Shen"} é {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Exemplo: Como emitir uma reivindicação manager com base em se os usuários têm subordinados  
A seguinte regra emite uma reivindicação manager somente se o usuário tem relatórios diretos:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Exemplo: Como emitir uma reivindicação PPID com base em um atributo LDAP  
A seguinte regra emite uma declaração privada pessoal identificador \(PPID\) com base no **windowsaccountname** e **originalissuer** atributos de usuários em um repositório de atributo LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Atributos comuns que podem ser usados para identificar exclusivamente a essa consulta do usuário incluem o seguinte:  
  
-   **SID de usuário**  
  
-   **windowsaccountname**  
  
-   **sAMAccountName**  
  

