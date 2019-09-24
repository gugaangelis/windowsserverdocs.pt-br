---
title: Expondo o hardware de monitoramento de desempenho Intel a uma máquina virtual Hyper-V
description: Descreve como expor o hardware Monitorning de desempenho da Intel a um computador Hyper-V. Também aborda como a habilitação afeta a migração ao vivo.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1bc821cc46a3a402778e1c76f4a2d8feb862fd75
ms.sourcegitcommit: 45415ba58907d650cfda45f4c57f6ddf1255dcbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213612"
---
# <a name="exposing-intel-performance-monitoring-hardware-to-a-hyper-v-virtual-machine"></a>Expondo o hardware de monitoramento de desempenho Intel a uma máquina virtual Hyper-V
 
## <a name="background"></a>Informações preliminares
Os processadores Intel contêm recursos coletivamente chamados de monitoramento de desempenho de hardware (por exemplo, PMU, PEBS, LBR). Esses recursos são usados pelo software de ajuste de desempenho, como o Intel VTune Amplificator, para analisar o desempenho do software.  Antes do Windows Server 2019 e do Windows 10 versão 1809, nem o sistema operacional do host nem as máquinas virtuais convidadas do Hyper-V podiam usar o hardware de monitoramento de desempenho quando o Hyper-V estava habilitado.  A partir do Windows Server 2019 e do Windows 10 versão 1809, o sistema operacional do host tem acesso ao hardware de monitoramento de desempenho por padrão.  As máquinas virtuais de convidado do Hyper-V não têm acesso por padrão, mas os administradores do Hyper-V podem optar por conceder acesso a uma ou mais máquinas virtuais convidadas.  Este documento descreve as etapas necessárias para expor o monitoramento de desempenho de hardware para máquinas virtuais convidadas.
 
## <a name="requirements"></a>Requisitos 
Para expor o monitoramento de desempenho de hardware em uma máquina virtual, você precisará de:
- Um processador Intel com hardware de monitoramento de desempenho (por exemplo, PMU, PEBS, XISTENTE)
- Windows Server 2019 ou Windows 10 versão 1809 (atualização de outubro de 2018) ou posterior
- Uma máquina virtual do Hyper-V _sem_ [virtualização aninhada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que está no estado parado
 
## <a name="exposing-the-pmu-capabilities-pmu-lbr-pebs-to-virtual-machines-via-powershells-set-vmprocessor-cmdlet"></a>Expondo os recursos de PMU (PMU, LBR, PEBS) para máquinas virtuais por meio do cmdlet Set-VMProcessor do PowerShell
Para habilitar diferentes componentes de monitoramento de desempenho para uma máquina virtual convidada `Set-VMProcessor` específica, use o cmdlet do PowerShell:
 
``` Powershell
# Enable all components
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```
 
``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```
 
``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
 
>Observação: Ao habilitar os componentes Perfmon, se `"pebs"` for especificado `"pmu"` , deverá ser especificado.  Além disso, a habilitação de um componente Perfmon que não tenha suporte dos processadores físicos do host resultará em uma falha de inicialização da máquina virtual.
 
## <a name="effect-of-pmu-enabelement-on-saverestore-export-and-live-migration"></a>Efeito do PMU enabelement ao salvar/restaurar, exportar e migrar ao vivo
 
A Microsoft não recomenda a migração ao vivo ou o salvamento/restauração de máquinas virtuais com o monitoramento de desempenho de hardware entre sistemas com hardware Intel diferente. O comportamento específico de monitoramento de desempenho de hardware geralmente é não-arquitetônico e muda entre os sistemas de hardware Intel.  Mover uma máquina virtual em execução entre sistemas diferentes pode resultar em comportamento imprevisível dos contadores que não são de arquitetura.