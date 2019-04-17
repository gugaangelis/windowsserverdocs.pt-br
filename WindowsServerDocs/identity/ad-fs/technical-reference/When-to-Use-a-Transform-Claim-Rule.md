---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: "Quando usar uma regra de declaração de transformação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c7b7ea2c8d9a08a4cbf6c89c2de2482043efe25b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-transform-claim-rule"></a>Quando usar uma regra de declaração de transformação
Você pode usar essa regra em serviços de Federação do Active Directory \(AD FS\) quando você precisa para mapear um tipo de declaração de entrada para um tipo de declaração de saída e, em seguida, aplicar uma ação que irá determinar quais saída deve ocorrer com base nos valores originados na declaração de entrada. Quando você usa essa regra, você passa por ou transforma declarações que correspondem a lógica de regra a seguir, com base em uma das opções que você configurar na regra, conforme descrito na tabela a seguir.  
  
|Opção de regra|Lógica de regra|  
|---------------|--------------|  
|Passar por todas as reclamações de entrada|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *qualquer valor*, em seguida, passamos a reivindicação por meio com igual a saída do tipo de declaração *especificado o tipo de declaração*|  
|Substituir um valor de declaração de entrada com um valor diferente de declaração de saída|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *especificado reivindicação valor*, transformar a declaração com o novo valor de declaração de saída *especificado o valor da declaração* com a saída de tipo de declaração e *especificado o tipo de declaração*|  
|Substituindo as declarações de sufixo e\ mail entrada com um novo sufixo e\ mail|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *qualquer valor de sufixo*, transformar a declaração com o novo valor de declaração de saída *especificado um valor de sufixo* com a saída de tipo de declaração e *especificado o tipo de declaração*|  
  
As seções a seguir fornecem uma introdução básica para reivindicar regras e fornecer mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma declaração de entrada, aplicar uma condição a ele \ (se x y\ then) e produzir uma declaração de saída com base nos parâmetros condição. A lista a seguir descreve dicas que você deve conhecer reivindicar regras antes de ler ainda mais importante neste tópico:  
  
-   No AD FS gerenciamento snap\-in, regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
  
-   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência nas regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
  
-   Modelos de regra de declaração sempre exigirá que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passar por todos os valores de declaração  
Ao usar essa ação, todos os valores de declaração de entrada que são mapeados para um tipo de declaração de entrada especificado são mapeados para um tipo de declaração de saída especificado antes de serem enviados como declarações de saída em tokens que foram assinados pelo seu serviço de Federação.  
  
Por exemplo, quando uma regra é definida com o **reivindica a passagem de todos os valores** opção lógica e a entrada reivindicar o tipo do grupo e o tipo de declaração de saída da função for especificado, todos os valores de declaração de entrada que fluam em de emissor são copiados individualmente para novas declarações de saída com o tipo de declaração da função.  
  
## <a name="transforming-a-claim"></a>Transformando uma reivindicação  
No AD FS, o termo *declarações transformação* significa substituir uma entrada reivindica com um valor diferente de declaração de saída. É a uma regra de declaração de entrada que possibilita a essa função de transformação. Dentro das propriedades dessa regra, você pode definir condições para transformar os valores de entrada com um valor de declaração saída diferente com base no tipo de declaração de entrada especificado.  
  
Por exemplo, conforme mostrado na ilustração a seguir, quando uma regra é definida com a condição para substituir um valor de entrada com um valor diferente de declaração de saída, todos os tipos de declaração de entrada do grupo são mapeados para novos tipos de declaração de saída da função. Nesse caso, o valor da entrada declaração do comprador é substituído com o novo valor de declaração de saída de administrador.  
  
![Quando usar uma transformação](media/adfs2_transform.gif)  
  
