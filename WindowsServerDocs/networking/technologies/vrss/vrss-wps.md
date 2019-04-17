---
title: Windows PowerShell Commands for RSS and vRSS
description: Neste tópico, você aprenderá a localizar rapidamente as informações de referência técnica sobre comandos do Windows PowerShell para receber lado dimensionamento (RSS) e RSS virtual (vRSS).
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
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339264"
---
# Windows PowerShell Commands for RSS and vRSS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

In this topic, you learn how to quickly locate technical reference information about Windows PowerShell commands for Receive Side Scaling \(RSS\) and virtual RSS \(vRSS\).

Use the following RSS commands to configure RSS on a physical computer with multiple processors or multiple cores. Você pode usar os mesmos comandos para configurar vRSS em uma máquina virtual \(VM\) que está executando um sistema operacional com suporte. Para obter mais informações, consulte [Os Cmdlets do adaptador de rede no Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## Configurar VMQ

vRSS requer que VMQ está habilitado e configurado. Você pode usar os seguintes comandos do Windows PowerShell para gerenciar as configurações de VMQ.

- [Desativar NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## Enable and configure RSS on a native host

Use the following PowerShell commands to configure RSS on a native host as well as manage RSS in a VM or on a host virtual NIC (vNIC). Alguns dos parâmetros desses comandos também podem afetar a fila de máquina Virtual \(VMQ\) no host do Hyper-V.  

>[!IMPORTANT]
>Enabling RSS in a VM or on a host vNIC is a prerequisite for enabling and using vRSS.

- [Desativar NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## Habilitar vRSS na porta do comutador Virtual do hyper\-v

In addition to enabling RSS in the VM, vRSS requires that you enable vRSS on the Hyper\-V Virtual Switch port. 

Determinar as configurações atuais para vRSS e habilitar ou desabilitar o recurso de uma VM.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitado o recurso:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## Habilitar ou desabilitar vRSS em uma vNIC do host

Determinar as configurações atuais para vRSS e habilitar ou desabilitar o recurso para uma vNIC do host.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar ou desabilitar o recurso:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## Configurar o modo de agendamento na porta do comutador virtual do Hyper-V 
>Aplica-se a: Windows Server 2019

No Windows Server 2019, vRSS pode atualizar os processadores lógicos usados para processar o tráfego de rede dinamicamente.  Dispositivos com drivers compatíveis têm esse modo agendamento habilitado por padrão. 

Determinar o modo de agendamento presente em um sistema ou modificar o modo de agendamento de uma VM.

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## Configurar o modo de agendamento em uma vNIC do host
>Aplica-se a: Windows Server 2019

Para determinar o modo de agendamento presente ou modificar o modo de agendamento para uma vNIC do host, use os seguintes comandos do Windows PowerShell:

   **Exiba as configurações atuais:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## Tópicos relacionados 
Para obter mais informações, consulte os seguintes tópicos de referência.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obter mais informações, consulte [Virtual Receive Side Scaling (vRSS)](vrss-top.md).