---
title: RDS - RemoteFX vGPU instalação e configuração
description: Informações de planejamento para configurar a virtualização de gráficos de vGPU do RemoteFX.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876837"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Instalar e configurar vGPU RemoteFX para os Serviços de Área de Trabalho Remota


O recurso de vGPU do RemoteFX torna possível para várias máquinas virtuais compartilhar um adaptador de gráficos físicos. As máquinas virtuais são capazes de descarregamento de processamento de informações gráficas do processador para o adaptador gráfico dedicado. Isso irá diminuir a carga da CPU e melhorar a escalabilidade para gráficas intensas cargas de trabalho que são executados nas máquinas virtuais do VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisitos de vGPU do RemoteFX

Requisitos para sistemas host: 

- Windows Server 2016 ou Windows 10
- 11.0 DX de GPU compatível com o driver de compatibilidade do WDDM 1.2 
- Função de Host de virtualização de área de trabalho remota do Windows Server habilitada (habilita a função Hyper-V) 
- Servidor com uma CPU com suporte para SLAT (conversão de endereços de segundo nível) 

Requisitos de VM convidada:

- VM convidada com um cliente empresarial do Windows (Windows 7 com Service Pack 1, Windows 8.1, Windows 10) ou o Windows Server (Windows Server 2012 R2 ou Windows Server 2016). Para OS adicionais de suporte veja [configuração com suporte para os serviços de área de trabalho remota](rds-supported-config.md).

Considerações adicionais para VMs convidadas:

- OpenGL e OpenCL funcionalidade só está disponível no Windows 10 ou Windows Server 2016.  
- 11.0 do DirectX só está disponível com o Windows 8 ou as VMs convidadas mais recentes. 
- Host de sessão de área de trabalho remota só é compatível com vGPU do RemoteFX se ele é executado como um [área de trabalho de sessão pessoal](rds-personal-session-desktops.md).

Para VMS convidadas, certifique-se de examinar [implantação de VDI – sistemas operacionais](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Install RemoteFX vGPU

Use as etapas a seguir para instalar e configurar o RemoteFX no host para o Windows Server 2016 e Windows 10:

1. Instalar o sistema operacional.
2. Instale o mais recente Windows 10/Windows drivers GPU do Server 2016 disponíveis do site de fornecedor de cartão de gráficos.
3. Instale o vGPU do RemoteFX no host do Windows 10/Windows Server 2016:
   1. Em um host do Windows 10, habilite o recurso do Hyper-V no painel de controle (vá para painel de controle/programas e recursos/Turn Windows recursos ativado ou desativado):

      ![Janela de recursos do Windows para habilitar o recurso Hyper-V](media/rds-hyperv-settings.png)

   2. Em um host do Windows Server 2016, instale a função de Host de virtualização de área de trabalho remota (RDVH).
   

4. Agora, crie e configure uma VM convidada:
   1. Crie uma VM com Windows 10 Enterprise ou Windows Server 2016.
   2. Adicione o adaptador de gráficos 3D RemoteFX. Ver [configurar o adaptador 3D do RemoteFX vGPU](#configure-the-remotefx-vgpu-3d-adapter) para obter informações sobre como fazer isso com os cmdlets do Gerenciador do Hyper-V ou o PowerShell. 

VGPU do RemoteFX usará todas as GPUs quando há mais de uma disponível. No entanto, em alguns casos, que você talvez queira limitar quais GPUs são usados pelo RemoteFX. No ambiente do Hyper-V, você controlar isso selecionando especificamente qual devem GPUs *não* ser usado pelo RemoteFX. Use as etapas a seguir: 

   1. Navegue até as configurações de Hyper-V no Gerenciador do Hyper-V.
   2. Clique em **GPUs físicos** nas configurações do Hyper-V.
   3. Selecione a GPU que você não deseja usar e, em seguida, desmarque **usar esse GPU com RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurar o adaptador 3D de vGPU do RemoteFX
Você pode usar a interface do usuário do Hyper-V Manager ou o PowerShell cmdlets para configurar o adaptador de gráficos 3D de vGPU do RemoteFX. 

#### <a name="through-hyper-v-manager"></a>Por meio do Gerenciador do Hyper-V:

1. Verifique se o sistema foi configurado com o Hyper-V e tem uma VM configurada.  
2. Pare a VM, se ele está em execução. 
3. No Gerenciador do Hyper-V, navegue até a **as configurações da VM**e, em seguida, clique em **Adicionar Hardware**.
4. Selecione **adaptador de elementos gráficos 3D RemoteFX**e clique em **Add**. 
5. Definir o número máximo de monitores, a resolução máxima do monitor e a memória de vídeo dedicada, ou deixe os valores padrão.

   > [!NOTE]
   > - Definir valores mais altos para qualquer uma dessas opções terá impacto para dimensionar, portanto, você só deve definir o que é absolutamente necessário.
   >
   > - Quando você precisar usar 1GB de VRAM dedicado, use uma VM convidada de 64 bits em vez de 32 bits (x86) para obter melhores resultados.
6. Clique em **Okey** para concluir a configuração.

#### <a name="with-powershell-cmdlets"></a>Com os cmdlets do PowerShell:

Execute os seguintes cmdlets para adicionar, revisar e configurar o adaptador: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obter detalhes, consulte [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Para obter detalhes, consulte [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obter detalhes, consulte [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Execute o seguinte cmdlet para examinar as GPUs físicas:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Para obter detalhes, consulte [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Monitorar o desempenho

O desempenho e dimensionamento de um sistema VDI são determinados por uma variedade de fatores como a memória total da GPU, quantidade de memória do sistema e a velocidade da memória, número de núcleos de CPU e a frequência do relógio da CPU, a velocidade de armazenamento e a implementação de NUMA.

Suporte a vGPU remoto no RDS inclui os seguintes contadores de desempenho, que podem ser exibidos no Monitor de desempenho (perfmon.exe) para coletar informações sobre a taxa de transferência de taxa de quadros.

- Gráficos do RemoteFX - contadores para compactação de elementos gráficos do protocolo de área de trabalho remota. Por exemplo, se você quiser examinar o número de quadros que está sendo apresentado ao RDP para compactação, examine os **Frames/Second entrada** contador.
- Rede do RemoteFX - contadores para o tráfego de rede do protocolo de área de trabalho remota. Por exemplo, **tempo de ida e volta (RTT)**.
- Gerenciamento de GPU do RemoteFX Root - medidas VRAM reservada e disponível.
- Software do RemoteFX - fornece contadores para taxa de captura, o tempo de resposta GPU e outros.

Para um nível mais baixo do monitoramento de desempenho, principalmente para solução de problemas, você pode usar os seguintes contadores de desempenho adicionais:

- Dispositivo de VM VSC RemoteFX Synth3D 
- Canal de transporte do RemoteFX Synth3D VM VSC 
- VSP RemoteFX Synth3D 
- Dispositivo de VM VSP RemoteFX Synth3D 
- Canal de VM VSP RemoteFX Synth3D
