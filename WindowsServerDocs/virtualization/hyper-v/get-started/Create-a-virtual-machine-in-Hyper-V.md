---
title: Criar uma máquina virtual no Hyper-V
description: Fornece instruções para criar uma máquina virtual usando o Gerenciador do Hyper-V ou o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 8bc0aba906066f1c2fdc4d906ebbc78b08e871f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852137"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Criar uma máquina virtual no Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Saiba como criar uma máquina virtual usando o Gerenciador do Hyper-V e Windows PowerShell e as opções que você tem quando você cria uma máquina virtual no Gerenciador do Hyper-V.  

## <a name="BKMK_HyperVManager"></a>Criar uma máquina virtual usando o Gerenciador do Hyper-V  

1.  Abra **Gerenciador do Hyper-V**.  

2.  Dos **ação** painel, clique em **New**e, em seguida, clique em **Máquina Virtual**.  

3.  Dos **Assistente de nova máquina Virtual**, clique em **próxima**.  

4.  Fazer as escolhas apropriadas para sua máquina virtual em cada uma das páginas. Para obter mais informações, consulte [novas opções de máquina virtual e os padrões no Gerenciador do Hyper-V](#BKMK_Options) mais adiante neste tópico.  

5.  Depois de verificar as escolhas feitas na **resumo** , clique em **concluir**.  

6.  No Gerenciador do Hyper-V, a máquina virtual com o botão direito e selecione **conectar-se**.  

7.  Na janela Conexão de máquina Virtual, selecione **ação** > **iniciar**.  

## <a name="BKMK_PowerShell"></a>Criar uma máquina virtual usando o Windows PowerShell  

1.  Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.  

2.  Clique com botão direito **Windows PowerShell** e selecione **executar como administrador**.  

3.  Obter o nome do comutador virtual que você deseja que a máquina virtual para usar por meio [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Por exemplo,  

    ```  
    Get-VMSwitch  * | Format-Table Name  
    ```  

4.  Use o [New-VM](https://technet.microsoft.com/library/hh848537.aspx) cmdlet para criar a máquina virtual.  Consulte os exemplos a seguir.  

    > [!NOTE]  
    > Se você pode mover esta máquina virtual para um host do Hyper-V que executa o Windows Server 2012 R2, use o parâmetro - Version com [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para definir a versão de configuração de máquina virtual como 5. Não há suporte para a versão de configuração de máquina virtual padrão para o Windows Server 2016, Windows Server 2012 R2 ou versões anteriores. Você não pode alterar a versão de configuração de máquina virtual depois que a máquina virtual é criada. Para obter mais informações, consulte [suporte para versões de configuração de máquina virtual](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#BKMK_SupportedConfigVersions).  

    - **Disco rígido virtual existente** – para criar uma máquina virtual com um disco rígido virtual existente, você pode usar o comando a seguir onde,  
        -   **-Nome** é o nome que você fornece para a máquina virtual que você está criando.  
        -   **-MemoryStartupBytes** é a quantidade de memória que está disponível para a máquina virtual na inicialização.  
        -   **-BootDevice** é o dispositivo que a máquina virtual é inicializado quando ele é iniciado, como o adaptador de rede (adaptador de rede) ou um disco rígido virtual (VHD).  
        -   **-VHDPath** é o caminho para o disco de máquina virtual que você deseja usar.  
        -   **-Path** é o caminho para armazenar os arquivos de configuração de máquina virtual.  
        -   **-Geração** é a geração de máquina virtual. Use a geração 1 para o VHD e geração 2 para VHDX. Consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
        -   **-Alternar** é o nome do comutador virtual que você deseja que a máquina virtual para usar para se conectar a outras máquinas virtuais ou a rede. Ver [criar um comutador virtual para máquinas virtuais Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

        ```  
        New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
        ```  

        Por exemplo:  

        ```  
        New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
        ```  

        Isso cria uma máquina virtual de geração 2 chamada Win10VM com 4GB de memória. Ele é inicializado da pasta VMs\Win10.vhdx no diretório atual e usa o comutador virtual chamado ExternalSwitch. Os arquivos de configuração de máquina virtual são armazenados na pasta VMData.  

    -   **Novo disco rígido virtual** – para criar uma máquina virtual com um novo disco rígido virtual, substitua o **- VHDPath** parâmetro do exemplo acima, com **- NewVHDPath** e adicione o **- NewVHDSizeBytes** parâmetro. Por exemplo,  

        ```  
        New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
        ```  

    -   **Novo disco rígido virtual que é inicializado para a imagem do sistema operacional** – para criar uma máquina virtual com um novo disco virtual que é inicializado para uma imagem de sistema operacional, consulte o exemplo do PowerShell no [criar instruções passo a passo de máquina virtual do Hyper-V no Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5.  Inicie a máquina virtual usando o [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet. Execute o seguinte cmdlet em que o nome é o nome da máquina virtual que você criou.  

    ```  
    Start-VM -Name <Name>  
    ```  

    Por exemplo:  

    ```  
    Start-VM -Name Win10VM  
    ```  

6.  Conecte-se à máquina virtual usando a Conexão de máquina Virtual (VMConnect).  

    ```  
    VMConnect.exe  
    ```  

## <a name="BKMK_Options"></a>Opções no Assistente de nova máquina Virtual do Hyper-V Manager  
A tabela a seguir lista as opções que você pode escolher quando você cria uma máquina virtual no Gerenciador do Hyper-V e os padrões para cada um.  

|Page|Padrão para o Windows Server 2016 e Windows 10|Outras opções|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Especificar o nome e o local**|Nome:  Nova Máquina Virtual.<br /><br />Local:  **C:\ProgramData\Microsoft\Windows\Hyper-V\\**.|Você também pode inserir seu próprio nome e escolher outro local para a máquina virtual.<br /><br />Isso é onde serão armazenados os arquivos de configuração de máquina virtual.|  
|**Especificar a geração**|1ª geração|Você também pode optar por criar uma máquina virtual de geração 2. Para obter mais informações, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Atribuir Memória**|Memória de inicialização: 1024 MB<br /><br />Memória dinâmica: **não selecionada**|Você pode definir a memória de inicialização de 32MB para 5902MB.<br /><br />Você também pode optar por usar a memória dinâmica. Para obter mais informações, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurar a rede**|Não conectado|Você pode selecionar uma conexão de rede para a máquina virtual usar em uma lista de comutadores virtuais existentes. Ver [criar um comutador virtual para máquinas virtuais Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Conectar o disco rígido virtual**|Criar um disco rígido virtual<br /><br />Name: <*vmname*>.vhdx<br /><br />**Local**: **Discos de rígidos C:\Users\Public\Documents\Hyper-V\Virtual\\**<br /><br />**Tamanho**: 127GB|Você também pode optar por usar um disco rígido virtual existente ou Aguarde e anexar um disco rígido virtual mais tarde.|  
|**Opções de instalação**|Instalar um sistema operacional mais tarde|Essas opções de alteram a ordem de inicialização da máquina virtual para que você possa instalar de um arquivo. ISO, disquete inicializável ou um serviço de instalação de rede, como o Windows Deployment Services (WDS).|  
|**Resumo**|Exibe as opções que você escolheu, para que você possa verificar que eles estão corretos.<br /><br />-Nome<br />-Geração<br />-Memória<br />-Rede<br />-Unidade de disco<br />-O sistema de operacional|**Dica:** Você pode copiar o resumo da página e colá-la em um email ou em outro lugar para ajudá-lo a manter o controle de suas máquinas virtuais.|  

## <a name="see-also"></a>Consulte também  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versões de configuração de máquina virtual com suporte](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#BKMK_SupportedConfigVersions)  

-   [Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Criar um comutador virtual para máquinas virtuais Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
