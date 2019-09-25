---
title: Habilitar o hardware de monitoramento de desempenho Intel em uma máquina virtual Hyper-V
description: Como habilitar o hardware de monitoramento de desempenho da Intel em um computador Hyper-V. Também aborda como habilitar o monitoramento de desempenho de hardware afeta a migração ao vivo.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 67f32a6e2ceeaf07701d558f473e2f997fbd8219
ms.sourcegitcommit: d12d9e6afd71d23e8a24682ad80d2cf3bc486588
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71226023"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Habilitar o hardware de monitoramento de desempenho Intel em uma máquina virtual Hyper-V

Os processadores Intel contêm recursos coletivamente chamados de monitoramento de desempenho de hardware (por exemplo, PMU, PEBS, LBR). Esses recursos são usados pelo software de ajuste de desempenho, como o Intel VTune Amplificator, para analisar o desempenho do software.  Antes do Windows Server 2019 e do Windows 10 versão 1809, nem o sistema operacional do host nem as máquinas virtuais convidadas do Hyper-V podiam usar o hardware de monitoramento de desempenho quando o Hyper-V estava habilitado.  A partir do Windows Server 2019 e do Windows 10 versão 1809, o sistema operacional do host tem acesso ao hardware de monitoramento de desempenho por padrão.  As máquinas virtuais de convidado do Hyper-V não têm acesso por padrão, mas os administradores do Hyper-V podem optar por conceder acesso a uma ou mais máquinas virtuais convidadas.  Este documento descreve as etapas necessárias para expor o monitoramento de desempenho de hardware para máquinas virtuais convidadas.

## <a name="requirements"></a>Requisitos

Para habilitar o monitoramento de desempenho de hardware em uma máquina virtual, você precisará de:

- Um processador Intel com hardware de monitoramento de desempenho (por exemplo, PMU, PEBS, XISTENTE)
- Windows Server 2019 ou Windows 10 versão 1809 (atualização de outubro de 2018) ou posterior
- Uma máquina virtual do Hyper-V _sem_ [virtualização aninhada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que também está no estado parado
 
## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Habilitando componentes de monitoramento de desempenho em uma máquina virtual

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
> [!NOTE]
> Ao habilitar os componentes de monitoramento de desempenho `"pebs"` , se for especificado `"pmu"` , deverá ser especificado.  Além disso, a habilitação de um componente que não é suportado pelos processadores físicos do host resultará em uma falha de inicialização da máquina virtual.
 
## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Efeitos de habilitar o monitoramento de desempenho de hardware em salvar/restaurar, exportar e migração dinâmica
 
A Microsoft não recomenda a migração ao vivo ou o salvamento/restauração de máquinas virtuais com o monitoramento de desempenho de hardware entre sistemas com hardware Intel diferente. O comportamento específico de monitoramento de desempenho de hardware geralmente é não-arquitetônico e muda entre os sistemas de hardware Intel.  Mover uma máquina virtual em execução entre sistemas diferentes pode resultar em comportamento imprevisível dos contadores que não são de arquitetura.