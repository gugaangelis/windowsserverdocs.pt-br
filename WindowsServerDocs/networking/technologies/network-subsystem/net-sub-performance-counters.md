---
title: Contadores de desempenho relacionados à rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: e9c4bc76e737c70d3d973e6fa77c57adefc7d5b1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953922"
---
# <a name="network-related-performance-counters"></a>Contadores de desempenho relacionados à rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico lista os contadores que são relevantes para gerenciar o desempenho de rede e contém as seções a seguir.

-   [Utilização de recursos](#bkmk_ru)

-   [Possíveis problemas de rede](#bkmk_np)

-   [Desempenho de União lateral de recebimento (RSC)](#bkmk_rsc)

##  <a name="resource-utilization"></a><a name="bkmk_ru"></a>Utilização de recursos

Os contadores de desempenho a seguir são relevantes para a utilização de recursos de rede.

- IPv4, IPv6

  -   Datagramas recebidos/s

  -   Datagramas enviados/s

- TCPv4, TCPv6

  -   Segmentos recebidos/s

  -   Segmentos Enviados/s

  -   Segmentos retransmitidos/s

- Interface de rede (*), adaptador de rede ( \* )

  - Bytes Recebidos/s

  - Bytes Enviados/s

  - Pacotes recebidos/s

  - Pacotes enviados/s

  - Tamanho da Fila de Saída

    Esse contador é o comprimento da fila de pacotes de saída \( em pacotes \) . Se isso for maior do que 2, ocorrerão atrasos. Você deve encontrar o afunilamento e eliminá-lo se puder. Como o NDIS enfileira as solicitações, esse comprimento deve ser sempre 0.

- Informações do processador

  - % Tempo do Processador

  - Interrupções/s

  - DPCs enfileirados/s

    Esse contador é uma taxa média na qual as DPCs foram adicionadas à fila de DPC do processador lógico. Cada processador lógico tem sua própria fila de DPC. Esse contador mede a taxa na qual as DPCs são adicionadas à fila, não o número de DPCs na fila. Ele exibe a diferença entre os valores que foram observados nos dois últimos exemplos, divididos pela duração do intervalo de amostragem.

##  <a name="potential-network-problems"></a><a name="bkmk_np"></a>Possíveis problemas de rede

Os contadores de desempenho a seguir são relevantes para possíveis problemas de rede.

-   Interface de rede (*), adaptador de rede ( \* )

    -   Pacotes Recebidos Descartados

    -   Erros de Pacotes Recebidos

    -   Pacotes de Saída Descartados

    -   Erros de Pacotes de Saída

-   WFPv4, WFPv6

    -   Pacotes descartados/s

-   UDPv4, UDPv6

    -   Erros de datagramas recebidos

-   TCPv4, TCPv6

    -   Falhas de Conexão

    -   Conexões Redefinidas

-   Política de QoS de rede

    -   Pacotes removidos

    -   Pacotes eliminados/s

-   Atividade da placa de interface de rede por processador

    -   Indicações de recebimento de recursos baixos/s

    -   Pacotes de recursos baixos recebidos/s

-   BSP Microsoft Winsock

    -   Datagramas removidos

    -   Datagramas removidos/s

    -   Conexões rejeitadas

    -   Conexões rejeitadas/s

##  <a name="receive-side-coalescing-rsc-performance"></a><a name="bkmk_rsc"></a>Desempenho de União lateral de recebimento (RSC)

Os contadores de desempenho a seguir são relevantes para o desempenho do RSC.

-   Adaptador de rede (*)

    -   Conexões TCP ativas RSC

    -   Tamanho médio de pacote RSC de TCP

    -   Pacotes de União RSC TCP/s

    -   Exceções RSC de TCP/s

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).