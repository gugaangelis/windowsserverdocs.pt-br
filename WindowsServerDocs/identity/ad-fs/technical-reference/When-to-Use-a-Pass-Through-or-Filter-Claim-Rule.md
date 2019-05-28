---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Quando usar uma regra de declaração de passagem ou filtro
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 09f26e256793d30936496f7a936550acb7b20025
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188346"
---
# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quando usar uma regra de declaração de passagem ou filtro
Você pode usar essa regra nos serviços de Federação do Active Directory \(do AD FS\) quando precisar usar um tipo específico de declaração de entrada e, em seguida, aplicar uma ação que determinará qual saída deve ocorrer com base nos valores de declaração de entrada. Quando você usa essa regra, passa ou filtra quaisquer declarações que correspondem à lógica de regra na tabela a seguir, com base em uma das opções configuradas na regra.  
  
|Opção de regras|Lógica de regras|  
|---------------|--------------|  
|Passar todos os valores da declaração|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *qualquer valor*, passe a declaração|  
|Passar apenas um valor de declaração específica|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *valor da declaração especificada*, passe a declaração|  
|Passar apenas valores de declaração que correspondem a um determinado e\-valor de sufixo de email|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *valor de sufixo especificado*, passe a declaração|  
|Passar apenas valores de declaração que iniciam com um valor específico|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor começar com *valor de declaração especificada*, passe a declaração|  
  
As seções a seguir fornecem uma introdução básica às regras de declaração e mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre regras de declaração  
Uma regra de declaração representa uma instância de lógica de negócios que irá levar uma declaração de entrada, aplicar uma condição a ela \(se x e y\) e produzir uma declaração de saída com base nos parâmetros da condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:  
  
-   No snap do gerenciamento do AD FS\-, declaração de regras só podem ser criadas usando modelos de regra de declaração  
  
-   Processo de regras de declaração entrada declarações diretamente de um provedor de declarações \(como o Active Directory ou outro serviço de Federação\) ou da saída da aceitação regras de transformação em uma relação de confiança do provedor de declarações.  
  
-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.  
  
-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter mais informações sobre regras de declaração e conjuntos de regras de declaração, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obter mais informações como os conjuntos de regras de declaração são processados, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passar todos os valores da declaração  
Ao usar essa ação, todos os valores de declaração de entrada para o tipo de declaração especificado são passados como declarações de saída. Por exemplo, quando o tipo de declaração de entrada é especificado como o tipo de declaração de função, todos os valores de declaração de entrada são copiados individualmente em novas declarações de saída com o tipo de saída de Função.  
  
## <a name="filtering-a-claim"></a>Filtrando uma declaração  
No AD FS, o termo *filtragem de declarações* significa filtrar ou restringir a entrada valores de declaração para que somente determinados valores são passados ou enviados como declarações de saída. É o modelo de regra **Passar ou filtrar uma declaração de entrada** que torna essa função possível. Dentro das propriedades dessa regra, você pode definir condições para filtrar valores de entrada, para que somente os valores que atendam aos critérios especificados sejam passados.  
  
Por exemplo, você pode usar essa regra para passar apenas declarações que correspondam ao o valor da declaração do Comprador quando o tipo de declaração de entrada corresponder ao tipo de declaração da Função, ou emita apenas declarações sobre o nome do usuário, mas não as declarações contendo cadastro de pessoas físicas do usuário.  
  
Quando você usa uma condição de filtro com essa regra, todas as declarações de entrada são examinadas para determinar quais declarações atendem ao conjunto de critérios definidos pela regra. Todas as outras declarações são ignoradas para que somente os valores da declaração especificada, que correspondem a um tipo de declaração selecionado, passem.  
  
Por exemplo, conforme mostrado na ilustração a seguir, quando uma regra é definida com a condição para filtrar declarações de entrada apenas que são criptografadas para o UPN de declaração de tipo e também terminar com @fabrikam.com, todas as outras declarações de entrada são ignoradas, a menos que eles atendem a esse critério. Isso inclui a declaração de entrada com o tipo de declaração de eletrônico\-endereço de email mesmo que o valor da declaração termina em @fabrikam.com. Nesse caso, somente a declaração contém o valor de Nick@fabrikam.com é enviada para a terceira parte confiável.  
  
