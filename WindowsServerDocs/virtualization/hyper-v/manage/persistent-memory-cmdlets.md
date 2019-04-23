---
title: Cmdlets para configurar dispositivos de memória persistentes para VMs do Hyper-V
description: Como configurar dispositivos de memória persistentes para VMs do Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878177"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlets para configurar dispositivos de memória persistentes para VMs do Hyper-V

>Aplica-se a: Windows Server 2019

Este artigo fornece aos administradores de sistema e profissionais de TI com informações sobre como configurar as VMs do Hyper-V com memória persistente (também conhecido como memória de classe de armazenamento ou NVDIMM). Dispositivos de memória persistente de NVDIMM-N compatíveis com JDEC têm suporte no Windows Server 2016 e Windows 10 e fornecem acesso de nível de byte para dispositivos de não-volátil de latência muito baixa. Dispositivos de memória persistente de VM têm suporte no Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Criar um dispositivo de memória persistente para uma VM

Use o **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** para criar um dispositivo de memória persistentes de uma VM. O dispositivo deve ser criado em um volume NTFS DAX existente.  A nova extensão de nome de arquivo (.vhdpmem) é usada para especificar que o dispositivo é um dispositivo de memória persistente. Há suporte para apenas o formato fixo de arquivo VHD.

**Exemplo:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Criar uma VM com um controlador de memória persistente



Use o **cmdlet New-VM** para criar uma VM de geração 2 com tamanho de memória especificado e o caminho para uma imagem VHDX. Em seguida, use **VMPmemController adicionar** para adicionar um controlador de memória persistentes a uma VM.

**Exemplo:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Anexe um dispositivo de memória persistentes a uma VM

Use **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** para anexar a um dispositivo de memória persistentes a uma VM

**Exemplo:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Dispositivos de memória persistente dentro de uma VM do Hyper-V aparecem como um dispositivo de memória persistente a serem consumidos e gerenciado pelo sistema operacional convidado. Sistemas operacionais convidados podem usar o dispositivo como um bloco ou volume DAX. Quando os dispositivos de memória persistente dentro de uma VM são usados como um volume DAX, eles se beneficiam do nível de byte de baixa latência, endereço-capacidade do dispositivo de host (sem virtualização de e/s no caminho de código). 

>[!NOTE] 
>Somente há suporte para memória persistente para VMs do Hyper-V Gen2. A migração ao vivo e a migração de armazenamento não têm suporte para VMs com memória persistente. Pontos de verificação de produção de VMs não incluem o estado de memória persistente. 