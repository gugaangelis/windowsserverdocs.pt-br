---
title: Histórico de desempenho para adaptadores de rede
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: e2379ce540cb26c02bc79f591d2a597874ab287c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856209"
---
# <a name="performance-history-for-network-adapters"></a>Histórico de desempenho para adaptadores de rede

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para adaptadores de rede. O histórico de desempenho do adaptador de rede está disponível para cada adaptador de rede física em cada servidor no cluster. O histórico de desempenho de RDMA (acesso remoto direto à memória) está disponível para cada adaptador de rede física com RDMA habilitado.

   > [!NOTE]
   > O histórico de desempenho não pode ser coletado para adaptadores de rede em um servidor que está inoperante. A coleta será retomada automaticamente quando o servidor voltar a ficar em funcionamento.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada adaptador de rede elegível:

| Série                               | Unidade            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits por segundo |
| `netadapter.bandwidth.outbound`      | bits por segundo |
| `netadapter.bandwidth.total`         | bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | bits por segundo |
| `netadapter.bandwidth.rdma.total`    | bits por segundo |

   > [!NOTE]
   > O histórico de desempenho do adaptador de rede é registrado em **bits** por segundo, não em bytes por segundo. O adaptador de rede 1 10 GbE pode enviar e receber aproximadamente 1.000.000.000 bits = 125 milhões bytes = 1,25 GB por segundo em seu máximo teórico.

## <a name="how-to-interpret"></a>Como interpretar

| Série                               | Como interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taxa de dados recebidos pelo adaptador de rede.                         |
| `netadapter.bandwidth.outbound`      | Taxa de dados enviados pelo adaptador de rede.                             |
| `netadapter.bandwidth.total`         | Taxa total de dados recebidos ou enviados pelo adaptador de rede.           |
| `netadapter.bandwidth.rdma.inbound`  | Taxa de dados recebidos por RDMA pelo adaptador de rede.               |
| `netadapter.bandwidth.rdma.outbound` | Taxa de dados enviados por RDMA pelo adaptador de rede.                   |
| `netadapter.bandwidth.rdma.total`    | Taxa total de dados recebidos ou enviados por RDMA pelo adaptador de rede. |

## <a name="where-they-come-from"></a>De onde vêm

A série de `bytes.*` é coletada do contador de desempenho de `Network Adapter` definido no servidor onde o adaptador de rede está instalado, uma instância por adaptador de rede.

| Série                           | Contador de origem           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

A série de `rdma.*` é coletada do contador de desempenho de `RDMA Activity` definido no servidor onde o adaptador de rede está instalado, uma instância por adaptador de rede com RDMA habilitado.

| Série                               | Contador de origem           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *soma dos itens acima*   |

   > [!NOTE]
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se o adaptador de rede estiver ocioso por 9 segundos, mas transferir 200 bits no décimo segundo, seu `netadapter.bandwidth.total` será gravado como 20 bits por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
