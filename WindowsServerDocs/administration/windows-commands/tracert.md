---
title: tracert
description: Artigo de referência para o tracert, que determina o caminho levado para um destino, enviando solicitações de eco do protocolo ICMP ou mensagens ICMPv6 para o destino com valores de campo TTL (tempo de vida) aumentando de forma incremental.
ms.topic: reference
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 767871e9100d8b14c8a893fd9d9dbb5862810ace
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156414"
---
# <a name="tracert"></a>tracert

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Essa ferramenta de diagnóstico determina o caminho levado para um destino enviando uma solicitação de eco do protocolo ICMP ou mensagens ICMPv6 para o destino com valores de campo TTL (tempo de vida crescente) incrementalmente. Cada roteador ao longo do caminho é necessário para diminuir o TTL em um pacote IP pelo menos 1 antes de encaminhá-lo. Efetivamente, o TTL é um contador de links máximo. Quando a TTL em um pacote chega a 0, espera-se que o roteador retorne uma mensagem de tempo ICMP excedido para o computador de origem.

Esse comando determina o caminho enviando a primeira mensagem de solicitação de eco com um TTL igual a 1 e incrementando a TTL em 1 em cada transmissão subsequente até que o destino responda ou o número máximo de saltos seja atingido. O número máximo de saltos é 30 por padrão e pode ser especificado usando o parâmetro **/h** .

O caminho é determinado examinando o tempo de ICMP que excedeu as mensagens retornadas por roteadores intermediários e a mensagem de resposta de eco retornada pelo destino. No entanto, alguns roteadores não retornam mensagens com tempo excedido para pacotes com valores de TTL expirados e são invisíveis para o comando **tracert** . Nesse caso, uma linha de asteriscos ( `*` ) é exibida para esse salto. O caminho exibido é a lista de interfaces do roteador próximo/lado dos roteadores no caminho entre um host de origem e um destino. A interface Near/lado é a interface do roteador que está mais próxima do host de envio no caminho.

> [!IMPORTANT]
> Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
>
> Para rastrear um caminho e fornecer latência de rede e perda de pacotes para cada roteador e link no caminho, use o comando de [comando pathping](pathping.md) .

## <a name="syntax"></a>Sintaxe

```
tracert [/d] [/h <maximumhops>] [/j <hostlist>] [/w <timeout>] [/R] [/S <srcaddr>] [/4][/6] <targetname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /d | Interrompe tentativas de resolver os endereços IP de roteadores intermediários para seus nomes. Isso pode acelerar o retorno dos resultados. |
| /h `<maximumhops>` | Especifica o número máximo de saltos no caminho para pesquisar o destino (destino). O padrão é 30 saltos. |
| /j `<hostlist>` | Especifica que as mensagens de solicitação de eco usam a opção de rota de origem flexível no cabeçalho IP com o conjunto de destinos intermediários especificado em `<hostlist>` . Com o roteamento de origem flexível, os destinos intermediários sucessivos podem ser separados por um ou vários roteadores. O número máximo de endereços ou nomes na lista é *9*. O `<hostlist>` é uma série de endereços IP (em notação decimal pontilhada) separados por espaços. Use esse parâmetro somente ao rastrear endereços IPv4. |
| /w `<timeout>` | Especifica a quantidade de tempo em milissegundos para aguardar o tempo de ICMP excedido ou a mensagem de resposta de eco correspondente a uma determinada mensagem de solicitação de eco a ser recebida. Se não for recebido dentro do tempo limite, um asterisco ( `*` ) será exibido. O tempo limite padrão é 4000 (4 segundos). |
| /R | Especifica que o cabeçalho de extensão de roteamento IPv6 deve ser usado para enviar uma mensagem de solicitação de eco para o host local, usando o destino como um destino intermediário e testando a rota inversa. |
| /S `<srcaddr>` | Especifica o endereço de origem a ser usado nas mensagens de solicitação de eco. Use esse parâmetro somente ao rastrear endereços IPv6. |
| /4 | Especifica que tracert.exe pode usar somente IPv4 para este rastreamento. |
| /6 | Especifica que tracert.exe pode usar somente IPv6 para este rastreamento. |
| `<targetname>` | Especifica o destino, identificado pelo endereço IP ou pelo nome do host. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para rastrear o caminho para o host chamado *corp7.Microsoft.com*, digite:

```
tracert corp7.microsoft.com
```

Para rastrear o caminho para o host chamado *corp7.Microsoft.com* e impedir a resolução de cada endereço IP para seu nome, digite:

```
tracert /d corp7.microsoft.com
```

Para rastrear o caminho para o host chamado *corp7.Microsoft.com* e usar a rota de origem flexível *10.12.0.1/10.29.3.1/10.1.44.1*, digite:

```
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando pathping](pathping.md)
