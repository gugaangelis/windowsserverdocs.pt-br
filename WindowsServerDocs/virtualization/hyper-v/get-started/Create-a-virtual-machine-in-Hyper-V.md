---
title: Criar uma máquina virtual no Hyper-V
description: Fornece instruções para criar uma máquina virtual usando o Gerenciador do Hyper-V ou o Windows PowerShell
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 739691650ce3cda8066e9f7ac77626f53f22affa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364251"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Criar uma máquina virtual no Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Saiba como criar uma máquina virtual usando o Gerenciador do Hyper-V e o Windows PowerShell e quais opções você tem ao criar uma máquina virtual no Gerenciador do Hyper-V.  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>Criar uma máquina virtual usando o Gerenciador do Hyper-V  

1.  Abra o **Gerenciador do Hyper-V**.  

2.  No painel **ação** , clique em **novo**e em **máquina virtual**.  

3.  No **Assistente de nova máquina virtual**, clique em **Avançar**.  

4.  Faça as escolhas apropriadas para sua máquina virtual em cada uma das páginas. Para obter mais informações, consulte [novas opções de máquina virtual e padrões no Gerenciador do Hyper-V](#options-in-hyper-v-manager-new-virtual-machine-wizard) mais adiante neste tópico.  

5.  Depois de verificar suas escolhas na página **Resumo** , clique em **concluir**.  

6.  No Gerenciador do Hyper-V, clique com o botão direito do mouse na máquina virtual e selecione **conectar**.  

7.  Na janela conexão de máquina virtual, selecione **ação** > **Iniciar**.  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>Criar uma máquina virtual usando o Windows PowerShell  

1. Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.  

2. Clique com o botão direito do mouse em **Windows PowerShell** e selecione **Executar como administrador**.  

3. Obtenha o nome do comutador virtual que você deseja que a máquina virtual use usando [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Por exemplo,  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. Use o cmdlet [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para criar a máquina virtual.  Consulte os exemplos a seguir.  

   > [!NOTE]  
   > Se você pode mover essa máquina virtual para um host Hyper-V que executa o Windows Server 2012 R2, use o parâmetro-version com [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para definir a versão de configuração da máquina virtual como 5. A versão de configuração de máquina virtual padrão para o Windows Server 2016 não é compatível com o Windows Server 2012 R2 ou versões anteriores. Você não pode alterar a versão de configuração da máquina virtual depois que a máquina virtual é criada. Para obter mais informações, consulte [versões de configuração de máquina virtual com suporte](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions).  

   - **Disco rígido virtual existente** -para criar uma máquina virtual com um disco rígido virtual existente, você pode usar o comando a seguir, em que,  
     - **-Name** é o nome que você fornece para a máquina virtual que você está criando.  
     - **-MemoryStartupBytes** é a quantidade de memória disponível para a máquina virtual na inicialização.  
     - **-BootDevice** é o dispositivo no qual a máquina virtual é inicializada quando começa como o adaptador de rede (adaptador) ou disco rígido virtual (VHD).  
     - **-VHDPath** é o caminho para o disco de máquina virtual que você deseja usar.  
     - **-Path** é o caminho para armazenar os arquivos de configuração da máquina virtual.  
     - **-Geração** é a geração da máquina virtual. Use a geração 1 para o VHD e a geração 2 para VHDX. Consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Switch** é o nome do comutador virtual que você deseja que a máquina virtual use para se conectar a outras máquinas virtuais ou à rede. Consulte [criar um comutador virtual para máquinas virtuais do Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       Por exemplo:  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       Isso cria uma máquina virtual de geração 2 chamada Win10VM com 4 GB de memória. Ele é inicializado a partir da pasta VMs\Win10.vhdx no diretório atual e usa o comutador virtual chamado ExternalSwitch. Os arquivos de configuração de máquina virtual são armazenados na pasta VMData.  

   - **Novo disco rígido virtual** -para criar uma máquina virtual com um novo disco rígido virtual, substitua o parâmetro **-VHDPath** do exemplo acima por **-NewVHDPath** e adicione o parâmetro **-NewVHDSizeBytes** . Por exemplo,  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **Novo disco rígido virtual que é inicializado na imagem do sistema operacional** – para criar uma máquina virtual com um novo disco virtual que é inicializado em uma imagem do sistema operacional, consulte o exemplo do PowerShell em [criar máquina virtual passo a passos para Hyper-V no Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5. Inicie a máquina virtual usando o cmdlet [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) . Execute o cmdlet a seguir, em que Name é o nome da máquina virtual que você criou.  

   ```  
   Start-VM -Name <Name>  
   ```  

   Por exemplo:  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. Conecte-se à máquina virtual usando a conexão de máquina virtual (VMConnect).  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Opções no assistente de nova máquina virtual do Gerenciador do Hyper-V  
A tabela a seguir lista as opções que você pode escolher ao criar uma máquina virtual no Gerenciador do Hyper-V e os padrões para cada uma.  

|Page|Padrão para Windows Server 2016 e Windows 10|Outras opções|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Especificar o nome e o local**|Nome: nova máquina virtual.<br /><br />Local: **C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|Você também pode inserir seu próprio nome e escolher outro local para a máquina virtual.<br /><br />É aí que os arquivos de configuração da máquina virtual serão armazenados.|  
|**Especificar a geração**|1ª geração|Você também pode optar por criar uma máquina virtual de geração 2. Para obter mais informações, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Atribuir Memória**|Memória de inicialização: 1024 MB<br /><br />Memória dinâmica: **não selecionada**|Você pode definir a memória de inicialização de 32MB para 5902MB.<br /><br />Você também pode optar por usar Memória Dinâmica. Para obter mais informações, consulte [visão geral do memória dinâmica do Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurar a rede**|Não conectado|Você pode selecionar uma conexão de rede para a máquina virtual usar a partir de uma lista de comutadores virtuais existentes. Consulte [criar um comutador virtual para máquinas virtuais do Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Conectar o disco rígido virtual**|Criar um disco rígido virtual<br /><br />Nome: <*vmname*>. vhdx<br /><br />**Local**: **C:\Users\Public\Documents\Hyper-V\Virtual discos rígidos\\**<br /><br />**Tamanho**: 127 GB|Você também pode optar por usar um disco rígido virtual existente ou aguardar e anexar um disco rígido virtual mais tarde.|  
|**Opções de instalação**|Instalar um sistema operacional mais tarde|Essas opções alteram a ordem de inicialização da máquina virtual para que você possa instalar a partir de um arquivo. ISO, um disquete inicializável ou um serviço de instalação de rede, como o WDS (serviços de implantação do Windows).|  
|**Resumo**|Exibe as opções escolhidas, para que você possa verificar se elas estão corretas.<br /><br />-Nome<br />-Geração<br />-Memória<br />-Rede<br />-Disco rígido<br />-Sistema operacional|**Dica:** Você pode copiar o resumo da página e colá-lo em email ou em outro lugar para ajudá-lo a acompanhar suas máquinas virtuais.|  

## <a name="see-also"></a>Consulte também  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versões de configuração de máquina virtual com suporte](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Criar uma opção virtual para máquinas virtuais Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
