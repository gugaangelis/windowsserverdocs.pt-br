---
title: Histórico de desempenho de adaptadores de rede
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239233"
---
# Histórico de desempenho de adaptadores de rede

> Aplicável a: Windows Server Insider Preview

Este tópico subpropriedade do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para adaptadores de rede. Histórico de desempenho do adaptador de rede está disponível para cada adaptador de rede física em cada servidor no cluster. Histórico de desempenho de acesso de memória Direct (RDMA) remoto está disponível para cada adaptador de rede física com RDMA habilitado.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para adaptadores de rede em um servidor que está abaixo. Coleção retome automaticamente quando o servidor de volta.

## Unidades e nomes de série

Essas séries são coletadas para cada adaptador de rede qualificado:

| Série                               | Unidade            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits por segundo |
| `netadapter.bandwidth.outbound`      | bits por segundo |
| `netadapter.bandwidth.total`         | bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | bits por segundo |
| `netadapter.bandwidth.rdma.total`    | bits por segundo |

   > [!NOTE]
   > Histórico de desempenho do adaptador de rede é registrado em **bits** por segundo, não em bytes por segundo. Um adaptador de rede 10 GbE pode enviar e receber aproximadamente 1.000.000.000 bits = 125,000,000 bytes = 1,25 GB por segundo em seu máximo teórico.

## Como interpretar

| Série                               | Como interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taxa de dados recebidos pelo adaptador de rede.                         |
| `netadapter.bandwidth.outbound`      | Taxa de dados enviados pelo adaptador de rede.                             |
| `netadapter.bandwidth.total`         | Taxa total de dados recebidos ou enviados pelo adaptador de rede.           |
| `netadapter.bandwidth.rdma.inbound`  | Taxa de dados recebidos por RDMA pelo adaptador de rede.               |
| `netadapter.bandwidth.rdma.outbound` | Taxa de dados enviados por RDMA, o adaptador de rede.                   |
| `netadapter.bandwidth.rdma.total`    | Taxa total de dados recebidos ou enviados sobre RDMA pelo adaptador de rede. |

## Onde eles vêm

O `bytes.*` série é coletada do `Network Adapter` contador de desempenho definido no servidor em que o adaptador de rede é instalado, uma instância por adaptador de rede.

| Série                           | Contador de origem           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

O `rdma.*` série é coletada do `RDMA Activity` contador de desempenho definido no servidor em que o adaptador de rede é instalado, uma instância por adaptador de rede com RDMA habilitado.

| Série                               | Contador de origem           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *soma das opções acima*   |

   > [!NOTE]
   > Contadores são medidas ao longo de todo o intervalo, não de amostra. Por exemplo, se o adaptador de rede estiver ocioso para transferências bits 200, mas 9 segundos na segunda 10, seu `netadapter.bandwidth.total` será gravado como 20 bits por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura toda a atividade e robusto para ruído.

## Uso do PowerShell

Use o cmdlet [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## Veja também

- [Histórico de desempenho de espaços de armazenamento diretos](performance-history.md)
