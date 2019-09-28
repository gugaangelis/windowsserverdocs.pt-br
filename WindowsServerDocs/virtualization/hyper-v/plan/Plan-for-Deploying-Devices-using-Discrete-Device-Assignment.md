---
title: Planejar a implantação de dispositivos usando a atribuição de dispositivo discreta
description: Saiba mais sobre como o DDA funciona no Windows Server
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: 7084f4951ebe1d1203f4c9e45bc5f73cc6487a84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364187"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Planejar a implantação de dispositivos usando a atribuição de dispositivo discreta
>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

A atribuição de dispositivo discreta permite que o hardware PCIe físico seja diretamente acessível de dentro de uma máquina virtual.  Este guia discutirá o tipo de dispositivos que podem usar a atribuição de dispositivo discreta, requisitos de sistema de host, limitações impostas nas máquinas virtuais, bem como as implicações de segurança da atribuição de dispositivo discreta.

Para a versão inicial da atribuição de dispositivo discreta, nos concentramos em duas classes de dispositivo para serem formalmente suportadas pela Microsoft: Adaptadores gráficos e dispositivos de armazenamento NVMe.  Outros dispositivos provavelmente funcionarão e os fornecedores de hardware poderão oferecer instruções de suporte para esses dispositivos.  Para esses outros dispositivos, entre em contato com os fornecedores de hardware para obter suporte.

Se você estiver pronto para experimentar uma atribuição de dispositivo discreta, poderá pular para a [implantação de dispositivos gráficos usando a atribuição de dispositivo discreta](../deploy/Deploying-graphics-devices-using-dda.md) ou [implantando dispositivos de armazenamento usando a atribuição de dispositivo discreta](../deploy/Deploying-storage-devices-using-dda.md) para começar!

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Máquinas virtuais com suporte e sistemas operacionais convidados
Há suporte para a atribuição de dispositivo discreto para VMs de geração 1 ou 2.  Além disso, os convidados com suporte incluem Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012r2 com [KB 3133690](https://support.microsoft.com/kb/3133690) aplicado e várias distribuições do [sistema operacional Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisitos de sistema
Além dos requisitos do [sistema para o Windows Server](../../../get-started/System-Requirements--and-Installation.md) e os [requisitos do sistema para o Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), a atribuição de dispositivo discreta requer hardware de classe de servidor capaz de conceder o controle do sistema operacional sobre a configuração do PCIe Fabric (controle nativo de PCI Express). Além disso, a raiz do PCIe complexa tem que dar suporte ao "ACS (Access Control Services"), que habilita o Hyper-V a forçar todo o tráfego de PCIe por meio do MMU de e/s.

Esses recursos geralmente não são expostos diretamente no BIOS do servidor e, em geral, são ocultos por trás de outras configurações.  Por exemplo, os mesmos recursos são necessários para o suporte a SR-IOV e, no BIOS, talvez seja necessário definir "Habilitar SR-IOV".  Entre em contato com o fornecedor do sistema se não for possível identificar a configuração correta em seu BIOS.

