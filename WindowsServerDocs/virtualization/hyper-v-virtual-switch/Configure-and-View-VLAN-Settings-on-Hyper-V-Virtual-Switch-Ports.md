---
title: Configurar e exibir configurações de VLAN em Portas de comutador virtual do Hyper-V
description: Você pode usar este tópico para aprender as práticas recomendadas para configurar e exibir as configurações de rede Local (VLAN) virtual em uma porta de comutador Virtual Hyper-V no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820547"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configurar e exibir configurações de VLAN em Portas de comutador virtual do Hyper-V

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender as práticas recomendadas para configurar e exibir as configurações de rede Local (VLAN) virtual em uma porta de comutador Virtual Hyper-V.

Quando você deseja definir configurações de VLAN em portas de comutador Virtual Hyper-V, você pode usar qualquer um dos Windows&reg; Server 2016 Hyper-V Manager ou System Center Virtual Machine Manager (VMM).

Se você estiver usando o VMM, o VMM usa o seguinte comando do Windows PowerShell para configurar a porta do comutador.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Se você não estiver usando o VMM e está configurando a porta do comutador no Windows Server, você pode usar o console do Gerenciador do Hyper-V ou o seguinte comando do Windows PowerShell.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

Devido a esses dois métodos para definir as configurações de VLAN em portas de comutador Virtual Hyper-V, é possível que quando você tenta exibir as configurações de porta do comutador, ele aparece para você que as configurações de VLAN não estão configuradas - mesmo quando eles são configurados.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Usar o mesmo método para configurar e exibir as configurações de VLAN de porta do comutador

Para garantir que você não tenha esses problemas, você deve usar o mesmo método para exibir suas configurações de VLAN de porta de comutador que você usou para configurar as configurações de VLAN de porta do comutador.

Para configurar e exibir configurações de porta do comutador VLAN, você deve fazer o seguinte:

- Se você estiver usando o VMM ou o controlador de rede para configurar e gerenciar sua rede e você tiver implantado o Software Defined Networking (SDN), você deve usar o **VMNetworkAdapterIsolation** cmdlets. 
- Se você estiver usando cmdlets do Windows Server 2016 Hyper-V Manager ou o Windows PowerShell, e você não tiver implantado o Software Defined Networking (SDN), você deve usar o **VMNetworkAdapterVlan** cmdlets.

### <a name="possible-issues"></a>Possíveis problemas

Se você não seguir essas diretrizes, você pode encontrar os problemas a seguir.

- Em circunstâncias em que você tiver implantado o SDN e usa o VMM, o controlador de rede, ou o **VMNetworkAdapterIsolation** cmdlets para definir configurações de VLAN em uma porta de comutador Virtual Hyper-V: Se você usar o Gerenciador do Hyper-V ou **VMNetworkAdapterVlan obter** para exibir as definições de configuração, a saída do comando não exibirá as configurações de VLAN. Em vez disso, você deve usar o **Get-VMNetworkIsolation** cmdlet para exibir as configurações de VLAN.
- Em circunstâncias em que você não tiver implantado o SDN e, em vez disso, use o Gerenciador do Hyper-V ou o **VMNetworkAdapterVlan** cmdlets para definir configurações de VLAN em uma porta de comutador Virtual Hyper-V: Se você usar o **Get-VMNetworkIsolation** cmdlet para exibir as definições de configuração, a saída do comando não exibirá as configurações de VLAN. Em vez disso, você deve usar o **VMNetworkAdapterVlan obter** cmdlet para exibir as configurações de VLAN.

Também é importante não tente configurar as mesmas configurações de VLAN de porta do comutador usando ambos os métodos de configuração. Se você fizer isso, a porta do comutador está configurada incorretamente, e o resultado pode ser uma falha na comunicação de rede.

### <a name="resources"></a>Recursos

Para obter mais informações sobre os comandos do Windows PowerShell que são mencionadas neste tópico, consulte o seguinte:

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





