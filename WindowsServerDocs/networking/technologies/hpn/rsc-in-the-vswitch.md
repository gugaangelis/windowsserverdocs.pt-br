---
title: Receber segmento Coalescing (RSC) em vSwitch
description: Receber segmento de união (RSC) em vSwitch é um recurso no Windows Server 2019 e atualização de 10 de outubro de 2018 do Windows que ajuda a reduzir a utilização de CPU do host e aumenta a taxa de transferência para cargas de trabalho virtuais pela União de vários segmentos TCP em menos, mas maiores segmentos. Processamento de menos, grandes segmentos (Unidos) é mais eficiente do que o processamento de inúmeros, segmentos pequenos.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827777"
---
# <a name="rsc-in-the-vswitch"></a>RSC em vSwitch
>Aplica-se a: Windows Server 2019

Receber segmento de união (RSC) em vSwitch é um recurso no Windows Server 2019 e atualização de 10 de outubro de 2018 do Windows que ajuda a reduzir a utilização de CPU do host e aumenta a taxa de transferência para cargas de trabalho virtuais pela União de vários segmentos TCP em menos, mas maiores segmentos. Processamento de menos, grandes segmentos (Unidos) é mais eficiente do que o processamento de inúmeros, segmentos pequenos.

Windows Server 2012 e posterior incluía uma versão de descarregamento de hardware (implementada no adaptador de rede física) da tecnologia, também conhecida como receber união de segmentos. Esta versão descarregado da RSC ainda está disponível em versões posteriores do Windows. No entanto, ele é incompatível com cargas de trabalho virtuais e foi desabilitado depois que um adaptador de rede física está anexado a um vSwitch. Para obter mais informações sobre a versão somente de hardware do RSC, consulte [receber segmento de união (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Cenários que se beneficiam de RSC em vSwitch

Cargas de trabalho cujo caminho de dados atravessa um comutador virtual se beneficia desse recurso.

Por exemplo: 

-   NICs virtuais do host incluindo:

    -   Rede definida pelo software

    -   Host do Hyper-V

    -   Espaços de Armazenamento Diretos

-   NICs virtuais de convidado do Hyper-V

-   Gateways GRE de rede definida pelo software

-   Contêiner

Cargas de trabalho que não são compatíveis com esse recurso incluem:

-   Gateways IPSEC do sistema de rede definida pelo software

-   SR-IOV habilitado NICs virtuais

-   SMB Direct

## <a name="configure-rsc-in-the-vswitch"></a>Configurar o RSC vSwitch


Por padrão, em vSwitches externo, a RSC é habilitada.

**Exiba as configurações atuais:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Habilitar ou desabilitar a RSC em vSwitch**


>[!IMPORTANT]
>Importante: A RSC em vSwitch pode ser habilitada e desabilitada em tempo real sem impacto para as conexões existentes.


**Desabilitar a RSC vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Reabilitar a RSC vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Para obter mais informações, consulte [Set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
