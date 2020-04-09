---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Linguagem de regras de transformação de declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f391c3f8ef2bb5b12f0dd15db55df4f861c05f9b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861269"
---
# <a name="claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O recurso de transformação de declarações entre florestas permite a ponte de declarações para controle de acesso dinâmico entre limites de floresta definindo políticas de transformação de declarações em relações de confiança entre florestas. O componente principal de todas as políticas são regras que são escritas em linguagem de regras de transformação de declarações. Este tópico fornece detalhes sobre essa linguagem e fornece orientação sobre a criação de regras de transformação de declarações.  
  
Os cmdlets do Windows PowerShell para políticas de transformação em relações de confiança entre florestas têm opções para definir políticas simples que são necessárias em cenários comuns. Esses cmdlets convertem a entrada do usuário em políticas e regras no idioma das regras de transformação de declarações e, em seguida, as armazenam em Active Directory no formato prescrito. Para obter mais informações sobre cmdlets para transformação de declarações, consulte os [cmdlets AD DS para o controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Dependendo da configuração de declarações e dos requisitos colocados na relação de confiança entre florestas em suas florestas de Active Directory, suas políticas de transformação de declarações podem precisar ser mais complexas do que as políticas com suporte dos cmdlets do Windows PowerShell para Active Directory. Para criar efetivamente essas políticas, é essencial compreender a semântica e a sintaxe da linguagem de regras de transformação de declarações. Essa linguagem de regras de transformação de declarações ("a linguagem") em Active Directory é um subconjunto do idioma usado pelo [serviços de Federação do Active Directory (AD FS)](https://go.microsoft.com/fwlink/?LinkId=243982) para fins semelhantes, e tem uma sintaxe e semântica muito semelhantes. No entanto, há menos operações permitidas e restrições de sintaxe adicionais são colocadas na versão Active Directory da linguagem.  
  
Este tópico explica brevemente a sintaxe e a semântica da linguagem de regras de transformação de declarações no Active Directory e considerações a serem feitas durante a criação de políticas. Ele fornece vários conjuntos de regras de exemplo para você começar, bem como exemplos de sintaxe incorreta e as mensagens que elas geram, para ajudá-lo a decifrar mensagens de erro ao criar as regras.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Ferramentas para criação de políticas de transformação de declarações  
**Cmdlets do Windows PowerShell para Active Directory**: essa é a maneira preferida e recomendada para criar e definir políticas de transformação de declarações. Esses cmdlets fornecem opções para políticas simples e verificam regras que são definidas para políticas mais complexas.  
  
**LDAP**: as políticas de transformação de declarações podem ser editadas em Active Directory por meio de LDAP (Lightweight Directory Access Protocol). No entanto, isso não é recomendado porque as políticas têm vários componentes complexos, e as ferramentas usadas podem não validar a política antes de gravá-la no Active Directory. Isso pode exigir uma quantidade considerável de tempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações Active Directory  
  
### <a name="syntax-overview"></a>Visão geral da sintaxe  
Aqui está uma breve visão geral da sintaxe e da semântica da linguagem:  
  
-   O conjunto de regras de transformação de declarações consiste em zero ou mais regras. Cada regra tem duas partes ativas: **Selecione a lista de condições** e a ação de **regra**. Se a **lista de condição de seleção** for avaliada como true, a ação de regra correspondente será executada.  
  
-   **Selecionar a lista de condições** tem zero ou mais **condições selecionadas**. Todas as **condições selecionadas** devem ser avaliadas como true para que a **lista de condição Select** seja avaliada como true.  
  
-   Cada **condição Select** tem um conjunto de zero ou mais **condições correspondentes**. Todas as **condições de correspondência** devem ser avaliadas como true para que a condição Select seja avaliada como true. Todas essas condições são avaliadas em uma única declaração. Uma declaração que corresponde a uma **condição Select** pode ser marcada por um **identificador** e referenciada na **ação de regra**.  
  
-   Cada **condição de correspondência** especifica a condição para corresponder ao **tipo** ou **valor** ou **ValueType** de uma declaração usando diferentes **operadores de condição** e **literais de cadeia de caracteres**.  
  
    -   Quando você especifica uma **condição de correspondência** para um **valor**, também deve especificar uma **condição de correspondência** para um **ValueType** específico e vice-versa. Essas condições devem ser próximas umas às outras na sintaxe.  
  
    -   As condições de correspondência **ValueType** devem usar somente literais **ValueType** específicos.  
  
-   Uma **ação de regra** pode copiar uma declaração que está marcada com um **identificador** ou emitir uma declaração com base em uma declaração que está marcada com um identificador e/ou literais de cadeia de caracteres especificados.  
  
**Regra de exemplo**  
  
Este exemplo mostra uma regra que pode ser usada para converter o tipo de declarações entre duas florestas, desde que elas usem os mesmos ValueTypes de declarações e tenham as mesmas interpretações para valores de declarações para esse tipo. A regra tem uma condição de correspondência e uma instrução Issue que usa literais de cadeia de caracteres e uma referência de declarações correspondente.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operação de tempo de execução  
É importante entender a operação de tempo de execução das transformações de declarações para criar as regras com eficiência. A operação de tempo de execução usa três conjuntos de declarações:  
  
1.  **Conjunto de declarações de entrada**: o conjunto de entradas de declarações que são dadas à operação de transformação de declarações.  
  
2.  **Conjunto de declarações de trabalho**: declarações intermediárias que são lidas e gravadas durante a transformação de declarações.  
  
3.  **Conjunto de declarações de saída**: saída da operação de transformação de declarações.  
  
Aqui está uma breve visão geral da operação de transformação de declarações de tempo de execução:  
  
1.  Declarações de entrada para transformação de declarações são usadas para inicializar o conjunto de declarações de trabalho.  
  
    1.  Ao processar cada regra, o conjunto de declarações de trabalho é usado para as declarações de entrada.  
  
    2.  A lista de condição de seleção em uma regra é correspondida em todos os conjuntos de declarações possíveis do conjunto de declarações de trabalho.  
  
    3.  Cada conjunto de declarações de correspondência é usado para executar a ação nessa regra.  
  
    4.  A execução de uma ação de regra resulta em uma declaração, que é anexada ao conjunto de declarações de saída e ao conjunto de declarações de trabalho. Assim, a saída de uma regra é usada como entrada para as regras subsequentes no conjunto de regras.  
  
2.  As regras no conjunto de regras são processadas em ordem sequencial, começando com a primeira regra.  
  
3.  Quando o conjunto de regras inteiro é processado, o conjunto de declarações de saída é processado para remover declarações duplicadas e outros problemas de segurança. As declarações resultantes são a saída do processo de transformação de declarações.  
  
É possível escrever transformações de declarações complexas com base no comportamento de tempo de execução anterior.  
  
**Exemplo: operação de tempo de execução**  
  
Este exemplo mostra a operação de tempo de execução de uma transformação de declarações que usa duas regras.  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>Semântica de regras especiais  
Veja a seguir uma sintaxe especial para regras:  
  
1.  Conjunto de regras vazio = = sem declarações de saída  
  
2.  Vazio Selecionar condição List = = cada declaração corresponde à lista de condições selecionadas  
  
    **Exemplo: selecionar lista de condição de seleção vazia**  
  
    A regra a seguir corresponde a cada declaração no conjunto de trabalho.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Vazio selecionar lista de correspondência = = cada declaração corresponde à lista de condições selecionadas  
  
    **Exemplo: condições de correspondência vazias**  
  
    A regra a seguir corresponde a cada declaração no conjunto de trabalho. Essa é a regra básica "permitir todos" se for usada sozinha.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerações de segurança  
**Declarações que entram em uma floresta**  
  
As declarações apresentadas por entidades de entrada para uma floresta precisam ser cuidadosamente inspecionadas para garantir que permitimos ou emitimos apenas as declarações corretas. As declarações inadequadas podem comprometer a segurança da floresta, e essa deve ser uma das principais considerações ao criar políticas de transformação para declarações que entram em uma floresta.  
  
Active Directory tem os seguintes recursos para impedir a configuração incorreta de declarações que entram em uma floresta:  
  
-   Se uma relação de confiança de floresta não tiver uma política de transformação de declarações definida para as declarações que entram em uma floresta, para fins de segurança, Active Directory descartará todas as declarações principais que entram na floresta.  
  
-   Se a execução do conjunto de regras em declarações que entra em uma floresta resultar em declarações que não estão definidas na floresta, as declarações indefinidas serão removidas das declarações de saída.  
  
**Declarações que deixam uma floresta**  
  
As declarações que deixam uma floresta apresentam uma preocupação de segurança menor para a floresta do que as declarações que entram na floresta. As declarações têm permissão para deixar a floresta como estão, mesmo quando não há nenhuma política de transformação de declarações correspondente em vigor. Também é possível emitir declarações que não são definidas na floresta como parte da transformação de declarações que deixam a floresta. Isso é para configurar facilmente relações de confiança entre florestas com declarações. Um administrador pode determinar se as declarações que entram na floresta precisam ser transformadas e configurar a política apropriada. Por exemplo, um administrador poderia definir uma política se houver a necessidade de ocultar uma declaração para evitar a divulgação de informações.  
  
**Erros de sintaxe em regras de transformação de declarações**  
  
Se uma determinada política de transformação de declarações tiver um conjunto de regras que está sintaticamente incorreto ou se houver outra sintaxe ou problemas de armazenamento, a política será considerada inválida. Isso é tratado de maneira diferente das condições padrão mencionadas anteriormente.  
  
Active Directory não é capaz de determinar a intenção nesse caso e entra em um modo à prova de falhas, em que nenhuma declaração de saída é gerada nessa relação de confiança + direção de passagem. A intervenção do administrador é necessária para corrigir o problema. Isso pode acontecer se o LDAP for usado para editar a política de transformação de declarações. Os cmdlets do Windows PowerShell para Active Directory têm validação in-loco para evitar a gravação de uma política com problemas de sintaxe.  
  
## <a name="other-language-considerations"></a>Outras considerações sobre linguagem  
  
1.  Há várias palavras-chave ou caracteres que são especiais nesse idioma (chamados de terminais). Eles são apresentados na tabela de [terminais de idioma](Claims-Transformation-Rules-Language.md#BKMK_LT) mais adiante neste tópico. As mensagens de erro usam as marcas para esses terminais para desambigüidade.  
  
2.  Às vezes, os terminais podem ser usados como literais de cadeia de caracteres. No entanto, esse uso pode entrar em conflito com a definição de linguagem ou ter consequências indesejadas. Esse tipo de uso não é recomendado.  
  
3.  A ação de regra não pode executar nenhuma conversões de tipo em valores de declaração e um conjunto de regras que contém essa ação de regra é considerado inválido. Isso causaria um erro de tempo de execução e nenhuma declaração de saída será produzida.  
  
4.  Se uma ação de regra se referir a um identificador que não foi usado na parte da lista de condições Select da regra, será um uso inválido. Isso causaria um erro de sintaxe.  
  
    **Exemplo: referência de identificador incorreta**  
    A regra a seguir ilustra um identificador incorreto usado na ação de regra.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Regras de transformação de exemplo  
  
-   **Permitir todas as declarações de um determinado tipo**  
  
    Tipo exato  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Usando Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Não permitir um determinado tipo de declaração**  
    Tipo exato  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Usando Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Exemplos de erros do analisador de regras  
As regras de transformação de declarações são analisadas por um analisador personalizado para verificar erros de sintaxe. Esse analisador é executado por cmdlets do Windows PowerShell relacionados antes de armazenar regras no Active Directory. Quaisquer erros na análise das regras, incluindo erros de sintaxe, são impressos no console do. Os controladores de domínio também executam o analisador antes de usar as regras para transformar declarações e registram erros no log de eventos (adicionar números de log de eventos).  
  
Esta seção ilustra alguns exemplos de regras que são escritas com sintaxe incorreta e os erros de sintaxe correspondentes gerados pelo analisador.  
  
1. Exemplo:  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   Este exemplo tem um ponto e vírgula usado incorretamente no lugar de dois-pontos.   
   **Mensagem de erro:**  
   *POLICY0002: não foi possível analisar os dados da política.*  
   *Número da linha: 1, número da coluna: 2, token de erro:;. Linha: ' C1; [] = > problema (declaração = C1); '.*  
   *Erro do analisador: ' POLICY0030: erro de sintaxe, '; ' inesperado, esperando um dos seguintes: ': '. '*  
  
2. Exemplo:  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   Neste exemplo, a marca de identificador na instrução de emissão de cópia é indefinida.   
   **Mensagem de erro**:   
   *POLICY0011: nenhuma condição na regra de declaração corresponde à marca de condição especificada em CopyIssuanceStatement: ' C2 '.*  
  
3. Exemplo:  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" não é um terminal no idioma e não é um ValueType válido. Os terminais válidos são listados na seguinte mensagem de erro.   
   **Mensagem de erro:**  
   *POLICY0002: não foi possível analisar os dados da política.*  
   Número da linha: 1, número da coluna: 39, token de erro: "bool". Linha: ' C1: [tipo = = "x1", valor = = "1", ValueType = = "bool"] = > problema (declaração = C1); '.   
   *Erro do analisador: ' POLICY0030: erro de sintaxe, ' STRING ' inesperado, esperando um dos seguintes: ' INT64_TYPE ' ' UINT64_TYPE ' ' STRING_TYPE ' ' BOOLEAN_TYPE ' ' IDENTIFIER '*  
  
4. Exemplo:  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   O número **1** neste exemplo não é um token válido no idioma e esse uso não é permitido em uma condição de correspondência. Ele precisa ser colocado entre aspas duplas para torná-la uma cadeia de caracteres.   
   **Mensagem de erro:**  
   *POLICY0002: não foi possível analisar os dados da política.*  
   *Número da linha: 1, número da coluna: 23, token de erro: 1. linha: ' C1: [tipo = = "x1", valor = = 1, ValueType = = "bool"] = > problema (declaração = C1); '.* <em>Erro do analisador: ' POLICY0029: entrada inesperada.</em>  
  
5. Exemplo:  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   Este exemplo usou um sinal de igualdade duplo (= =) em vez de um único sinal de igual (=).   
   **Mensagem de erro:**  
   *POLICY0002: não foi possível analisar os dados da política.*  
   *Número da linha: 1, número da coluna: 91, token de erro: = =. Linha: ' C1: [type = = "x1", valor = = "1",*  
   *ValueType = = "booliano"] = > problema (tipo = C1. Type, valor = "0", ValueType = = "booliano"); ".*  
   *Erro do analisador: ' POLICY0030: erro de sintaxe, ' = = ' inesperado, esperando um dos seguintes: ' = '*  
  
6. Exemplo:  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   Este exemplo é sintaticamente e semanticamente correto. No entanto, o uso de "booliano" como um valor de cadeia de caracteres é limitado para causar confusão e deve ser evitado. Como mencionado anteriormente, o uso de terminais de idioma como valores de declarações deve ser evitado sempre que possível.  
  
## <a name="language-terminals"></a><a name="BKMK_LT"></a>Terminais de idioma  
A tabela a seguir lista o conjunto completo de cadeias de caracteres de terminal e os terminais de idioma associados que são usados no idioma das regras de transformação de declarações. Essas definições usam cadeias de caracteres UTF-16 que não diferenciam maiúsculas de minúsculas.  
  
|String|Terminal|  
|----------|------------|  
|"= >"|DIZ|  
|";"|PONTO E vírgula|  
|":"|PONTOS|  
|","|PONTOS|  
|"."|FINAL|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|Cancele|  
|"& &"|AND|  
|lo|PROBLEMA|  
|Escreva|TYPE|  
|valor|VALOR|  
|ValueType|VALUE_TYPE|  
|enção|ENÇÃO|  
|"[_A-Za-z] [_A-Za-z0-9] *"|ID|  
|"\\" [^\\"\n] *\\" "|Strings|  
|UInt64|UINT64_TYPE|  
|Int64|INT64_TYPE|  
|Strings|STRING_TYPE|  
|Boolean|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxe da linguagem  
O idioma das regras de transformação de declarações a seguir é especificado no formulário ABNF. Essa definição usa os terminais especificados na tabela anterior, além das produções ABNF definidas aqui. As regras devem ser codificadas em UTF-16 e as comparações de cadeia de caracteres devem ser tratadas como não diferenciam maiúsculas de minúsculas.  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


