---
title: Planejamento da capacidade do DirectAccess
description: Você pode usar este tópico para obter um relatório sobre o desempenho do servidor DirectAccess do Windows Server 2012 para ajudá-lo com o planejamento de capacidade para o DirectAccess no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 85ac9d40ec0de0a896e99d56679b325cdfc387ff
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517681"
---
# <a name="directaccess-capacity-planning"></a>Planejamento da capacidade do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este documento é um relatório sobre o desempenho do servidor DirectAccess do Windows Server 2012. Foram executados testes para determinar a capacidade de rendimento usando hardware de computador de alta e de baixa gama. O desempenho da CPU de alta e de baixa gama era dependente da produtividade do tráfego da rede e dos tipos de clientes usados. Uma implantação típica do DirectAccess (e a base para esses testes) é constituída por 1/3 (30%) de clientes IPHTTPS e 2/3 (70%) de clientes Teredo. Os clientes Teredo superam os clientes IPHTTPS em parte porque o Windows Server 2012 utiliza RSS (Receive Side Scaling), que permite usar todos os núcleos da CPU. Nesses testes, como o RSS está habilitado, o Hyper threading está desabilitado. Além disso, o protocolo TCP/IP no Windows Server 2012 oferece suporte ao tráfego de UDP, permitindo aos clientes Teredo o balanceamento de carga nas CPUs.

Os dados foram coletados de um servidor de baica gama (4 núcleos, 4 Gig) e do hardware que se espera que seja mais comum em um servidor de alta gama (8 núcleos, 8 Gig).  Abaixo está uma captura de tela do novo Gerenciador de tarefas do Windows 8 em hardware de low-end com 750 clientes (562 Teredo, 188 IPHTTPS) em execução ~ 77 Mbits/s. Isso é para simular usuários que não apresentam credenciais de cartão inteligente.

Esses resultados de teste indicam que o Teredo funciona melhor que o IPHTTPS no Windows 8, mas que o uso da largura de banda Teredo e IPHTTPS melhorou em comparação com o Windows 7.

![Resultados do teste](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)

## <a name="high-end-hardware-test-environment"></a>Ambiente de teste de hardware de alta gama
A tabela a seguir mostra os resultados do ambiente de teste de desempenho de hardware de alta gama. Todos os resultados de teste e análise são detalhados neste documento.

| Configuração-hardware | Hardware de baixa gama (4 GB de RAM, 4 núcleos) | Hardware de alta gama (8 GB, 8 núcleos) |
|--|--|--|
| Túnel Duplo<p>-PKI<p>-Incluindo DNS64/NAT64 | 750 conexões simultâneas a 50% da CPU, 50% da Memória com produtividade da Corpnet NIC de 75 Mbps. O destino de ampliação é 1000 usuários a 50% da CPU. | 1500 conexões simultâneas em 50% CPU, 50% de memória com taxa de transferência de NIC corpnet 150 Mbps. |

## <a name="test-environment"></a>Ambiente de Teste

**Topologia de banco de perf**

![Ambiente de Teste](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)

O ambiente de teste de desempenho é um banco de 5 máquinas. Para o teste de baixa gama, um servidor DirectAccess de 4 núcleos, 4 Gig foi usado e, para o teste de hardware de alta gama, foi usado um servidor DirectAccess de 8 núcleos, 16 Gig. Para ambientes de teste de alta e de baixa gama, o seguinte foi usado: um servidor de back-end (o emissor) e dois computadores clientes (os receptores).  Os receptores são divididos entre os dois computadores clientes. Caso contrário, os receptores seriam associados à CPU e limitarariam o número de clientes e a largura de banda. No lado do recebimento, um simulador simula centenas de clientes (são simulados clientes HTTPS ou Teredo). IPsec, DOSp são ambos configurados. RSS é habilitado no servidor DirectAccess. O tamanho da fila de RSS é definido como 8.  Sem configurar o RSS, um único processador será indexado em uma alta utilização, enquanto os outros núcleos serão pouco utilizados. Também deve-se observar que o servidor DirectAccess é um computador de 4 núcleos com hyper threading desativado.  O hyper threading está desativado porque o RSS funciona apenas em núcleos físicos e o uso de hyper threading produz resultados distorcidos. (Isso significa que nem todos os núcleos serão carregados uniformemente.)

## <a name="testing-results-for-low-end-hardware"></a>Resultados do teste para hardware de baixa gama:

O teste foi realizado com 1000 & com 750 clientes.  Em todos os casos, a divisão do tráfego foi 70% Teredo e 30% IPHTTPS.  Todos os testes envolveram o tráfego TCP sobre Nat64 usando dois túneis IPsec por cliente.  Em todos os testes, a utilização de memória foi pouca e a utilização de CPU foi aceitável.

**Resultados de testes individuais:**

As seções a seguir descrevem testes individuais. Cada título de seção destaca os principais elementos de cada teste, seguido por uma descrição resumida dos resultados e, em seguida, uma tabela que mostra os dados detalhados dos resultados.

**Desempenho em baixa gama: 750 clientes, divisão de 70/30, produtividade de 84,17 Mbits/seg:**

Os três testes a seguir representam hardware de baixa gama.  Nas execuções de teste abaixo, houve 750 clientes com uma produtividade de 84,17 Mbits/seg. e uma divisão de tráfego de 562 Teredo e 188 IPHTTPS. A MTU Teredo foi definida como 1472 e a Derivação Teredo foi habilitada. A média de utilização da CPU é de 46,42% nos três testes e a média de utilização da memória, expressa como uma porcentagem de bytes comprometidos do total de memória disponível de 4 GB, foi de 25,95%.

