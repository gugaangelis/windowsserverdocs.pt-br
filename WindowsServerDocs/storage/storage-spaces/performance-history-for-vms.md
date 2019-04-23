---
title: Histórico de desempenho para as máquinas virtuais
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890867"
---
# <a name="performance-history-for-virtual-machines"></a>Histórico de desempenho para as máquinas virtuais

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para as máquinas virtuais (VM). Histórico de desempenho está disponível para cada execução, VM clusterizada.

   > [!NOTE]
   > Pode levar vários minutos para que a coleção começar a VMs recém-criado ou renomeadas.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para todas as VMs qualificadas:

| série                            | Unidade             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | Por cento          |
| `vm.memory.assigned`              | bytes            |
| `vm.memory.available`             | bytes            |
| `vm.memory.maximum`               | bytes            |
| `vm.memory.minimum`               | bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | bytes            |
| `vm.memory.total`                 | bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | Bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | Bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | Bits por segundo |

Além disso, todas as séries de disco rígido virtual (VHD), tais como `vhd.iops.total`, são agregados para todos os VHDS anexados à VM.

## <a name="how-to-interpret"></a>Como interpretar


| série                            | Descrição                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentagem da máquina virtual está usando do processador (es) do seu servidor de host.                                   |
| `vm.memory.assigned`              | A quantidade de memória atribuída à máquina virtual.                                                      |
| `vm.memory.available`             | A quantidade de memória que permanece disponível, do valor atribuído.                                       |
| `vm.memory.maximum`               | Se o uso de memória dinâmica, isso é a quantidade máxima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.minimum`               | Se o uso de memória dinâmica, isso é a quantidade mínima de memória que pode ser atribuída à máquina virtual. |
| `vm.memory.pressure`              | A taxa de memória exigida pela máquina virtual ao longo de memória alocada para a máquina virtual.            |
| `vm.memory.startup`               | A quantidade de memória necessária para a máquina virtual seja iniciada.                                            |
| `vm.memory.total`                 | Total de memória. |
| `vmnetworkadapter.bandwidth.inbound`  | Taxa de dados recebidos pela máquina virtual em todos os seus adaptadores de rede virtual.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taxa de dados enviados pela máquina virtual em todos os seus adaptadores de rede virtual.                            |
| `vmnetworkadapter.bandwidth.total`    | Taxa total de dados recebidos ou enviados pela máquina virtual em todos os seus adaptadores de rede virtual.          |

   > [!NOTE]
   > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se a VM está ociosa por 9 segundos, mas picos usar 50% da CPU do host na segunda, 10 de seus `vm.cpu.usage` serão registradas como 5% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > O cmdlet Get-VM retorna apenas as máquinas virtuais no servidor local (ou especificado), não em todo o cluster.

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
