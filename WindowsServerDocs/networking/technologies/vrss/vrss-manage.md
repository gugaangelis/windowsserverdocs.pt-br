---
title: Gerenciar vRSS
description: Neste tópico, você usa os comandos do Windows PowerShell para gerenciar o vRSS em VMs (máquinas virtuais) e em hosts Hyper-V.
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 553be94dd3fe94a74ee9deb84a5ac6d47265afec
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955533"
---
# <a name="manage-vrss"></a>Gerenciar vRSS

Neste tópico, você usa os comandos do Windows PowerShell para gerenciar o vRSS em VMs de máquinas virtuais \( \) e em \- hosts Hyper V.

>[!NOTE]
>Para obter mais informações sobre os comandos mencionados neste tópico, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ em hosts Hyper-V

No host Hyper-V, você deve usar as palavras-chave que controlam os processadores de VMQ.

**Exibir as configurações atuais:**

```PowerShell
Get-NetAdapterVmq
```

**Defina as configurações de VMQ:**

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS em portas de comutador Hyper-V

No host Hyper-V, você também deve habilitar o vRSS na porta do \- comutador virtual Hyper-v.

**Exibir as configurações atuais:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```

As duas configurações a seguir devem ser **verdadeiras**.

- VrssEnabledRequested: true
- VrssEnabled: true

>[!IMPORTANT]
>Em algumas condições de limitação de recursos, uma \- porta de comutador virtual Hyper-V pode não ser capaz de habilitar esse recurso. Essa é uma condição temporária e o recurso pode estar disponível em um momento subsequente.
>
>Se **VrssEnabled** for **true**, o recurso será habilitado para essa porta do \- comutador virtual do Hyper-V, ou seja, para essa VM ou vNIC.

**Defina as configurações de vRSS da porta do comutador:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE

Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS em VMs e host vNICs

Você pode usar os mesmos comandos usados para RSS nativo para definir configurações de vRSS em VMs e host vNICs, que também é a maneira de habilitar o RSS no host vNICs.

**Exibir as configurações atuais:**

```PowerShell
Get-NetAdapterRSS
```

**Definir configurações de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Definir o perfil dentro da VM não afeta o agendamento do trabalho. O Hyper \- V faz todas as decisões de agendamento e ignora o perfil dentro da VM.

## <a name="disable-vrss"></a>Desabilitar vRSS

Você pode desabilitar o vRSS para desabilitar qualquer uma das configurações mencionadas anteriormente.

- Desabilite a VMQ para a NIC física ou para a VM.

  >[!CAUTION]
  >Desabilitar a VMQ na NIC física afeta seriamente a capacidade de seu host Hyper- \- V de lidar com os pacotes de entrada.

- Desabilite o vRSS para uma VM na \- porta do comutador virtual Hyper-v no host Hyper- \- v.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Desabilite o vRSS para um host vNIC na \- porta do comutador virtual Hyper-v no host Hyper- \- v.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Desabilitar RSS na VM \( ou no host vNIC \) dentro da VM \( ou no host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
