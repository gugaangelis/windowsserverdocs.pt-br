---
title: Receber segmento Coalescing (RSC) em vSwitch
description: A União de segmento de recebimento (RSC) no vSwitch é um recurso do Windows Server 2019 e atualização de 10 de outubro de 2018 que ajuda a reduzir a utilização da CPU do host e aumenta a taxa de transferência para cargas de trabalho virtuais, unindo vários segmentos TCP em menos, mas maior reto. O processamento de menos segmentos grandes (Unidos) é mais eficiente do que o processamento de vários segmentos pequenos.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: 0ffb417728bbdb73d8fb462ff7783b17b511bcd3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814769"
---
# <a name="rsc-in-the-vswitch"></a>RSC no vSwitch
>Aplica-se a: Windows Server 2019

A União de segmento de recebimento (RSC) no vSwitch é um recurso do Windows Server 2019 e atualização de 10 de outubro de 2018 que ajuda a reduzir a utilização da CPU do host e aumenta a taxa de transferência para cargas de trabalho virtuais, unindo vários segmentos TCP em menos, mas maior reto. O processamento de menos segmentos grandes (Unidos) é mais eficiente do que o processamento de vários segmentos pequenos.

O Windows Server 2012 e posterior incluiu uma versão de descarregamento somente de hardware (implementada no adaptador de rede física) da tecnologia também conhecida como União de segmento de recebimento. Essa versão descarregada do RSC ainda está disponível em versões posteriores do Windows. No entanto, ele é incompatível com cargas de trabalho virtuais e foi desabilitado quando um adaptador de rede física é anexado a um vSwitch. Para saber mais sobre a versão somente de hardware do RSC, confira [RSC (União de segmento de recebimento)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Cenários que se beneficiam de RSC no vSwitch

As cargas de trabalho cujo caminho de datapercurso atravessa um comutador virtual se beneficiam desse recurso.

Por exemplo:

-   Hospedar NICs virtuais, incluindo:

    -   Rede definida pelo software

    -   Host do Hyper-V

    -   Espaços de Armazenamento Diretos

-   NICs virtuais de convidado do Hyper-V

-   Gateways GRE de rede definida pelo software

-   Contêiner

As cargas de trabalho que não são compatíveis com esse recurso incluem:

-   Gateways IPSEC de rede definidos pelo software

-   NICs virtuais habilitadas para SR-IOV

-   SMB Direct

## <a name="configure-rsc-in-the-vswitch"></a>Configurar RSC no vSwitch


Por padrão, no vSwitches externo, o RSC é habilitado.

**Exibir as configurações atuais:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Habilitar ou desabilitar RSC no vSwitch**


>[!IMPORTANT]
>Importante: os RSC no vSwitch podem ser habilitados e desabilitados imediatamente sem afetar as conexões existentes.


**Desabilitar RSC no vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Reabilitar RSC no vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Para obter mais informações, consulte [set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
