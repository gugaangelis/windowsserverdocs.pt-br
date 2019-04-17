---
title: Histórico de desempenho para máquinas virtuais
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239213"
---
# Histórico de desempenho para máquinas virtuais

> Aplicável a: Windows Server Insider Preview

Este tópico subpropriedade do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para máquinas virtuais (VM). Histórico de desempenho está disponível para cada execução, VM em cluster.

   > [!NOTE]
   > Pode levar vários minutos para coleção para começar para VMs recém-criado ou renomeadas.

## Unidades e nomes de série

Essas séries são coletadas para cada VM qualificado:

| Série                            | Unidade             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              |  bytes            |
| `vm.memory.available`             |  bytes            |
| `vm.memory.maximum`               |  bytes            |
| `vm.memory.minimum`               |  bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               |  bytes            |
| `vm.memory.total`                 |  bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | bits por segundo |

Além disso, todas as séries de disco rígido virtual (VHD), como `vhd.iops.total`, são agregados para cada VHD anexado à VM.

## Como interpretar


| Série                            | Descrição                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentagem da máquina virtual está usando de processador (es) do seu servidor de host.                                   |
| `vm.memory.assigned`              | A quantidade de memória atribuída à máquina virtual.                                                      |
| `vm.memory.available`             | A quantidade de memória que permanece disponível, do valor atribuído.                                       |
| `vm.memory.maximum`               | Se o uso de memória dinâmica, essa é a quantidade máxima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.minimum`               | Se o uso de memória dinâmica, essa é a quantidade mínima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.pressure`              | A taxa de memória exigida pela máquina virtual ao longo de memória alocada para a máquina virtual.            |
| `vm.memory.startup`               | A quantidade de memória necessária para a máquina virtual iniciar.                                            |
| `vm.memory.total`                 | Memória total. |
| `vmnetworkadapter.bandwidth.inbound`  | Taxa de dados recebidos pela máquina virtual em todos os seus adaptadores de rede virtual.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taxa de dados enviados, a máquina virtual em todos os seus adaptadores de rede virtual.                            |
| `vmnetworkadapter.bandwidth.total`    | Taxa total de dados recebidos ou enviados por máquina virtual em todos os seus adaptadores de rede virtual.          |

   > [!NOTE]
   > Contadores são medidas ao longo de todo o intervalo, não de amostra. Por exemplo, se a VM estiver ociosa por 9 segundos, mas picos usar 50% de CPU do host na segunda 10, seu `vm.cpu.usage` será gravado como 5% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura toda a atividade e robusto para ruído.

## Uso do PowerShell

Use o cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > O cmdlet Get-VM retorna somente as máquinas virtuais no servidor local (ou especificado), não no cluster.

## Veja também

- [Histórico de desempenho de espaços de armazenamento diretos](performance-history.md)
