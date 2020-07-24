---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Quando usar uma regra de declaração de passagem ou filtro
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d41a255d38cae1144d73b70b8dc433c222911b96
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958588"
---
# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quando usar uma regra de declaração de passagem ou filtro
Você pode usar essa regra em Serviços de Federação do Active Directory (AD FS) \( AD FS \) quando precisar usar um tipo de declaração de entrada específico e, em seguida, aplicar uma ação que determinará qual saída deve ocorrer com base nos valores na declaração de entrada. Quando você usa essa regra, passa ou filtra quaisquer declarações que correspondem à lógica de regra na tabela a seguir, com base em uma das opções configuradas na regra.  
  
|Opção de regras|Lógica de regras|  
|---------------|--------------|  
|Passar todos os valores da declaração|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *qualquer valor*, passe a declaração|  
|Passar apenas um valor de declaração específica|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *valor da declaração especificada*, passe a declaração|  
|Passar apenas valores de declaração que correspondam a um valor de sufixo de email e específico \-|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *valor de sufixo especificado*, passe a declaração|  
|Passar apenas valores de declaração que iniciam com um valor específico|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor começar com *valor de declaração especificada*, passe a declaração|  
  
As seções a seguir fornecem uma introdução básica às regras de declaração e mais detalhes sobre quando usar essa regra.  
  
## <a name="about-claim-rules"></a>Sobre as regras de declaração  
Uma regra de declaração representa uma instância da lógica de negócios que usará uma declaração de entrada, aplicará uma condição a ela \( se x depois y \) e produzir uma declaração de saída com base nos parâmetros de condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:  
  
-   No snap in de gerenciamento de AD FS \- , as regras de declaração só podem ser criadas usando modelos de regra de declaração  
  
-   As regras de declaração processam declarações de entrada diretamente de um provedor de declarações \( , como Active Directory ou outro serviço de Federação \) ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.  
  
-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.  
  
-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.  
  
Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Passar todos os valores da declaração  
Ao usar essa ação, todos os valores de declaração de entrada para o tipo de declaração especificado são passados como declarações de saída. Por exemplo, quando o tipo de declaração de entrada é especificado como o tipo de declaração de função, todos os valores de declaração de entrada são copiados individualmente em novas declarações de saída com o tipo de saída de Função.  
  
## <a name="filtering-a-claim"></a>Filtrando uma declaração  
Em AD FS, o termo *filtragem de declarações* significa filtrar ou restringir valores de declaração de entrada para que apenas determinados valores sejam passados ou enviados por meio de declarações de saída. É o modelo de regra **Passar ou filtrar uma declaração de entrada** que torna essa função possível. Dentro das propriedades dessa regra, você pode definir condições para filtrar valores de entrada, para que somente os valores que atendam aos critérios especificados sejam passados.  
  
Por exemplo, você pode usar essa regra para passar apenas declarações que correspondam ao o valor da declaração do Comprador quando o tipo de declaração de entrada corresponder ao tipo de declaração da Função, ou emita apenas declarações sobre o nome do usuário, mas não as declarações contendo cadastro de pessoas físicas do usuário.  
  
Quando você usa uma condição de filtro com essa regra, todas as declarações de entrada são examinadas para determinar quais declarações atendem ao conjunto de critérios definidos pela regra. Todas as outras declarações são ignoradas para que somente os valores da declaração especificada, que correspondem a um tipo de declaração selecionado, passem.  
  
Por exemplo, conforme mostrado na ilustração a seguir, quando uma regra é definida com a condição para filtrar somente as declarações de entrada que são digitadas para o tipo de declaração UPN e também terminam com @fabrikam.com , todas as outras declarações de entrada são ignoradas, a menos que atendam a esses critérios. Isso inclui a declaração de entrada com o tipo de declaração de \- endereço de email, embora seu valor de declaração termine @fabrikam.com . Nesse caso, somente a declaração que contém o valor de Nick@fabrikam.com é enviada para a terceira parte confiável.  
  
![Quando usar passagem](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações  
Quando você usa uma relação de confiança do provedor de declarações, essa regra pode ser configurada para passar apenas declarações de entrada do provedor de declarações que correspondam a determinadas restrições. Por exemplo, talvez você queira aceitar apenas \- declarações de email do provedor de declarações; portanto, você usaria esse modelo de regra para aceitar tipos de \- declaração de email que terminem no nome DNS do sistema de nome de domínio do provedor de declarações \( \) .  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurando essa regra em um objeto de confiança de terceira parte confiável  
Quando você usa um objeto de confiança de terceira parte confiável, essa regra pode ser configurada para passar ou filtrar declarações de saída que serão enviadas para a terceira parte confiável. Algumas partes confiáveis talvez não compreenda certos tipos de declaração ou determinadas declarações podem conter informações confidenciais que não devem ser enviadas para determinadas terceiras partes. Esse modelo de regra pode ajudar a impor essas políticas para um objeto de confiança de terceira parte confiável específico.  
  
## <a name="how-to-create-this-rule"></a>Como criar essa regra  
Você pode criar essa regra usando o idioma da regra de declaração ou usando o modelo passagem ou filtrar um regra de declaração de entrada no snap-in de gerenciamento de AD FS \- . Essa regra fornece as seguintes opções de configuração:  
  
-   Especificar um nome de regra de declaração  
  
-   Especificar um tipo de declaração de entrada  
  
-   Passar todos os valores da declaração  
  
-   Passar apenas um valor de declaração específica  
  
-   Passar apenas valores de declaração que correspondam a um valor de sufixo de email e específico \-  
  
-   Passar apenas valores de declaração que iniciam com um valor específico  
  
Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para passar ou filtrar uma declaração de entrada](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807060(v=ws.11)) no guia de implantação de AD FS.  
  
## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração  
Se uma declaração dever ser enviada apenas quando o valor da declaração corresponder a um padrão personalizado, você deverá usar uma regra personalizada. Para obter mais informações, consulte Quando usar uma regra personalizada.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Exemplos de como construir uma sintaxe de regras de passagem ou filtro  
Uma regra de filtragem simples filtraria declarações com base em uma das propriedades descritas acima. Por exemplo, a regra a seguir passará por todas as \- declarações de email:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"]  => issue(claim  = c);  
```  
  
Os filtros podem ser logicamente e \- Ed juntos. Por exemplo, a regra a seguir aceitará todas as \- declarações de email com valorjohndoe@fabrikam.com:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", value == "johndoe@fabrikam.com "]  => issue(claim  = c);  
```  
  
Nos exemplos acima, os filtros usaram sempre um operador de igualdade. A linguagem de regra de declaração dá suporte aos seguintes operadores:  
  
-   \=\=\- \( diferenciar maiúsculas de minúsculas \-\)  
  
-   \!\=\-não é igual a \( maiúsculas e \- minúsculas\)  
  
-   \=~\-correspondência de expressão regular  
  
-   \!~ \-expressão regular sem \- correspondência  
  
Por exemplo, a regra a seguir aceitará todas as \- declarações de email não emitidas pelo servidor de federação local que têm um sufixo de Boeing.com:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", value =~ "^.*@boeing\.com$" , issuer != "LOCAL AUTHORITY"]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Práticas recomendadas para criar regras personalizadas  
Um filtro pode ser aplicado a uma ou mais das propriedades de cada declaração, conforme descrito na tabela a seguir.  
  

| Propriedade da declaração |                                                                                                                                                                                                                                                                                                                                                  Descrição                                                                                                                                                                                                                                                                                                                                                  |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Type      |                                                                                                                                                                                        O tipo de Declaração \( geralmente representado como um URI \) reflete um acordo implícito entre parceiros em uma federação sobre que tipo de informação é transmitida na declaração. Por exemplo, as declarações do tipo http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ Identity \/ Claims EmailAddress conterá \/ o \- endereço de email e do usuário.                                                                                                                                                                                         |
|     Valor      |                                                                                                                                                                                                                                                                   O valor da declaração. Por exemplo, uma declaração do tipo http: \/ \/schemas.xmlsoap.org \/ WS \/ 2005 \/ 05 \/ Identity \/ Claims \/ EmailAddress pode ter um valor dejohndoe@fabrikam.com                                                                                                                                                                                                                                                                    |
|   ValueType    |                                                                                                                                                                                                  O ValueType representa como as informações contidas no valor da declaração são interpretadas. Normalmente, o ValueType será definido como http: \/ \/ www.w3.org \/ 2001 \/ XmlSchema \# String, mas o valor da declaração poderá conter dados codificados Base64Binary, \( por exemplo, uma imagem \) ou uma data, booliano e assim por diante.                                                                                                                                                                                                  |
|     Emissor     | O emissor representa a parte que emitiu por último as declarações sobre o usuário. Se as declarações forem obtidas em um servidor de Federação do provedor de declarações, o emissor de todas as declarações será definido como "autoridade LOCAL". Se as declarações foram recebidas por um servidor de federação do Provedor de Federações, o emissor das declarações será definido como o identificador do provedor de declarações do provedor de declarações que assinou o token. Assim, durante o processamento de regras de declarações recebidas de um provedor de declarações, o emissor de todas as declarações será definido como o mesmo valor. Ao criar regras para uma terceira parte confiável, a propriedade emissora pode ser usada para distinguir entre solicitações originadas de provedores de declarações diferentes. |
| OriginalIssuer |                                                                                                   Essa propriedade de declaração deve transmitir o servidor de federação emitiu originalmente a declaração. Como a propriedade emissor de declarações é definida como o último servidor de Federação que assinou o token, o emissor original é útil em cenários em que uma declaração fluiu por mais de um servidor de Federação \( , por exemplo, uma terceira parte confiável que recebe um token de um servidor de Federação do provedor de Federação pode estar interessada em que o servidor de Federação do provedor de declarações específico autenticou o usuário\)                                                                                                   |
|   Propriedades   |                                                                                                                             Além das cinco propriedades descritas acima, cada declaração também tem um recipiente de propriedades no qual as propriedades nomeadas podem ser armazenadas. Essas propriedades não são serializadas no token e só fazem sentido para transmitir informações entre os componentes do pipeline de emissão de declarações dentro do escopo de um servidor de federação único. Por exemplo, configuração de uma propriedade durante processamento de regras do provedor de declarações e depois fazer referência a ela nas regras da terceira parte confiável.                                                                                                                              |
