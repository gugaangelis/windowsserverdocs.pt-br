---
title: Implantar dispositivos gráficos usando vGPU RemoteFX
description: Saiba como implantar e configurar vGPU RemoteFX no Windows Server
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7111899557279d825191948e737d83d7467ce786
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923908"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>Implantar dispositivos gráficos usando vGPU RemoteFX

> Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016

O recurso vGPU para o RemoteFX possibilita que várias máquinas virtuais compartilhem uma GPU física. Os recursos de renderização e de computação são compartilhados dinamicamente entre máquinas virtuais, tornando o RemoteFX vGPU apropriado para cargas de trabalho de alta intermitência em que os recursos de GPU dedicados não são necessários. Por exemplo, em um serviço de VDI, o vGPU RemoteFX pode ser usado para descarregar os custos de renderização de aplicativo para a GPU, com o efeito de diminuir a carga da CPU e melhorar a escalabilidade do serviço.

## <a name="remotefx-vgpu-requirements"></a>Requisitos de vGPU do RemoteFX

Requisitos do sistema de host:

- Windows Server 2016
- Uma GPU compatível com DirectX 11,0 com um driver compatível com WDDM 1,2
- Uma CPU com suporte a SLAT (conversão de endereços de segundo nível)

Requisitos da VM convidada:

- SO convidado com suporte. Para obter mais informações, consulte [suporte a vGPU (adaptador de vídeo 3D) do RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).

Considerações adicionais para VMs convidadas:

- A funcionalidade OpenGL e OpenCL só está disponível em convidados que executam o Windows 10 ou o Windows Server 2016.  
- O DirectX 11,0 só está disponível para convidados que executam o Windows 8 ou posterior.

## <a name="enable-remotefx-vgpu"></a>Habilitar vGPU do RemoteFX

Para configurar o vGPU RemoteFX no host do Windows Server 2016:

