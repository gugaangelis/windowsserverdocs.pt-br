---
title: Gerenciar vRSS
description: Neste tópico, você pode usar os comandos do Windows PowerShell para gerenciar o vRSS em máquinas virtuais (VMs) e nos hosts Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856187"
---
# <a name="manage-vrss"></a>Gerenciar vRSS

Neste tópico, você deve usar os comandos do Windows PowerShell para gerenciar o vRSS em máquinas virtuais \(VMs\) e no Hyper\-hosts V.

>[!NOTE]
>Para obter mais informações sobre os comandos mencionados neste tópico, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ em Hosts do Hyper-V

No host do Hyper-V, você deve usar as palavras-chave que controlam os processadores VMQ.

**Exiba as configurações atuais:** 

```PowerShell
Get-NetAdapterVmq
```

**Defina as configurações de VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>portas de comutador vRSS no Hyper-V

No host do Hyper-V, você também deve habilitar o vRSS sobre o Hyper\-porta do comutador Virtual V.

**Exiba as configurações atuais:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Ambas as configurações a seguir devem ser **verdadeira**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>Sob algumas condições de limitação de recursos, um Hyper\-porta do comutador Virtual V talvez não consiga ter esse recurso habilitado. Essa é uma condição temporária, e o recurso pode se tornar disponível por vez subsequente.
>
>Se **VrssEnabled** é **verdadeiro**, em seguida, o recurso está habilitado para este Hyper\-porta do comutador Virtual V — ou seja, para essa VM ou vNIC.

**Defina as configurações de vRSS de porta do comutador:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS em VMs e vNICs do host

Você pode usar os mesmos comandos usados para o RSS nativo para definir configurações de vRSS em VMs e vNICs do host, que também é a maneira de habilitar o RSS em vNICs do host.  

**Exiba as configurações atuais:**

```PowerShell
Get-NetAdapterRSS
```

**Defina as configurações de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Definindo o perfil dentro da VM não afeta o agendamento do trabalho. Hyper\-V torna todos o agendamento decisões e ignora o perfil dentro da VM.

## <a name="disable-vrss"></a>Desabilitar o vRSS

Você pode desabilitar o vRSS para desabilitar as configurações mencionadas anteriormente.

- Desabilite a VMQ para a NIC física ou a VM.

  >[!CAUTION]
  >Desabilitando a VMQ em físico NIC afetará seriamente a capacidade do seu Hyper\-host V para lidar com os pacotes de entrada.

- Desabilitar o vRSS para uma VM em que o Hyper\-porta do Hyper V Virtual Switch\-host V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Desabilitar o vRSS para uma vNIC do host do Hyper\-porta do Hyper V Virtual Switch\-host V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Desabilitar o RSS em VM \(ou vNIC do host\) dentro da VM \(ou no host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
