---
title: Use expressões regulares no NPS
description: Este tópico explica o uso de expressões regulares para correspondência de padrões no NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de política de rede e territórios RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a985df9fea31e5ee180caef4e69899ae8468ff71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865257"
---
# <a name="use-regular-expressions-in-nps"></a>Use expressões regulares no NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico explica o uso de expressões regulares para correspondência de padrões no NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de política de rede e territórios RADIUS.

## <a name="pattern-matching-reference"></a>Correspondência de padrão de referência

Você pode usar a tabela a seguir como uma fonte de referência durante a criação de expressões regulares com a sintaxe de correspondência de padrões.

|Caractere|Descrição|Exemplo|
|---------|-----------|-------|
|`\`  |Marca o próximo caractere como um caractere para corresponder. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Corresponde ao início da entrada ou da linha. | &nbsp; |
|`$`  |Corresponde ao final da entrada ou da linha. | &nbsp; |
|`*`  |Corresponde ao caractere anterior zero ou mais vezes. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Corresponde ao caractere anterior uma ou mais vezes. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Corresponde os tempos de caractere zero ou um anterior. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Corresponde a qualquer caractere único, exceto o caractere de nova linha.  | &nbsp; |
|`(pattern)`  |Corresponde a "padrão" e lembra a correspondência.<br />Para corresponder aos caracteres literais `(` e `)` (parênteses), use `\(` ou `\)`.   | &nbsp;  |
|`x|y `  |Corresponde a x ou y.  |`/z|food?/ matches "zoo" or "food."` |
|`{n} `  |Corresponde exatamente a n vezes \(n é um não\-inteiro negativo\).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{n,}`  |Corresponde a pelo menos n vezes \(n é um não\-inteiro negativo\).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{n,m}`  |Corresponde a pelo menos n e no máximo m vezes \(m e n são não\-números inteiros negativos\).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[xyz]`  |Corresponde a qualquer um dos caracteres incluídos \(um conjunto de caracteres\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^xyz]`  |Corresponde a qualquer caractere que não está entre \(um conjunto de caracteres negativos\).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Corresponde a um limite de palavra \(por exemplo, um espaço\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Corresponde a um limite de não palavra.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Corresponde a um caractere de dígito \(equivalentes aos dígitos de 0 a 9\).  | &nbsp; |
|`\D`  |Corresponde a um caractere não dígito \(equivalente a `[^0-9]` \).  | &nbsp; |
|`\f`  |Corresponde ao caractere de feed de um formulário.  | &nbsp; |
|`\n`  |Corresponde ao caractere de feed de uma linha.  | &nbsp; |
|`\r`  |Corresponde a um caractere de retorno de carro.  | &nbsp; |
|`\s`  |Corresponde a qualquer caractere de espaço em branco, incluindo espaço, tabulação e avanço de página \(equivalente a `[ \f\n\r\t\v]` \).  | &nbsp; |
|`\S`  |Corresponde a qualquer caractere não seja espaço em branco \(equivalente a `[^ \f\n\r\t\v]` \).  | &nbsp; |
|`\t`  |Corresponde a um caractere de tabulação.  | &nbsp; |
|`\v`  |Corresponde a um caractere de tabulação vertical.  | &nbsp; |
|`\w`  |Corresponde a qualquer caractere de palavra, incluindo o caractere de sublinhado \(equivalente a `[A-Za-z0-9_]` \).  | &nbsp; |
|`\W`  |Correspondências não qualquer\-caractere de palavra, exceto o sublinhado \(equivalente a `[^A-Za-z0-9_]` \).  | &nbsp; |
|`\num`  |Refere-se às correspondências lembradas \( `?num`, onde num é um inteiro positivo\).  Essa opção pode ser usada somente na **substituir** caixa de texto ao configurar a manipulação de atributos.| `\1` substitui o que está armazenado na primeira correspondência lembrada.  |
|`/n/ `  |Permite a inserção de códigos ASCII em expressões regulares \( `?n`, onde n é um valor de escape octal, hexadecimal ou decimal\).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Exemplos de atributos de política de rede

Os exemplos a seguir descrevem o uso da sintaxe de correspondência de padrões para especificar atributos de política de rede:

- Para especificar todos os números de telefone no código de área 899, a sintaxe é:

     `899.*`

- Para especificar um intervalo de endereços IP que começam com 192.168.1, a sintaxe é:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemplos de manipulação do nome do território no atributo de nome de usuário

Os exemplos a seguir descrevem o uso da sintaxe de correspondência de padrões para manipular nomes de realm para o atributo de nome de usuário, que está localizada na **atributo** guia nas propriedades de uma diretiva de solicitação de conexão.

**Para remover a parte do território do atributo de nome de usuário**

Em um cenário de discagem terceirizado na qual provedor de serviços de Internet \(ISP\) roteia as solicitações de conexão para uma organização NPS, o proxy RADIUS do ISP pode exigir um nome de realm para rotear a solicitação de autenticação. No entanto, o NPS pode não reconhecer a parte do nome do território do nome de usuário. Portanto, o nome de realm deve ser removido pelo proxy RADIUS do ISP antes de ser encaminhado para a organização NPS.

- Localizar: @microsoft \.com

- Substitua:

**Para substituir *user@example.microsoft.com* com *example.microsoft.com\user***

- Localize:`(.*)@(.*)`

- Substitua:`$2\$1`



**Para substituir *domínio \ usuário* com *specific_domain\user***

- Localize:`(.*)\\(.*)`

- Replace: *specific_domain*`\$2`



**Para substituir *usuário* com *user@specific_domain***

- Localize:`$`

- Replace: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemplo de encaminhamento de mensagens RADIUS por um servidor proxy

Você pode criar regras de roteamento que encaminham mensagens RADIUS com um nome de realm especificado para um conjunto de servidores RADIUS quando NPS é usado como um proxy RADIUS. A seguir está a sintaxe recomendada para rotear solicitações com base no nome de realm.

- **Nome NetBIOS**: `WCOAST`
- **Padrão de**:      `^wcoast\\`

No exemplo a seguir, wcoast.microsoft.com é um sufixo de nome principal (UPN) do usuário exclusivo para o wcoast.microsoft.com de domínio DNS ou Active Directory. Usando o padrão fornecido, o proxy NPS pode rotear mensagens com base no nome NetBIOS do domínio ou o sufixo UPN.

- **Nome NetBIOS**: `WCOAST`
- **Sufixo UPN**:   `wcoast.microsoft.com`
- **Padrão de**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