![Quando usar passagem por meio de](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Quando você usa uma relação de confiança do provedor de declarações, essa regra pode ser configurada para passar apenas declarações de entrada do provedor de declarações que correspondam a determinadas restrições. Por exemplo, você talvez queira apenas aceitar e\-declarações de email do provedor de declarações; portanto, você usaria esse modelo de regra para aceitar e\-tipos de declaração que terminam no sistema de nomes de domínio do provedor de declarações de email \(DNS\) nome.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurando essa regra em um objeto de confiança de terceira parte confiável  
Quando você usa um objeto de confiança de terceira parte confiável, essa regra pode ser configurada para passar ou filtrar declarações de saída que serão enviadas para a terceira parte confiável. Algumas partes confiáveis talvez não compreenda certos tipos de declaração ou determinadas declarações podem conter informações confidenciais que não devem ser enviadas para determinadas terceiras partes. Esse modelo de regra pode ajudar a impor essas políticas para um objeto de confiança de terceira parte confiável específico.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Crie essa regra usando a linguagem de regra de declaração ou usando a passagem ou filtrar um modelo de regra de declaração de entrada no snap do gerenciamento do AD FS\-no. Esse modelo de regra fornece as seguintes opções de configuração:  
  
-   Especificar um nome de regra de declaração  
  
-   Especificar um tipo de declaração de entrada  
  
-   Passar todos os valores da declaração  
  
-   Passar apenas um valor de declaração específica  
  
-   Passar apenas valores de declaração que correspondem a um determinado e\-valor de sufixo de email  
  
-   Passar apenas valores de declaração que iniciam com um valor específico  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para passar ou filtrar uma declaração de entrada](https://technet.microsoft.com/library/dd807060.aspx) no guia de implantação do AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Se uma declaração dever ser enviada apenas quando o valor da declaração corresponder a um padrão personalizado, você deverá usar uma regra personalizada. Para obter mais informações, consulte Quando usar uma regra personalizada.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Exemplos de como construir uma sintaxe de regras de passagem ou filtro  
Uma regra de filtragem simples filtraria declarações com base em uma das propriedades descritas acima. Por exemplo, a seguinte regra passará por meio de todas as e\-declarações de email:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
Os filtros podem ser logicamente AND\-ed juntos. Por exemplo, a seguinte regra aceitará todas as e\-declarações com valor de email johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Nos exemplos acima, os filtros usaram sempre um operador de igualdade. A linguagem de regra de declaração dá suporte aos seguintes operadores:  
  
-   \=\= \- é igual a \(caso\-confidenciais\)  
  
-   \!\= \- não é igual a \(caso\-confidenciais\)  
  
-   \=~\- correspondência de expressão regular  
  
-   \!~ \- a expressão regular não\-corresponder  
  
Por exemplo, a seguinte regra aceitará todas as e\-de email não emitidas pelo servidor de federação local de declarações que têm um sufixo de boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Práticas recomendadas para criar regras personalizadas  
Um filtro pode ser aplicado a uma ou mais das propriedades de cada declaração, conforme descrito na tabela a seguir.  
  
|Propriedade da declaração|Descrição|  
|------------------|---------------|  
|Tipo|O tipo de declaração \(normalmente é representado como um Uri\) reflete um contrato implícito entre parceiros em uma federação sobre que tipo de informação é transmitido na declaração. Por exemplo, declarações de tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identidade\/declarações\/emailaddress conterá ae\-endereço do usuário de email.|  
|Valor|O valor da declaração. Por exemplo, uma declaração de tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identidade\/declarações\/emailaddress pode ter um valor de johndoe@fabrikam.com|  
|ValueType|ValueType representa como as informações contidas no valor da declaração devem ser interpretadas. Normalmente ValueType será definido como http:\/\/www.w3.org\/2001\/XMLSchema\#cadeia de caracteres, mas o valor da declaração pode conter dados codificados de Base64Binary \(, por exemplo, uma imagem \) ou uma data, booliano e assim por diante.|  
|Emissor|O emissor representa a parte que emitiu por último as declarações sobre o usuário. Se as declarações forem obtidas em um servidor de federação do provedor de declarações, o emissor de todas as declarações será definido como "LOCAL AUTHORITY". Se as declarações foram recebidas por um servidor de federação do Provedor de Federações, o emissor das declarações será definido como o identificador do provedor de declarações do provedor de declarações que assinou o token. Assim, durante o processamento de regras de declarações recebidas de um provedor de declarações, o emissor de todas as declarações será definido como o mesmo valor. Ao criar regras para uma terceira parte confiável, a propriedade emissora pode ser usada para distinguir entre solicitações originadas de provedores de declarações diferentes.|  
|OriginalIssuer|Essa propriedade de declaração deve transmitir o servidor de federação emitiu originalmente a declaração. Como a propriedade emissor de declarações está definida para o último servidor de federação que assinou o token, o emissor original é útil em cenários em que uma declaração passou por meio de mais de um servidor de Federação \(por exemplo, uma terceira parte confiável que recebe um token servidor de federação de um provedor de Federação pode estar interessado qual determinado servidor de Federação do provedor de declarações de usuário autenticarem\)|  
|Propriedades|Além das cinco propriedades descritas acima, cada declaração também tem um recipiente de propriedades no qual as propriedades nomeadas podem ser armazenadas. Essas propriedades não são serializadas no token e só fazem sentido para transmitir informações entre os componentes do pipeline de emissão de declarações dentro do escopo de um servidor de federação único. Por exemplo, configuração de uma propriedade durante processamento de regras do provedor de declarações e depois fazer referência a ela nas regras da terceira parte confiável.|  
  

