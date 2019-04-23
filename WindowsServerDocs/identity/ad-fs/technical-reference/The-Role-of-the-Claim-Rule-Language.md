---
title: A função da linguagem da regra de declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 05728f04f6fb924cf3793bc843df3832c7c383f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855687"
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>A função da linguagem da regra de declaração
Os serviços de Federação do Active Directory (AD FS) de declaração de linguagem da regra atua como o bloco de construção administrativo para o comportamento das declarações de entrada e saídas, enquanto o mecanismo de declarações atua como o mecanismo de processamento para a lógica na linguagem de regra de declaração que Define a regra personalizada. Para obter mais informações sobre como todas as regras são processadas pelo mecanismo de declarações, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Criando regras de declaração personalizadas usando a linguagem de regras de declaração  
O AD FS fornece aos administradores as opções para definir regras personalizadas que podem usar para determinar o comportamento das declarações de identidade com a linguagem de regra de declaração. Você pode usar os exemplos de sintaxe de linguagem de regra de declaração contidos neste tópico para criar uma regra personalizada que enumere, adicione, exclua e modifique as declarações para atender às necessidades de sua organização. Você pode criar regras personalizadas digitando a sintaxe da linguagem de regra de declaração no modelo de regra **Enviar Declarações Usando uma Declaração Personalizada**.  
  
As regras são separadas por ponto e vírgula.  
  
