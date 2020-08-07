---
title: ipconfig
description: Artigo de referência para o comando ipconfig, que exibe todos os valores atuais de configuração de rede TCP/IP e atualiza as configurações de protocolo DHCP e DNS (sistema de nomes de domínio).
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0cff8ef691eb9b7adf04b9928a962cda760fdf9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888267"
---
# <a name="ipconfig"></a>ipconfig

Exibe todos os valores atuais de configuração de rede TCP/IP e atualiza as configurações de protocolo DHCP e DNS (sistema de nomes de domínio). Usado sem parâmetros, o **ipconfig** exibe endereços IPv4 (protocolo IP versão 4) e IPv6, máscara de sub-rede e gateway padrão para todos os adaptadores.

## <a name="syntax"></a>Sintaxe

```
ipconfig [/allcompartments] [/all] [/renew [<adapter>]] [/release [<adapter>]] [/renew6[<adapter>]] [/release6 [<adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <adapter>] [/setclassid <adapter> [<classID>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /all | Exibe a configuração de TCP/IP completa para todos os adaptadores. Os adaptadores podem representar interfaces físicas, como adaptadores de rede instalados, ou interfaces lógicas, como conexões dial-up. |
| /displaydns | Exibe o conteúdo do cache do resolvedor de cliente DNS, que inclui as entradas pré-carregadas a partir do arquivo hosts local e quaisquer registros de recursos obtidos recentemente para consultas de nome resolvidas pelo computador. O serviço cliente DNS usa essas informações para resolver rapidamente nomes consultados com frequência, antes de consultar seus servidores DNS configurados. |
| /flushdns | Libera e redefine o conteúdo do cache do resolvedor de cliente DNS. Durante a solução de problemas de DNS, você pode usar este procedimento para descartar entradas de cache negativas do cache, bem como quaisquer outras entradas que foram adicionadas dinamicamente. |
| /registerdns | Inicia o registro dinâmico manual para os nomes DNS e endereços IP configurados em um computador. Você pode usar esse parâmetro para solucionar problemas de um registro de nome DNS com falha ou resolver um problema de atualização dinâmica entre um cliente e o servidor DNS sem reinicializar o computador cliente. As configurações de DNS nas propriedades avançadas do protocolo TCP/IP determinam quais nomes são registrados no DNS. |
| /Release`[<adapter>]` | Envia uma mensagem DHCPRELEASE ao servidor DHCP para liberar a configuração do DHCP atual e descartar a configuração do endereço IP para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o parâmetro do *adaptador* estiver incluído. Esse parâmetro desabilita o TCP/IP para adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros. |
| /release6`[<adapter>]` | Envia uma mensagem DHCPRELEASE ao servidor DHCPv6 para liberar a configuração do DHCP atual e descartar a configuração do endereço IPv6 para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o parâmetro do *adaptador* estiver incluído. Esse parâmetro desabilita o TCP/IP para adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros. |
| /Renew`[<adapter>]` | Renova a configuração do DHCP para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o parâmetro do *adaptador* estiver incluído. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros. |
| /renew6`[<adapter>]` | Renova a configuração de DHCPv6 para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o parâmetro do *adaptador* estiver incluído. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IPv6 automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros. |
| /setclassid`<adapter>[<classID>]` | Configura a ID de classe DHCP para um adaptador especificado. Para definir a ID de classe DHCP para todos os adaptadores, use o caractere curinga asterisco (**&#42;**) no lugar do *adaptador*. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente. Se uma ID de classe DHCP não for especificada, a ID de classe atual será removida. |
| /showclassid`<adapter>` | Exibe a ID de classe DHCP para um adaptador especificado. Para ver a ID de classe DHCP para todos os adaptadores, use o caractere curinga asterisco (**&#42;**) no lugar do *adaptador*. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Esse comando é mais útil em computadores configurados para obter um endereço IP automaticamente. Isso permite que os usuários determinem quais valores de configuração de TCP/IP foram configurados pelo DHCP, pelo APIPA (endereçamento IP privado automático) ou por uma configuração alternativa.

- Se o nome que você fornecer para o *adaptador* contiver espaços, use aspas em volta do nome do adaptador (por exemplo, "nome do adaptador").

- Para nomes de adaptador, o **ipconfig** dá suporte ao uso do caractere curinga asterisco (*) para especificar os adaptadores com nomes que começam com uma cadeia de caracteres especificada ou adaptadores com nomes que contêm uma cadeia de caracteres especificada. Por exemplo, `Local*` corresponde a todos os adaptadores que começam com a cadeia de caracteres local e `*Con*` corresponde a todos os adaptadores que contêm a cadeia de caracteres con.

### <a name="examples"></a>Exemplos

Para exibir a configuração básica de TCP/IP para todos os adaptadores, digite:

```
ipconfig
```

Para exibir a configuração de TCP/IP completa para todos os adaptadores, digite:

```
ipconfig /all
```

Para renovar uma configuração de endereço IP atribuída pelo DHCP somente para o adaptador de conexão de área local, digite:

```
ipconfig /renew Local Area Connection
```

Para liberar o cache do resolvedor de DNS ao solucionar problemas de resolução de nomes DNS, digite:

```
ipconfig /flushdns
```

Para exibir a ID de classe DHCP para todos os adaptadores com nomes que começam com local, digite:

```
ipconfig /showclassid Local*
```

Para definir a ID de classe DHCP para o adaptador de conexão de área local a ser TESTada, digite:

```
ipconfig /setclassid Local Area Connection TEST
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
