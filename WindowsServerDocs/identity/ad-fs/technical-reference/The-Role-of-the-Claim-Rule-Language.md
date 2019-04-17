---
title: "A função da linguagem de regra de declaração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 206da33d33cbded0450db29615dc74eb1161cbce
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>A função da linguagem de regra de declaração
Os serviços de Federação Active Directory (AD FS) reivindicar idioma regra age como o bloco de construção administrativo para o comportamento das declarações de entrada e saídas, enquanto o mecanismo de requerimentos judiciais ou Extrajudiciais atua como o mecanismo de processamento para a lógica no idioma reivindicação regra que define a regra personalizada. Para obter mais informações sobre como todas as regras são processadas pelo mecanismo de declarações, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>Criar regras de declaração personalizada usando a linguagem de regra de declaração  
AD FS fornece aos administradores com a opção para definir regras personalizadas que eles podem usar para determinar o comportamento de declarações de identidade com o idioma de regra de declaração. Você pode usar os exemplos de sintaxe de idioma de regra reivindicação neste tópico para criar uma regra personalizada que enumera, adiciona, exclui e modifica declarações para atender às necessidades da sua organização. Você pode criar regras personalizadas digitando a sintaxe da linguagem reivindicação regra no **enviar requerimentos judiciais ou Extrajudiciais usando uma declaração personalizada** modelo de regra.  
  
As regras são separadas umas das outras com ponto e vírgula.  
  
Para saber mais sobre quando usar regras personalizadas, veja [quando usar uma regra personalizada da reivindicação](When-to-Use-a-Custom-Claim-Rule.md).  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>Usando modelos de regra de declaração para saber mais sobre a sintaxe da linguagem reivindicação regra  
AD FS também fornece um conjunto de emissão de declaração predefinida e reivindicação modelos de regra de aceitação que você pode usar para implementar comuns reivindicar regras. No **editar regras reivindicar** caixa de diálogo para uma relação de confiança determinada, você pode criar uma regra predefinida — e ver a sintaxe da linguagem regra reivindicação que compõe a essa regra — clicando o **idioma de regra de modo de exibição** guia para essa regra. Usando as informações nesta seção e a **idioma de regra de modo de exibição** técnica pode fornecer informações sobre como construir regras personalizadas.  
  
Para obter mais informações sobre as regras de declaração e modelos de regra de declaração, veja [a função de reivindicar regras](The-Role-of-Claim-Rules.md).  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>Noções básicas sobre os componentes da linguagem de regra de declaração  
O idioma de regra de declaração consiste nos seguintes componentes, separados o "= >" operador:  
  
-   Uma condição  
  
-   Uma instrução de emissão  
  
### <a name="conditions"></a>Condições  
Você pode usar condições em uma regra para verificar declarações de entrada e determinar se a instrução de emissão da regra deve ser executada. Uma condição representa uma expressão lógica que deve ser avaliada como true para executar a parte do corpo de regra. Se estiver falta, essa parte é considerada uma verdadeira lógica; ou seja, o corpo da regra sempre é executado. A parte de condições contém uma lista de condições que são combinados com o operador lógico do conjunto ("& &"). Todas as condições na lista devem ser avaliadas como true para a parte totalmente condicional a ser avaliado como true. A condição pode ser um operador de seleção requerimentos judiciais ou Extrajudiciais ou uma chamada de função agregada. Esses dois são mutuamente exclusivos, o que significa que reivindicam seletores e funções agregadas não podem ser combinadas em uma parte de condições de regra única.  
  
Condições são opcionais nas regras. Por exemplo, a seguinte regra não tem uma condição:  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
Há três tipos de condições:  
  
-   Única condição — isso é a forma mais simples de uma condição. As verificações são feitas para apenas uma expressão; Por exemplo, nome da conta do windows = usuário de domínio.  
  
-   Vários condição — essa condição requer verificações adicionais para processar vários expressões no corpo da regra; Por exemplo, nome da conta do windows = usuário de domínio e group = contosopurchasers.  
  
> [!NOTE]  
> Outra condição existe, mas é um subconjunto de única condição ou a condição de vários. Ele é conhecido como uma condição de expressão regular (Regex). Ele é usado para tirar uma expressão de entrada e corresponder à expressão com um determinado padrão. Um exemplo de como ele é pode ser usado é mostrado abaixo.  
  
Os exemplos a seguir mostram algumas das construções a sintaxe, que são baseadas nos tipos de condição, que você pode usar para criar regras personalizadas.  
  
