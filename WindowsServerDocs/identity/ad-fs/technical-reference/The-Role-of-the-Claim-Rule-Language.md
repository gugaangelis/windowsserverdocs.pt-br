---
title: A função da linguagem da regra de declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: ff4c43bb8dc5582716638f0a3f6e4f6a8022aece
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407370"
---
# <a name="the-role-of-the-claim-rule-language"></a>A função da linguagem da regra de declaração
O idioma da regra de declaração de Serviços de Federação do Active Directory (AD FS) (AD FS) atua como o bloco de construção administrativo para o comportamento de declarações de entrada e saída, enquanto o mecanismo de declarações atua como mecanismo de processamento para a lógica no idioma da regra de declaração que define a regra personalizada. Para obter mais informações sobre como todas as regras são processadas pelo mecanismo de declarações, consulte [a função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md).  

## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Criando regras de declaração personalizadas usando a linguagem de regras de declaração  
AD FS fornece aos administradores a opção de definir regras personalizadas que podem ser usadas para determinar o comportamento de declarações de identidade com o idioma da regra de declaração. Você pode usar os exemplos de sintaxe de linguagem de regra de declaração contidos neste tópico para criar uma regra personalizada que enumere, adicione, exclua e modifique as declarações para atender às necessidades de sua organização. Você pode criar regras personalizadas digitando a sintaxe da linguagem de regra de declaração no modelo de regra **Enviar Declarações Usando uma Declaração Personalizada**.  

As regras são separadas por ponto e vírgula.  

Para obter mais informações sobre quando usar regras personalizadas, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).  

## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Usando modelos de regra de declaração para saber mais sobre a sintaxe da linguagem de regras de declaração  
AD FS também fornece um conjunto de modelos de regra de emissão de declaração predefinidos e de regras de aceitação de declaração que você pode usar para implementar regras de declaração comuns. Na caixa de diálogo **Editar Regras de Declaração** de uma determinada relação de confiança, você pode criar uma regra pré-definida — e exibir a sintaxe da linguagem de regra de declaração que compõe essa regra — clicando na guia **Exibir Linguagem da Regra** guia para essa regra. O uso das informações desta seção e da técnica **Exibir Linguagem da Regra** pode fornecer informações sobre como criar suas próprias regras personalizadas.  

Para obter informações mais detalhadas sobre regras de declaração e modelos de regra de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md).  

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Entendendo os componentes da linguagem de regras de declaração  
O idioma da regra de declaração consiste nos seguintes componentes, separados pelo operador "= >":  

-   Uma condição  

-   Uma instrução de emissão  

### <a name="conditions"></a>Condições  
Você pode usar as condições de uma regra para verificar as declarações de entrada e determinar se a instrução de emissão da regra deve ser executada. Uma condição representa uma expressão lógica que deve ser avaliada como true para executar a parte do corpo da regra. Se essa parte estiver ausente, será assumida uma verdadeira lógica; ou seja, o corpo da regra é sempre executado. A parte Conditions contém uma lista de condições que são combinadas junto com o operador lógico conjunta ("& &"). Todas as condições da lista devem ser avaliadas como true para que toda a parte condicional seja avaliada como true. A condição pode ser um operador de seleção de declarações ou uma chamada de função de agregação. Essas duas são mutuamente exclusivas, o que significa que os seletores de declaração e as funções de agregação não podem ser combinados em uma só parte de condições de regra.  

As condições são opcionais nas regras. Por exemplo, a seguinte regra não tem uma condição:  

```  
=> issue(type = "http://test/role", value = "employee");  
```  

Há três tipos de condições:  

-   Condição única — é a forma mais simples de uma condição. As verificações são feitas para apenas uma expressão; por exemplo, nome da conta do Windows = domínio usuário.  

-   Várias condições — essa condição requer verificações adicionais para processar várias expressões no corpo da regra; por exemplo, nome da conta do Windows = domínio usuário e grupo = contosopurchasers.  

> [!NOTE]  
> Existe outra condição, mas ela é um subconjunto da condição única ou das várias condições. Ele é conhecido como uma condição de expressão regular (Regex). Essa condição é usada para obter uma expressão de entrada e correspondê-la a um determinado padrão. Um exemplo de como ela pode ser usada é exibido a seguir.  

Os exemplos a seguir mostram algumas das construções de sintaxe, que são baseadas nos tipos de condição e que podem ser usadas para criar regras personalizadas.  

#### <a name="single--condition-examples"></a>Exemplos de condição única  
Condições de expressão única são descritas na tabela a seguir. Elas são construídas para simplesmente verificar uma declaração com um tipo de declaração especificado ou para uma declaração com um tipo e um valor de declaração especificados.  


