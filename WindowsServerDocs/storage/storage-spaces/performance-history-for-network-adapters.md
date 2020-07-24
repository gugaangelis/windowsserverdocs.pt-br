---
title: Histórico de desempenho para adaptadores de rede
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 0b4391812c89a193113fcd442d220e45fbb8c257
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964478"
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

A `bytes.*` série é coletada do `Network Adapter` contador de desempenho definido no servidor onde o adaptador de rede está instalado, uma instância por adaptador de rede.

| Série                           | Contador de origem           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 ×`Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 ×`Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 ×`Bytes Total/sec`    |

A `rdma.*` série é coletada do `RDMA Activity` contador de desempenho definido no servidor onde o adaptador de rede está instalado, uma instância por adaptador de rede com RDMA habilitado.

| Série                               | Contador de origem           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 ×`Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 ×`Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *soma dos itens acima*   |

   > [!NOTE]
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se o adaptador de rede estiver ocioso por 9 segundos, mas transferir 200 bits no 10º segundo, seu `netadapter.bandwidth.total` será registrado como 20 bits por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-netadapter](/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
