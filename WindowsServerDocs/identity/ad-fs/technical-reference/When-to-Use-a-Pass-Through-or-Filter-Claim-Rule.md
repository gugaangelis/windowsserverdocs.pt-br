---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: "Quando usar uma passagem ou uma regra de declaração de filtro"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quando usar uma passagem ou uma regra de declaração de filtro
Você pode usar essa regra nos serviços de Federação do Active Directory \(AD FS\) quando você precisa levar um tipo específico de declaração de entrada e, em seguida, aplicar uma ação que irá determinar quais saída deve ocorrer com base nos valores da declaração de entrada. Quando você usa essa regra, você passa por ou filtrar qualquer reivindicação que correspondem a lógica de regra na tabela a seguir, com base em uma das opções que você configurar na regra.  
  
|Opção de regra|Lógica de regra|  
|---------------|--------------|  
|Passar por todos os valores de declaração|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e o valor é igual a *qualquer valor*, em seguida, passamos a reivindicação por meio de|  
|Passagem somente um determinado valor de solicitação|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e valor é igual a *especificado o valor da declaração*, em seguida, passamos a reivindicação por meio de|  
|Passar por meio de apenas os valores reivindicação que correspondem a um valor de sufixo e\-mensagens específicas|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e valor é igual a *especificado um valor de sufixo*, em seguida, passamos a reivindicação por meio de|  
|Passar por meio de apenas os valores de declaração que começam com um valor específico|Se for igual a entrada tipo de declaração *especificado o tipo de declaração* e valor começa com *especificado o valor da declaração*, em seguida, passamos a reivindicação por meio de|  
  
As seções a seguir fornecem uma introdução básica para reivindicar regras e fornecer mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que vai levar uma declaração de entrada, aplicar uma condição a ele \ (se x y\ then) e produzir uma declaração de saída com base nos parâmetros condição. A lista a seguir descreve dicas que você deve conhecer reivindicar regras antes de ler ainda mais importante neste tópico:  
  
-   No AD FS gerenciamento snap\-in, regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   Declarações do processo de regras de declaração entrada a partir de um provedor de declarações \ (como Active Directory ou outro intelegente federação) ou da saída da aceitação transformar regras em uma relação de confiança do provedor de declarações.  
  
-   Declaração regras são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um conjunto de regras de determinado. Definindo precedência nas regras, você pode ainda mais refinar ou filtrar requerimentos judiciais ou Extrajudiciais gerados por regras anteriores dentro de um conjunto de regras de determinado.  
  