Você também pode usar essa regra para aplicar uma condição que substituirão todas as reclamações de entrada com um valor de sufixo especificado e\-mail com um novo valor. Por exemplo, você pode definir uma condição nessa regra para alterar todos os valores de declaração com o sufixo da sales.corp.fabrikam.com para fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Quando você usa uma relação de confiança do provedor de declarações, essa regra pode ser configurada para transformar declarações de entrada do provedor de declarações em equivalentes confiáveis. Tipos de declaração ou reivindicar valores podem ter um significado diferente em sua organização que das organizações de provedor de declarações. Você pode usar essa regra para normalizar os valores que vem do provedor de declarações para que seus equivalentes da declaração de saída podem ser compreendidas pelo terceiro e tipos de declaração.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurando essa regra em uma terceira confiança de terceiros  
Quando você usa uma relação de confiança terceira parte, essa regra pode ser configurada para transformar declarações para a parte específica. Tipos de declaração ou reivindicar valores podem ter um significado diferente para um terceiro específico, e essa regra torna possível para alterar os valores e tipos de declaração de saída para um terceiro único.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Criar essa regra usando a linguagem de regra de declaração ou usando o **transformar uma entrada reivindicar** modelo de regra no AD FS gerenciamento snap\-in. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especifique um nome de regra de declaração  
  
-   Transformar um tipo específico de declaração de entrada para um tipo de declaração de saída especificado  
  
-   Passar por todos os valores de declaração  
  
-   Substituir um valor de declaração de entrada com um valor diferente de declaração de saída  
  
-   Substitua declarações de sufixo e\ email de entrada um novo sufixo e\ mail  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para transformar uma declaração de entrada](https://technet.microsoft.com/library/dd807068.aspx) no guia de implantação do AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Se a declaração de saída deve ser construída a partir do conteúdo de mais de uma declaração de entrada, você deve usar uma regra personalizada. Se o valor da declaração da reivindicação saída deve ser baseado no valor da entrada reivindicação — mas com conteúdo adicional, você também precisa usar uma regra personalizada nesse contexto. Para obter mais informações, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Exemplos de como construir uma sintaxe de regra de transformação  
Ao usar a sintaxe da linguagem reivindicação regra para transformar requerimentos judiciais ou Extrajudiciais, você pode definir uma propriedade da reivindicação transformada para um novo valor literal. Por exemplo, a seguinte regra muda o valor da função requerimentos judiciais ou Extrajudiciais de "Administradores" para "raiz", mantendo o mesmo tipo de declaração:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Expressões regulares também podem ser usadas para transformações de declaração. Por exemplo, a seguinte regra definirá o domínio em declarações de nome de usuário windows no formato domínio \ \ usuário Fabrikam:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Práticas recomendadas para criar regras personalizadas  
Transformações de declarações podem ser aplicadas seletivamente alegações selecionado usando os recursos de filtragem básicos. Cada uma das propriedades reivindicação usadas para filtragem pode ser atribuída valores, com as seguintes condições:  
  
|Propriedade declaração|Descrição|  
|------------------|---------------|  
|Tipo de valor; ValueType|Essas propriedades serão utilizadas com mais frequência para atribuições. O tipo e o valor mínimo deve ser especificado para a declaração transformada resultante.|  
|Emissor|Enquanto o idioma de regra de declaração permite definir o emissor uma reivindicação, isso geralmente não é recomendável. O emissor de uma declaração não é serializado no token. Quando um token é recebido a propriedade emissor de todas as reclamações é definida como o identificador do servidor de Federação assinado o token. Assim, definir o emissor de uma declaração nas regras não terá efeito sobre o conteúdo do token e a configuração serão perdida após a declaração é empacotada em um token. O único cenário em que definir o emissor de uma declaração faz sentido é se ele está definido como um valor específico no conjunto de regras de provedor de declarações e terceira regra de terceiros conjunto é criado com regras que fazem referência a esse valor específico. Se a propriedade Issuer não está explicitamente definida como um valor em uma regra de declaração que o mecanismo de emissão requerimentos judiciais ou Extrajudiciais define como "Autoridade LOCAL".|  
|OriginalIssuer|Da mesma forma para o emissor, OriginalIssuer geralmente não deve ser explicitamente atribuído um valor. Ao contrário de emissor, a propriedade OriginalIssuer é serializada no token, mas a expectativa dos consumidores tokens é que, se definido, ele conterá o identificador do servidor de federação que emitiu originalmente uma reivindicação.|  
|Propriedades|Conforme descrito na seção anterior, o recipiente de propriedades de uma declaração não é mantido no token, portanto atribuições às propriedades só devem ser feitas se for para referenciar as informações armazenadas na propriedade subsequentes políticas locais.|  
  
Para obter mais informações sobre como usar a linguagem de regra de declaração, veja [a função da linguagem de regra reivindicar](The-Role-of-the-Claim-Rule-Language.md).  
  

