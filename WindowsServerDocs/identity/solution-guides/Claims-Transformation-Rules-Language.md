---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: Linguagem de regras de transformação de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856207"
---
# <a name="claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A entre as florestas declarações transformação recurso permite que a ponte de declarações para o controle de acesso dinâmico entre limites de florestas, definindo políticas de transformação de declarações em relações de confiança entre as florestas. O componente principal de todas as políticas é regras que são escritas na linguagem de regras de transformação de declarações. Este tópico fornece detalhes sobre essa linguagem e fornece orientação sobre a criação de regras de transformação de declarações.  
  
Os cmdlets do Windows PowerShell para políticas de transformação em entre as florestas confia tem opções para definir diretivas simples que são necessárias em comum cenários. Esses cmdlets traduzir a entrada do usuário em políticas e regras na linguagem de regras de transformação de declarações e, em seguida, armazená-las no Active Directory no formato prescrito. Para obter mais informações sobre os cmdlets para transformação de declarações, consulte a [Cmdlets do AD DS para controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Dependendo da configuração de declarações e os requisitos estabelecidos para a relação de confiança entre as florestas em suas florestas do Active Directory, suas políticas de transformação de declarações talvez precise ser mais complexo do que as políticas de suporte para os cmdlets do Windows PowerShell para Active Directory Diretório. Para criar efetivamente dessas políticas, é essencial para entender a sintaxe de linguagem de regras de transformação de declarações e a semântica. Isso de declarações da linguagem de regras de transformação ("idioma") no Active Directory é um subconjunto da linguagem que é usado pelo [serviços de Federação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=243982) para semelhantes fins e ele tem uma sintaxe muito semelhante e a semântica. No entanto, há menos operações permitidas e restrições de sintaxe adicionais são colocadas na versão do idioma do Active Directory.  
  
Este tópico explica rapidamente a sintaxe e semântica da linguagem de regras de transformação de declarações no Active Directory e as considerações para ser feita durante a criação de políticas. Ele fornece vários conjuntos de regras de exemplo para você começar e exemplos de sintaxe incorreta e as mensagens que eles geram, para ajudá-lo a decifrar as mensagens de erro quando você criar regras.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Ferramentas para criação de políticas de transformação de declarações  
**Cmdlets do Windows PowerShell para Active Directory**: Essa é a maneira preferencial e recomendada para criar e definir políticas de transformação de declarações. Esses cmdlets fornecem opções para políticas simples e verificar as regras que são definidas para diretivas mais complexas.  
  
**LDAP**: Políticas de transformação de declarações podem ser editadas no Active Directory por meio de LDAP Lightweight Directory Access Protocol (). No entanto, isso não é recomendado porque as políticas têm vários componentes complexos, e as ferramentas que você usa não podem validar a política antes de gravar no Active Directory. Subsequentemente, isso pode exigir uma quantidade considerável de tempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações do Active Directory  
  
### <a name="syntax-overview"></a>Visão geral da sintaxe  
Aqui está uma breve visão geral da sintaxe e semântica da linguagem:  
  
-   O conjunto de regras de transformação de declarações consiste em zero ou mais regras. Cada regra tem duas partes de Active Directory: **Selecione a lista de condições** e **Rule Action**. Se o **selecione a lista de condições** for avaliada como TRUE, a ação de regra correspondente é executada.  
  
-   **Selecione a lista de condições** tem zero ou mais **selecionar condições**. Todos os **selecionar condições** deve ser avaliada como TRUE para o **selecione lista de condições** ser avaliada como TRUE.  
  
-   Cada **Selecionar condição** tem um conjunto de zero ou mais **condições de correspondência de**. Todos os **condições de correspondência de** deve ser avaliada como TRUE para selecionar condição ser avaliada como TRUE. Todas essas condições são avaliadas em relação a uma única declaração. Uma declaração que corresponde a um **Selecionar condição** podem ser marcados por um **identificador** e refere-se aos **ação de regra**.  
  
-   Cada **condição de correspondência** Especifica a condição para corresponder a **tipo** ou **valor** ou **ValueType** de uma declaração por meio de diferentes  **Operadores de condição** e **literais de cadeia de caracteres**.  
  
    -   Quando você especifica um **condição de correspondência** para um **valor**, você também deve especificar um **condição de correspondência** para um determinado **ValueType** e vice versa. Essas condições devem ser próximos uns dos outros na sintaxe.  
  
    -   **ValueType** condições de correspondência deve usar específicos **ValueType** apenas literais.  
  
-   Um **ação de regra** pode copiar uma declaração que é marcada com um **identificador** ou emitir uma declaração com base em uma declaração que é marcada com um identificador de e/ou dado literais de cadeia de caracteres.  
  
**Regra de exemplo**  
  
Este exemplo mostra uma regra que pode ser usada para converter as tipo de declarações entre duas florestas, desde que eles usam as mesmas declarações ValueTypes e tem as mesmas interpretações para valores de declarações para esse tipo. A regra tem uma condição de correspondência e uma instrução de problema que use literais de cadeia de caracteres e uma referência de declarações correspondente.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operação de tempo de execução  
É importante entender a operação de tempo de execução de transformações de declarações para criar regras com eficiência. A operação de tempo de execução usa três conjuntos de declarações:  
  
1.  **Conjunto de declarações de entrada**: O conjunto de entrada de declarações que são fornecidos para a operação de transformação de declarações.  
  
2.  **Conjunto de declarações de trabalho**: Declarações intermediárias que são lidos do e gravadas durante a transformação de declarações.  
  
3.  **Conjunto de declarações de saída**: Saída da operação de transformação de declarações.  
  
Aqui está uma visão geral sobre a operação de transformação de declarações de tempo de execução:  
  
1.  Declarações de entrada para transformação de declarações são usadas para inicializar o conjunto de declarações de trabalho.  
  
    1.  Durante o processamento de cada regra, o conjunto de declarações de trabalho é usado para declarações de entrada.  
  
    2.  A lista de condições de seleção em uma regra é comparada com todos os possíveis conjuntos de declarações do conjunto de declarações de trabalho.  
  
    3.  Cada conjunto de declarações de correspondência é usado para executar a ação na regra.  
  
    4.  Executando uma regra de resultados de ação em uma declaração, que é acrescentado à saída de declarações conjunto e o conjunto de declarações de trabalho. Assim, a saída de uma regra é usada como entrada para regras subsequentes no conjunto de regras.  
  
2.  As regras no conjunto de regras são processadas em ordem sequencial, começando com a primeira regra.  
  
3.  Quando o conjunto de regras inteiro é processado, o conjunto de declarações de saída é processado para remover declarações duplicadas e para outros problemas de segurança. As declarações resultantes são a saída do processo de transformação de declarações.  
  
É possível escrever transformações de declarações complexas com base no comportamento de tempo de execução anterior.  
  
**Exemplo: Operação de tempo de execução**  
  
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
Veja a seguir a sintaxe especial para regras:  
  
1.  Esvaziar o conjunto de regras de = = nenhuma declaração de saída  
  
2.  Selecione lista de condições de esvaziar = = correspondências de cada declaração Select condição List  
  
    **Exemplo: Selecione a condição vazia lista**  
  
    A regra a seguir corresponde a cada declaração no conjunto de trabalho.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Esvaziar selecione lista de correspondência de = = todas as correspondências de declaração lista Selecionar condição  
  
    **Exemplo: Condições de correspondência vazia**  
  
    A regra a seguir corresponde a cada declaração no conjunto de trabalho. Isso é a regra básica "Allow-all", se ele for usado sozinho.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerações sobre segurança  
**Declarações que inserir uma floresta**  
  
As declarações apresentadas por entidades de segurança de entrada para uma floresta precisará ser inspecionada cuidadosamente para garantir que podemos permitir ou emitir apenas as declarações corretas. Declarações inadequadas podem comprometer a segurança de floresta, e isso deve ser uma consideração importante ao criar políticas de transformação de declarações que inserir uma floresta.  
  
Active Directory tem os seguintes recursos para impedir que uma configuração incorreta de declarações que inserir uma floresta:  
  
-   Se uma relação de confiança de floresta não tem nenhuma política de transformação de declarações definidas para as declarações que insira uma floresta, para fins de segurança do Active Directory descarta todas as declarações UPN que insere a floresta.  
  
-   Se executar a regra definida em declarações que insere os resultados de uma floresta em declarações que não estão definidas na floresta, as declarações indefinidas são descartadas de declarações de saída.  
  
**Declarações que deixam uma floresta**  
  
Declarações que deixam uma floresta apresentam uma preocupação de segurança menor para a floresta que as declarações que insira a floresta. Declarações são permitidas para deixar a floresta como-está mesmo quando não há nenhuma declaração correspondente a política de transformação em vigor. Também é possível emitir declarações que não estão definidas na floresta como parte de transformação de declarações que deixam a floresta. Isso é facilmente configurar relações de confiança entre as florestas com declarações. Um administrador pode determinar se as declarações que insere a floresta precisam ser transformados e configurar a política apropriada. Por exemplo, um administrador pode definir uma política, se houver uma necessidade para ocultar uma declaração para evitar a divulgação de informações.  
  
**Erros de sintaxe nas regras de transformação de declarações**  
  
Se uma política de transformação de declarações fornecido tem um conjunto de regras está sintaticamente incorreto ou se há outros problemas de sintaxe ou de armazenamento, a política é considerada inválida. Isso é tratado de forma diferente do que as condições padrão mencionadas anteriormente.  
  
Active Directory não pode determinar a intenção nesse caso e entra em um modo à prova de falhas, onde nenhuma declaração de saída é geradas nessa relação de confiança + a direção da passagem. Intervenção do administrador é necessária para corrigir o problema. Isso pode acontecer se o LDAP é usado para editar a política de transformação de declarações. Cmdlets do Windows PowerShell para Active Directory tem validação em vigor para impedir que escrever uma política com problemas de sintaxe.  
  
## <a name="other-language-considerations"></a>Outras considerações sobre idioma  
  
1.  Há várias palavras-chave ou caracteres que são especiais neste idioma (conhecido como terminais). Eles são apresentados na [terminais de linguagem](Claims-Transformation-Rules-Language.md#BKMK_LT) tabela mais adiante neste tópico. As mensagens de erro usam as marcas para os terminais de Desambiguidade.  
  
2.  Terminais, às vezes, podem ser usados como literais de cadeia de caracteres. No entanto, esse uso pode entrar em conflito com a definição de linguagem ou ter consequências não intencionais. Esse tipo de uso não é recomendado.  
  
3.  A ação de regra não pode executar as conversões de tipo em valores de declaração e um conjunto de regras que contém tal uma ação de regra é considerado inválido. Isso causaria um erro de tempo de execução e nenhuma declaração de saída é produzidas.  
  
4.  Se uma ação de regra refere-se a um identificador que não foi usado na parte Select List de condição da regra, é um uso inválido. Isso causaria um erro de sintaxe.  
  
    **Exemplo: Referência incorreta do identificador**  
    A regra a seguir ilustra um identificador incorreto usado na ação de regra.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Exemplos de regras de transformação  
  
-   **Permitir todas as declarações de um determinado tipo**  
  
    Tipo exato  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **Não permitir a um determinado tipo de declaração**  
    Tipo exato  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    Usar Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>Exemplos de erros do analisador de regras  
As regras de transformação de declarações são analisadas por um analisador personalizado para verificar se há erros de sintaxe. Esse analisador é executado pelos cmdlets do Windows PowerShell relacionados antes de armazenar as regras no Active Directory. Quaisquer erros ao analisar as regras, incluindo erros de sintaxe, são impressas no console. Controladores de domínio também executam o analisador antes de usar as regras para transformar declarações, e eles registrar erros no log de eventos (Adicionar números de log de eventos).  
  
Esta seção ilustra alguns exemplos de regras que são escritos com sintaxe incorreta e a sintaxe correspondente erros gerados pelo analisador.  
  
1.  Exemplo:  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Este exemplo tem uma ponto e vírgula incorretamente usada no lugar de dois-pontos.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar os dados de política.*  
    *Número da linha: 1, número de coluna: Erro token 2,:;. Linha: ' c1; [] = > Issue(claim=c1);'.*  
    *Erro do analisador: 'POLICY0030: Erro de sintaxe inesperado ';', esperando um dos seguintes: ':'.'*  
  
2.  Exemplo:  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    Neste exemplo, a marca de identificador na instrução de emissão de cópia é indefinida.   
    **Mensagem de erro**:   
    *POLICY0011: Nenhuma das condições na regra de declaração corresponde à marca de condição especificada no CopyIssuanceStatement: 'c2'.*  
  
3.  Exemplo:  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    "bool" não é um Terminal no idioma e não é um ValueType válido. Terminais válidos são listados na seguinte mensagem de erro.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar os dados de política.*  
    Número da linha: 1, número de coluna: 39, token de erro: "bool". Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
    *Erro do analisador: 'POLICY0030: Erro de sintaxe, inesperado 'STRING', esperando um dos seguintes: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4.  Exemplo:  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    O numeral **1** neste exemplo não é um token válido no idioma, e tal uso não é permitido em uma condição de correspondência. Ele deve ser colocado entre aspas duplas para torná-lo em uma cadeia de caracteres.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar os dados de política.*  
    *Número da linha: 1, número de coluna: 23, token de erro: 1. Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.**Parser error: 'POLICY0029: Entrada inesperada.*  
  
5.  Exemplo:  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    Este exemplo usou um sinal de igualdade duplo (= =), em vez de um único sinal de igual (=).   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar os dados de política.*  
    *Número da linha: 1, número de coluna: 91, token de erro: = =. Line: 'c1:[type=="x1", value=="1",*  
    *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
    *Erro do analisador: 'POLICY0030: Erro de sintaxe, inesperado '= =', esperando um dos seguintes: '='*  
  
6.  Exemplo:  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Este exemplo é sintaticamente e semanticamente correto. No entanto, usando "boolean" como um valor de cadeia de caracteres é associado a causar confusão, e ele deve ser evitado. Como mencionado anteriormente, usando os terminais de linguagem como valores de declarações devem ser evitados sempre que possível.  
  
## <a name="BKMK_LT"></a>Terminais de idioma  
A tabela a seguir lista o conjunto completo de cadeias de caracteres terminal e os terminais de idiomas associados que são usados na linguagem de regras de transformação de declarações. Essas definições usem cadeias de caracteres UTF-16 diferencia maiusculas de minúsculas.  
  
|String|Terminal|  
|----------|------------|  
|"=>"|IMPLICA|  
|";"|PONTO E VÍRGULA|  
|":"|DOIS-PONTOS|  
|","|COMMA|  
|"."|PONTO|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|ATRIBUIR|  
|"&&"|AND|  
|"problema"|PROBLEMA|  
|"tipo"|TYPE|  
|"valor"|VALOR|  
|"valuetype"|VALUE_TYPE|  
|"declaração"|DECLARAÇÃO|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICADOR|  
|"\\"[^\\"\n]*\\""|CADEIA DE CARACTERES|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxe de linguagem  
A seguinte linguagem de regras de transformação de declarações é especificada no formulário ABNF. Essa definição usa os terminais que são especificados na tabela anterior, além de produções ABNF definidas aqui. As regras devem ser codificadas em UTF-16, e as comparações de cadeia de caracteres devem ser tratadas como diferencia maiusculas de minúsculas.  
  
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
  