-   Modelos de regra de declaração sempre exigirá que você especificar um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre a reivindicação e reivindicação conjuntos de regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, veja [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md). Para saber mais como os conjuntos de regra de declarações são processados, veja [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passar por todos os valores de declaração  
Ao usar essa ação, todos os valores de declaração de entrada para o tipo de declaração especificado são passados como declarações de saída. Por exemplo, quando o tipo de declaração de entrada é especificado como o tipo de declaração de função, todos os valores de declaração de entrada são copiados individualmente para novas declarações de saída com o tipo de declaração de saída da função.  
  
## <a name="filtering-a-claim"></a>Filtrando uma reivindicação  
No AD FS, o termo *declarações filtragem* significa filtrar ou restringir a entrada reivindica valores de modo que apenas determinados valores são passados ou enviadas por meio de como declarações de saída. É o **passagem ou filtro uma declaração de entrada** modelo de regra que possibilita a essa função. Dentro das propriedades dessa regra, você pode definir condições para filtrar valores de entrada para que apenas os valores que atendem aos critérios especificados são passados.  
  
Por exemplo, você pode usar essa regra para passar apenas por meio de declarações que correspondem ao valor de declaração do comprador quando a entrada reivindicar correspondências de tipo, o tipo de declaração de função ou você talvez queira emitir apenas declarações sobre o nome do usuário, mas não declarações que contém o número de CPF do usuário.  
  
Quando você usa uma condição de filtro com essa regra, todas as reclamações de entrada são examinadas para determinar quais requerimentos judiciais ou Extrajudiciais atendem aos critérios definidos pela regra. Todos os outros requerimentos são ignorados para que apenas os valores de declaração especificado que correspondem a um tipo de declaração selecionado irão passar.  
  
Por exemplo, conforme mostrado na ilustração a seguir, quando uma regra é definida com a condição para filtrar apenas entradas declarações que são mapeadas para o UPN tipo de declaração e também termina com @fabrikam.com, todos os outros requerimentos de entrada são ignorados, a menos que eles atendem a esses critérios. Isso inclui a declaração de entrada com o tipo de declaração do endereço de email E\, mesmo que seu valor reivindicação termina em @fabrikam.com. Nesse caso, somente a declaração que contém o valor de Nick@fabrikam.comé enviada para o terceiro.  
  
![Quando usar o pass por meio de](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Quando você usa uma relação de confiança do provedor de declarações, essa regra pode ser configurada para passar por meio de entrada única declarações do provedor de declarações que corresponderem a determinadas restrições. Por exemplo, convém aceitar apenas email e\ declarações do provedor de declarações; Portanto, você usaria esse modelo de regra para aceitar os tipos de declaração e\ email que terminam em nome de Domain Name System \(DNS\) do provedor de declarações.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurando essa regra em uma terceira confiança de terceiros  
Quando você usa uma relação de confiança terceira parte, essa regra pode ser configurada para passar por ou filtrar declarações de saída que serão enviadas para o terceiro. Algumas partes confiantes podem não entender determinados tipos de declaração ou determinadas declarações podem conter informações confidenciais que não devem ser enviadas para determinadas partes confiantes. Esse modelo de regra pode ajudar a impor essas políticas para uma relação de confiança terceira por terceiros específico.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Criar essa regra usando a linguagem de regra de declaração ou usando a passagem ou filtrar um modelo de regra entrada declaração no AD FS gerenciamento snap\-in. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especifique um nome de regra de declaração  
  
-   Especificar um tipo de declaração de entrada  
  
-   Passar por todos os valores de declaração  
  
-   Passagem somente um determinado valor de solicitação  
  
-   Passar por meio de apenas os valores reivindicação que correspondem a um valor de sufixo e\-mensagens específicas  
  
-   Passar por meio de apenas os valores de declaração que começam com um valor específico  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para passagem ou filtrar uma declaração de entrada](https://technet.microsoft.com/library/dd807060.aspx) no guia de implantação do AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando a linguagem de regra de declaração  
Se uma reivindicação deve ser enviada apenas quando o valor da declaração corresponde a um padrão personalizado, você deve usar uma regra personalizada. Para obter mais informações, consulte quando usar uma regra personalizada.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Exemplos de como construir uma passagem ou sintaxe de regra de filtro  
Uma regra de filtragem simple seria filtrar requerimentos judiciais ou Extrajudiciais com base em uma das propriedades descritas acima. Por exemplo, a seguinte regra passará por meio de todas as reclamações e\ email:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Filtros podem ser logicamente e \ ed juntos. Por exemplo, a seguinte regra aceitará todas as reclamações e\ mail com valor johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Nos exemplos acima os filtros sempre usado um operador de igualdade. O idioma de regra reivindicação aceita os seguintes operadores:  
  
-   \ = \ = \-é igual a \(case\-sensitive\)  
  
-   \! \ = \-não é igual a \(case\-sensitive\)  
  
-   \ = ~ \-correspondência de expressão regular  
  
-   \! ~ \-expressão regular non\-match  
  
Por exemplo, a seguinte regra aceitará todas as reclamações e\ email não emitidas pelo servidor de federação local que têm um sufixo de boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Práticas recomendadas para criar regras personalizadas  
Um filtro pode ser aplicado a um ou mais das propriedades de cada declaração, conforme descrito na tabela a seguir.  
  
|Propriedade declaração|Descrição|  
|------------------|---------------|  
|Tipo|O tipo de declaração \ (normalmente representada como um Uri\) reflete um contrato implícito entre parceiros em uma reunião sobre que tipo de informações transportado em reivindicação. Por exemplo, declarações de tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress conterá o endereço de email e\ do usuário.|  
|Valor|O valor da reivindicação. Por exemplo, uma declaração de tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress pode ter um valor de johndoe@fabrikam.com|  
|ValueType|O ValueType representa como as informações contidas no valor da declaração são deve ser interpretado. Normalmente ValueType será definido como http:///\/www.w3.org\/2001\/XMLSchema\#string, mas o valor da declaração pode conter dados Base64Binary codificado \ (por exemplo, um image\) ou uma data, Boolean e assim por diante.|  
|Emissor|O emissor representa a parte que emitiu última as declarações sobre o usuário. Se as declarações são obtidas em um servidor de Federação do provedor de declarações que o emissor de todas as reclamações vai ser definido como "Autoridade LOCAL". Se as declarações foram recebidas por um servidor de Federação do provedor de federação, o emissor das declarações vai ser definido como o identificador de provedor de declarações do provedor requerimentos judiciais ou Extrajudiciais assinado o token. Assim, ao processar regras em declarações recebidas de um provedor de declarações o emissor de todas as reclamações vai ser definido como o mesmo valor. Ao criar regras para um terceiro, a propriedade issuer pode ser usada para distinguir entre os requerimentos judiciais ou Extrajudiciais originários de provedores de declarações diferentes.|  
|OriginalIssuer|Essa propriedade declaração serve para transmitir qual servidor de Federação originalmente emitiu a declaração. Como a propriedade issuer de reivindicações está definida para o último servidor de Federação assinado o token, o emissor original é útil em cenários onde uma reivindicação tem flui através de mais de um servidor de Federação \ (por exemplo, um terceiro que recebe um token de um servidor de Federação do provedor de Federação pode estar interessado quais determinada declarações do servidor de Federação do provedor autenticado o user\)|  
|Propriedades|Além dos cinco propriedades descritas acima, cada declaração também tem um recipiente de propriedades onde propriedades nomeadas podem ser armazenadas. Essas propriedades não são serializadas no token e apenas fazem sentido para passar informações entre os componentes do pipeline de emissão requerimentos judiciais ou Extrajudiciais no escopo de um servidor de Federação único. Por exemplo, definir uma propriedade durante requerimentos judiciais ou Extrajudiciais regras de provedor de processamento e, em seguida, faz referência a ele na terceira regras de terceiros.|  
  

