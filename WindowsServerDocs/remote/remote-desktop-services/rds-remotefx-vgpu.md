---
title: RDS – instalação e configuração de vGPU do RemoteFX
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712132"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Instalar e configurar o vGPU do RemoteFX para os Serviços de Área de Trabalho Remota


O recurso vGPU do RemoteFX possibilita que várias máquinas virtuais compartilhem um adaptador de gráficos físico. As máquinas virtuais são capazes de descarregar a renderização de informações gráficas do processador no adaptador gráfico dedicado. Isso diminuirá a carga da CPU e melhorará a escalabilidade de cargas de trabalho gráficas de uso intensivo de recursos que são executadas nas máquinas virtuais da VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisitos de vGPU do RemoteFX

Requisitos para sistemas host: 

- Windows Server 2016 ou Windows 10
- GPU compatível com DX 11.0 com o driver compatível com WDDM 1.2 
- Função de Host de Virtualização de Área de Trabalho Remota do Windows Server habilitada (habilita a função do Hyper-V) 
- Servidor com uma CPU compatível com SLAT (Conversão de Endereços de Segundo Nível) 

Requisitos da VM convidada:

- VM convidada executando um cliente do Windows Enterprise (Windows 7 com Service Pack 1, Windows 8.1, Windows 10) ou o Windows Server (Windows Server 2012 R2 ou Windows Server 2016). Para mais suporte ao SO, veja [Configuração compatível com Serviços de Área de Trabalho Remota](rds-supported-config.md).

Considerações adicionais para VMs convidadas:

- As funcionalidades OpenGL e OpenCL só estão disponíveis no Windows 10 ou Windows Server 2016.  
- O DirectX 11.0 só está disponível com o Windows 8 ou VMs convidadas mais recentes. 
- O Host da Sessão da Área de Trabalho Remota só será compatível com vGPU do RemoteFX se ele estiver executando como uma [área de trabalho de sessão pessoal](rds-personal-session-desktops.md).

Para VMs convidadas, examine [Implantação de VDI – sistemas operacionais convidados compatíveis](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Instalar o vGPU do RemoteFX

Use as etapas a seguir para instalar e configurar o RemoteFX no host para o Windows Server 2016 e Windows 10:

1. Instalar o sistema operacional.
2. Instale os drivers mais recentes do Windows 10/Windows Server 2016 disponíveis no site do fornecedor da placa gráfica.
3. Instale o vGPU do RemoteFX no host do Windows 10/Windows Server 2016:
   1. Em um host do Windows 10, habilite o recurso Hyper-V no Painel de Controle (acesse Painel de Controle/Programas e Recursos/Ligar ou desligar recursos do Windows):

      ![Janela Recursos do Windows para habilitar o recurso Hyper-V](media/rds-hyperv-settings.png)

   2. Em um host do Windows Server 2016, instale a função RDVH (Host de Virtualização de Área de Trabalho Remota).
   

4. Agora, crie e configure uma VM convidada:
   1. Crie uma VM com Windows 10 Enterprise ou Windows Server 2016.
   2. Adicione a placa gráfica 3D RemoteFX. Consulte [Configurar o adaptador 3D do vGPU do RemoteFX](#configure-the-remotefx-vgpu-3d-adapter) para obter informações sobre como fazer isso com o Gerenciador do Hyper-V ou os cmdlets do PowerShell. 

O vGPU do RemoteFX usará todas as GPUs quando houver mais de uma disponível. No entanto, em alguns casos pode ser interessante limitar quais GPUs são usadas pelo RemoteFX. No ambiente do Hyper-V, você controla isso selecionando especificamente quais GPUs *não* devem ser usadas pelo RemoteFX. Use as etapas a seguir: 

   1. Navegue até as configurações do Hyper-V no Gerenciador do Hyper-V.
   2. Clique em **GPUs Físicas** nas Configurações de Hyper-V.
   3. Selecione a GPU que você não deseja usar e, em seguida, desmarque **Usar esta GPU com RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurar o adaptador 3D do vGPU do RemoteFX
Você pode usar a interface do usuário do Gerenciador do Hyper-V ou cmdlets do PowerShell para configurar a placa gráfica 3D do vGPU do RemoteFX. 

#### <a name="through-hyper-v-manager"></a>Usando o Gerenciador do Hyper-V:

1. Verifique se o sistema foi configurado com o Hyper-V e tem uma VM configurada.  
2. Pare a VM, se ela estiver em execução. 
3. No Gerenciador do Hyper-V, navegue até **Configurações da VM** e, em seguida, clique em **Adicionar Hardware**.
4. Selecione **Placa Gráfica 3D do RemoteFX** e clique em **Adicionar**. 
5. Defina o número máximo de monitores, a resolução máxima do monitor e a memória de vídeo dedicada ou deixe os valores padrão.

   > [!NOTE]
   > - A definição de valores mais altos para qualquer uma dessas opções terá impacto na escala, portanto, você só deverá definir o que for absolutamente necessário.
   >
   > - Quando você precisar usar 1GB de VRAM dedicada, use uma VM convidada de 64 bits em vez de 32 bits (x86) para obter melhores resultados.
6. Clique em **OK** para concluir a configuração.

#### <a name="with-powershell-cmdlets"></a>Com cmdlets do PowerShell:

Execute os seguintes cmdlets para adicionar, analisar e configurar o adaptador: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obter detalhes, confira [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Para obter detalhes, confira [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Para obter detalhes, confira [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Execute o seguinte cmdlet para examinar as GPUs físicas:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Para obter detalhes, confira [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Monitorar o desempenho

O desempenho e a escala de um sistema de VDI são determinados por uma variedade de fatores como a memória total da GPU, a quantidade de memória do sistema e a velocidade da memória, o número de núcleos de CPU e a frequência do relógio da CPU, a velocidade de armazenamento e a implementação de NUMA.

O suporte a vGPU remoto no RDS inclui os seguintes contadores de desempenho, que podem ser exibidos no Monitor de Desempenho (perfmon.exe) para coletar informações sobre a taxa de transferência da taxa de quadros.

- Gráficos do RemoteFX – contadores da compactação de elementos gráficos do protocolo RDP. Por exemplo, se você quiser examinar o número de quadros que está sendo apresentado ao RDP para compactação, veja o contador **Quadros de Entrada/Segundo**.
- Rede do RemoteFX – contadores do tráfego de rede do protocolo RDP. Por exemplo, **RTT (Tempo de Resposta)** .
- Gerenciamento da GPU Raiz do RemoteFX – mede a VRAM disponível e reservada.
- Software RemoteFX – fornece contadores para taxa de captura, tempo de resposta da GPU dentre outros.

Para um nível de monitoramento de desempenho mais baixo, principalmente para solução de problemas, você pode usar os contadores de desempenho adicionais a seguir:

- Dispositivo da VM do RemoteFX Synth3D VSC 
- Canal de Transporte da VM do RemoteFX Synth3D VSC 
- RemoteFX Synth3D VSP 
- Dispositivo VM RemoteFX Synth3D VSP 
- Canal de Transporte VM do RemoteFX Synth3D VSP
