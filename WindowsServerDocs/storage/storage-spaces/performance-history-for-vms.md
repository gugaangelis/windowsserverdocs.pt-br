---
title: Histórico de desempenho para máquinas virtuais
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6077e72ba36c0ef2d0d34da4768aaf9fa5137fbe
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955218"
---
# <a name="performance-history-for-virtual-machines"></a>Histórico de desempenho para máquinas virtuais

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para máquinas virtuais (VM). O histórico de desempenho está disponível para cada VM clusterizada em execução.

   > [!NOTE]
   > Pode levar vários minutos para que a coleta comece para VMs recém-criadas ou renomeadas.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada VM qualificada:

| Série                            | Unidade             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | {1&gt;percent&lt;1}          |
| `vm.memory.assigned`              | bytes            |
| `vm.memory.available`             | bytes            |
| `vm.memory.maximum`               | bytes            |
| `vm.memory.minimum`               | bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | bytes            |
| `vm.memory.total`                 | bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | bits por segundo |

Além disso, todas as séries de VHD (disco rígido virtual), como `vhd.iops.total` , são agregadas para cada VHD anexado à VM.

## <a name="how-to-interpret"></a>Como interpretar


| Série                            | Descrição                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentagem que a máquina virtual está usando do (s) processador (es) do servidor host.                                   |
| `vm.memory.assigned`              | A quantidade de memória atribuída à máquina virtual.                                                      |
| `vm.memory.available`             | A quantidade de memória que permanece disponível, da quantidade atribuída.                                       |
| `vm.memory.maximum`               | Se estiver usando memória dinâmica, essa é a quantidade máxima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.minimum`               | Se estiver usando memória dinâmica, essa é a quantidade mínima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.pressure`              | A taxa de memória exigida pela máquina virtual sobre a memória alocada para a máquina virtual.            |
| `vm.memory.startup`               | A quantidade de memória necessária para que a máquina virtual seja iniciada.                                            |
| `vm.memory.total`                 | Memória total. |
| `vmnetworkadapter.bandwidth.inbound`  | Taxa de dados recebidos pela máquina virtual em todos os seus adaptadores de rede virtual.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taxa de dados enviados pela máquina virtual em todos os seus adaptadores de rede virtual.                            |
| `vmnetworkadapter.bandwidth.total`    | Taxa total de dados recebidos ou enviados pela máquina virtual em todos os seus adaptadores de rede virtual.          |

   > [!NOTE]
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se a VM estiver ociosa por 9 segundos, mas picos para usar 50% da CPU do host no 10º segundo, sua `vm.cpu.usage` será registrada como 5% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-VM](/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > O cmdlet Get-VM só retorna máquinas virtuais no servidor local (ou especificado), não no cluster.

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
