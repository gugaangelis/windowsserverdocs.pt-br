---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: Quando usar uma regra de declaração de transformação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3d39a4763eebe3e2fa28252c785a3b4d87e60143
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958734"
---
# <a name="when-to-use-a-transform-claim-rule"></a>Quando usar uma regra de declaração de transformação
Você pode usar essa regra em Serviços de Federação do Active Directory (AD FS) \( AD FS \) quando precisar mapear um tipo de declaração de entrada para um tipo de declaração de saída e, em seguida, aplicar uma ação que determinará qual saída deve ocorrer com base nos valores originados na declaração de entrada. Quando você usa essa regra, passa ou transforma declarações que correspondem à lógica da regra a seguir, com base em uma das opções configuradas na regra, conforme descrito na tabela abaixo:

|Opção de regras|Lógica de regras|
|---------------|--------------|
|Passar todas as declarações de entrada|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *qualquer valor*, passe a declaração com um tipo de declaração de saída igual a *tipo de declaração especificada*|
|Substituir um valor de declaração de entrada por um valor diferente de declaração de saída|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *valor de declaração especificada*, transforme a declaração no novo valor de declaração de saída *valor de declaração especificada* e no tipo de declaração de saída *tipo de declaração especificada*|
|Substituindo \- declarações de sufixo de email de entrada por um novo \- sufixo de email|Se o tipo de declaração de entrada for igual a *tipo de declaração especificada* e o valor for igual a *qualquer valor de sufixo*, transforme a declaração no novo valor de declaração de saída *valor de declaração especificada* e no tipo de declaração de saída *tipo de declaração especificada*|

As seções a seguir fornecem uma introdução básica às regras de declaração e mais detalhes sobre quando usar essa regra.

## <a name="about-claim-rules"></a>Sobre as regras de declaração
Uma regra de declaração representa uma instância da lógica de negócios que usará uma declaração de entrada, aplicará uma condição a ela \( se x depois y \) e produzir uma declaração de saída com base nos parâmetros de condição. A lista a seguir descreve dicas importantes que você deve conhecer sobre as regras de declaração antes de ler mais neste tópico:

-   No snap in de gerenciamento de AD FS \- , as regras de declaração só podem ser criadas usando modelos de regra de declaração

-   As regras de declaração processam declarações de entrada diretamente de um provedor de declarações \( , como Active Directory ou outro serviço de Federação \) ou da saída das regras de transformação de aceitação em uma confiança do provedor de declarações.

-   As regras de declaração são processadas pelo mecanismo de emissão de declarações em ordem cronológica dentro de um determinado conjunto de regras. Ao definir a precedência em regras, você pode refinar ou filtrar mais as declarações geradas pelas regras anteriores dentro de um determinado conjunto de regras.

-   Os modelos de regra de declaração sempre exigirão que você especifique um tipo de declaração de entrada. No entanto, você pode processar vários valores de declaração com o mesmo tipo de declaração usando uma única regra.

Para obter informações mais detalhadas sobre regras de declaração e conjuntos de regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como as regras são processadas, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md). Para obter mais informações sobre como os conjuntos de regras de declaração são processados, consulte [a função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md).

## <a name="pass-through-all-claim-values"></a>Passar todos os valores da declaração
Ao usar essa ação, todos os valores de declaração de entrada inseridos em um tipo de declaração de entrada especificada são mapeados para um tipo de declaração de saída especificada antes de serem enviados como declarações de saída para tokens assinados pelo Serviço de Federação.

Por exemplo, quando uma regra é definida com a opção lógica **Passar todos os valores da declaração** e o tipo de declaração de entrada do Grupo e o tipo de declaração de saída Função é especificado, todos os valores de declaração de entrada que fluem do emissor são copiados individualmente nas novas declarações de saída com o tipo de declaração de Função.

## <a name="transforming-a-claim"></a>Transformando uma declaração
Em AD FS, o termo *transformação de declarações* significa substituir um valor de declaração de entrada por um valor de declaração de saída diferente. É a regra Transformar uma declaração de entrada que torna essa função possível. Dentro das propriedades dessa regra, você pode definir condições para transformar os valores de entrada em um valor de declaração de saída diferente com base no tipo de declaração de entrada especificado.

Por exemplo, conforme mostrado na ilustração a seguir, quando uma regra é definida com a condição de substituir um valor de entrada por um valor diferente de declaração de saída, todos os tipos de declaração de entrada do Grupo são mapeados para novos tipos de declaração de saída da Função. Nesse caso, o valor da declaração de entrada do comprador é substituído pelo novo valor de declaração de saída do administrador.

![Quando usar uma transformação](media/adfs2_transform.gif)

Você também pode usar essa regra para aplicar uma condição que substituirá todas as declarações de entrada por um \- valor de sufixo de email especificado por um novo valor. Por exemplo, você pode definir uma condição nessa regra para alterar todos os valores de declaração com o sufixo sales.corp.fabrikam.com para fabrikam.com.

## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurando essa regra em uma relação de confiança do provedor de declarações
Quando você usa uma relação de confiança do provedor de declarações, essa regra pode ser configurada para transformar regras de entrada do provedor de declarações em equivalentes confiáveis. Tipos ou valores de declaração podem ter um significado diferente em sua organização do que nas organizações do provedor de declarações. Você pode usar essa regra para normalizar os tipos e valores de declaração do provedor de declarações para que seus equivalentes de declaração de saída possam ser compreendidos pela terceira parte confiável.

## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurando essa regra em um objeto de confiança de terceira parte confiável
Quando você usa um objeto de confiança de terceira parte confiável, essa regra pode ser configurada para transformar declarações para a terceira parte confiável específica. Tipos ou valores de declaração podem ter um significado diferente para uma terceira parte confiável específica e essa regra possibilita alterar os tipos e valores de declaração de saída para uma única parte confiável.

## <a name="how-to-create-this-rule"></a>Como criar essa regra
Você cria essa regra usando o idioma da regra de declaração ou usando o modelo **transformar uma** regra de declaração de entrada no snap-in de gerenciamento de AD FS \- . Essa regra fornece as seguintes opções de configuração:

-   Especificar um nome de regra de declaração

-   Transformar um tipo específico de declaração de entrada em um tipo de declaração de saída especificada

-   Passar todos os valores da declaração

-   Substituir um valor de declaração de entrada por um valor diferente de declaração de saída

-   Substituir declarações de \- sufixo de email de entrada por um novo \- sufixo de email

Para obter mais instruções sobre como criar esse modelo, consulte [criar uma regra para transformar uma declaração de entrada](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807068(v=ws.11)) no guia de implantação de AD FS.

## <a name="using-the-claim-rule-language"></a>Usando linguagem de regra de declaração
Se a declaração de saída for construída por meio do conteúdo de mais de uma declaração de entrada, você deverá usar uma regra personalizada. Se o valor de declaração da declaração de saída for baseada no valor de declaração de entrada, mas com conteúdo adicional, você também deverá usar uma regra personalizada nesse contexto. Para obter mais informações, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).

### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Exemplos de como construir uma sintaxe de regra de transformação
Ao usar a sintaxe de linguagem de regra de declaração para transformar declarações, você pode definir uma propriedade da declaração transformada em um novo valor literal. Por exemplo, a regra a seguir altera o valor das declarações de função de "administradores" para "raiz", mantendo o mesmo tipo de declaração:

```
c:[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/role", value == "Administrators"]  => issue(type = c.type, value = "root");
```

Expressões regulares também podem ser usadas para transformações de declaração. Por exemplo, a regra a seguir definirá o domínio nas declarações de nome de usuário do Windows no formato de usuário de domínio \\ para a Fabrikam:

```
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));
```

### <a name="best-practices-for-creating-custom-rules"></a>Práticas recomendadas para criar regras personalizadas
Transformação de declarações podem ser aplicadas seletivamente para declarações selecionadas usando recursos básicos de filtragem. A cada uma das propriedades da declaração usadas para filtragem podem ser atribuídos valores, com as seguintes condições:

|Propriedade da declaração|Descrição|
|------------------|---------------|
|Tipo, Valor, ValueType|Essas propriedades serão usadas com mais frequência para atribuições. No mínimo, tipo e valor devem ser especificados para a declaração transformada resultante.|
|Emissor|Embora a linguagem da regra de declaração permita definir o Emissor de uma declaração, isso geralmente não é aconselhável. O emissor de uma declaração não é serializado no token. Quando um token é recebido, a propriedade Emissor de todas as declarações é definida como o identificador do servidor de federação que assinou o token. Portanto, a definição do emissor de uma declaração nas regras não terá efeito no conteúdo do token e a definição será perdida quando a declaração for empacotada em um token. O único cenário em que a definição do emissor de uma declaração faz sentido é se ela for definida como um valor específico no conjunto de regras do provedor de declarações e o conjunto de regras da terceira parte confiável for criado com regras que fazem referência a esse valor específico. Se a propriedade emissor não estiver explicitamente definida como um valor em uma regra de declaração, o mecanismo de emissão de declarações a definirá como "autoridade LOCAL".|
|OriginalIssuer|Da mesma forma do Emissor, OriginalIssuer não deve, geralmente, receber a atribuição explícita de um valor. Ao contrário de Emissor, a propriedade OriginalIssuer foi serializada no token, mas a expectativa dos consumidores de tokens é que, se definida, ela conterá o identificador do servidor de federação que originalmente emitiu uma declaração.|
|Propriedades|Conforme descrito na seção anterior, o recipiente de propriedades de uma declaração não é mantido no token, para que atribuições às propriedades só possam ser feitas se políticas locais subsequentes fizerem referência às informações armazenadas na propriedade.|

Para obter mais informações sobre como usar o idioma da regra de declaração, consulte [a função do idioma da regra de declaração](The-Role-of-the-Claim-Rule-Language.md).