#### <a name="single--condition-examples"></a>Único - condição exemplos  
Single - condições de expressão são descritas na tabela a seguir. Eles são construídos para valor de declaração e verificar simplesmente para um pedido com um tipo de declaração especificado ou para um pedido com um tipo de declaração especificado.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição para verificar se há uma declaração de entrada com um tipo de declaração especificado ("http://test/name"). Se for uma reivindicação correspondente nas declarações entradas, a regra copia os reivindicação correspondente ou declarações para o conjunto de declarações de saída.|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|Essa regra tem uma condição para verificar se há uma declaração de entrada com um tipo de declaração especificado ("http://test/name") e reivindicar valor ("Terry"). Se for uma reivindicação correspondente nas declarações entradas, a regra copia os reivindicação correspondente ou declarações para o conjunto de declarações de saída.|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
Mais - condições complexas são mostradas na próxima seção, incluindo as condições para verificar se há várias declarações condições para verificar o emissor de uma declaração e condições verificar se há valores que correspondem a um padrão de expressão regular.  
  
#### <a name="multiple--condition-examples"></a>Exemplos de condição vários-  
A tabela a seguir fornece um exemplo de várias - condições de expressão.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição para verificar se há duas declarações de entrada, cada um com um tipo de declaração especificado ("http://test/name" e "http://test/email"). Se as duas declarações correspondentes são nas declarações entradas, a regra copia a declaração de nome para o conjunto de declarações de saída.|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>Normal - condição exemplos  
A tabela a seguir fornece um exemplo de uma expressão regular,-com base em condição.  
  
|Descrição da condição|Exemplo de sintaxe da condição|  
|-------------------------|----------------------------|  
|Essa regra tem uma condição que usa uma expressão regular para procurar um e-declaração de email termina em "@fabrikam.com". Se uma reivindicação correspondência for encontrada nas declarações entradas, a regra copia a declaração correspondente para o conjunto de declarações de saída.|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>Instruções de emissão  
Regras personalizadas são processadas com base em políticas de emissão (*problema* ou *adicionar* ) que você programa para a regra de declaração. Dependendo do resultado desejado, a declaração de problema ou adicionar declaração pode ser escrita para a regra para preencher o conjunto de declarações de entrada ou saída do conjunto de declarações. Uma regra personalizada que usa a declaração de adicionar explicitamente preenche reivindicação valores somente para a entrada do conjunto de declarações enquanto uma regra de declaração personalizada que usa a declaração de problema preenche reivindicação valores em ambas as a entrada do conjunto de declarações e na saída do conjunto de declarações. Isso pode ser útil quando um valor de declaração se destina a ser usado somente por regras futuras no conjunto de regras de declaração.  
  
Por exemplo, na ilustração a seguir, a declaração de entrada é adicionada para a declaração de entrada definida pelo mecanismo de emissão de declarações. Quando a primeira regra declaração personalizada é executado e os critérios de usuário de domínio estiver satisfeito, o mecanismo de emissão requerimentos judiciais ou Extrajudiciais processa a lógica na regra usando a declaração de adicionar e o valor de **Editor** é adicionada ao conjunto de solicitação de entrada. Porque o valor do Editor está presente no conjunto de solicitação de entrada, regra 2 podem processar a declaração do problema na sua lógica com êxito e gerar um novo valor de **Hello**, que é adicionado para ambas as a saída do conjunto de declarações e para a declaração de entrada definido para ser usado pela seguinte regra na regra definido. Regra 3 agora pode usar todos os valores que estão presentes na declaração de entrada definido como entrada para processar sua lógica.  
  