1. Instale os drivers gráficos recomendados pelo seu fornecedor de GPU para o Windows Server 2016.
2. Crie uma VM que esteja executando um SO convidado com suporte pelo vGPU do RemoteFX. Para saber mais, consulte [suporte a vGPU (adaptador de vídeo 3D) do RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).
3. Adicione o adaptador gráfico 3D RemoteFX à VM. Para saber mais, confira [Configurar o adaptador 3D VGPU RemoteFX](#configure-the-remotefx-vgpu-3d-adapter).

Por padrão, vGPU RemoteFX usará todas as GPUs disponíveis e com suporte. Para limitar quais GPUs o RemoteFX vGPU usa, siga estas etapas:

1. Navegue até as configurações do Hyper-V no Gerenciador do Hyper-V.
2. Selecione **GPUs físicas** nas configurações do Hyper-V.
3. Selecione a GPU que você não deseja usar e, em seguida, desmarque **Usar esta GPU com RemoteFX**.

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurar o adaptador 3D do vGPU do RemoteFX

Você pode usar a interface do usuário do Gerenciador do Hyper-V ou cmdlets do PowerShell para configurar a placa gráfica 3D do vGPU do RemoteFX.

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>Configurar vGPU RemoteFX com o Gerenciador do Hyper-V

1. Pare a VM se ela estiver em execução no momento.
2. Abra o Gerenciador do Hyper-V, navegue até **configurações da VM**e, em seguida, selecione **adicionar hardware**.
3. Selecione **adaptador gráfico 3D RemoteFX**e, em seguida, selecione **Adicionar**.
4. Defina o número máximo de monitores, a resolução máxima do monitor e a memória de vídeo dedicada ou deixe os valores padrão.

   > [!NOTE]
   > - A definição de valores mais altos para qualquer uma dessas opções afetará a escala do serviço, portanto, você deve definir apenas o que é necessário.
   > - Quando você precisar usar 1 GB de VRAM dedicado, use uma VM de convidado de 64 bits em vez de 32 bits (x86) para obter melhores resultados.

5. Selecione **OK** para concluir a configuração.

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>Configurar vGPU RemoteFX com cmdlets do PowerShell

Use os seguintes cmdlets do PowerShell para adicionar, revisar e configurar o adaptador:

- [Add-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/add-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefx3dvideoadapter?view=win10-ps)
- [Set-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/set-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFXPhysicalVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter?view=win10-ps)

## <a name="monitor-performance"></a>Monitorar o desempenho

O desempenho e a escala de um serviço habilitado para vGPU do RemoteFX são determinados por uma variedade de fatores, como o número de GPUs em seu sistema, memória da GPU total, quantidade de memória do sistema e velocidade da memória, número de núcleos de CPU e frequência de relógio de CPU, velocidade de armazenamento e NUMA implementação.

### <a name="host-system-memory"></a>Memória do sistema de host

Para cada VM habilitada com uma vGPU, o RemoteFX usa a memória do sistema no sistema operacional convidado e no servidor host. O hipervisor garante a disponibilidade da memória do sistema para um sistema operacional convidado. No host, cada área de trabalho virtual habilitada para vGPU precisa anunciar seu requisito de memória do sistema para o hipervisor. Quando a área de trabalho virtual habilitada para vGPU é iniciada, o hipervisor reserva memória de sistema adicional no host.

O requisito de memória para o servidor habilitado para RemoteFX é dinâmico porque a quantidade de memória consumida no servidor habilitado para RemoteFX depende do número de monitores associados às áreas de trabalho virtuais habilitadas para vGPU e à resolução máxima de esses monitores.

### <a name="host-gpu-video-memory"></a>Memória de vídeo da GPU do host

Cada área de trabalho virtual habilitada para vGPU usa a memória de vídeo de hardware de GPU no servidor de host para renderizar a área de trabalho. Além disso, um codec usa a memória de vídeo para compactar a tela processada. A quantidade de memória necessária para renderização e compactação é diretamente baseada no número de monitores provisionados para a máquina virtual. A quantidade de memória de vídeo reservada varia de acordo com a resolução da tela do sistema e quantos monitores existem. Alguns usuários exigem uma resolução de tela mais alta para tarefas específicas, mas há maior escalabilidade com configurações de resolução mais baixa se todas as outras configurações permanecerem constantes.

### <a name="host-cpu"></a>CPU do host

O hipervisor agenda o host e as VMs na CPU. A sobrecarga é aumentada em um host habilitado para RemoteFX porque o sistema executa um processo adicional (rdvgm. exe) por área de trabalho virtual habilitada para vGPU. Esse processo usa o driver de dispositivo de gráficos para executar comandos na GPU. O codec também usa a CPU para compactar dados de tela que precisam ser enviados de volta ao cliente.

Mais processadores virtuais significam uma melhor experiência do usuário. É recomendável alocar pelo menos duas CPUs virtuais por área de trabalho virtual habilitada para vGPU. Também é recomendável usar a arquitetura x64 para áreas de trabalho virtuais habilitadas para vGPU porque o desempenho em máquinas virtuais x64 é melhor em comparação com as máquinas virtuais x86.

### <a name="gpu-processing-power"></a>Capacidade de processamento de GPU

Cada área de trabalho virtual habilitada para vGPU tem um processo do DirectX correspondente que é executado no servidor host. Esse processo repete todos os comandos gráficos que ele recebe da área de trabalho virtual do RemoteFX para a GPU física. Isso é como executar vários aplicativos do DirectX ao mesmo tempo na mesma GPU física.

Normalmente, os drivers e dispositivos gráficos são ajustados para executar apenas alguns aplicativos na área de trabalho de cada vez, mas o RemoteFX amplia as GPUs para ficar ainda mais. vGPUs vêm com contadores de desempenho que medem a resposta de GPU para solicitações do RemoteFX e ajudam a garantir que as GPUs não sejam esticadas muito longe.

Quando uma GPU está com poucos recursos, as operações de leitura e gravação levam muito tempo para serem concluídas. Os administradores podem usar contadores de desempenho para saber quando ajustar recursos e evitar tempo de inatividade para os usuários.

Saiba mais sobre contadores de desempenho para monitorar o comportamento de vGPU do RemoteFX em [diagnosticar problemas de desempenho de gráficos no área de trabalho remota](https://docs.microsoft.com/azure/virtual-desktop/remotefx-graphics-performance-counters).
