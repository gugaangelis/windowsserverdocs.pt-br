---
title: Habilitar o hardware de monitoramento de desempenho Intel em uma máquina virtual Hyper-V
description: Como habilitar o hardware de monitoramento de desempenho da Intel em um computador Hyper-V. Também aborda como habilitar o monitoramento de desempenho de hardware afeta a migração ao vivo.
ms.prod: windows-server
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1165ce58cf781d6ef5f905cb8b01c00fa4552edb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860259"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Habilitar o hardware de monitoramento de desempenho Intel em uma máquina virtual Hyper-V

Os processadores Intel contêm recursos coletivamente chamados de monitoramento de desempenho de hardware (por exemplo, PMU, PEBS, LBR). Esses recursos são usados pelo software de ajuste de desempenho, como o Intel VTune Amplificator, para analisar o desempenho do software.  Antes do Windows Server 2019 e do Windows 10 versão 1809, nem o sistema operacional do host nem as máquinas virtuais convidadas do Hyper-V podiam usar o hardware de monitoramento de desempenho quando o Hyper-V estava habilitado.  A partir do Windows Server 2019 e do Windows 10 versão 1809, o sistema operacional do host tem acesso ao hardware de monitoramento de desempenho por padrão.  As máquinas virtuais de convidado do Hyper-V não têm acesso por padrão, mas os administradores do Hyper-V podem optar por conceder acesso a uma ou mais máquinas virtuais convidadas.  Este documento descreve as etapas necessárias para expor o monitoramento de desempenho de hardware para máquinas virtuais convidadas.

## <a name="requirements"></a>{1&gt;{2&gt;Requisitos&lt;2}&lt;1}

Para habilitar o monitoramento de desempenho de hardware em uma máquina virtual, você precisará de:

- Um processador Intel com hardware de monitoramento de desempenho (por exemplo, PMU, PEBS, LBR).  Consulte [este documento]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) da Intel para determinar a qual hardware de monitoramento de desempenho o seu sistema dá suporte.
- Windows Server 2019 ou Windows 10 versão 1809 (atualização de outubro de 2018) ou posterior
- Uma máquina virtual do Hyper-V _sem_ [virtualização aninhada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que também está no estado parado

Para habilitar o hardware de monitoramento de desempenho do XISTENTE (rastreamento do processador Intel) no futuro em uma máquina virtual, você precisará de:

- Um processador Intel que dá suporte a XISTENTE e ao recurso PT2GPA.  Consulte [este documento]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) da Intel para determinar a qual hardware de monitoramento de desempenho o seu sistema dá suporte.
- Windows Server versão 1903 (SAC) ou Windows 10 versão 1903 (atualização de maio de 2019) ou posterior
- Uma máquina virtual do Hyper-V _sem_ [virtualização aninhada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) que também está no estado parado

## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Habilitando componentes de monitoramento de desempenho em uma máquina virtual

Para habilitar diferentes componentes de monitoramento de desempenho para uma máquina virtual convidada específica, use o cmdlet `Set-VMProcessor` PowerShell durante a execução como administrador:

``` Powershell
# Enable all components except IPT
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```

``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```

``` Powershell
# Enable IPT 
Set-VMProcessor MyVMName -Perfmon @("ipt")
```

``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Ao habilitar os componentes de monitoramento de desempenho, se `"pebs"` for especificado, `"pmu"` também deverá ser especificado. Só há suporte para PEBS em hardware que tenha uma versão PMU > = 4. Habilitar um componente que não é suportado pelos processadores físicos do host resultará em uma falha de inicialização da máquina virtual.

## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Efeitos de habilitar o monitoramento de desempenho de hardware em salvar/restaurar, exportar e migração dinâmica

A Microsoft não recomenda a migração ao vivo ou o salvamento/restauração de máquinas virtuais com o monitoramento de desempenho de hardware entre sistemas com hardware Intel diferente. O comportamento específico de monitoramento de desempenho de hardware geralmente é não-arquitetônico e muda entre os sistemas de hardware Intel.  Mover uma máquina virtual em execução entre sistemas diferentes pode resultar em comportamento imprevisível dos contadores que não são de arquitetura.

