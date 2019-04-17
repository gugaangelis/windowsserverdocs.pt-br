---
title: Use expressões regulares no NPS
description: Este tópico explica o uso de expressões regulares para correspondência de padrão de NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de política de rede e territórios RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>Use expressões regulares no NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico explica o uso de expressões regulares para correspondência de padrão de NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de política de rede e territórios RADIUS.

## <a name="pattern-matching-reference"></a>Correspondência de padrão de referência

Você pode usar a tabela a seguir como uma fonte de referência ao criar expressões regulares com a sintaxe de padrões coincidentes.

|Caractere|Descrição|Exemplo|
|---------|-----------|-------|
|`\`  |Marca o próximo caractere como um caractere para corresponder. |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |Corresponde ao início da entrada ou linha. | &nbsp; |
|`$`  |Corresponde ao fim da entrada ou linha. | &nbsp; |
|`*`  |Corresponde ao caractere precedente zero ou mais vezes. |`/zo*/ matches either "z" or "zoo."` |
|`+`  |Corresponde ao caractere precedente uma ou mais vezes. |`/zo+/ matches "zoo" but not "z."` |
|`?`  |Corresponda os tempos de caractere zero ou uma anterior. |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |Corresponde a qualquer caractere único, exceto um caractere.  | &nbsp; |
|`( pattern )`  |Corresponde a "pattern" e lembra a correspondência.   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|' x | y '  |Corresponde a x ou y.  |' /z|Alimentos? / coincide com "zoo" ou "receitas". ` |
|`{ n } `  |Corresponde exatamente n vezes \ (n é um integer\ non\ negativo).  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |Corresponde a pelo menos n vezes \ (n é um integer\ non\ negativo).  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |Corresponde a pelo menos n e no máximo m vezes \ (m e n são integers\ non\ negativo).  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |Corresponde a qualquer um dos caracteres incluídos \(a character set\).  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |Corresponde a quaisquer caracteres que não são incluídos \ (um caractere negativo Set \).  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |Corresponde a um limite de palavra \ (por exemplo, um space\).  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |Corresponde a um limite de que não seja palavra.  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |Corresponde a um caractere de dígito \ (equivalente a dígitos de 0 a 9 \).  | &nbsp; |
|`\D`  |Corresponde a um caractere não dígito \ (equivalente a `[^0-9]`\).  | &nbsp; |
|`\f`  |Corresponde ao caractere de feed de um formulário.  | &nbsp; |
|`\n`  |Corresponde ao caractere de feed de uma linha.  | &nbsp; |
|`\r`  |Corresponde a um caractere de retorno de carro.  | &nbsp; |
|`\s`  |Corresponde a qualquer caractere de espaço em branco, incluindo espaço, guia e avanço \ (equivalente a `[ \f\n\r\t\v]`\).  | &nbsp; |
|`\S`  |Corresponde a qualquer caractere de espaço de branco \ (equivalente a `[^ \f\n\r\t\v]`\).  | &nbsp; |
|`\t`  |Corresponde a um caractere de tabulação.  | &nbsp; |
|`\v`  |Corresponde a um caractere de barra vertical.  | &nbsp; |
|`\w`  |Corresponde a qualquer caractere, incluindo sublinhado \ (equivalente a `[A-Za-z0-9_]`\).  | &nbsp; |
|`\W`  |Corresponde a qualquer caractere non\ word, excluindo sublinhado \ (equivalente a `[^A-Za-z0-9_]`\).  | &nbsp; |
|`\ num`  |Refere-se às correspondências lembradas \ (`?num`, onde num é um integer\ positiva).  Essa opção pode ser usada somente no **substituir** caixa de texto ao configurar a manipulação de atributos.| `\1` Substitui o que é armazenado na primeira correspondência lembrada.  |
|`/ n / `  |Permite a inserção de códigos ASCII em expressões regulares \ (`?n`, onde n é um value\ de escape octal, hexadecimal ou decimal).  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>Exemplos de atributos de política de rede

Os exemplos a seguir descrevem o uso da sintaxe de correspondência de padrões para especificar os atributos de política de rede:

- Para especificar todos os números de telefone no código de área 899, a sintaxe é:

     `899.*`

- Para especificar um intervalo de endereços IP que começam com 192.168.1, a sintaxe é:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemplos de manipulação do nome do território no atributo de nome de usuário

Os exemplos a seguir descrevem o uso da sintaxe para manipular os nomes de territórios para o atributo de nome de usuário, que está localizado na correspondência de padrões a **atributo** guia nas propriedades de uma política de solicitação de conexão.

**Para remover a parte do atributo de nome de usuário território**

Em um cenário de dial-up terceirizado em que uma conexão de rotas Internet serviço provedor \(ISP\) solicitações para um servidor NPS da organização, o proxy RADIUS ISP pode exigir um nome de território para rotear a solicitação de autenticação. No entanto, o servidor NPS não pode reconhecer a parte do nome do território do nome do usuário. Portanto, o nome do território deve ser removido pelo proxy ISP RADIUS antes que ele é encaminhado ao servidor NPS da organização.

- Localização:@microsoft\.com

- Substitua:

**Para substituir *user@example.microsoft.com*com *example.microsoft.com\user***

- Localização:`(.*)@(.*)`

- Substitua:`$2\$1`



**Para substituir *domínio \ usuário* com *specific_domain\user***

- Localização:`(.*)\\(.*)`

- Substituir: *domínio_específico*`\$2`



**Para substituir *usuário* com*user@specific_domain***

- Localização:`$`

- Substituir: @*domínio_específico*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemplo de encaminhamento de mensagens RADIUS por um servidor proxy

Você pode criar regras de roteamento que encaminham mensagens de RAIO com um nome de domínio especificado para um conjunto de servidores RADIUS quando NPS é usado como um proxy RADIUS. A seguir é uma sintaxe recomendada para solicitações de roteamento com base no nome território.

- **Nome NetBIOS **: `WCOAST`
- **Padrão **:      `^wcoast\\`

No exemplo a seguir, wcoast.microsoft.com é um sufixo de UPN (UPN) exclusivo para o domínio do Active Directory ou DNS wcoast.microsoft.com. Usando o padrão fornecido, o proxy NPS pode encaminhar mensagens com base no nome NetBIOS do domínio ou sufixo.

- **Nome NetBIOS **: `WCOAST`
- **Sufixo **:   `wcoast.microsoft.com`
- **Padrão **:      `^wcoast\\|@wcoast\.microsoft\.com$`


Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
