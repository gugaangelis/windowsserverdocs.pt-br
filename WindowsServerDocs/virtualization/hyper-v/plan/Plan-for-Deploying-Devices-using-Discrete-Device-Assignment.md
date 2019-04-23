---
title: Planejar a implantação de dispositivos usando a atribuição de dispositivo discretos
description: Saiba mais sobre como DDA funciona no Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840197"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Planejar a implantação de dispositivos usando atribuição de dispositivo discretos
>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

Atribuição de dispositivo discretos permite que o hardware físico de PCIe ser diretamente acessíveis de dentro de uma máquina virtual.  Este guia discutirá o tipo de dispositivos que podem usar a atribuição de dispositivo discretos, requisitos do sistema de host, limitações impostas em máquinas virtuais, bem como as implicações de segurança de atribuição de dispositivo discretos.

Para a versão inicial da atribuição de dispositivo discretos, nos concentramos em duas classes de dispositivo para formalmente suporte da Microsoft: Adaptadores de gráficos e armazenamento NVMe dispositivos.  Outros dispositivos são prováveis a operar e fornecedores de hardware são capazes de fornecer instruções de suporte para esses dispositivos.  Para esses dispositivos, entre em contato com os fornecedores de hardware para suporte.

Se você estiver pronto para experimentar a atribuição de dispositivo discretos, você pode pular para [Implantando gráficos dispositivos usando discretos atribuição de dispositivo](../deploy/Deploying-graphics-devices-using-dda.md) ou [Implantando dispositivos de armazenamento usando a atribuição de dispositivo discretos](../deploy/Deploying-storage-devices-using-dda.md) Para começar!

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Máquinas virtuais com suporte e sistemas operacionais convidados
Atribuição de dispositivo discretos tem suporte para a geração 1 ou 2 VMs.  Além disso, os convidados com suporte incluem Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 com [KB 3133690](https://support.microsoft.com/kb/3133690) aplicada e várias distribuições do [sistema operacional Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisitos do sistema
Além de [requisitos de sistema para o Windows Server](../../../get-started/System-Requirements--and-Installation.md) e o [requisitos de sistema do Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), atribuição de dispositivo discretos requer hardware de classe de servidor que é capaz de conceder o controle de sistema operacional sobre Configurando a malha de PCIe (PCI Express controle nativo). Além disso, a raiz de PCIe complexa tem que oferecer suporte a "Access Control Services" (ACS), que permite que o Hyper-V forçar todo o tráfego de PCIe pela MMU de e/s.

Esses recursos normalmente não são expostos diretamente no BIOS do servidor e geralmente ficam ocultos por trás de outras configurações.  Por exemplo, os mesmos recursos são necessários para suporte a SR-IOV e no BIOS você talvez precise definir "Habilitar SR-IOV".  Entre em contato com o fornecedor do sistema se não for possível identificar a configuração correta no seu BIOS.

Para ajudar a garantir que o hardware de hardware é capaz de atribuição de dispositivo discretos, nossos engenheiros desenvolveram uma [Script de perfil de computador](#machine-profile-script) que você pode executar em um host Hyper-V habilitado para testar se o seu servidor estiver corretamente configurado e o que os dispositivos são capazes de atribuição de dispositivo discretos.

## <a name="device-requirements"></a>Requisitos do dispositivo
Nem todo dispositivo PCIe pode ser usado com a atribuição de dispositivo discretos.  Por exemplo, não há suporte para dispositivos mais antigos que aproveitam a interrupções de PCI (INTx) herdados. De Jake Oshin [postagens de blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) vá em mais detalhes – no entanto, para o consumidor, executando o [Script de perfil de computador](#machine-profile-script) exibirá a quais dispositivos são capazes de que está sendo usado para atribuição de dispositivo discretos.

Os fabricantes de dispositivos podem em contato com seu representante da Microsoft para obter mais detalhes.

## <a name="device-driver"></a>Driver de dispositivo
Que a atribuição de dispositivo discretos passa todo o dispositivo PCIe na VM convidada, um driver de host não é necessário ser instalado antes do dispositivo que está sendo montado dentro da VM.  O único requisito no host é que o dispositivo [caminho do local de PCIe](#pcie-location-path) pode ser determinado.  Driver do dispositivo pode ser instalado opcionalmente se isso ajuda a identificar o dispositivo.  Por exemplo, uma GPU sem seu driver de dispositivo instalado no host pode aparecer como um dispositivo de renderização básica de Microsoft.  Se o driver de dispositivo estiver instalado, o fabricante e modelo provavelmente serão exibidas.

Depois que o dispositivo está montado no convidado, o driver de dispositivo do fabricante agora pode ser instalado como o normal na máquina virtual convidada.  

## <a name="virtual-machine-limitations"></a>Limitações de máquina virtual
Devido à natureza de como a atribuição de dispositivo discretos é implementada, alguns recursos de uma máquina virtual são restritos enquanto estiver anexado a um dispositivo.  Os seguintes recursos não estão disponíveis:
- Salvar/restaurar a VM
- Migração ao vivo de uma VM
- O uso de memória dinâmica
- Adicionar a VM a um cluster de alta disponibilidade (HA)

## <a name="security"></a>Segurança
Atribuição de dispositivo discretos passa todo o dispositivo para a VM.  Isso significa que todos os recursos do dispositivo são acessíveis a partir do sistema operacional convidado. Alguns recursos, como atualização de firmware, podem afetar negativamente a estabilidade do sistema. Dessa forma, vários avisos são apresentados para o administrador quando desmontando o dispositivo do host. É altamente recomendável que a atribuição de dispositivo discretos só é usada em que os locatários das VMs são confiáveis.  

Se o administrador deseja usar um dispositivo com um locatário não confiável, nós fornecemos os fabricantes de dispositivos com a capacidade de criar um driver de dispositivo de mitigação que pode ser instalado no host.  Entre em contato com o fabricante do dispositivo para obter detalhes sobre se eles fornecem um Driver de dispositivo de mitigação.

Se você quiser ignorar as verificações de segurança para um dispositivo que não tem um Driver de dispositivo de mitigação, será necessário passar o `-Force` parâmetro para o `Dismount-VMHostAssignableDevice` cmdlet.  Entender que ao fazer isso, você alterou o perfil de segurança desse sistema e isso só é recomendado durante a criação de protótipos ou confiável ambientes.

## <a name="pcie-location-path"></a>Caminho do local de PCIe
O caminho do local de PCIe é necessária para desmontar e montar o dispositivo a partir do Host.  Um exemplo de caminho de local é semelhante ao seguinte: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   O [Script de perfil de computador](#machine-profile-script) também retornará o caminho do local do dispositivo PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtendo o caminho do local usando o Gerenciador de dispositivos
![Gerenciador de Dispositivos](../deploy/media/dda-devicemanager.png)
- Abra o Gerenciador de dispositivo e localize o dispositivo.  
- Clique com botão direito no dispositivo e selecione "Propriedades".
- Navegue até a guia Detalhes e selecione "Caminhos de local" na lista suspensa propriedade.  
- Clique com botão direito na entrada que começa com "PCIROOT" e selecione "Copiar".  Agora você tem o caminho do local para o dispositivo.

## <a name="mmio-space"></a>MMIO espaço
Alguns dispositivos, especialmente GPUs, exigem espaço MMIO adicional a ser alocada para a máquina virtual para a memória do que o dispositivo seja acessível. Por padrão, cada VM começa com 128MB de espaço MMIO baixa e 512MB de espaço MMIO alto alocado para ela. No entanto, um dispositivo pode exigir mais espaço MMIO ou vários dispositivos podem ser passados por meio de modo que os requisitos de combinados excederem esses valores.  Alterar espaço MMIO é muito simples e pode ser executada no PowerShell usando os seguintes comandos:

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
A maneira mais fácil para determinar a quantidade de espaço para alocar MMIO é usar o [Script de perfil de computador](#machine-profile-script).  Como alternativa, você poderá calculá-lo usando o Gerenciador de dispositivos. Consulte o blog do TechNet lançar [atribuição de dispositivo discretos - GPUs](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/) para obter mais detalhes.

## <a name="machine-profile-script"></a>Script de perfil de computador
Para simplificar a identificar se o servidor está configurado corretamente e quais dispositivos estão disponíveis a serem passados por meio de atribuição de dispositivo discretos, um dos nossos engenheiros juntar o seguinte script do PowerShell: [SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Antes de usar o script, verifique se você tem a função Hyper-V instalada e execute o script em uma janela de comando do PowerShell que tenha privilégios de administrador.

Se o sistema estiver configurado incorretamente para dar suporte à atribuição de dispositivo discretos, a ferramenta exibirá uma mensagem de erro sobre o que é errado. Se a ferramenta encontrar o sistema configurado corretamente, ele irá enumerar todos os dispositivos que ele pode encontrar no barramento PCIe.

Para cada dispositivo que ele encontra, a ferramenta exibirá se ele é capaz de ser usado com a atribuição de dispositivo discretos. Se um dispositivo for identificado como sendo compatível com a atribuição de dispositivo discretos, o script fornecerá um motivo.  Quando um dispositivo com êxito é identificado como sendo compatível, o caminho do local do dispositivo será exibido.  Além disso, se exigir que o dispositivo [espaço MMIO](#mmio-space), ele será exibido assim.

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>Como a tecnologia de vGPU do RemoteFX do Desktop remoto se relacionam a atribuição de dispositivo discretos?
Eles são completamente separadas de tecnologias. VGPU do RemoteFX não precisa ser instalado para a atribuição de dispositivo discretos trabalhar. Além disso, não há funções adicionais são necessárias para ser instalado. VGPU do RemoteFX requer a função RDVH para ser instalado no pedido para o driver de vGPU do RemoteFX estar presente na VM. Para a atribuição de dispositivo discretos, como você irá instalar drivers de Hardware do fornecedor na máquina virtual, não há funções adicionais precisam estar presente.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>Eu passou uma GPU em uma VM, mas um aplicativo ou área de trabalho remota não está reconhecendo a GPU
Há vários motivos para que isso poderia acontecer, mas vários problemas comuns estão listados abaixo.
- Verifique se o driver do fornecedor GPU mais recente está instalado e não está relatando um erro ao verificar o estado do dispositivo no Gerenciador de dispositivos.
- Certifique-se de que o dispositivo tem suficiente [espaço MMIO](#mmio-space) alocado para ele dentro da VM.
- Verifique se que você estiver usando uma GPU fornecedor com suporte que está sendo usado nesta configuração. Por exemplo, alguns fornecedores de impedir que seus cartões de consumidor quando passado para uma VM de trabalho.
- Verifique se o aplicativo que está sendo executado dá suporte à execução dentro de uma VM, e que a GPU e seus drivers associados são compatíveis com o aplicativo. Alguns aplicativos têm listas de permissões de GPUs e ambientes.
- Se você estiver usando a função de Host de sessão de área de trabalho remota ou o Windows Multipoint Services na convidada, você precisará garantir que uma entrada específica da política de grupo é definida para permitir o uso do padrão GPU. Usando um objeto de diretiva de grupo aplicada para o convidado (ou o Editor de diretiva de Grupo Local na convidada), navegue até o item de diretiva de grupo a seguir:
   - Configuração do Computador
   - Modelos do administrador
   - Componentes do Windows
   - Serviços da Área de Trabalho Remota
   - Host de Sessão de Área de Trabalho Remota
   - Ambiente de sessão remota
   - Usar o adaptador de gráfico de padrão de hardware para todas as sessões de serviços de área de trabalho remota

    Definir esse valor como habilitado e, em seguida, reinicializar a VM depois que a política foi aplicada.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>Atribuição de dispositivo discretos se aproveite de codec de AVC444 do área de trabalho remota?
Sim, visite esta postagem de blog para obter mais informações: [Melhorias de AVC/H.264 de protocolo RDP (Desktop) 10 remotas no Windows 10 e Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>Pode usar o PowerShell para obter o caminho do local?
Sim, há várias maneiras de fazer isso. Aqui está um exemplo:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>Atribuição de dispositivo discretos pode ser usada para passar um dispositivo USB para uma VM?
Embora não seja oficialmente suportados, nossos clientes usaram a atribuição de dispositivo discretos para fazer isso, passando o USB3 controlador inteiro em uma VM.  O controlador de inteiro que está sendo passado, cada dispositivo USB conectado ao controlador em questão também estarão acessível na VM.  Observe que apenas alguns controladores USB3 podem funcionar e USB2 controladores não podem ser usados com a atribuição de dispositivo discretos.