||||||||
|-|-|-|-|-|-|-|
|**Cenário**|**CPUAvg (do contador)**|**Mbit/s (Lado Corp)**|**Mbit/s (Lado da internet)**|**QMSA Ativo**|**MMSA Ativo**|**Utilização de memória (sistema de 4 Gig)**|
|**HW de low-end.  562 clientes Teredo.  188 clientes do IPHTTPS.**|47,7472542|84,3|119,13|1502, 5|1502,1|26,27%|
|**HW de low-end.  562 clientes Teredo.  188 clientes do IPHTTPS.**|46,3889778|84,146|118,73|1501,25|1501,2|25,90%|
|**HW de low-end.  562 clientes Teredo.  188 clientes do IPHTTPS.**|45,113082|84, 494|118,43|1546,14|1546,1|25,68%|

**1000 clientes, divisão de 70/30, produtividade de 78 Mbits/seg:**

Os três testes a seguir representam desempenho em hardware de baixa gama. Nas execuções de teste abaixo, houve 1000 clientes com uma produtividade média de ~78,64 Mbits/seg. e uma divisão de tráfego de 700 Teredo e 300 IPHTTPS.  A MTU Teredo foi definida como 1472 e a Derivação Teredo foi habilitada.  A média de utilização da CPU é de ~50,7% e a média de utilização da memória, expressa como uma porcentagem de bytes comprometidos do total de memória disponível de 4 GB, foi de ~28,7%.

||||||||
|-|-|-|-|-|-|-|
|**Cenário**|**CPUAvg (do contador)**|**Mbit/s (Lado Corp)**|**Mbit/s (Lado da internet)**|**QMSA Ativo**|**MMSA Ativo**|**Utilização de memória (sistema de 4 Gig)**|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|51,28406247|78,6432|113,19|2002,42|1502,1|25,59%|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|51, 6993128|78,6402|113,22|2001,4|1501,2|30,87%|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|49,75235617|78,6387|113,2|2002,6|1546,1|30,66%|

**1000 clientes, 70/30 divisão, taxa de transferência de 109 Mbits/s:**

Nas execuções de teste abaixo, houve 1000 clientes com uma produtividade de ~109,2 Mbits/seg. e uma divisão de tráfego de 700 Teredo e 300 IPHTTPS. A MTU Teredo foi definida como 1472 e a Derivação Teredo foi habilitada. A média de utilização da CPU é de ~59,06% nos três testes e a média de utilização da memória, expressa como uma porcentagem de bytes comprometidos do total de memória disponível de 4 GB, foi de ~27,34%.

||||||||
|-|-|-|-|-|-|-|
|**Cenário**|**CPUAvg (do contador)**|**Mbit/s (Lado Corp)**|**Mbit/s (Lado da internet)**|**QMSA Ativo**|**MMSA Ativo**|**Utilização de memória (sistema de 4 Gig)**|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|59,81640675|108,305|153,14|2001,64|2001,6|24,38%|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|59,46473798|110,969|157,53|2005,22|2005,2|28,72%|
|**HW de low-end.  700 clientes Teredo.  300 clientes do IPHTTPS.**|57,89089768|108,305|153,14|1999,53|2018,3|24,38%|

## <a name="testing-results-for-high-end-hardware"></a>Resultados do teste para hardware de alta gama:
O teste foi executado com 1500 clientes. A divisão do tráfego foi 70% Teredo e 30% IPHTTPS. Todos os testes envolveram o tráfego TCP sobre Nat64 usando dois túneis IPsec por cliente. Em todos os testes, a utilização de memória foi pouca e a utilização de CPU foi aceitável.

**Resultados de testes individuais:**

As seções a seguir descrevem testes individuais. Cada título de seção destaca os principais elementos de cada teste, seguido por uma descrição resumida dos resultados e, em seguida, uma tabela que contém os dados detalhados dos resultados.

**1500 clientes, divisão de 70/30, produtividade de 153,2 Mbits/seg**

Os cinco testes a seguir representam hardware de alta gama. Nas execuções de teste abaixo, houve 1500 clientes com uma produtividade de ~153,2 Mbits/seg. e uma divisão de tráfego de 1050 Teredo e 450 IPHTTPS. A média de utilização da CPU é de 50,68% nos cinco testes e a média de utilização da memória, expressa como uma porcentagem de bytes comprometidos do total de memória disponível de 8 GB, foi de 22,25%.

||||||||
|-|-|-|-|-|-|-|
|**Cenário**|**CPUAvg (do contador)**|**Mbit/s (Lado Corp)**|**Mbit/s (Lado da internet)**|**QMSA Ativo**|**MMSA Ativo**|**Utilização de memória (sistema de 4 Gig)**|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|51,712437|157, 29|216,29|3000,31|3046|21,58%|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|48,86020205|151, 12|206,53|3002,86|3045,3|21,15%|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|52,23979519|155,511|213,45|3001,15|3002,9|22,90%|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|51,26269767|155, 9|212,92|3000,74|3002,4|22,91%|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|50,15751307|154,772|211,92|3000,9|3002,1|22,93%|
|**HW de ponta.  1050 clientes Teredo.  450 clientes do IPHTTPS.**|49,83665607|145,994|201,92|3000,51|3006|22, 3%|

![Resultados de teste de hardware de alto nível](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)



