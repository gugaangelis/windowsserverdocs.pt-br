---
title: Implantar NVMe dispositivos de armazenamento usando a atribuição de dispositivo discretos
description: Saiba como usar DDA para implantar dispositivos de armazenamento
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841357"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Implantar NVMe dispositivos de armazenamento usando a atribuição de dispositivo discretos

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016

Começando com o Windows Server 2016, você pode usar a atribuição de dispositivo discretos ou DDA, para passar todo um dispositivo de PCIe para uma VM.  Isso permitirá que o acesso de alto desempenho para dispositivos como o armazenamento de NVMe ou placas de vídeo de dentro de uma VM ao mesmo tempo, sendo capazes de aproveitar os drivers nativos de dispositivos.  Visite o [planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obter mais detalhes sobre quais dispositivos funcionam, quais são as possíveis implicações de segurança, etc. Há três etapas para usar um dispositivo com DDA:
-   Configurar a VM para DDA
-   Desmontar o dispositivo da partição de Host
-   Atribuir o dispositivo a VM convidada

Todos os de comando podem ser executado no Host em um console do Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar a VM para DDA
Atribuição de dispositivo discretos impõe algumas restrições para as VMs e a seguinte etapa precisará ser colocado.

1.  Configure a "parar ação automático" de uma VM para desativação executando

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar o dispositivo da partição de Host

### <a name="locating-the-devices-location-path"></a>Localizando o caminho do local do dispositivo
O caminho do local de PCI é necessária para desmontar e montar o dispositivo a partir do Host.  Um exemplo de caminho de local é semelhante ao seguinte: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Mais detalhes sobre localizado o caminho do local podem ser encontrado aqui: [Planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Desativar o dispositivo
Usando o Gerenciador de dispositivos ou o PowerShell, verifique se o dispositivo está "desativado".  

### <a name="dismount-the-device"></a>Desmontar o dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Atribuir o dispositivo a VM convidada
A etapa final é informar ao Hyper-V que uma VM deve ter acesso ao dispositivo.  Além do caminho local encontrado acima, você precisará saber o nome da vm.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Próximas etapas
Depois que um dispositivo é montado com êxito em uma VM, você agora é capaz de iniciar essa VM e interagir com o dispositivo, como você faria se estivesse sendo executado em um sistema sem sistema operacional.  Você pode verificar isso abrindo o Gerenciador de dispositivos na VM convidada e ver que o hardware é exibido.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Remoção de um dispositivo e retorná-lo para o Host
Se você quiser retornar a ele dispositivo volta ao estado original, você precisará interromper a VM e emitir o seguinte:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Você pode, em seguida, habilitar novamente o dispositivo no Gerenciador de dispositivos e sistema operacional do host será capaz de interagir com o dispositivo novamente.
