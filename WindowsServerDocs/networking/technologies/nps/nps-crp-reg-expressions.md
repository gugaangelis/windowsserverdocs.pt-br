---
title: Use expressões regulares no NPS
description: Este tópico explica o uso de expressões regulares para correspondência de padrões no NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de diretiva de rede e territórios RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fec546e0608c36f9b3d907e486a0a3a24e7d1728
ms.sourcegitcommit: e04565e4a1fb7aaed04addd2bc87cc6ec4c82e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529905"
---
# <a name="use-regular-expressions-in-nps"></a>Use expressões regulares no NPS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico explica o uso de expressões regulares para correspondência de padrões no NPS no Windows Server 2016. Você pode usar essa sintaxe para especificar as condições de atributos de diretiva de rede e territórios RADIUS.

## <a name="pattern-matching-reference"></a>Referência de correspondência de padrões

Você pode usar a tabela a seguir como uma origem de referência ao criar expressões regulares com a sintaxe de correspondência de padrões.


|  Espaço  |                                                                                 Descrição                                                                                  |                                                                 Exemplo                                                                 |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
|     `\`|                                                              Marca o próximo caractere como um caractere a ser correspondido.                                                               |                      `/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`                       |
|     `^`     |                                                                 Corresponde ao início da entrada ou linha.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Corresponde ao fim da entrada ou linha.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Corresponde ao caractere anterior zero ou mais vezes.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Corresponde ao caractere anterior uma ou mais vezes.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Corresponde ao caractere anterior zero ou uma vez.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Faz a correspondência de qualquer caractere único, exceto um caractere de nova linha.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Corresponde a "Pattern" e lembra a correspondência.<br />Para `(` corresponder os caracteres literais e `)` (parênteses), `\(` use `\)`ou.                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Corresponde a x ou y.                                                          |
|   `{n} `    |                                                          Corresponde exatamente às n \(vezes que n é\-um inteiro\)não negativo.                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Corresponde a pelo menos n \(vezes que n é\-um inteiro\)não negativo.                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Corresponde a pelo menos n e no máximo m \(vezes m e n são\-inteiros\)não negativos.                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Faz a correspondência de qualquer um dos \(caracteres incluídos em\)um conjunto de caracteres.                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Corresponde a qualquer caractere que não esteja \(embutido em um\)conjunto de caracteres negativo.                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Corresponde a um limite \(de palavra por exemplo,\)um espaço.                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Corresponde a um limite de não palavra.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Corresponde a um caractere \(de dígito equivalente a dígitos de 0\)a 9.                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Corresponde a um caractere \(não dígito equivalente a. `[^0-9]` \)                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Corresponde a um caractere de feed de formulário.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Corresponde a um caractere de alimentação de linha.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Corresponde a um caractere de retorno de carro.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Corresponde a qualquer caractere de espaço em branco, incluindo espaço, tabulação e `[ \f\n\r\t\v]`avanço \(de formulário equivalente a \).                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Corresponde a qualquer caractere \(que não seja espaço em branco equivalente a. `[^ \f\n\r\t\v]` \)                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Corresponde a um caractere de tabulação.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Corresponde a um caractere de tabulação vertical.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Corresponde a qualquer caractere de palavra, incluindo \(sublinhado equivalente a `[A-Za-z0-9_]` \).                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Corresponde a qualquer\-caractere que não seja palavra, \(excluindo `[^A-Za-z0-9_]`o sublinhado equivalente a \).                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Refere-se a \(correspondências `?num`lembradas, em que\)num é um inteiro positivo.  Essa opção pode ser usada somente na caixa de texto **substituir** ao configurar a manipulação de atributos. |                                       `\1`Substitui o que está armazenado na primeira correspondência lembrada.                                       |
|   `/n/ `    |                      Permite a inserção de códigos ASCII em expressões \( `?n`regulares, em que n é um valor\)de escape octal, hexadecimal ou Decimal.                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Exemplos de atributos de política de rede

Os exemplos a seguir descrevem o uso da sintaxe de correspondência de padrões para especificar atributos de política de rede:

- Para especificar todos os números de telefone dentro do código de área 899, a sintaxe é:

     `899.*`

- Para especificar um intervalo de endereços IP que começam com 192.168.1, a sintaxe é:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Exemplos de manipulação do nome de realm no atributo de nome de usuário

Os exemplos a seguir descrevem o uso da sintaxe de correspondência de padrões para manipular nomes de territórios para o atributo de nome de usuário, que está localizado na guia **atributo** nas propriedades de uma política de solicitação de conexão.

**Para remover a parte de realm do atributo de nome de usuário**

Em um cenário de dial-up terceirizado no qual um ISP \(\) do provedor de serviços de Internet roteia solicitações de conexão para um NPS da organização, o proxy RADIUS do ISP pode exigir um nome de realm para rotear a solicitação de autenticação. No entanto, o NPS pode não reconhecer a parte do nome de realm do nome de usuário. Portanto, o nome do Realm deve ser removido pelo proxy RADIUS do ISP antes de ser encaminhado para o NPS da organização.

- Localizar: @microsoft \.com

- Substitua:

**Para substituir <em>user@example.microsoft.com</em> por _example. Microsoft. com\user_**

- Considerar`(.*)@(.*)`

- Substitua`$2\$1`



**Para substituir o _domínio \ usuário_ por _specific_domain\user_**

- Considerar`(.*)\\(.*)`

- Substitua: *specific_domain*`\$2`



<strong>Para substituir o *usuário* por *user@specific_domain</strong>*

- Considerar`$`

- Substituir: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Exemplo de encaminhamento de mensagens RADIUS por um servidor proxy

Você pode criar regras de roteamento que encaminham mensagens RADIUS com um nome de realm especificado para um conjunto de servidores RADIUS quando o NPS é usado como um proxy RADIUS. A seguir, uma sintaxe recomendada para roteamento de solicitações com base no nome de realm.

- **Nome NetBIOS**:`WCOAST`
- **Padrão**:`^wcoast\\`

No exemplo a seguir, wcoast.microsoft.com é um sufixo UPN (nome principal de usuário) exclusivo para o DNS ou Active Directory domínio wcoast.microsoft.com. Usando o padrão fornecido, o proxy NPS pode rotear mensagens com base no nome NetBIOS do domínio ou no sufixo UPN.

- **Nome NetBIOS**:`WCOAST`
- **Sufixo UPN**:`wcoast.microsoft.com`
- **Padrão**:`^wcoast\\|@wcoast\.microsoft\.com$`


Para obter mais informações sobre como gerenciar o NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
