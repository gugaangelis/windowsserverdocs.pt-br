---
title: Contadores de desempenho relacionados à rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcb0c1c5a08a306fbd9b419d0c458c3bc54e1786
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446199"
---
# <a name="network-related-performance-counters"></a>Contadores de desempenho relacionados à rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico lista os contadores que são relevantes para o gerenciamento de desempenho de rede e contém as seções a seguir.  
  
-   [Utilização de recursos](#bkmk_ru)  
  
-   [Possíveis problemas de rede](#bkmk_np)  
  
-   [Receber o desempenho do lado união (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> Utilização de recursos  

Os contadores de desempenho a seguir são relevantes para a utilização de recursos de rede.  
  
- IPv4, IPv6  
  
  -   Datagramas recebidos/s  
  
  -   Datagramas enviados/s  
  
- TCPv4, TCPv6  
  
  -   Segmentos recebidos/s  
  
  -   Segmentos enviados/s  
  
  -   Segmentos retransmitidos/s  
  
- Interface(*) de rede, adaptador de rede (\*)  
  
  - Bytes recebidos/s  
  
  - Bytes enviados/s  
  
  - Pacotes recebidos/s  
  
  - Pacotes enviados/s  
  
  - Comprimento da fila de saída  
  
    Esse contador é o comprimento da fila de pacotes de saída \(em pacotes\). Se isso for maior que 2, ocorrer atrasos. Você deve encontrar o afunilamento e eliminá-lo, se possível. Porque o NDIS enfileira as solicitações, esse comprimento deve ser sempre 0.  
  
- Informações do processador  
  
  - % Tempo do processador  
  
  - Interrupções/s  
  
  - DPCs enfileiradas/s  
  
    Esse contador é uma taxa média no qual as DPCs foram adicionadas à fila de DPCS do processador lógico. Cada processador lógico tem sua própria fila DPC. Esse contador mede a taxa na qual as DPCs são adicionadas à fila, não o número de DPCs na fila. Ele exibe a diferença entre os valores que foram observados nas últimas duas amostras, divididas pela duração do intervalo de amostragem.  
  
##  <a name="bkmk_np"></a> Possíveis problemas de rede  

Os contadores de desempenho a seguir são relevantes para possíveis problemas de rede.  
  
-   Interface(*) de rede, adaptador de rede (\*)  
  
    -   Pacotes recebidos descartados  
  
    -   Erros de pacotes recebidos  
  
    -   Pacotes de saída descartados  
  
    -   Erros de pacotes de saída  
  
-   WFPv4, WFPv6  
  
    -   Pacotes descartados/s

-   UDPv4, UDPv6

    -   Erros de datagramas recebidos  
  
-   TCPv4, TCPv6  
  
    -   Falhas de Conexão  
  
    -   Conexões redefinidas  
  
-   Política de QoS de rede  
  
    -   Pacotes eliminados  
  
    -   Pacotes descartados/s  
  
-   Por Processor Network Interface Card Activity  
  
    -   Recursos insuficientes de recebimento indicações/s  
  
    -   Recursos insuficientes recebido pacotes/s  
  
-   Microsoft Winsock BSP  
  
    -   Datagramas descartadas  
  
    -   Datagramas descartadas/s  
  
    -   Conexões rejeitadas  
  
    -   Conexões rejeitadas/s  
  
##  <a name="bkmk_rsc"></a> Receber o desempenho do lado união (RSC)  

Os contadores de desempenho a seguir são relevantes para o desempenho de RSC.  
  
-   Adaptador de rede(*)  
  
    -   Conexões TCP RSC Active Directory  
  
    -   Tamanho da média de pacotes TCP RSC  
  
    -   TCP RSC unidas pacotes/s  
  
    -   TCP RSC exceções/s

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).