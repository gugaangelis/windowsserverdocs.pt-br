---
title: Gerenciar serviços de integração do Hyper-V
description: Descreve como ativar e desativar os serviços de integração e instalá-las se necessário
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: e2c14e471abb9af7a9182100969a8dd94a17205a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812199"
---
>Aplica-se a: Windows 10, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gerenciar serviços de integração do Hyper-V

Serviços de integração do Hyper-V aprimorar o desempenho da máquina virtual e fornecer recursos de conveniência, aproveitando a comunicação bidirecional com o host do Hyper-V. Muitos desses serviços são conveniências, como a cópia do arquivo de convidado, enquanto outras são importantes para a funcionalidade da máquina virtual, como drivers de dispositivos sintéticos. Esse conjunto de serviços e drivers, às vezes, são chamados de "componentes de integração". Você pode controlar ou não os serviços individuais de conveniência operam para qualquer determinada máquina virtual. Os componentes do driver não devem ser atendidos manualmente.

Para obter detalhes sobre cada serviço de integração, consulte [serviços de integração do Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Cada serviço que você deseja usar deve ser habilitado no host e convidado para funcionar. Todos os serviços de integração, exceto "Interface de serviço de convidado do Hyper-V" são ativados por padrão em sistemas operacionais convidados do Windows. Os serviços podem ser ativados e desativado individualmente. As seções a seguir mostram como fazer isso.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Ativar ou desativar usando o Gerenciador do Hyper-V a um serviço de integração

1. No painel central, clique com botão direito na máquina virtual e clique em **configurações**.
  
2. No painel à esquerda do **configurações** janela, em **Management**, clique em **Integration Services**.
  
O painel do Integration Services lista todos os serviços de integração disponíveis no host Hyper-V, e se o host tiver habilitado a máquina virtual para usá-los.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Ativar ou desativar usando o PowerShell a um serviço de integração

Para fazer isso no PowerShell, use [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) e [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Os exemplos a seguir demonstram a transformar o arquivo cópia integração serviço de convidado e desativar para uma máquina virtual chamada "demovm".

1. Obter uma lista de serviços de integração em execução:
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. A saída deve parecer com esta:

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. Ative a Interface do serviço de convidado:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Verifique se que a Interface do serviço de convidado está habilitado:

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Desative a Interface do serviço de convidado:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Verificando a versão de serviços de integração do convidado
Alguns recursos podem não funcionar corretamente ou em todos os se os serviços de integração do convidado não forem atuais. Para obter as informações de versão para um Windows, faça logon no sistema operacional convidado, abra um prompt de comando e execute este comando:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Em sistemas operacionais convidados anteriores não terá todos os serviços disponíveis. Por exemplo, os convidados do Windows Server 2008 R2 não podem ter "Hyper-V convidado Service Interface".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Iniciar e parar um serviço de integração de um convidado do Windows
Para um serviço de integração para que seja totalmente funcional que, seu serviço correspondente deve estar executando no convidado além do que está sendo ativado no host. Em convidados do Windows, cada serviço de integração está listado como um serviço do Windows padrão. Você pode usar o miniaplicativo Serviços no painel de controle ou o PowerShell para parar e iniciar esses serviços.

> [!IMPORTANT]
> Interromper um serviço de integração pode afetar gravemente a capacidade dos hosts para gerenciar sua máquina virtual. Para funcionar corretamente, cada serviço de integração que você deseja usar deve ser habilitado no host e convidado.
> Como prática recomendada, você deve controlar apenas a serviços de integração do Hyper-V usando as instruções acima. O serviço correspondente no sistema operacional convidado será interromper ou iniciar automaticamente quando você altera seu status no Hyper-V.
> Se você iniciar um serviço no sistema operacional convidado, mas ela está desabilitada no Hyper-V, o serviço será interrompido. Se você parar um serviço no sistema operacional convidado que está habilitado no Hyper-V, do Hyper-V será, eventualmente, inicie-o novamente. Se você desabilitar o serviço no convidado, Hyper-V será possível iniciá-lo.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar os serviços do Windows para iniciar ou parar um serviço de integração dentro de um convidado do Windows

1. Abra o Gerenciador de serviços executando ```services.msc``` como um administrador ou clicando duas vezes no ícone Serviços no painel de controle.

    ![Captura de tela que mostra o painel de serviços do Windows](media/HVServices.png) 

1. Localize os serviços que começam com "Hyper-V". 

1. Clique com botão direito do serviço que você deseja iniciar ou parar. Clique na ação desejada.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar o Windows PowerShell para iniciar ou parar um serviço de integração dentro de um convidado do Windows

1. Para obter uma lista de serviços de integração, execute:

    ```
    Get-Service -Name vm*
    ```

1.  A saída deve ser semelhante a este:

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. Execute o [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) ou [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Por exemplo, para desativar o Windows PowerShell Direct, execute:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Iniciar e parar um serviço de integração de um convidado do Linux 

Os serviços de integração do Linux geralmente são fornecidos por meio do kernel do Linux. O driver de serviços de integração do Linux é denominado **hv_utils**.

1. Para descobrir se **hv_utils** é carregado, use este comando:

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. A saída deve ser semelhante a este:  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Para descobrir se os daemons necessários estão em execução, use este comando.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. A saída deve ser semelhante a este: 
  
    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv          
    ```

5. Para ver quais daemons estão disponíveis, execute:

    ``` BASH
    compgen -c hv_
    ```
  
6. A saída deve ser semelhante a este:
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   Daemons do serviço de integração que podem ser listados incluem o seguinte. Se qualquer um estiver ausente, eles não podem ter suporte em seu sistema ou não pode ser instalados. Localizar detalhes, consulte [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: Esse daemon é necessário para criar backups de máquina virtual Linux em tempo real.
   - **hv_kvp_daemon**: Esse daemon permite definir e consultar pares chave-valor intrínsecos e extrínsecos.
   - **hv_fcopy_daemon**: Esse daemon implementa um serviço entre o host e convidado de cópia de arquivos.  

### <a name="examples"></a>Exemplos

Esses exemplos demonstram parar e iniciar o daemon KVP, chamado `hv_kvp_daemon`.

1. Use a ID de processo \(PID\) para interromper o processo do daemon. Para localizar o PID, examine a segunda coluna da saída, ou use `pidof`. Daemons do Hyper-V é executado como raiz, portanto, você precisará de permissões de raiz.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Para verificar se todos os `hv_kvp_daemon` processo tiver saído, execute:

    ```
    ps -ef | hv
    ```

1. Para iniciar o daemon novamente, execute o daemon como raiz:

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Para verificar se o `hv_kvp_daemon` processo está listado com uma nova ID de processo, execute:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Manter os serviços de integração atualizado

É recomendável que você mantenha os serviços de integração atualizado para obter o melhor desempenho e recursos mais recentes para suas máquinas virtuais. Isso acontece para a maioria dos convidados do Windows por padrão se eles são configurados para obter atualizações importantes do Windows Update. Convidados do Linux usando kernels atuais receberá os componentes de integração mais recentes quando você atualiza o kernel.

**Para máquinas virtuais em execução em hosts do Windows 10:**

> [!NOTE]
> O vmguest do arquivo de imagem não está incluído com o Hyper-V no Windows 10, porque ele não for mais necessário.

| Convidado  | Mecanismo de atualização | Observações |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Exige o serviço de integração de Troca de Dados.* |
| Windows 7 | Windows Update | Exige o serviço de integração de Troca de Dados.* |
| Windows Vista (SP 2) | Windows Update | Exige o serviço de integração de Troca de Dados.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, o canal semestral | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Exige o serviço de integração de Troca de Dados.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Exige o serviço de integração de Troca de Dados.* |
| Windows Server 2008 (SP 2) | Windows Update | Suporte estendido apenas no Windows Server 2016 ([Leia mais](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | Não terá suporte no Windows Server 2016 ([Leia mais](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | Não há suporte base ([leia mais](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Convidados do Linux | gerenciador de pacotes | Serviços de integração para Linux são incorporados à distribuição, mas pode haver atualizações opcionais disponíveis. ******** |

\* Se o serviço de integração de troca de dados não pode ser habilitado, os serviços de integração para esses convidados estarão disponíveis do [Centro de Download](https://support.microsoft.com/kb/3071740) como um arquivo de gabinete (cab). Instruções para aplicação de um cab estão disponíveis nesta [postagem de blog](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx).

**Para máquinas virtuais em execução em hosts do Windows 8.1:**

| Convidado  | Mecanismo de atualização | Observações |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows 7 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Vista (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows XP (SP 2, SP 3) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, o canal semestral | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2008 R2 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2008 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Home Server 2011 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Small Business Server 2011 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2003 R2 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2003 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| - | | |
| Convidados do Linux | gerenciador de pacotes | Serviços de integração para Linux são incorporados à distribuição, mas pode haver atualizações opcionais disponíveis. ** |


**Para máquinas virtuais em execução em hosts do Windows 8:**

| Convidado  | Mecanismo de atualização | Observações |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows 7 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Vista (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows XP (SP 2, SP 3) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2008 R2 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo.|
| Windows Server 2008 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Home Server 2011 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Small Business Server 2011 | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2003 R2 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| Windows Server 2003 (SP 2) | Disco dos Serviços de Integração | Ver [instruções](#install-or-update-integration-services), abaixo. |
| - | | |
| Convidados do Linux | gerenciador de pacotes | Serviços de integração para Linux são incorporados à distribuição, mas pode haver atualizações opcionais disponíveis. ** |

Para obter mais detalhes sobre convidados do Linux, consulte [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Instalar ou atualizar serviços de integração

Para hosts anteriores ao Windows Server 2016 e Windows 10, você precisará instalar manualmente ou atualizar os serviços de integração em sistemas operacionais convidados. 
  
1.  Abra o Gerenciador Hyper-V. No menu Ferramentas do Gerenciador do servidor, clique em **Gerenciador do Hyper-V**.  
  
2.  Conecte-se a uma máquina virtual. A máquina virtual com o botão direito e clique em **Connect**.  
  
3.  No menu Ação de Conexão de Máquina Virtual, clique em **Inserir Disco de Instalação dos Serviços de Integração**. Essa ação carrega o disco de instalação na unidade de DVD virtual. Dependendo do sistema operacional convidado, você talvez precise iniciar a instalação manualmente.  
  
4.  Após a conclusão da instalação, todos os serviços de integração estarão disponíveis para uso.

Essas etapas não podem ser automatizadas ou feitas dentro de uma sessão do Windows PowerShell para máquinas virtuais online. Você pode aplicá-las para imagens offline do VHDX; [consulte este blog a postagem](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/).
