---
title: Implantar dispositivos de gráficos usando a atribuição de dispositivo discretos
description: Saiba como usar DDA para implantar dispositivos de gráficos no Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 6c528535fd34f57957a37992843933d4cd9f8824
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447875"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Implantar dispositivos de gráficos usando a atribuição de dispositivo discretos

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

Começando com o Windows Server 2016, você pode usar a atribuição de dispositivo discretos ou DDA, para passar todo um dispositivo de PCIe para uma VM.  Isso permite o acesso de alto desempenho para dispositivos, como [NVMe armazenamento](./Deploying-storage-devices-using-dda.md) ou placas de vídeo de dentro de uma VM ao mesmo tempo, sendo capazes de aproveitar os drivers nativos de dispositivos.  Visite o [planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obter mais detalhes sobre quais dispositivos funcionam, quais são as possíveis implicações de segurança, etc.

Há três etapas para usar um dispositivo com a atribuição de dispositivo discretos:
-   Configurar a VM para atribuição de dispositivo discretos
-   Desmontar o dispositivo da partição de Host
-   Atribuir o dispositivo a VM convidada

Todos os de comando podem ser executado no Host em um console do Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar a VM para DDA
Atribuição de dispositivo discretos impõe algumas restrições para as VMs e a seguinte etapa precisará ser colocado.

1.  Configure a "parar ação automático" de uma VM para desativação executando

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Algumas preparações de VM adicional são necessária para dispositivos de gráficos

Alguns hardwares um desempenho melhor se a VM no configurado em uma determinada maneira.  Para obter detalhes sobre se você precisa ou não as seguintes configurações para o seu hardware, entre em contato com o fornecedor do hardware. Detalhes adicionais podem ser encontrados em [planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) e nesta [postagem de blog.](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1. Ativar ao combinar na CPU
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurar o espaço MMIO de 32 bits
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurar o maior espaço MMIO de 32 bits
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   Observe que os valores de espaço MMIO acima são valores razoáveis para definir para experimentar com uma única GPU.  Se depois de iniciar a máquina virtual, o dispositivo está relatando um erro relacionado a recursos insuficientes, provavelmente você precisará modificar esses valores.  Além disso, se você atribuir várias GPUs, você precisará aumentar esses valores também.

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar o dispositivo da partição de Host
### <a name="optional---install-the-partitioning-driver"></a>Opcional - instale o Driver de particionamento
Atribuição de dispositivo discretos fornecem Vendors hardware a capacidade de fornecer um driver de atenuação de segurança com seus dispositivos.  Observe que este driver não é o mesmo que o driver de dispositivo será instalado na VM convidada.  Ele tem até a critério do fornecedor de hardware para fornecer esse driver, no entanto, se eles fornecem, instale-prévia para desmontar o dispositivo da partição de host.  Entre em contato com o fornecedor do hardware para obter mais informações sobre se eles tiverem um driver de mitigação
> Não se for fornecido nenhum driver de particionamento, durante a desmontagem deverá usar o `-force` opção para ignorar o aviso de segurança. Leia mais sobre as implicações de segurança de fazer isso no [planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Localizando o caminho do local do dispositivo
O caminho do local de PCI é necessária para desmontar e montar o dispositivo a partir do Host.  Um exemplo de caminho de local é semelhante ao seguinte: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Mais detalhes sobre localizado o caminho do local podem ser encontrado aqui: [Planejar a implantação de dispositivos usando atribuição de dispositivo discretos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Desativar o dispositivo
Usando o Gerenciador de dispositivos ou o PowerShell, verifique se o dispositivo está "desativado".  

### <a name="dismount-the-device"></a>Desmontar o dispositivo
Dependendo se o fornecedor forneceu um driver de atenuação, você precisará usar o "-force" opção ou não.
- Se um Driver de mitigação foi instalado
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Se um Driver de atenuação não tiver sido instalado
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Atribuir o dispositivo a VM convidada
A etapa final é informar ao Hyper-V que uma VM deve ter acesso ao dispositivo.  Além do caminho local encontrado acima, você precisará saber o nome da vm.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Próximas etapas
Depois que um dispositivo é montado com êxito em uma VM, você agora é capaz de iniciar essa VM e interagir com o dispositivo, como você faria se estivesse sendo executado em um sistema sem sistema operacional.  Isso significa que agora é possível instalar drivers de Hardware do fornecedor na VM e aplicativos poderão ver esse hardware presente.  Você pode verificar isso abrindo o Gerenciador de dispositivos na VM convidada e ver que o hardware é exibido.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Remoção de um dispositivo e retorná-lo para o Host
Se você quiser retornar a ele dispositivo volta ao estado original, você precisará interromper a VM e emitir o seguinte:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Você pode, em seguida, habilitar novamente o dispositivo no Gerenciador de dispositivos e sistema operacional do host será capaz de interagir com o dispositivo novamente.

## <a name="examples"></a>Exemplos

### <a name="mounting-a-gpu-to-a-vm"></a>Montar uma GPU em uma VM
Neste exemplo, usamos do PowerShell para configurar uma VM denominada "ddatest1" pegar primeiro GPU disponível pelo fabricante da NVIDIA e atribuí-lo na VM.  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```
