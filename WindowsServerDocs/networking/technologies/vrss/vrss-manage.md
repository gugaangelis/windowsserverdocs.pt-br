---
title: Gerenciar vRSS
description: Neste tópico, você pode usar os comandos do Windows PowerShell para gerenciar vRSS em máquinas virtuais (VMs) e em hosts Hyper-V.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133832"
---
# Gerenciar vRSS

Neste tópico, você pode usar os comandos do Windows PowerShell para gerenciar vRSS em máquinas virtuais \(VMs\) e em hosts do hyper\-v.

>[!NOTE]
>For more information about the commands mentioned in this topic, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

## VMQ em Hosts Hyper-V

No host do Hyper-V, você deve usar as palavras-chave que controlam os processadores VMQ.

**Exiba as configurações atuais:** 

```PowerShell
Get-NetAdapterVmq
```

**Defina as configurações de VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## portas do switch vRSS no Hyper-V

No host do Hyper-V, você também deve habilitar vRSS na porta do comutador Virtual do hyper\-v.

**Exiba as configurações atuais:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Ambas as seguintes configurações devem ser **True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>Em algumas condições de limitação de recurso, uma porta de comutador Virtual do hyper\-v talvez consiga ter esse recurso ativado. Essa é uma condição temporária, e o recurso pode ser disponibilizado em um momento subsequente.
>
>Se **VrssEnabled** for **True**, o recurso é habilitado para a porta do comutador Virtual do hyper\-v — ou seja, para essa VM ou vNIC.

**Defina as configurações do switch porta vRSS:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## vRSS em VMs e os vNICs do host

You can use the same commands used for native RSS to configure vRSS settings in VMs and host vNICs, which is also the way to enable RSS on host vNICs.  

**Exiba as configurações atuais:**

```PowerShell
Get-NetAdapterRSS
```

**Defina configurações de vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Definir o perfil dentro da VM não afeta o agendamento do trabalho. O Hyper\-v faz com que todas as decisões de planejamento e ignora o perfil dentro da VM.

## Desativar vRSS

Você pode desabilitar vRSS para desabilitar qualquer uma das configurações mencionadas anteriormente.

- Desabilite VMQ para a placa de rede física ou a VM.

  >[!CAUTION]
  >Desativar VMQ em físico NIC afeta gravemente a capacidade do host do hyper\-v-V para lidar com pacotes de entrada.

- Desabilite vRSS para uma VM na porta de comutador Virtual do hyper\-v no host do hyper\-v-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Desabilite vRSS para uma vNIC do host na porta de comutador Virtual do hyper\-v no host do hyper\-v-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Disable RSS in the VM \(or host vNIC\) inside the VM \(or on the host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