|                                                                                                                   Descrição da condição                                                                                                                    |                           Exemplo de sintaxe da condição                            |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
|               Essa regra tem uma condição para verificar uma declaração de entrada com um tipo de declaração especificado (<http://test/name>""). Se houver uma declaração correspondente nas declarações de entrada, a regra copia a(s) declaração(ões) correspondente no conjunto de declarações de saída.               |         ``` c: [type == "http://test/name"] => issue(claim = c );```          |
| Essa regra tem uma condição para verificar uma declaração de entrada com um tipo de declaração especificado (<http://test/name>"") e um valor de declaração ("Terry"). Se houver uma declaração correspondente nas declarações de entrada, a regra copia a(s) declaração(ões) correspondente no conjunto de declarações de saída. | ``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);``` |

Condições mais complexas são mostradas na próxima seção, incluindo condições para verificar várias declarações, condições para verificar o emissor de uma declaração e condições para verificar se há valores que correspondem a um padrão de expressão regular.  

#### <a name="multiple--condition-examples"></a>Exemplos de várias condições  
A tabela a seguir fornece um exemplo de condições de várias expressões.  


|                                                                                                                   Descrição da condição                                                                                                                    |                                        Exemplo de sintaxe da condição                                        |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Essa regra tem uma condição de verificação de duas declarações, cada um com um tipo de declaração especificado de entrada ("<http://test/name>" e "<http://test/email>"). Se houver duas declarações correspondentes nas declarações de entrada, a regra copia a declaração de nome no conjunto de declarações de saída. | ``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );``` |

#### <a name="regular--condition-examples"></a>Exemplos de condição regular  
A tabela a seguir fornece um exemplo de uma condição regular baseada em expressão.  

|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição que usa uma expressão regular para verificar se há uma declaração de e-mail terminando em "@fabrikam.com". Se houver uma declaração correspondente nas declarações de entrada, a regra copia a declaração correspondente no conjunto de declarações de saída.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  

### <a name="issuance-statements"></a>Instruções de emissão  
As regras personalizadas são processadas com base nas instruções de emissão (*problema* ou *adição* ) que você programa na regra de declaração. Dependendo do resultado desejado, a instrução de emissão ou a instrução de adição pode ser gravada na regra para preencher o conjunto de declarações de entrada ou o conjunto de declarações de saída. Uma regra personalizada que usa a instrução de adição explicitamente preenche os valores de declaração apenas para o conjunto de declarações de entrada, enquanto uma regra de declaração personalizada que usa a instrução de emissão preenche os valores de declarações no conjunto de declarações de entrada e no conjunto de declarações de saída. Isso pode ser útil quando um valor de declaração precisar ser usado somente por regras futuras no conjunto de regras de declaração.  

Por exemplo, na ilustração a seguir, a declaração de entrada é adicionada ao conjunto de declarações de entrada definido pelo mecanismo de emissão de declarações. Quando a primeira regra de declaração personalizada é executada e os critérios de usuário do domínio são satisfeitos, o mecanismo de emissão de declarações processa a lógica na regra usando a instrução add, e o valor do **Editor** é adicionado ao conjunto de declarações de entrada. Como o valor do editor está presente no conjunto de declarações de entrada, a regra 2 pode processar com êxito a instrução Issue em sua lógica e gerar um novo valor de **Hello**, que é adicionado ao conjunto de declarações de saída e ao conjunto de declarações de entrada para uso pela próxima regra no conjunto de regras. A regra 3 agora pode usar todos os valores que estão presentes no conjunto de declarações de entrada como entrada para processar sua lógica.  

![AD FS funções](media/adfs2_customrule.gif)  

#### <a name="claim-issuance-actions"></a>Ações de emissão de declarações  
O corpo da regra representa uma ação de emissão de declaração. Há duas ações de emissão de declaração que a linguagem reconhece:  

-   **Instrução de emissão:** A instrução de emissão cria uma declaração que vai para os conjuntos de declarações de entrada e de saída. Por exemplo, a instrução a seguir emite uma nova declaração com base em seu conjunto de declarações de entrada:  

    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  

-   **Instrução de adição:** A instrução de adição cria uma nova declaração que é adicionada apenas à coleção do conjunto de declarações de entrada. Por exemplo, a instrução a seguir adiciona uma nova declaração ao conjunto de declarações de entrada:  

    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 

A instrução de emissão de uma regra define quais declarações serão emitidas pela regra quando as condições forem atendidas. Há duas formas de instruções de emissão relacionadas aos argumentos e ao comportamento da instrução:  

-   **Normal** — as instruções de emissão normais podem emitir declarações usando valores literais na regra ou os valores de declarações que correspondem às condições. Uma instrução de emissão normal pode consistir em um ou ambos os seguintes formatos:  

    -   *Cópia de declaração*: A cópia de declaração cria uma cópia da declaração existente no conjunto de declarações de saída. Essa forma de emissão só faz sentido quando é combinada com a instrução "emissão". Quando ela é combinada com a instrução “adição”, não tem nenhum efeito.  

    -   *Nova declaração*: Esse formato cria uma nova declaração, dado os valores para várias propriedades de declaração. A propriedade Claim.Type deve ser especificada; todas as outras propriedades de declaração são opcionais. A ordem dos argumentos desse formato é ignorada.  

-   **Repositório de atributos**— este formato cria declarações com valores que são recuperados de um repositório de atributos. É possível criar vários tipos de declaração usando uma única instrução de emissão, que é importante para repositórios de atributos que fazem operações de e/s (entrada/saída) de rede ou disco durante a recuperação do atributo. Portanto, é recomendável limitar o número de viagens de ida e volta entre o mecanismo de políticas e o repositório de atributos. Também é permitido criar várias declarações para um determinado tipo de declaração. Quando o repositório de atributos retorna vários valores para um determinado tipo de declaração, a instrução de emissão cria automaticamente uma declaração para cada valor de declaração retornado. Uma implementação de repositório de atributos usa os argumentos de parâmetro para substituir os espaços reservados no argumento de consulta por valores que são fornecidas nos argumentos de parâmetro. Os espaços reservados usam a mesma sintaxe que a função String. Format () do .net (por exemplo {1} {2},, e assim por diante). A ordem dos argumentos para esse formato de emissão é importante e deve ser a ordem indicada na gramática a seguir.  

A tabela a seguir descreve algumas construções comuns de sintaxe para ambos os tipos de instruções de emissão em regras de declaração.  

|Tipo de instrução de emissão|Descrição de instrução de emissão|Exemplo de sintaxe de instrução de emissão|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|A seguinte regra sempre emite a mesma declaração sempre que um usuário tem o tipo de declaração especificado e o valor:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|A regra a seguir converte um tipo de declaração em outro. Observe que o valor da declaração que corresponde à condição "c" é usado na instrução de emissão.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Repositório de atributos|A regra a seguir usa o valor de uma declaração de entrada para consultar o repositório de atributos Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Repositório de atributos|A regra a seguir usa o valor de uma declaração de entrada para consultar um repositório de atributos de linguagem SQL (SQL) configurado anteriormente:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  

#### <a name="expressions"></a>Expressões  
As expressões são usadas no lado direito para as restrições do seletor de declarações e os parâmetros de instruções de emissão. Há vários tipos de expressões às quais a linguagem dá suporte. Todas as expressões da linguagem são baseadas em cadeias de caracteres, o que significa que elas usam as cadeias de caracteres como entrada e produzem cadeias de caracteres. Não há suporte para números ou outros tipos de dados, como data/hora, em expressões. A seguir, há os tipos de expressões às quais a linguagem dá suporte:  

-   Cadeia de caracteres literal: Valor da cadeia de caracteres, delimitado pelo caractere de aspas (") em ambos os lados.  

-   Concatenação de expressões da cadeia de caracteres: O resultado é uma cadeia de caracteres que é produzida pela concatenação dos valores à esquerda e à direita.  

-   Chamada de função: A função é identificada por um identificador e os parâmetros são passados como uma lista delimitada por vírgulas de expressões entre colchetes ("()").  

-   Acesso à propriedade da declaração no formato de nome da variável nome da propriedade ponto: O resultado do valor da propriedade da declaração identificada para uma determinada validação de variável. Primeiro, a variável deve ser associada a um seletor de declarações antes que possa ser usada dessa maneira. Não é possível usar a variável que está associada a um seletor de declarações dentro das restrições para esse mesmo seletor de declarações.  

As propriedades de declaração a seguir estão disponíveis para acesso:  

-   Claim.Type  

-   Claim.Value  

-   Claim.Issuer  

-   Claim.OriginalIssuer  

-   Claim.ValueType  

-   Claim. Properties @ no__t-0property @ no__t-1Name @ no__t-2 (essa propriedade retornará uma cadeia de caracteres vazia se a propriedade _name não puder ser encontrada na coleção de propriedades da declaração. ) simples  

Você pode usar a função RegexReplace para chamar dentro de uma expressão. Essa função usa uma expressão de entrada e faz a correspondência com o padrão fornecido. Se o padrão corresponder, a saída da correspondência é substituída pelo valor de substituição.  

#### <a name="exists-functions"></a>Função Exists  
A função Exists pode ser usada em uma condição para avaliar se uma declaração que corresponde à condição existe no conjunto de declarações de entrada. Se houver qualquer declaração correspondente, a instrução de emissão é chamada apenas uma vez. No exemplo a seguir, a declaração "origem" é emitida exatamente uma vez – se houver pelo menos uma declaração na coleção do conjunto de declarações de entrada que tenha o emissor definido como "MSFT", não importa quantas declarações tenham o emissor definido para "MSFT". Usar essa função impede a emissão de declarações duplicadas.  

```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  

## <a name="rule-body"></a>Corpo da regra  
O corpo da regra pode conter apenas uma só instrução de emissão. Se as condições forem usadas sem usar a função Exists, o corpo da regra é executado uma vez para cada vez em que houver a correspondência da parte de condições.  

## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar declarações usando uma regra personalizada](https://technet.microsoft.com/library/dd807049.aspx)  


