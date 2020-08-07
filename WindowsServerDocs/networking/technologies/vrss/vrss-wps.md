---
title: Comandos do Windows PowerShell para RSS e vRSS
description: Neste tópico, você aprenderá a localizar rapidamente informações de referência técnica sobre os comandos do Windows PowerShell para RSS (recepção lateral) e o vRSS (RSS virtual).
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/05/2018
ms.openlocfilehash: 6b44cdfec4778cf7f36f541021f23a073cb17806
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964002"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandos do Windows PowerShell para RSS e vRSS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, você aprenderá a localizar rapidamente informações de referência técnica sobre os comandos do Windows PowerShell para receber o RSS de escala lateral e o RSS \( \) virtual \( vRSS \) .

Use os comandos RSS a seguir para configurar o RSS em um computador físico com vários processadores ou vários núcleos. Você pode usar os mesmos comandos para configurar o vRSS em uma VM de máquina virtual \( \) que esteja executando um sistema operacional com suporte. Para obter mais informações, consulte [cmdlets do adaptador de rede no Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurar a VMQ

o vRSS requer que a VMQ esteja habilitada e configurada. Você pode usar os seguintes comandos do Windows PowerShell para gerenciar as configurações de VMQ.

- [Desabilitar-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Habilitar-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Habilitar e configurar o RSS em um host nativo

Use os comandos do PowerShell a seguir para configurar o RSS em um host nativo, bem como gerenciar o RSS em uma VM ou em uma NIC virtual do host (vNIC). Alguns dos parâmetros desses comandos também podem afetar Fila de Máquina Virtual \( VMQ \) no host Hyper-V.

>[!IMPORTANT]
>Habilitar o RSS em uma VM ou em um host vNIC é um pré-requisito para habilitar e usar o vRSS.

- [Desabilitar-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Habilitar-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Habilitar o vRSS na \- porta do comutador virtual Hyper-V

Além de habilitar o RSS na VM, o vRSS exige que você habilite o vRSS na \- porta do comutador virtual Hyper-V.

Determine as configurações atuais para vRSS e habilite ou desabilite o recurso para uma VM.

   **Exibir as configurações atuais:**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Habilitado o recurso:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Habilitar ou desabilitar o vRSS em um host vNIC

Determine as configurações atuais para vRSS e habilite ou desabilite o recurso para um host vNIC.

   **Exibir as configurações atuais:**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Habilitar ou desabilitar o recurso:**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurar o modo de agendamento na porta do comutador virtual do Hyper-V
>Aplica-se a: Windows Server 2019

No Windows Server 2019, o vRSS pode atualizar os processadores lógicos usados para processar o tráfego de rede dinamicamente.  Os dispositivos com drivers com suporte têm esse modo de agendamento habilitado por padrão.

Determine o modo de agendamento atual em um sistema ou modifique o modo de agendamento de uma VM.

   **Exibir as configurações atuais:**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurar o modo de agendamento em um host vNIC
>Aplica-se a: Windows Server 2019

Para determinar o modo de agendamento atual ou para modificar o modo de agendamento de um host vNIC, use os seguintes comandos do Windows PowerShell:

   **Exibir as configurações atuais:**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Definir ou modificar o modo de agendamento:**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Tópicos relacionados
Para obter mais informações, consulte os tópicos de referência a seguir.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Para obter mais informações, consulte [vRSS (recepção virtual)](vrss-top.md).