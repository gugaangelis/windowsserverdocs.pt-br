---
title: Histórico de desempenho para adaptadores de rede
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849977"
---
# <a name="performance-history-for-network-adapters"></a>Histórico de desempenho para adaptadores de rede

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para adaptadores de rede. Histórico de desempenho do adaptador de rede está disponível para cada adaptador de rede física em cada servidor no cluster. Histórico de desempenho de acesso direto à memória (RDMA) remoto está disponível para cada adaptador de rede física com RDMA habilitado.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para adaptadores de rede em um servidor que está inativo. Coleção será retomada automaticamente quando o servidor de volta a funcionar.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada adaptador de rede qualificados:

| série                               | Unidade            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | Bits por segundo |
| `netadapter.bandwidth.outbound`      | Bits por segundo |
| `netadapter.bandwidth.total`         | Bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | Bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | Bits por segundo |
| `netadapter.bandwidth.rdma.total`    | Bits por segundo |

   > [!NOTE]
   > Histórico de desempenho do adaptador de rede é registrado no **bits** por segundo, não em bytes por segundo. Um adaptador de rede 10 GbE pode enviar e receber aproximadamente 1.000.000.000 bits = 125,000,000 bytes = 1,25 GB por segundo no seu máximo teórico.

## <a name="how-to-interpret"></a>Como interpretar

| série                               | Como interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taxa de dados recebidos pelo adaptador de rede.                         |
| `netadapter.bandwidth.outbound`      | Taxa de dados enviados pelo adaptador de rede.                             |
| `netadapter.bandwidth.total`         | Taxa total de dados recebidos ou enviados pelo adaptador de rede.           |
| `netadapter.bandwidth.rdma.inbound`  | Taxa de dados recebidos em RDMA pelo adaptador de rede.               |
| `netadapter.bandwidth.rdma.outbound` | Taxa de dados enviados por RDMA, o adaptador de rede.                   |
| `netadapter.bandwidth.rdma.total`    | Taxa total de dados recebidos ou enviados em RDMA pelo adaptador de rede. |

## <a name="where-they-come-from"></a>Onde eles vêm

O `bytes.*` série é coletada a partir de `Network Adapter` contador de desempenho definido no servidor em que o adaptador de rede está instalado, uma instância por adaptador de rede.

| série                           | Contador de origem           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

O `rdma.*` série é coletada a partir de `RDMA Activity` contador de desempenho definido no servidor em que o adaptador de rede está instalado, uma instância por adaptador de rede com RDMA habilitado.

| série                               | Contador de origem           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *soma dos itens acima*   |

   > [!NOTE]
   > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se o adaptador de rede ficar ocioso por 9 segundos, mas transferências bits 200 na segunda, 10 de seus `netadapter.bandwidth.total` serão registradas como 20 bits por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
