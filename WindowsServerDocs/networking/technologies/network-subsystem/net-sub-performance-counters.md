---
title: Contadores de desempenho relacionados à rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ebff972d670f3fd0b8d12959d161bce03ac487e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401843"
---
# <a name="network-related-performance-counters"></a>Contadores de desempenho relacionados à rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico lista os contadores que são relevantes para gerenciar o desempenho de rede e contém as seções a seguir.  
  
-   [Utilização de recursos](#bkmk_ru)  
  
-   [Possíveis problemas de rede](#bkmk_np)  
  
-   [Desempenho de União lateral de recebimento (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilização de recursos  

Os contadores de desempenho a seguir são relevantes para a utilização de recursos de rede.  
  
- IPv4, IPv6  
  
  -   Datagramas recebidos/s  
  
  -   Datagramas enviados/s  
  
- TCPv4, TCPv6  
  
  -   Segmentos recebidos/s  
  
  -   Segmentos enviados/s  
  
  -   Segmentos retransmitidos/s  
  
- Interface de rede (*), adaptador de rede (\*)  
  
  - Bytes Recebidos/s  
  
  - Bytes Enviados/s  
  
  - Pacotes recebidos/s  
  
  - Pacotes enviados/s  
  
  - Comprimento da fila de saída  
  
    Este contador é o comprimento da fila de pacotes de saída \(em pacotes\). Se isso for maior do que 2, ocorrerão atrasos. Você deve encontrar o afunilamento e eliminá-lo se puder. Como o NDIS enfileira as solicitações, esse comprimento deve ser sempre 0.  
  
- Informações do processador  
  
  - % Tempo do processador  
  
  - Interrupções/s  
  
  - DPCs enfileirados/s  
  
    Esse contador é uma taxa média na qual as DPCs foram adicionadas à fila de DPC do processador lógico. Cada processador lógico tem sua própria fila de DPC. Esse contador mede a taxa na qual as DPCs são adicionadas à fila, não o número de DPCs na fila. Ele exibe a diferença entre os valores que foram observados nos dois últimos exemplos, divididos pela duração do intervalo de amostragem.  
  
##  <a name="bkmk_np"></a>Possíveis problemas de rede  

Os contadores de desempenho a seguir são relevantes para possíveis problemas de rede.  
  
-   Interface de rede (*), adaptador de rede (\*)  
  
    -   Pacotes recebidos descartados  
  
    -   Erros de pacotes recebidos  
  
    -   Pacotes de saída descartados  
  
    -   Erros de pacotes de saída  
  
-   WFPv4, WFPv6  
  
    -   Pacotes descartados/s

-   UDPv4, UDPv6

    -   Erros de datagramas recebidos  
  
-   TCPv4, TCPv6  
  
    -   Falhas de conexão  
  
    -   Conexões redefinidas  
  
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
  
##  <a name="bkmk_rsc"></a>Desempenho de União lateral de recebimento (RSC)  

Os contadores de desempenho a seguir são relevantes para o desempenho do RSC.  
  
-   Adaptador de rede(*)  
  
    -   Conexões TCP ativas RSC  
  
    -   Tamanho médio de pacote RSC de TCP  
  
    -   Pacotes de União RSC TCP/s  
  
    -   Exceções RSC de TCP/s

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).