Para obter mais informações sobre quando usar regras personalizadas, consulte [quando usar uma regra de declaração personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Usando modelos de regra de declaração para saber mais sobre a sintaxe da linguagem de regras de declaração  
O AD FS também fornece um conjunto de emissão de declarações pré-definidas e modelos de regras de aceitação que você pode usar para implementar comum de regras de declaração de declaração. Na caixa de diálogo **Editar Regras de Declaração** de uma determinada relação de confiança, você pode criar uma regra pré-definida — e exibir a sintaxe da linguagem de regra de declaração que compõe essa regra — clicando na guia **Exibir Linguagem da Regra** guia para essa regra. O uso das informações desta seção e da técnica **Exibir Linguagem da Regra** pode fornecer informações sobre como criar suas próprias regras personalizadas.  
  
Para obter mais informações sobre regras de declaração e modelos de regra de declaração, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Entendendo os componentes da linguagem de regras de declaração  
A linguagem de regra de declaração consiste dos seguintes componentes, separados pela "= >" operador:  
  
-   Uma condição  
  
-   Uma instrução de emissão  
  
### <a name="conditions"></a>Condições  
Você pode usar as condições de uma regra para verificar as declarações de entrada e determinar se a instrução de emissão da regra deve ser executada. Uma condição representa uma expressão lógica que deve ser avaliada como true para executar a parte do corpo da regra. Se essa parte estiver ausente, será considerada uma condição lógica true; ou seja, o corpo da regra sempre será executado. A parte de condições contém uma lista de condições são combinadas juntamente com o operador lógico de conjunção ("& &"). Todas as condições da lista devem ser avaliadas como true para que toda a parte condicional seja avaliada como true. A condição pode ser um operador de seleção de declarações ou uma chamada de função de agregação. Essas duas são mutuamente exclusivas, o que significa que os seletores de declaração e as funções de agregação não podem ser combinados em uma só parte de condições de regra.  
  
As condições são opcionais nas regras. Por exemplo, a seguinte regra não tem uma condição:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Há três tipos de condições:  
  
-   Condição única — é a forma mais simples de uma condição. As verificações são feitas para apenas uma expressão; Por exemplo, nome da conta do windows = domínio usuário.  
  
-   Várias condições — essa condição exige verificações adicionais para processar várias expressões no corpo da regra; Por exemplo, nome da conta do windows = domínio usuário e grupo = contosopurchasers.  
  
> [!NOTE]  
> Existe outra condição, mas ela é um subconjunto da condição única ou das várias condições. Ele é conhecido como uma condição de expressão regular (Regex). Essa condição é usada para obter uma expressão de entrada e correspondê-la a um determinado padrão. Um exemplo de como ela pode ser usada é exibido a seguir.  
  
Os exemplos a seguir mostram algumas das construções de sintaxe, que são baseadas nos tipos de condição e que podem ser usadas para criar regras personalizadas.  
  
#### <a name="single--condition-examples"></a>Única - exemplos de condição  
Single - condições de expressão são descritas na tabela a seguir. Elas são construídas para simplesmente verificar uma declaração com um tipo de declaração especificado ou para uma declaração com um tipo e um valor de declaração especificados.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição para verificar se há uma declaração de entrada com um tipo de declaração especificado ("http://test/name"). Se houver uma declaração correspondente nas declarações de entrada, a regra copia a(s) declaração(ões) correspondente no conjunto de declarações de saída.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Essa regra tem uma condição para verificar se há uma declaração de entrada com um tipo de declaração especificado ("http://test/name") e ("Terry") do valor de declaração. Se houver uma declaração correspondente nas declarações de entrada, a regra copia a(s) declaração(ões) correspondente no conjunto de declarações de saída.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Mais - condições complexas são mostradas na próxima seção, incluindo condições para verificar se há várias declarações, condições para verificar o emissor de uma declaração e as condições verificar se há valores que correspondem a um padrão de expressão regular.  
  
#### <a name="multiple--condition-examples"></a>Vários - exemplos de condição  
A tabela a seguir fornece um exemplo de várias - condições de expressão.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição de verificação de duas declarações, cada um com um tipo de declaração especificado de entrada ("http://test/name"e"http://test/email"). Se houver duas declarações correspondentes nas declarações de entrada, a regra copia a declaração de nome no conjunto de declarações de saída.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Regular - exemplos de condição  
A tabela a seguir fornece um exemplo de uma expressão regular,-com base em condição.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição que usa uma expressão regular para procurar um e-declaração de email terminando em "@fabrikam.com". Se houver uma declaração correspondente nas declarações de entrada, a regra copia a declaração correspondente no conjunto de declarações de saída.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Instruções de emissão  
Regras personalizadas são processadas com base nas instruções de emissão (*problema* ou *adicionar* ) que você programar para a regra de declaração. Dependendo do resultado desejado, a instrução de emissão ou a instrução de adição pode ser gravada na regra para preencher o conjunto de declarações de entrada ou o conjunto de declarações de saída. Uma regra personalizada que usa a instrução de adição explicitamente preenche os valores de declaração apenas para o conjunto de declarações de entrada, enquanto uma regra de declaração personalizada que usa a instrução de emissão preenche os valores de declarações no conjunto de declarações de entrada e no conjunto de declarações de saída. Isso pode ser útil quando um valor de declaração precisar ser usado somente por regras futuras no conjunto de regras de declaração.  
  
Por exemplo, na ilustração a seguir, a declaração de entrada é adicionada ao conjunto de declarações de entrada definido pelo mecanismo de emissão de declarações. Quando a primeira regra de declaração personalizada é executada e os critérios de usuário de domínio for atendida, o mecanismo de emissão de declarações processa a lógica da regra usando a instrução de adição e o valor de **Editor** é adicionado ao conjunto de declarações de entrada. Já que o valor de Editor está presente no conjunto de declarações de entrada, a regra 2 pode processar a instrução de emissão com sucesso em sua lógica e gerar um novo valor de **Olá**, que é adicionado ao conjunto de declarações de saída e ao conjunto de declarações de entrada definidos para uso pela próxima regra do conjunto de regras. Agora, a regra 3 pode usar todos os valores que estiverem presentes no conjunto de declarações de entrada definido como entrada para o processamento de sua lógica.  
  
![Funções do AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Ações de emissão de declarações  
O corpo da regra representa uma ação de emissão de declaração. Há duas ações de emissão de declaração que a linguagem reconhece:  
  
-   **Instrução de emissão:** A instrução de emissão cria uma declaração que vai para os conjuntos de declarações de entrada e de saída. Por exemplo, a instrução a seguir emite uma nova declaração com base em seu conjunto de declarações de entrada:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Instrução de adição:** A instrução de adição cria uma nova declaração que é adicionada apenas à coleção do conjunto de declarações de entrada. Por exemplo, a instrução a seguir adiciona uma nova declaração ao conjunto de declarações de entrada:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
A instrução de emissão de uma regra define quais declarações serão emitidas pela regra quando as condições forem atendidas. Há duas formas de instruções de emissão relacionadas aos argumentos e ao comportamento da instrução:  
  
-   **Normal** — as instruções de emissão normais podem emitir declarações usando valores literais na regra ou os valores de declarações que correspondem às condições. Uma instrução de emissão normal pode consistir em um ou ambos os seguintes formatos:  
  
    -   *Cópia de declaração*: A cópia de declaração cria uma cópia da declaração existente no conjunto de declarações de saída. Essa forma de emissão só faz sentido quando é combinada com a instrução "emissão". Quando ela é combinada com a instrução “adição”, não tem nenhum efeito.  
  
    -   *Nova declaração*: Esse formato cria uma nova declaração, considerando os valores para diversas propriedades de declaração. A propriedade Claim.Type deve ser especificada; todas as outras propriedades de declaração são opcionais. A ordem dos argumentos desse formato é ignorada.  
  
-   **Repositório de atributos**— este formato cria declarações com valores que são recuperados de um repositório de atributos. É possível criar vários tipos de declaração usando uma instrução de emissão único, que é importante para os repositórios de atributos que tornam as operações de e/s () de entrada/saída de rede ou disco durante a recuperação de atributo. Portanto, é recomendável limitar o número de viagens de ida e volta entre o mecanismo de políticas e o repositório de atributos. Também é permitido criar várias declarações para um determinado tipo de declaração. Quando o repositório de atributos retorna vários valores para um determinado tipo de declaração, a instrução de emissão cria automaticamente uma declaração para cada valor de declaração retornado. Uma implementação de repositório de atributos usa os argumentos de parâmetro para substituir os espaços reservados no argumento de consulta por valores que são fornecidas nos argumentos de parâmetro. Os espaços reservados usam a mesma sintaxe que a função () de String. Format do .NET (por exemplo, {1}, {2}e assim por diante). A ordem dos argumentos para esse formato de emissão é importante e deve ser a ordem indicada na gramática a seguir.  
  
A tabela a seguir descreve algumas construções comuns de sintaxe para ambos os tipos de instruções de emissão em regras de declaração.  
  
|Tipo de instrução de emissão|Descrição de instrução de emissão|Exemplo de sintaxe de instrução de emissão|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|A seguinte regra sempre emite a mesma declaração sempre que um usuário tem o tipo de declaração especificado e o valor:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|A regra a seguir converte um tipo de declaração em outro. Observe que o valor da declaração que corresponde à condição "c" é usado na instrução de emissão.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Repositório de atributos|A regra a seguir usa o valor de uma declaração de entrada para consultar o repositório de atributos do Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Repositório de atributos|A regra a seguir usa o valor de uma declaração de entrada para consultar um repositório de atributos SQL Structured Query Language () configurado anteriormente:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Expressões  
As expressões são usadas no lado direito para as restrições do seletor de declarações e os parâmetros de instruções de emissão. Há vários tipos de expressões às quais a linguagem dá suporte. Todas as expressões da linguagem são baseadas em cadeias de caracteres, o que significa que elas usam as cadeias de caracteres como entrada e produzem cadeias de caracteres. Não há suporte para números ou outros tipos de dados, como data/hora, em expressões. A seguir, há os tipos de expressões às quais a linguagem dá suporte:  
  
-   Cadeia de caracteres literal: Valor, delimitado pelo caractere de aspas (") em ambos os lados da cadeia de caracteres.  
  
-   Concatenação de expressões da cadeia de caracteres: O resultado é uma cadeia de caracteres que é produzida pela concatenação dos valores à esquerda e à direita.  
  
-   Chamada de função: A função é identificada por um identificador e os parâmetros são passados como uma vírgula – lista delimitada por ponto de expressões entre colchetes ("()").  
  
-   O acesso às propriedades da declaração na forma de um nome de propriedade DOT do nome da variável: O resultado do valor da propriedade da declaração identificada para um determinado valor de variável. Primeiro, a variável deve ser associada a um seletor de declarações antes que possa ser usada dessa maneira. Não é possível usar a variável que está associada a um seletor de declarações dentro das restrições para esse mesmo seletor de declarações.  
  
As propriedades de declaração a seguir estão disponíveis para acesso:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[propriedade\_nome\] (essa propriedade retorna uma cadeia de caracteres vazia se o nome de propriedade _ não pode ser encontrado na coleção de propriedades da declaração. ) simples  
  
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
  

