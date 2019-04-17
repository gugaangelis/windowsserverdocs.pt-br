---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: "Linguagem de regras de transformação de declarações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Entre-floresta declarações permite de recurso de transformação a ponte diz para o controle de acesso dinâmico limites da floresta definindo requerimentos judiciais ou Extrajudiciais políticas de transformação em relações de confiança através de floresta. O componente principal de todas as políticas é regras que são escritas em linguagem de regras de transformação de declarações. Este tópico fornece detalhes sobre essa linguagem e fornece orientação sobre a criação de regras de transformação de declarações.  
  
Os cmdlets do Windows PowerShell para políticas de transformação em floresta confia tem opções para definir políticas simples que são necessários em comum cenários. Esses cmdlets traduzir a entrada do usuário em políticas e regras na linguagem de regras de transformação de declarações e armazená-los no Active Directory no formato prescrito. Para saber mais sobre cmdlets de transformação de declarações, consulte o [Cmdlets do AD DS para o controle de acesso dinâmico](https://go.microsoft.com/fwlink/?LinkId=243150).  
  
Dependendo da configuração de declarações e os requisitos colocados na relação de confiança entre florestas em seu florestas do Active Directory, as políticas de transformação requerimentos judiciais ou Extrajudiciais talvez precise ser mais complexo do que as políticas compatíveis com os cmdlets do Windows PowerShell para o Active Directory. Para criar efetivamente referidas políticas, é essencial para compreender a sintaxe de linguagem de regras de transformação de declarações e a semântica. Isso diz linguagem de regras de transformação ("idioma") no Active Directory é um subconjunto do idioma que é usado pelo [serviços de Federação do Active Directory](https://go.microsoft.com/fwlink/?LinkId=243982) para fins de semelhantes e ele tem uma sintaxe e a semântica muito semelhante. No entanto, há menos operações permitidas e restrições de sintaxe adicionais são colocadas na versão do Active Directory do idioma.  
  
Este tópico explica rapidamente a sintaxe e a semântica da linguagem de regras de transformação de declarações no Active Directory e as considerações a serem tomadas ao criar políticas. Ele fornece vários conjuntos de regras de exemplo para começar e os exemplos de sintaxe incorreta e as mensagens que eles geram, para ajudá-lo a decifrá mensagens de erro quando você criar as regras.  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>Ferramentas para a criação de políticas de transformação de declarações  
**Cmdlets do Windows PowerShell para o Active Directory**: essa é a maneira preferida e recomendada para criar e definir políticas de transformação de declarações. Esses cmdlets fornecem opções para políticas simples e verifique se as regras que são definidas para políticas mais complexas.  
  
**LDAP**: políticas de transformação de declarações podem ser editadas no Active Directory por meio de LDAP Lightweight Directory Access Protocol (). No entanto, isso não é recomendável porque as políticas têm vários componentes complexos e as ferramentas que você usa não podem validar a política antes de gravá-la ao Active Directory. Subsequentemente, isso pode exigir uma quantidade considerável de tempo para diagnosticar problemas.  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Linguagem de regras de transformação de declarações do Active Directory  
  
### <a name="syntax-overview"></a>Visão geral da sintaxe  
Aqui está uma breve visão geral da sintaxe e a semântica do idioma:  
  
-   O conjunto de regras de transformação de declarações consiste em zero ou mais regras. Cada regra tem duas partes ativos: **selecione lista de condição** e **ação de regra**. Se o **selecione lista de condição** for avaliado como TRUE, a ação de regra correspondente é executada.  
  
-   **Selecione a condição lista** tem zero ou mais **selecione condições**. Todos os **selecione condições** deve avaliar como TRUE para o **selecione lista de condição** para avaliar como TRUE.  
  
-   Cada **selecione condição** tem um conjunto de zero ou mais **correspondência condições**. Todos os o **correspondência condições** deve avaliar como TRUE para que a condição selecione avaliar como TRUE. Todas essas condições são avaliadas em relação uma única declaração. Uma reivindicação que corresponde a um **selecione condição** pode ser marcado por um **identificador** e refere-se ao **ação de regra**.  
  
-   Cada **condição de correspondência** Especifica a condição de acordo com o **tipo** ou **valor** ou **ValueType** uma reivindicação usando diferentes **condição operadores** e **literais de cadeia de caracteres**.  
  
    -   Quando você especificar um **condição de correspondência** para um **valor**, você também deve especificar um **condição de correspondência** para um determinado **ValueType** e vice-versa. Essas condições devem ser próximos um do outro na sintaxe.  
  
    -   **ValueType** correspondência condições deve usar específico **ValueType** apenas literais.  
  
-   A **ação de regra** pode copiar uma declaração marcada com um **identificador** ou emitir uma declaração com base em uma declaração marcada com um identificador de e/ou dado literais de cadeia de caracteres.  
  
**Regra de exemplo**  
  
Este exemplo mostra uma regra que pode ser usada para traduzir as declarações de tipo entre duas florestas, desde que eles usam os mesmo requerimentos judiciais ou Extrajudiciais ValueTypes e tem as mesmo interpretações declarações valores para esse tipo. A regra tem uma condição de correspondência e uma instrução de problema que utiliza literais de cadeia de caracteres e uma referência de requerimentos judiciais ou Extrajudiciais correspondente.  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>Operação de tempo de execução  
É importante compreender a operação de tempo de execução de transformações de declarações para criar as regras de forma eficiente. A operação de tempo de execução usa três conjuntos de declarações:  
  
1.  **Conjunto de solicitações de entrada**: O conjunto de entrada de declarações que são fornecidos para a operação de transformação de declarações.  
  
2.  **Conjunto de declarações do trabalho**: intermediários declarações que são lidos da e gravadas durante a transformação de declarações.  
  
3.  **Conjunto de solicitações de saída**: resultado da operação de transformação de declarações.  
  
Aqui está uma breve visão geral da operação de transformação de declarações de tempo de execução:  
  
1.  Declarações de entrada para a transformação de declarações são usadas para inicializar o conjunto de declarações de trabalho.  
  
    1.  Ao processar cada regra, o conjunto de declarações de trabalho é usado para as declarações de entrada.  
  
    2.  A lista de condição de seleção em uma regra é comparada à todos os conjuntos de possíveis de declarações do conjunto de declarações de trabalho.  
  
    3.  Cada conjunto de declarações de correspondência é usado para executar a ação na regra.  
  
    4.  Executando uma regra de ação resulta em uma declaração, que é acrescentado à saída declarações do conjunto e o conjunto de declarações de trabalho. Assim, a saída de uma regra é usada como entrada para regras subsequentes no conjunto de regras.  
  
2.  As regras no conjunto de regras são processadas na ordem sequencial, começando com a primeira regra.  
  
3.  Quando o conjunto inteiro de regra é processado, o conjunto de declarações de saída é processado para remover declarações duplicadas e para outra segurança issues.The são a saída do processo de transformação de declarações.  
  
É possível escrever transformações de declarações complexos com base no comportamento de tempo de execução anterior.  
  
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
  
### <a name="special-rules-semantics"></a>Semântica regras especiais  
Eis uma sintaxe especial para as regras:  
  
1.  Esvaziar conjunto de regras = = nenhum declarações de saída  
  
2.  Esvaziar a lista de condição de selecionar = = reivindicação todas as correspondências a lista de condição selecione  
  
    **Exemplo: Lista de condição de selecionar vazio**  
  
    A seguinte regra corresponde a cada declaração no conjunto de trabalho.  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  Esvaziar a lista de correspondência selecionar = = todas as correspondências de declaração a lista de condição selecione  
  
    **Exemplo: Condições de correspondência vazio**  
  
    A seguinte regra corresponde a cada declaração no conjunto de trabalho. Essa é a regra básica "permitir a todos" se ele é usado somente.  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>Considerações de segurança  
**Declarações do que inserir uma floresta**  
  
As declarações apresentadas por entidades de entrada para uma floresta precisam ser inspecionado cuidadosamente para garantir que podemos permitir ou emitir apenas as reivindicações corretas. Requerimentos judiciais ou Extrajudiciais indevidos podem comprometer a segurança de floresta e deve ser uma consideração superior ao criar políticas de transformação para declarações que inserir uma floresta.  
  
Active Directory possui os seguintes recursos para evitar a configuração incorreta de declarações que inserir uma floresta:  
  
-   Se uma relação de confiança de floresta não tem nenhuma política de transformação de declarações definida para que as declarações que inserir uma floresta, para fins de segurança, o Active Directory cai todas as declarações de entidade de segurança que inserir a floresta.  
  
-   Se executando o conjunto nas declarações de regras que insere um resultados de floresta em declarações que não são definidas na floresta, as declarações indefinidas são descartadas das declarações de saída.  
  
**Declarações que deixar uma floresta**  
  
Declarações que deixar uma floresta apresentam uma preocupação de segurança menor da floresta que as declarações que inserir a floresta. Requerimentos judiciais ou Extrajudiciais têm permissão para sair da floresta como-mesmo quando não houver nenhum declarações correspondentes política de transformação no lugar. Também é possível emitir declarações que não são definidas na floresta como parte de transformar declarações que sair da floresta. Isso é facilmente configurar relações de confiança através de floresta com declarações. Um administrador pode determinar se declarações que inserir floresta precisam ser transformado e configurar a política apropriada. Por exemplo, um administrador pode definir uma política se houver uma necessidade para ocultar uma declaração para evitar a divulgação de informações.  
  
**Erros de sintaxe em regras de transformação de declarações**  
  
Se uma política de transformação de determinado requerimentos judiciais ou Extrajudiciais tem um conjunto de regras que é sintaticamente incorreto ou se houver outros problemas de sintaxe ou armazenamento, a política é considerada inválida. Isso é tratado diferente as condições padrão mencionadas anteriormente.  
  
Active Directory é possível determinar a intenção nesse caso e entra em um modo sem falhas, onde não há declarações de saída são geradas nessa confiança + direção de passagem. Intervenção do administrador é necessária para corrigir o problema. Isso pode acontecer se LDAP é usado para editar a política de transformação de declarações. Cmdlets do Windows PowerShell para o Active Directory ter validação para prevenir que escrever uma política com problemas de sintaxe.  
  
## <a name="other-language-considerations"></a>Outras considerações de idioma  
  
1.  Há várias palavras-chave ou caracteres especiais neste idioma (chamado de terminais). Esses são apresentados na [terminais de idioma](Claims-Transformation-Rules-Language.md#BKMK_LT) tabela posteriormente neste tópico. As mensagens de erro usam as marcas para esses terminais de diferenciação.  
  
2.  Terminais, às vezes, podem ser usados como literais de cadeia de caracteres. No entanto, tal uso pode entrar em conflito com a definição de linguagem ou ter consequências indesejadas. Esse tipo de uso não é recomendado.  
  
3.  A ação de regra não é possível executar qualquer conversões de tipo em valores de declaração e um conjunto de regras que contenha tal uma ação de regra é considerado inválido. Isso pode causar um erro de tempo de execução, e nenhuma declarações de saída são geradas.  
  
4.  Quando uma ação de regra se refere a um identificador que não foi usado na parte selecione lista de condição de regra, é um uso inválido. Isso poderia causar um erro de sintaxe.  
  
    **Exemplo: Referência de identificador incorreta**  
    A regra a seguir ilustra um identificador incorreto usado em ação de regra.  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>Exemplos de regras de transformação  
  
-   **Permitir que todos os requerimentos de um determinado tipo**  
  
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
Regras de transformação de declarações são analisadas por um analisador personalizado para verificar se há erros de sintaxe. Este analisador é executado por cmdlets do Windows PowerShell relacionados antes de armazenar regras no Active Directory. Os erros em analisar as regras, incluindo erros de sintaxe, são impressos no console. Controladores de domínio também executam o analisador antes de usar as regras para transformar declarações e que fazem logon erros no log de eventos (Adicionar números de log de eventos).  
  
Esta seção ilustra alguns exemplos de regras que são escritos com a sintaxe correspondente e sintaxe incorreta erros gerados pelo analisador.  
  
1.  Exemplo:  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    Este exemplo tem uma ponto e vírgula incorretamente usada no lugar de dois-pontos.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar dados de política.*  
    *Número da linha: 1, número de colunas: 2, token de erro:;. Linha: ' c1; [] = > Issue(claim=c1);'.*  
    *Erro do analisador: ' POLICY0030: erros de sintaxe, inesperado ';', esperando um destes procedimentos: ':'.'*  
  
2.  Exemplo:  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    Neste exemplo, a marca de identificador na política de emissão de cópia é indefinida.   
    **Mensagem de erro**:   
    *POLICY0011: Nenhuma condição na regra declaração coincide com a marca de condição especificada no CopyIssuanceStatement: 'c2'.*  
  
3.  Exemplo:  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    "bool" não é um Terminal no idioma, e não é um ValueType válido. Terminais válidos são listados na mensagem de erro a seguir.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar dados de política.*  
    Número da linha: 1, número de colunas: 39, token de erro: "bool". Linha: ' c1: [tipo = = "x1", valor = = "1", valuetype = = "bool"] = > Issue(claim=c1);'.   
    *Erro do analisador: ' POLICY0030: erros de sintaxe, inesperado 'cadeia de caracteres', esperando um destes procedimentos: 'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'Identificador'*  
  
4.  Exemplo:  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    O número **1** neste exemplo não é um token válido no idioma e, tal uso não é permitido em uma condição de correspondência. Ele precisa estar entre aspas para torná-lo em uma cadeia de caracteres.   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar dados de política.*  
    *Número da linha: 1, número de colunas: 23, token de erro: 1. linha: ' c1: [tipo = = "x1", valor = = 1, valuetype = = "bool"] = > Issue(claim=c1);'. **Erro do analisador: ' POLICY0029: entrada inesperada.*  
  
5.  Exemplo:  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    Este exemplo usou um double sinal de igual (= =) em vez de um único sinal de igual (=).   
    **Mensagem de erro:**  
    *POLICY0002: Não foi possível analisar dados de política.*  
    *Número da linha: 1, número de colunas: 91, token de erro: = =. Linha: ' c1: [tipo = = "x1", valor = = "1"*  
    *ValueType = = "boolean"] = > problema (type=c1.type, valor = "0", valuetype = = "boolean");'.*  
    *Erro do analisador: ' POLICY0030: erros de sintaxe, inesperado '= =', esperando um destes procedimentos: '='*  
  
6.  Exemplo:  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    Este exemplo é sintaticamente e semanticamente correto. No entanto, usando "boolean" como um valor de cadeia de caracteres é associado ao causar confusão e ele deve ser evitado. Conforme mencionado anteriormente, usando terminais de linguagem como requerimentos judiciais ou Extrajudiciais valores devem ser evitadas onde for possível.  
  
## <a name="BKMK_LT"></a>Terminais de idioma  
A tabela a seguir lista o conjunto completo de terminal cadeias de caracteres e os terminais de idiomas associados são usados na linguagem de regras de transformação de declarações. Essas definições usam cadeias de caracteres UTF-16 maiusculas de minúsculas.  
  
|Cadeia de caracteres|Terminal|  
|----------|------------|  
|"=>"|IMPLICA|  
|";"|PONTO E VÍRGULA|  
|":"|DOIS-PONTOS|  
|","|VÍRGULA|  
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
|"&&"|E|  
|"problema"|PROBLEMA|  
|"tipo"|TIPO|  
|"value"|VALOR|  
|"valuetype"|VALUE_TYPE|  
|"declaração de"|DECLARAÇÃO|  
|"[_A-Za-z][_A-Za-z0-9]*"|IDENTIFICADOR|  
|"\\"[^\\"\n]*\\""|CADEIA DE CARACTERES|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"cadeia de caracteres"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>Sintaxe da linguagem  
A linguagem de regras de transformação requerimentos judiciais ou Extrajudiciais seguinte é especificada no formulário ABNF. Essa definição usa os terminais que são especificados na tabela anterior além das produções ABNF definida aqui. As regras devem estar codificadas em UTF-16, e as comparações de cadeias de caracteres devem ser tratadas como maiusculas de minúsculas.  
  
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
  