![Funções do AD FS](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>Ações de emissão de declaração  
O corpo da regra representa uma ação de emissão de declaração. Há duas ações de emissão de declaração que reconhece o idioma:  
  
-   **Emita instrução:** a declaração de problema cria uma reivindicação conjuntos de declarações vai para entrada e saída. Por exemplo, a instrução a seguir emite uma nova declaração com base em seu conjunto de solicitação de entrada:  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **Adicione a instrução:** a instrução de adicionar cria uma nova declaração é adicionada somente à coleção de conjunto de solicitação de entrada. Por exemplo, a instrução a seguir adiciona uma nova declaração para o conjunto de declarações de entrada:  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
A declaração de emissão de uma regra define que requerimentos judiciais ou Extrajudiciais será emitido pela regra quando as condições são atendidas. Há duas formas de declarações de emissão sobre o comportamento de declaração e argumentos:  
  
-   **Normal**— instruções de emissão Normal podem emitir requerimentos judiciais ou Extrajudiciais usando valores literais na regra ou os valores de declarações que correspondam às condições. Uma instrução de emissão normal pode consistir em um ou ambos dos seguintes formatos:  
  
    -   *Reivindicar cópia*: A cópia de declaração cria uma cópia da reivindicação existente no conjunto de declaração de saída. Essa forma de emissão só faz sentido quando for combinado com a declaração de emissão "problema". Quando for combinado com a declaração de emissão "Adicionar", ele não tem qualquer efeito.  
  
    -   *Nova declaração*: isso Formatar reates uma nova declaração, fornecida os valores para várias propriedades de declaração. Claim.Type deve ser especificado; todas as outras propriedades de declaração são opcionais. A ordem dos argumentos para esse formulário é ignorada.  
  
-   **Atributo Store**— este formulário cria requerimentos judiciais ou Extrajudiciais com valores que são recuperadas de um repositório de atributo. É possível criar vários tipos de declaração usando uma instrução de emissão único, que é importante para as lojas de atributo que tornam as operações de (e/s) de entrada/saída de rede ou disco durante a recuperação de atributo. Portanto, é desejável para limitar o número de idas e vindas entre o mecanismo de política e o armazenamento de atributo. Também é permitido para criar várias declarações para um tipo de declaração determinado. Quando o repositório de atributo retorna vários valores de um tipo de declaração determinado, a política de emissão cria automaticamente uma declaração para cada valor retornado reivindicação. Uma implementação de repositório do atributo usa os argumentos param para substituir os espaços reservados no argumento de consulta com valores que são fornecidos nos argumentos de param. Os espaços reservados usam a mesma sintaxe da função () do .NET Format (por exemplo, {1}, {2} e assim por diante). A ordem dos argumentos para essa forma de emissão é importante e deve ser a ordem que é prescrita na gramática a seguir.  
  
A tabela a seguir descreve algumas construções de sintaxe comuns para ambos os tipos de instruções de emissão nas regras de declaração.  
  
|Tipo de política de emissão|Descrição da política de emissão|Exemplo de sintaxe da instrução de emissão|  
|---------------------------|----------------------------------|-------------------------------------|  
|Normal|A seguinte regra sempre emite a mesma declaração sempre que um usuário tem o valor e o tipo de declaração especificado:|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|Normal|A seguinte regra converte um tipo de declaração em outro. Observe que o valor da reivindicação que corresponde a condição "c" é usado na política de emissão.|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|Loja de atributo|A seguinte regra usa o valor de uma declaração de entrada para consultar o armazenamento de atributo do Active Directory:|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|Loja de atributo|A seguinte regra usa o valor de uma declaração de entrada para consultar um repositório de atributo de linguagem de consulta estruturada (SQL) predefinidas:|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>Expressões  
Expressões são usadas no lado direito para restrições de seletor de declarações e parâmetros de política de emissão. Existem vários tipos de expressões que dá suporte o idiomas. Todas as expressões no idioma são baseados em cadeia de caracteres, que significa que elas obtêm as cadeias de caracteres como entrada e produzem cadeias de caracteres. Números ou outros tipos de dados, como data/hora, em expressões não são permitidos. Estes são os tipos de expressões que dá suporte o idiomas:  
  
-   Cadeia de caracteres literal: cadeia de caracteres de valor, delimitada pelo caractere de aspas (") em ambos os lados.  
  
-   Concatenação de expressões de cadeia de caracteres: O resultado é uma cadeia de caracteres que é produzida pelo concatenação dos valores esquerdas e direita.  
  
-   Chamada de função: A função é identificada por um identificador, e os parâmetros são passados como uma vírgula-lista delimitada de expressões entre colchetes ("()").  
  
-   Acesso de propriedade da declaração na forma de um nome de propriedade do ponto de nome de variável: O resultado do valor da propriedade da declaração identificado para uma determinado avaliação variável. A variável primeiro deve ser vinculada a um seletor de requerimentos judiciais ou Extrajudiciais antes que ele pode ser usado dessa forma. Não é possível usar a variável que está vinculada a um seletor de requerimentos judiciais ou Extrajudiciais dentro as restrições para esse mesmo seletor requerimentos judiciais ou Extrajudiciais.  
  
As seguintes propriedades de declaração estão disponíveis para acesso:  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[property\_name\] (essa propriedade retorna uma cadeia de caracteres vazia se a propriedade _name não pode ser encontrado na coleção de propriedades da declaração. )  
  
Você pode usar a função RegexReplace para chamar dentro de uma expressão. Esta função usa uma expressão de entrada e combina-os com o padrão de determinado. Se o padrão de corresponder, a saída da correspondência é substituída com o valor de substituição.  
  
#### <a name="exists-functions"></a>Existe funções  
A função Exists pode ser usada em uma condição para avaliar se uma reivindicação correspondências de que existe na entrada de uma condição conjunto de declarações. Se existir qualquer reivindicação correspondente, a política de emissão é chamada apenas uma vez. No exemplo a seguir, a declaração de "origin" é emitida exatamente uma vez — se houver pelo menos uma declaração da coleção de conjunto de solicitação de entrada que tenha o emissor definido como "MSFT", independentemente de quantas requerimentos judiciais ou Extrajudiciais têm o emissor definido para "MSFT". Usar essa função impede que as declarações duplicadas de emissão.  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>Corpo da regra  
O corpo da regra pode conter somente uma instrução de emissão único. Se as condições forem usadas sem usar a função Exists, o corpo de regra é executado uma vez para cada vez que corresponde a parte de condições.  
  
## <a name="additional-references"></a>Referências adicionais  
[Criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada](https://technet.microsoft.com/library/dd807049.aspx)  
  

