---
title: pathping
description: Artigo de referência para o comando pathping, que obtém informações sobre a latência de rede e a perda de rede em saltos intermediários entre uma origem e um destino.
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d5ce12d950356c5ebb5ad671de09aaebbc91b9fb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885096"
---
# <a name="pathping"></a>pathping

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Fornece informações sobre a latência de rede e a perda de rede em saltos intermediários entre uma origem e um destino. Esse comando envia várias mensagens de solicitação de eco para cada roteador entre uma origem e um destino, ao longo de um período de tempo e, em seguida, computa os resultados com base nos pacotes retornados de cada roteador. Como esse comando exibe o grau de perda de pacotes em um determinado roteador ou link, você pode determinar quais roteadores ou sub-redes podem estar tendo problemas de rede. Usado sem parâmetros, este comando exibe a ajuda.

> [!NOTE]
> Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.
>
> Além disso, esse comando identifica quais roteadores estão no caminho, o mesmo que usar o [comando tracert](tracert.md). Howevever, esse comando também envia pings periodicamente para todos os roteadores em um período de tempo especificado e computa estatísticas com base no número retornado de cada um.

## <a name="syntax"></a>Sintaxe

```
pathping [/n] [/h <maximumhops>] [/g <hostlist>] [/p <Period>] [/q <numqueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<targetname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /n | Impede o **pathping** de tentar resolver os endereços IP de roteadores intermediários para seus nomes. Isso pode agilizar a exibição dos resultados do **pathping** . |
| /h`<maximumhops>` | Especifica o número máximo de saltos no caminho para pesquisar o destino (destino). O padrão é 30 saltos. |
| /g`<hostlist>` | Especifica que as mensagens de solicitação de eco usam a opção de **rota de origem flexível** no cabeçalho IP com o conjunto de destinos intermediários especificado em *hostlist*. Com o roteamento de origem flexível, os destinos intermediários sucessivos podem ser separados por um ou vários roteadores. O número máximo de endereços ou nomes na lista de hosts é **9**. A *hostlist* é uma série de endereços IP (em notação decimal pontilhada) separados por espaços. |
| /p`<period>` | Especifica o número de milissegundos a aguardar entre pings consecutivos. O padrão é 250 milissegundos (1/4 segundo). Esse parâmetro envia pings individuais para cada salto intermediário. Por isso, o intervalo entre dois pings enviados para o mesmo salto é o *período* multiplicado pelo número de saltos. |
| /q`<numqueries>` | Especifica o número de mensagens de solicitação de eco enviadas a cada roteador no caminho. O padrão é 100 consultas. |
| /w`<timeout>` | Especifica o número de milissegundos para aguardar cada resposta. O padrão é 3000 milissegundos (3 segundos). Esse parâmetro envia vários pings em paralelo. Por isso, a quantidade de tempo especificada no parâmetro *Timeout* não é limitada pela quantidade de tempo especificada no parâmetro *period* para aguardar entre pings. |
| /i`<IPaddress>` | Especifica o endereço de origem. |
| /4`<IPv4>` | Especifica que pathping usa somente IPv4. |
| /6`<IPv6>` | Especifica que o pathping usa somente IPv6. |
| `<targetname>` | Especifica o destino, que é identificado pelo endereço IP ou pelo nome do host. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Todos os parâmetros diferenciam maiúsculas de minúsculas.

- Para evitar o congestionamento da rede e minimizar os efeitos das perdas de intermitência, os pings devem ser enviados em um ritmo suficientemente lento.

### <a name="example-of-the-pathping-command-output"></a>Exemplo da saída do comando pathping

```
D:\>pathping /n contoso1
Tracing route to contoso1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```

Quando o **pathping** é executado, os primeiros resultados listam o caminho. Em seguida, uma mensagem ocupada é exibida por aproximadamente 90 segundos (o tempo varia de acordo com a contagem de saltos). Durante esse tempo, as informações são coletadas de todos os roteadores listados anteriormente e dos links entre eles. No final desse período, os resultados do teste são exibidos.

No relatório de exemplo acima, **este nó/link**, **perdido/enviado = PCT** e colunas de **endereço** mostram que o link entre *172.16.87.218* e *192.168.52.1* está removendo 13% dos pacotes. Os roteadores em saltos 2 e 4 também descartam pacotes endereçados a eles, mas essa perda não afeta a capacidade de encaminhar o tráfego que não é endereçado a eles.

As tarifas de perda exibidas para os links, identificadas como uma barra vertical ( **|** ) na coluna **endereço** , indicam o congestionamento do link que está causando a perda de pacotes que estão sendo encaminhados no caminho. As tarifas de perda exibidas para roteadores (identificadas por seus endereços IP) indicam que esses roteadores podem estar sobrecarregados.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando tracert](tracert.md)
