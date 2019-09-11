---
title: Implantar dispositivos de armazenamento NVMe usando a atribuição de dispositivo discreta
description: Saiba como usar DDA para implantar dispositivos de armazenamento
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: 001d8addba2cdb8cbbd5d72ffa224d16f62d1245
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872123"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Implantar dispositivos de armazenamento NVMe usando a atribuição de dispositivo discreta

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016

A partir do Windows Server 2016, você pode usar a atribuição de dispositivo discreta ou DDA para passar um dispositivo PCIe inteiro para uma VM.  Isso permitirá o acesso de alto desempenho a dispositivos como o armazenamento da NVMe ou placas gráficas de dentro de uma VM, ao mesmo tempo em que pode aproveitar os drivers nativos dos dispositivos.  Visite o [plano para implantar dispositivos usando a atribuição de dispositivo discreto](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obter mais detalhes sobre quais dispositivos funcionam, quais são as possíveis implicações de segurança, etc. Há três etapas para usar um dispositivo com DDA:
-   Configurar a VM para DDA
-   Desmontar o dispositivo da partição de host
-   Atribuindo o dispositivo à VM convidada

Todos os comandos podem ser executados no host em um console do Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar a VM para DDA
A atribuição de dispositivo discreta impõe algumas restrições às VMs e a etapa a seguir precisa ser executada.

1.  Configurar a "ação de parada automática" de uma VM para desligamento executando

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar o dispositivo da partição de host

### <a name="locating-the-devices-location-path"></a>Localizando o caminho do local do dispositivo
O caminho do local PCI é necessário para desmontar e montar o dispositivo do host.  Um caminho de local de exemplo é semelhante ao `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`seguinte:.   Para obter mais detalhes sobre o local, encontre o caminho de localização: [Planeje a implantação de dispositivos usando a atribuição de dispositivo discreta](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Desabilitar o dispositivo
Usando o Device Manager ou o PowerShell, verifique se o dispositivo está "desabilitado".  

### <a name="dismount-the-device"></a>Desmontar o dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Atribuindo o dispositivo à VM convidada
A etapa final é informar ao Hyper-V que uma VM deve ter acesso ao dispositivo.  Além do caminho do local encontrado acima, você precisará saber o nome da VM.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Novidades
Depois que um dispositivo é montado com êxito em uma VM, agora você pode iniciar essa VM e interagir com o dispositivo como faria normalmente se estivesse executando em um sistema bare-metal.  Você pode verificar isso abrindo o Gerenciador de dispositivos na VM convidada e vendo que o hardware agora aparece.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Removendo um dispositivo e retornando-o para o host
Se você quiser retornar o dispositivo de volta para seu estado original, será necessário interromper a VM e emitir o seguinte:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Você pode reabilitar o dispositivo no Gerenciador de dispositivos e o sistema operacional do host poderá interagir com o dispositivo novamente.
