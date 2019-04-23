---
title: pathping
description: Saiba como obter usando o comando pathping de informações de perda e latência de rede.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837237"
---
# <a name="pathping"></a>pathping

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Fornece informações sobre a latência de rede e perda de rede em saltos intermediários entre uma origem e destino. **pathping** envia o eco de várias mensagens de solicitação para cada roteador entre uma origem e destino durante um período de tempo e, em seguida, calcula os resultados com base nos pacotes retornados de cada roteador. Porque **pathping** exibe o grau de perda de pacotes em um determinado roteador ou link, você pode determinar quais roteadores ou sub-redes podem estar tendo problemas de rede. 

**pathping** executa o equivalente a **tracert** comando ao identificar quais roteadores estão no caminho. Em seguida, ele envia pings periodicamente para todos os roteadores durante um período de tempo especificado e calcula estatísticas com base no número retornado por cada. Usado sem parâmetros, **pathping** exibe a Ajuda. 

## <a name="syntax"></a>Sintaxe
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/n|Impede **pathping** da tentativa de resolver os endereços IP dos roteadores intermediários para seus nomes. Isso pode agilizar a exibição de **pathping** resultados.|
|/h \<MaximumHops>|Especifica o número máximo de saltos no caminho de pesquisa para o destino (destino). O padrão é 30 saltos.|
|/g \<Hostlist>|Especifica que o uso de mensagens de solicitação a rota de origem flexível de eco de opção no cabeçalho IP com o conjunto de destinos intermediários especificado em *Hostlist*. Com o roteamento de origem flexível, os destinos intermediários sucessivos podem ser separados por um ou vários roteadores. O número máximo de endereços ou nomes na lista de hosts é 9. O *Hostlist* é uma série de endereços IP (em notação decimal pontilhada) separada por espaços.|
|/p \<Period>|Especifica o número de milissegundos de espera entre pings consecutivos. O padrão é 250 milissegundos (4/1 segundo).|
|/q \<NumQueries>|Especifica o número de eco mensagens enviadas para cada roteador no caminho de solicitação. O padrão é 100 consultas.|
|/w \<timeout>|Especifica o número de milissegundos de espera para cada resposta. O padrão é 3000 milissegundos (3 segundos).|
|/i \<IPaddress>|Especifica o endereço de origem.|
|/4 \<IPv4>|Especifica que pathping usará apenas IPv4.|
|/6 \<IPv6>|Especifica que pathping usa apenas o IPv6.|
|\<TargetName >|Especifica o destino, que é identificado pelo nome do host ou endereço IP.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   **pathping** parâmetros diferenciam maiusculas de minúsculas.
-   Para evitar o congestionamento da rede, os pings devem ser enviados em um ritmo suficientemente lento.
-   Para minimizar os efeitos de perdas de intermitente, não envie pings com muita frequência.
-   Ao usar o **/p** parâmetro, os pings são enviados individualmente para cada salto intermediário. Por isso, é o intervalo entre dois pings enviados para o mesmo salto *período* multiplicado pelo número de saltos.
-   Ao usar o **/w** vários pings de parâmetro, podem ser enviados em paralelo. Por causa disso, a quantidade de tempo especificado na *timeout* parâmetro não é limitado pela quantidade de tempo especificado na *período* parâmetro de espera entre pings.
-   Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="BKMK_Examples"></a>Exemplos

A exemplo a seguir mostra **pathping** saída do comando:

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
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
Quando **pathping** é executado, os primeiros resultados listam o caminho. Isso é o mesmo caminho que é mostrado usando o **tracert** comando. Em seguida, será exibida uma mensagem de ocupado por aproximadamente 90 segundos (o tempo varia por contagem de saltos). Durante esse tempo, são coletadas informações de todos os roteadores listados anteriormente e dos links entre eles. no final desse período, os resultados do teste são exibidos.

No relatório de exemplo acima, o **este nó/vínculo**, **perdido/enviado = Pct** e **endereço** colunas mostram que o link entre 172.16.87.218 e 192.168.52.1 está descartando 13 Porcentagem de pacotes. Os roteadores nos saltos 2 e 4 também estão perdendo pacotes endereçados a elas, mas essa perda não afeta sua capacidade de encaminhar o tráfego não endereçado a elas.

As taxas de perda exibidas para os links, identificados como uma barra vertical (**|**) na **endereço** coluna, indique o congestionamento de link que está causando a perda de pacotes que estão sendo encaminhados no caminho. As taxas de perda exibidas para os roteadores (identificados por seus endereços IP) indicam que esses roteadores podem estar sobrecarregados.

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)