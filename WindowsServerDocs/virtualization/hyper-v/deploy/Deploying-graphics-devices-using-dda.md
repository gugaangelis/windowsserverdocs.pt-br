---
title: Implantar dispositivos gráficos usando a atribuição de dispositivo discreta
description: Saiba como usar o DDA para implantar dispositivos gráficos no Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 94ba561f35ea257a897f51cb3522196f7988eb71
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872103"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Implantar dispositivos gráficos usando a atribuição de dispositivo discreta

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

A partir do Windows Server 2016, você pode usar a atribuição de dispositivo discreta ou DDA para passar um dispositivo PCIe inteiro para uma VM.  Isso permitirá o acesso de alto desempenho a dispositivos como o [armazenamento da NVMe](./Deploying-storage-devices-using-dda.md) ou placas gráficas de dentro de uma VM, ao mesmo tempo em que pode aproveitar os drivers nativos dos dispositivos.  Visite o [plano para implantar dispositivos usando a atribuição de dispositivo discreto](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obter mais detalhes sobre quais dispositivos funcionam, quais são as possíveis implicações de segurança, etc.

Há três etapas para usar um dispositivo com atribuição de dispositivo discreta:
-   Configurar a VM para atribuição de dispositivo discreto
-   Desmontar o dispositivo da partição de host
-   Atribuindo o dispositivo à VM convidada

Todos os comandos podem ser executados no host em um console do Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar a VM para DDA
A atribuição de dispositivo discreta impõe algumas restrições às VMs e a etapa a seguir precisa ser executada.

1.  Configurar a "ação de parada automática" de uma VM para desligamento executando

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Uma preparação adicional de VM é necessária para dispositivos gráficos

Alguns hardwares têm melhor desempenho se a VM estiver configurada de uma determinada maneira.  Para obter detalhes sobre se você precisa ou não das seguintes configurações para seu hardware, entre em contato com o fornecedor do hardware. Detalhes adicionais podem ser encontrados em [planejar a implantação de dispositivos usando a atribuição de dispositivo discreta](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) e nesta [postagem de blog.](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. Habilitar a combinação de gravação na CPU
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurar o espaço MMIO de 32 bits
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurar o espaço MMIO de mais de 32 bits
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > Os valores de espaço MMIO acima são valores razoáveis a serem definidos para experimentar com uma única GPU.  Se depois de iniciar a VM, o dispositivo estiver relatando um erro relacionado a recursos insuficientes, você provavelmente precisará modificar esses valores. Consulte [planejar a implantação de dispositivos usando a atribuição de dispositivo discreta](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para saber como calcular precisamente os requisitos de MMIO.

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar o dispositivo da partição de host
### <a name="optional---install-the-partitioning-driver"></a>Opcional – instalar o driver de particionamento
A atribuição de dispositivos discretos fornece aos dispositivos de hardware a capacidade de fornecer um driver de mitigação de segurança com seus equipamentos.  Observe que esse driver não é o mesmo que o driver de dispositivo que será instalado na VM convidada.  Cabe à vontade do fornecedor de hardware fornecer esse driver, no entanto, se eles o fornecerem, instale-o antes de desmontar o dispositivo da partição de host.  Entre em contato com o fornecedor do hardware para obter mais informações sobre se eles têm um driver de mitigação
> Se nenhum driver de particionamento for fornecido, durante a desmontagem, você `-force` deverá usar a opção para ignorar o aviso de segurança. Leia mais sobre as implicações de segurança de fazer isso em [planejar a implantação de dispositivos usando a atribuição de dispositivo discreta](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Localizando o caminho do local do dispositivo
O caminho do local PCI é necessário para desmontar e montar o dispositivo do host.  Um caminho de local de exemplo é semelhante ao `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`seguinte:.  Para obter mais detalhes sobre o local, encontre o caminho de localização: [Planeje a implantação de dispositivos usando a atribuição de dispositivo discreta](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Desabilitar o dispositivo
Usando o Device Manager ou o PowerShell, verifique se o dispositivo está "desabilitado".  

### <a name="dismount-the-device"></a>Desmontar o dispositivo
Dependendo de se o fornecedor forneceu um driver de mitigação, você precisará usar a opção "-Force" ou não.
- Se um driver de mitigação tiver sido instalado
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Se um driver de mitigação não tiver sido instalado
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Atribuindo o dispositivo à VM convidada
A etapa final é informar ao Hyper-V que uma VM deve ter acesso ao dispositivo.  Além do caminho do local encontrado acima, você precisará saber o nome da VM.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Novidades
Depois que um dispositivo é montado com êxito em uma VM, agora você pode iniciar essa VM e interagir com o dispositivo como faria normalmente se estivesse executando em um sistema bare-metal.  Isso significa que agora você pode instalar os drivers do fornecedor de hardware na VM e os aplicativos poderão ver o hardware presente.  Você pode verificar isso abrindo o Gerenciador de dispositivos na VM convidada e vendo que o hardware agora aparece.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Removendo um dispositivo e retornando-o para o host
Se você quiser retornar o dispositivo de volta para seu estado original, será necessário interromper a VM e emitir o seguinte:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Você pode reabilitar o dispositivo no Gerenciador de dispositivos e o sistema operacional do host poderá interagir com o dispositivo novamente.

## <a name="examples"></a>Exemplos

### <a name="mounting-a-gpu-to-a-vm"></a>Montando uma GPU em uma VM
Neste exemplo, usamos o PowerShell para configurar uma VM chamada "ddatest1" para pegar a primeira GPU disponível pelo fabricante NVIDIA e atribuí-la à VM.  
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
