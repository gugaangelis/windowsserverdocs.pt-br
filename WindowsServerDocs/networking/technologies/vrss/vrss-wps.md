---
title: Comandos do Windows PowerShell para RSS e vRSS
description: Neste tópico, você aprenderá a localizar rapidamente as informações de referência técnica sobre comandos do Windows PowerShell para RSS Receive Side Scaling () e o vRSS (RSS virtual).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833257"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandos do Windows PowerShell para RSS e vRSS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá a localizar rapidamente as informações de referência técnica sobre comandos do Windows PowerShell para Receive Side Scaling \(RSS\) e o RSS virtual \(vRSS\).

Use os seguintes comandos RSS para configurar o RSS em um computador físico com vários processadores ou vários núcleos. Você pode usar os mesmos comandos para configurar o vRSS em uma máquina virtual \(VM\) que está executando um sistema operacional com suporte. Para obter mais informações, consulte [Cmdlets do adaptador de rede no Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurar a VMQ

o vRSS requer que a VMQ está habilitado e configurado. Você pode usar os seguintes comandos do Windows PowerShell para gerenciar as configurações de VMQ.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Habilitar e configurar o RSS em um host nativo

Use os seguintes comandos do PowerShell para configurar o RSS em um host nativo, bem como gerenciar o RSS em uma VM ou em um host virtual vNIC (NIC). Alguns dos parâmetros desses comandos também podem afetar a fila de máquina Virtual \(VMQ\) no host do Hyper-V.  

>[!IMPORTANT]
>Habilitar o RSS em uma VM ou em uma vNIC do host é um pré-requisito para habilitar e usar o vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Habilite o vRSS sobre o Hyper\-porta do comutador Virtual V

Além de habilitar o RSS em VM, o vRSS requer que você habilite o vRSS sobre o Hyper\-porta do comutador Virtual V. 

Determinar as configurações de presentes de vRSS e habilitar ou desabilitar o recurso de uma VM.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitado o recurso:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Habilitar ou desabilitar o vRSS em uma vNIC do host

Determinar as configurações de presentes de vRSS e habilitar ou desabilitar o recurso para uma vNIC do host.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar ou desabilitar o recurso:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurar o modo de agendamento na porta do comutador virtual Hyper-V 
>Aplica-se a: Windows Server 2019

2019 do Windows Server, o vRSS pode atualizar os processadores lógicos usados para processar o tráfego de rede dinamicamente.  Dispositivos com drivers com suporte têm esse modo agendamento habilitado por padrão. 

Determinar o modo de agendamento presente em um sistema ou modificar o modo de agendamento para uma VM.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurar o modo de agendamento em uma vNIC do host
>Aplica-se a: Windows Server 2019

Para determinar o modo de apresentação de agendamento ou modificar o modo de agendamento para uma vNIC do host, use os seguintes comandos do Windows PowerShell:

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Tópicos relacionados 
Para obter mais informações, consulte os seguintes tópicos de referência.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obter mais informações, consulte [Virtual Receive Side Scaling (vRSS)](vrss-top.md).