Para ajudar a garantir o hardware que o hardware é capaz de atribuir uma atribuição de dispositivo discreta, nossos engenheiros criaram um [script de perfil de computador](#machine-profile-script) que você pode executar em um host habilitado para Hyper-V para testar se o servidor está configurado corretamente e quais dispositivos são capazes de Atribuição de dispositivo discreta.

## <a name="device-requirements"></a>Requisitos do dispositivo
Nem todo dispositivo PCIe pode ser usado com atribuição de dispositivo discreta.  Por exemplo, dispositivos mais antigos que utilizam interrupções de PCI herdadas (INTx) não têm suporte. As [Postagens de blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) de Jake Oshin entram em mais detalhes – no entanto, para o consumidor, a execução do [script de perfil do computador](#machine-profile-script) exibirá quais dispositivos são capazes de serem usados para atribuição de dispositivo discreta.

Os fabricantes de dispositivos podem entrar em contato com o representante da Microsoft para obter mais detalhes.

## <a name="device-driver"></a>Driver de dispositivo
À medida que a atribuição de dispositivo discreta passa o dispositivo PCIe inteiro para a VM convidada, um driver de host não precisa ser instalado antes do dispositivo que está sendo montado na VM.  O único requisito no host é que o [caminho do local do PCIe](#pcie-location-path) do dispositivo possa ser determinado.  O driver do dispositivo pode, opcionalmente, ser instalado se isso ajudar a identificar o dispositivo.  Por exemplo, uma GPU sem seu driver de dispositivo instalado no host pode aparecer como um dispositivo de renderização básico da Microsoft.  Se o driver de dispositivo estiver instalado, seu fabricante e modelo provavelmente serão exibidos.

Depois que o dispositivo é montado dentro do convidado, o driver de dispositivo do fabricante agora pode ser instalado como normal dentro da máquina virtual convidada.  

## <a name="virtual-machine-limitations"></a>Limitações da máquina virtual
Devido à natureza de como a atribuição de dispositivo discreta é implementada, alguns recursos de uma máquina virtual são restritos enquanto um dispositivo é anexado.  Os seguintes recursos não estão disponíveis:
- Salvar/restaurar VM
- Migração dinâmica de uma VM
- O uso de memória dinâmica
- Adicionando a VM a um cluster de alta disponibilidade (HA)

## <a name="security"></a>Segurança
A atribuição de dispositivo discreta passa o dispositivo inteiro para a VM.  Isso significa que todos os recursos desse dispositivo podem ser acessados do sistema operacional convidado. Alguns recursos, como a atualização de firmware, podem afetar negativamente a estabilidade do sistema. Assim, vários avisos são apresentados ao administrador ao desmontar o dispositivo do host. É altamente recomendável que a atribuição de dispositivo discreta seja usada apenas onde os locatários das VMs são confiáveis.  

Se o administrador quiser usar um dispositivo com um locatário não confiável, fornecemos fabricantes de dispositivos com a capacidade de criar um driver de mitigação de dispositivo que pode ser instalado no host.  Entre em contato com o fabricante do dispositivo para obter detalhes sobre se eles fornecem um driver de mitigação de dispositivo.

Se você quiser ignorar as verificações de segurança de um dispositivo que não tem um driver de mitigação de dispositivo, será necessário passar o parâmetro `-Force` para o cmdlet `Dismount-VMHostAssignableDevice`.  Entenda que, ao fazer isso, você alterou o perfil de segurança desse sistema e isso é recomendado apenas durante o protótipo ou ambientes confiáveis.

## <a name="pcie-location-path"></a>Caminho do local do PCIe
O caminho do local do PCIe é necessário para desmontar e montar o dispositivo do host.  Um caminho de local de exemplo é semelhante ao `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`seguinte:.   O [script de perfil do computador](#machine-profile-script) também retornará o caminho do local do dispositivo PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtendo o caminho do local usando Device Manager
![Gerenciador de Dispositivos](../deploy/media/dda-devicemanager.png)
- Abra Device Manager e localize o dispositivo.  
- Clique com o botão direito do mouse no dispositivo e selecione "Propriedades".
- Navegue até a guia detalhes e selecione "caminhos de localização" na lista suspensa propriedade.  
- Clique com o botão direito do mouse na entrada que começa com "PCIROOT" e selecione "copiar".  Agora você tem o caminho do local para esse dispositivo.

## <a name="mmio-space"></a>Espaço MMIO
Alguns dispositivos, especialmente GPUs, exigem espaço MMIO adicional para serem alocados para a VM para que a memória desse dispositivo seja acessível. Por padrão, cada VM começa com 128 MB de espaço de MMIO baixo e 512 MB de espaço de MMIO alto alocado para ele. No entanto, um dispositivo pode exigir mais espaço MMIO, ou vários dispositivos podem ser passados de tal forma que os requisitos combinados excedam esses valores.  A alteração do espaço MMIO é direta e pode ser executada no PowerShell usando os seguintes comandos:

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

A maneira mais fácil de determinar a quantidade de espaço MMIO a ser alocada é usar o [script de perfil de computador](#machine-profile-script). Para baixar e executar o script de perfil de computador, execute os seguintes comandos em um console do PowerShell:

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

Para dispositivos que podem ser atribuídos, o script exibirá os requisitos de MMIO de um determinado dispositivo, como o exemplo abaixo:

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

O espaço MMIO baixo é usado somente por sistemas operacionais de 32 bits e dispositivos que usam endereços de 32 bits. Na maioria das circunstâncias, definir o espaço MMIO alto de uma VM será suficiente, uma vez que as configurações de 32 bits não são muito comuns.

> [!IMPORTANT]
> Ao atribuir o espaço de MMIO a uma VM, o usuário precisa ter certeza de definir o espaço de MMIO para a soma do espaço de MMIO solicitado para todos os dispositivos atribuídos desejados mais um buffer adicional se houver outros dispositivos virtuais que exigem alguns MB de espaço de MMIO. Use os valores de MMIO padrão descritos acima como buffer para baixo e alto MMIO (128 MB e 512 MB, respectivamente).

Se um usuário fosse atribuir uma única GPU K520 como no exemplo acima, ele deve definir o espaço MMIO da VM para o valor de saída pelo script de perfil do computador mais um buffer--176 MB + 512 MB. Se um usuário fosse atribuir três GPUs K520, ele deve definir o espaço MMIO para três vezes 176 MB mais um buffer ou 528 MB + 512 MB.

Para obter uma visão mais detalhada do espaço de MMIO, confira [GPUs de atribuição de dispositivo discretos](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) no blog do TechCommunity.

## <a name="machine-profile-script"></a>Script de perfil de computador
Para simplificar a identificação se o servidor está configurado corretamente e quais dispositivos estão disponíveis para serem transmitidos usando a atribuição de dispositivo discreta, um de nossos engenheiros reúne o seguinte script do PowerShell: [SurveyDDA. ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Antes de usar o script, verifique se você tem a função Hyper-V instalada e execute o script em uma janela de comando do PowerShell que tenha privilégios de administrador.

Se o sistema estiver configurado incorretamente para dar suporte à atribuição de dispositivo discreta, a ferramenta exibirá uma mensagem de erro como o que está errado. Se a ferramenta encontrar o sistema configurado corretamente, ele irá enumerar todos os dispositivos que ele pode encontrar no barramento do PCIe.

Para cada dispositivo localizado, a ferramenta exibirá se é possível usá-la com a atribuição de dispositivo discreta. Se um dispositivo for identificado como compatível com a atribuição de dispositivo discreta, o script fornecerá um motivo.  Quando um dispositivo for identificado com êxito como compatível, o caminho do local do dispositivo será exibido.  Além disso, se esse dispositivo exigir [espaço MMIO](#mmio-space), ele também será exibido.

![SurveyDDA. ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>Como a tecnologia vGPU do RemoteFX do Área de Trabalho Remota está relacionada à atribuição de dispositivo discreta?
Eles são tecnologias completamente separadas. O vGPU RemoteFX não precisa ser instalado para que a atribuição de dispositivo discreta funcione. Além disso, nenhuma função adicional precisa ser instalada. VGPU RemoteFX requer que a função RDVH seja instalada para que o driver vGPU do RemoteFX esteja presente na VM. Para a atribuição de dispositivo discreta, como você instalará o driver do fornecedor de hardware na máquina virtual, nenhuma função adicional precisará estar presente.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>Passei uma GPU em uma VM, mas Área de Trabalho Remota ou um aplicativo não está reconhecendo a GPU
Há várias razões que isso pode ocorrer, mas vários problemas comuns estão listados abaixo.
- Verifique se o driver mais recente do fornecedor de GPU está instalado e não está relatando um erro verificando o estado do dispositivo no Device Manager.
- Verifique se o dispositivo tem [espaço MMIO](#mmio-space) suficiente alocado para ele na VM.
- Verifique se você está usando uma GPU à qual o fornecedor oferece suporte para uso nessa configuração. Por exemplo, alguns fornecedores impedem que seus cartões de consumidor funcionem quando passados para uma VM.
- Verifique se o aplicativo que está sendo executado dá suporte à execução dentro de uma VM e se a GPU e seus drivers associados têm suporte no aplicativo. Alguns aplicativos têm as listas de permissões de GPUs e ambientes.
- Se você estiver usando a função de Host da Sessão da Área de Trabalho Remota ou os serviços do Windows MultiPoint no convidado, será necessário garantir que uma entrada de Política de Grupo específica seja definida para permitir o uso da GPU padrão. Usando um objeto Política de Grupo aplicado ao convidado (ou ao Editor de Política de Grupo Local no convidado), navegue até o seguinte Política de Grupo item:
   - Configuração do Computador
   - Modelos de administrador
   - Componentes do Windows
   - Serviços da Área de Trabalho Remota
   - Host de Sessão de Área de Trabalho Remota
   - Ambiente de sessão remota
   - Usar o adaptador gráfico de hardware padrão para todas as sessões de Serviços de Área de Trabalho Remota

    Defina esse valor como habilitado e reinicialize a VM depois que a política for aplicada.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>A atribuição de dispositivo discreta pode tirar proveito do codec AVC444 do Área de Trabalho Remota?
Sim, visite esta postagem no blog para obter mais informações: [O protocolo RDP (RDP) 10 AVC/H. 264 aprimoramentos no Windows 10 e no Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>Posso usar o PowerShell para obter o caminho do local?
Sim, há várias maneiras de fazer isso. Aqui está um exemplo:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>A atribuição de dispositivo discreta pode ser usada para passar um dispositivo USB para uma VM?
Embora não tenha suporte oficialmente, nossos clientes usaram uma atribuição de dispositivo discreta para fazer isso passando o controlador USB3 inteiro para uma VM.  Como o controlador inteiro está sendo passado, cada dispositivo USB conectado ao controlador também estará acessível na VM.  Observe que apenas alguns controladores USB3 podem funcionar e os controladores USB2 não podem ser usados com a atribuição de dispositivo discreta.
