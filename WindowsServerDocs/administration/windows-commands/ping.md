---
title: ping
description: Use o ping para verificar a conectividade de rede.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816747"
---
# <a name="ping"></a>ping

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O **ping** comando verifica a conectividade de nível de IP para outro computador TCP/IP enviando mensagens de solicitação de eco do ICMP Internet Control Message Protocol (). O recebimento de mensagens são exibidas, junto com os tempos de ida e volta de resposta de eco correspondente. ping é o principal comando TCP/IP usado para solucionar problemas de conectividade, a acessibilidade e a resolução de nome. Usado sem parâmetros, **ping** exibe a Ajuda.

## <a name="syntax"></a>Sintaxe

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|/t|Especifica que o ping continuar enviando mensagens de solicitação de eco para o destino até que seja interrompido. Para interromper e exibir estatísticas, pressione CTRL + break. Para interromper e sair **ping**, pressione CTRL + C.|
|/a|Especifica que a resolução de nome inversa é executada no endereço IP de destino. Se isso for bem-sucedido, o ping exibe o nome de host correspondente.|
|/n \<contagem\>|Especifica o número de eco solicitar mensagens enviadas. O padrão é 4.|
|/l \<Size\>|Especifica o tamanho, em bytes, do campo de dados em que o eco de mensagens de solicitação enviadas. O padrão é 32. O tamanho máximo é 65.527.|
|/f|Especifica que recuperar mensagens de solicitação são enviadas com o não fragmentar no cabeçalho IP definido como 1 (disponível somente no IPv4). O mensagem de solicitação de eco não pode ser fragmentado por roteadores no caminho para o destino. Esse parâmetro é útil para solucionar problemas do caminho de unidade de transmissão máxima (PMTU).|
|/I \<TTL\>|Especifica o valor do campo TTL no cabeçalho IP de eco solicitar mensagens enviadas. O padrão é o valor TTL padrão para o host. O número máximo *TTL* é 255.|
|/v \<TOS\>|Especifica o valor do tipo de campo de serviço (TOS) no cabeçalho IP de eco solicitar mensagens enviadas (disponíveis somente no IPv4). O padrão é 0. *TOS* é especificado como um valor decimal de 0 a 255.|
|/r \<contagem\>|Especifica que a opção de rota de registro no cabeçalho IP é usada para registrar o caminho tomada pelo mensagem de solicitação de eco e correspondente (disponível somente no IPv4) a mensagem de resposta de eco. Cada salto no caminho usa uma entrada na opção de registro de rota. Se possível, especifique um *contagem* que é igual ou maior que o número de saltos entre a origem e destino. O *contagem* deve conter um mínimo de 1 e um máximo de 9.|
|/s \<contagem\>|Especifica que a opção de carimbo de hora de Internet no cabeçalho IP é usada para registrar a hora de chegada para a mensagem de solicitação de eco e correspondente ecoar a mensagem de resposta para cada salto. O *contagem* deve ser um mínimo de 1 e um máximo de 4. Isso é necessário para os endereços de destino de conexão local.|
|/j \<Hostlist\>|Especifica que o uso de mensagens de solicitação a rota de origem flexível de eco de opção no cabeçalho IP com o conjunto de destinos intermediários especificado em *Hostlist* (disponível somente no IPv4). Com o roteamento de origem flexível, os destinos intermediários sucessivos podem ser separados por um ou vários roteadores. O número máximo de endereços ou nomes na lista de hosts é 9. A lista de hosts é uma série de endereços IP (em notação decimal pontilhada) separada por espaços.|
|/k \<Hostlist\>|Especifica que o uso de mensagens de solicitação a rota de eco de opção no cabeçalho IP com o conjunto de destinos intermediários especificado em *Hostlist* (disponível somente no IPv4). Com o roteamento de origem estrita, o próximo destino intermediário deve ser acessado diretamente (ela deve ser um vizinho em uma interface do roteador). O número máximo de endereços ou nomes na lista de hosts é 9. A lista de hosts é uma série de endereços IP (em notação decimal pontilhada) separada por espaços.|
|/w \<timeout\>|Especifica a quantidade de tempo, em milissegundos, para aguardar o eco de mensagem de resposta que corresponde a uma mensagem de solicitação de eco determinado para ser recebida. Se a mensagem de resposta de eco não for recebido dentro do tempo limite, a mensagem de erro "Atingiu o tempo limite de solicitação" é exibida. O tempo limite padrão é 4000 (4 segundos).|
|/R|Especifica que o caminho de ida e volta é rastreado (disponível somente em IPv6).|
|/S \<Srcaddr\>|Especifica o endereço de origem a ser usado (disponível somente em IPv6).|
|/4|Especifica que o IPv4 é usado para executar o ping. Este parâmetro não é necessário para identificar o host de destino com um endereço IPv4. Só é necessário para identificar o host de destino por nome.|
|/6|Especifica que o IPv6 é usado para executar o ping. Este parâmetro não é necessário para identificar o host de destino com um endereço IPv6. Só é necessário para identificar o host de destino por nome.|
|\<TargetName\>|Especifica o nome do host ou endereço IP de destino.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Você pode usar **ping** para testar o nome do computador e o endereço IP do computador. Se o ping do endereço IP for bem-sucedida, mas não executar o ping para o nome do computador é, você pode ter um problema de resolução de nome. Nesse caso, verifique se o nome do computador que você está especificando podem ser resolvidos por meio do arquivo de Hosts local por meio de consultas de sistema de nome de domínio (DNS), ou por meio do NetBIOS técnicas de resolução de nome.
-   Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="BKMK_Examples"></a>Exemplos

A exemplo a seguir mostra **ping** saída do comando:

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Para executar o ping de destino 10.0.99.221 e resolver 10.0.99.221 para seu nome de host, digite:

```
ping /a 10.0.99.221
```

Para executar o ping de destino 10.0.99.221 com mensagens de solicitação de eco de 10, cada um deles tem um campo de dados de 1.000 bytes, digite:

```
ping /n 10 /l 1000 10.0.99.221
```

Para executar o ping de destino 10.0.99.221 e gravar a rota para 4 saltos, digite:

```
ping /r 4 10.0.99.221
```

Para executar o ping de destino 10.0.99.221 e especificar a rota de origem flexível de 10.12.0.1-10.29.3.1-10.1.44.1, digite:

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
