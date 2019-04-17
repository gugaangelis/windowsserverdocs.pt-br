---
title: Contadores de desempenho relacionados à rede
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>Contadores de desempenho relacionados à rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico lista os contadores que são relevantes ao gerenciamento de desempenho de rede e contém as seguintes seções.  
  
-   [Utilização de recursos](#bkmk_ru)  
  
-   [Possíveis problemas de rede](#bkmk_np)  
  
-   [Receber desempenho lado unir (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilização de recursos  

Os seguintes contadores de desempenho são relevantes para a utilização de recursos de rede.  
  
-   IPv4, IPv6  
  
    -   Datagramas recebidos por segundo  
  
    -   Datagramas enviados por segundo  
  
-   TCPv4, TCPv6  
  
    -   Segmentos recebidos/s  
  
    -   Segmentos enviados/s  
  
    -   Segmentos retransmitidos por segundo  
  
-   Redes Interface(*), Adapter(\*) de rede  
  
    -   Bytes recebidos/s  
  
    -   Bytes enviados/s  
  
    -   Pacotes recebidos/s  
  
    -   Pacotes enviados/s  
  
    -   Tamanho da fila de saída  
  
     Esse contador é o comprimento da saída pacote fila \(in packets\). Se isso for maior que 2, ocorrem atrasos. Você deverá encontrar o afunilamento e eliminá-lo se possível. Porque NDIS enfileira as solicitações, esse tamanho deve ser sempre 0.  
  
-   Informações do processador  
  
    -   Porcentagem de tempo de processador  
  
    -   Interrupções/s  
  
    -   DPCs enfileirados/s  
  
     Esse contador é uma taxa média em que os DPCs foram adicionadas à fila DPC do processador lógico. Cada processador lógico tem sua própria fila DPC. Esse contador mede a taxa na qual DPCs são adicionadas à fila, não o número de DPCs na fila. Ele exibe a diferença entre os valores que foram observados nos dois últimos exemplos, divididos pela duração do intervalo de exemplo.  
  
##  <a name="bkmk_np"></a>Possíveis problemas de rede  

Os seguintes contadores de desempenho são relevantes para possíveis problemas de rede.  
  
-   Redes Interface(*), Adapter(\*) de rede  
  
    -   Pacotes recebidos descartados  
  
    -   Erros de pacotes recebidos  
  
    -   Pacotes de saída descartados  
  
    -   Erros de saída de pacotes  
  
-   WFPv4, WFPv6  
  
    -   Pacotes descartados/s

-   UDPv4, UDPv6

    -   Erros de datagramas recebidos  
  
-   TCPv4, TCPv6  
  
    -   Falhas de Conexão  
  
    -   Restauração de conexões  
  
-   Política de QoS de rede  
  
    -   Pacotes removidos  
  
    -   Pacotes enviados/s  
  
-   Por atividade de cartão de Interface de rede de processador  
  
    -   Recursos insuficientes receber indicações/s  
  
    -   Recursos insuficientes recebidos pacotes/s  
  
-   Microsoft Winsock BSP  
  
    -   Datagramas soltas  
  
    -   Solto datagramas/s  
  
    -   Conexões rejeitadas  
  
    -   Conexões rejeitadas/s  
  
##  <a name="bkmk_rsc"></a>Receber desempenho lado unir (RSC)  

Os seguintes contadores de desempenho são relevantes para o desempenho de RSC.  
  
-   Adapter(*) de rede  
  
    -   Conexões TCP RSC ativa  
  
    -   Tamanho do pacote média RSC TCP  
  
    -   TCP RSC ocultas pacotes/s  
  
    -   Exceções RSC TCP/s

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).