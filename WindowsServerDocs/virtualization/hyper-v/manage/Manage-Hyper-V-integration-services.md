---
title: Gerenciar Integration Services do Hyper-V
description: Descreve como ativar e desativar o Integration Services e instalá-los, se necessário
ms.technology: compute-hyper-v
author: kbdazure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: 2c5e2d67b391cd53a6995957da5dab108a34e1a9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820709"
---
>Aplica-se a: Windows 10, Windows Server 2012, Windows Server 2012R2, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gerenciar Integration Services do Hyper-V

O Hyper-V Integration Services aprimorar o desempenho da máquina virtual e fornecer recursos de conveniência aproveitando a comunicação bidirecional com o host Hyper-V. Muitos desses serviços são conveniências, como a cópia do arquivo convidado, enquanto outros são importantes para a funcionalidade da máquina virtual, como drivers de dispositivo sintéticos. Esse conjunto de serviços e drivers às vezes são chamados de "componentes de integração". Você pode controlar se os serviços individuais de conveniência operam ou não para qualquer máquina virtual específica. Os componentes do driver não devem ser atendidos manualmente.

Para obter detalhes sobre cada serviço de integração, consulte [Hyper-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Cada serviço que você deseja usar deve ser habilitado no host e no convidado para funcionar. Todos os serviços de integração, exceto "Interface do Serviço de Convidado do Hyper-V", estão ativados por padrão em sistemas operacionais convidados do Windows. Os serviços podem ser ativados e desligados individualmente. As próximas seções mostram como.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Ativar ou desativar um serviço de integração usando o Gerenciador do Hyper-V

1. No painel central, clique com o botão direito do mouse na máquina virtual e clique em **configurações**.
  
2. No painel esquerdo da janela **configurações** , em **Gerenciamento**, clique em **Integration Services**.
  
O painel de Integration Services lista todos os serviços de integração disponíveis no host Hyper-V e se o host habilitou a máquina virtual para usá-los.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Ativar ou desativar um serviço de integração usando o PowerShell

Para fazer isso no PowerShell, use [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) e [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Os exemplos a seguir demonstram como ativar e desativar o serviço de integração de cópia de arquivo convidado para uma máquina virtual chamada "demovm".

1. Obtenha uma lista de serviços de integração em execução:
  
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

1. Ativar a interface do serviço de convidado:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Verifique se a interface do serviço de convidado está habilitada:

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Desative a interface do serviço de convidado:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Verificando a versão dos serviços de integração do convidado
Alguns recursos podem não funcionar corretamente ou se os serviços de integração do convidado não forem atuais. Para obter as informações de versão de um Windows, faça logon no sistema operacional convidado, abra um prompt de comando e execute este comando:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Os sistemas operacionais convidados anteriores não terão todos os serviços disponíveis. Por exemplo, os convidados do Windows Server 2008 R2 não podem ter o "Interface do Serviço de Convidado do Hyper-V".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Iniciar e parar um serviço de integração de um convidado do Windows
Para que um serviço de integração seja totalmente funcional, seu serviço correspondente deve estar em execução dentro do convidado, além de ser habilitado no host. Em convidados do Windows, cada serviço de integração é listado como um serviço padrão do Windows. Você pode usar o applet Serviços no painel de controle ou no PowerShell para parar e iniciar esses serviços.

> [!IMPORTANT]
> Parar um serviço de integração pode afetar seriamente a capacidade do host de gerenciar sua máquina virtual. Para funcionar corretamente, cada serviço de integração que você deseja usar deve ser habilitado no host e no convidado.
> Como prática recomendada, você só deve controlar o Integration Services do Hyper-V usando as instruções acima. O serviço de correspondência no sistema operacional convidado será interrompido ou iniciado automaticamente quando você alterar seu status no Hyper-V.
> Se você iniciar um serviço no sistema operacional convidado, mas ele estiver desabilitado no Hyper-V, o serviço será interrompido. Se você parar um serviço no sistema operacional convidado que está habilitado no Hyper-V, o Hyper-V eventualmente o iniciará novamente. Se você desabilitar o serviço no convidado, o Hyper-V não poderá iniciá-lo.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar os serviços do Windows para iniciar ou parar um serviço de integração em um convidado do Windows

1. Abra o Gerenciador de serviços executando ```services.msc``` como administrador ou clicando duas vezes no ícone serviços no painel de controle.

    ![Captura de tela que mostra o painel serviços do Windows](media/HVServices.png) 

1. Localize os serviços que começam com "Hyper-V". 

1. Clique com o botão direito do mouse no serviço que você deseja iniciar ou parar. Clique na ação desejada.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usar o Windows PowerShell para iniciar ou parar um serviço de integração em um convidado do Windows

1. Para obter uma lista de serviços de integração, execute:

    ```
    Get-Service -Name vm*
    ```

1.  A saída deve ser semelhante a esta:

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

1. Execute o [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) ou o [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Por exemplo, para desativar o Windows PowerShell Direct, execute:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Iniciar e parar um serviço de integração de um convidado do Linux 

Os serviços de integração do Linux geralmente são fornecidos por meio do kernel do Linux. O driver do Integration Services para Linux é denominado **hv_utils**.

1. Para descobrir se **hv_utils** está carregado, use este comando:

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. A saída deve ser semelhante a esta:  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Para descobrir se os daemons necessários estão em execução, use este comando.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. A saída deve ser semelhante a esta: 
  
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
  
6. A saída deve ser semelhante a esta:
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   Os daemons do serviço de integração que podem ser listados incluem o seguinte. Se algum estiver ausente, talvez ele não tenha suporte no seu sistema ou talvez não esteja instalado. Encontre detalhes, consulte [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: esse daemon é necessário para criar backups de máquina virtual Linux em tempo real.
   - **hv_kvp_daemon**: esse daemon permite configurar e consultar pares de valor de chave intrínsecos e extrínsecos.
   - **hv_fcopy_daemon**: esse daemon implementa um serviço de cópia de arquivos entre o host e o convidado.  

### <a name="examples"></a>Exemplos

Esses exemplos demonstram como parar e iniciar o daemon KVP, chamado `hv_kvp_daemon`.

1. Use a ID do processo \(\) PID para interromper o processo do daemon. Para localizar o PID, examine a segunda coluna da saída ou use `pidof`. Os daemons do Hyper-V são executados como raiz, portanto, você precisará de permissões de raiz.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Para verificar se todos os processos de `hv_kvp_daemon` desapareceram, execute:

    ```
    ps -ef | hv
    ```

1. Para iniciar o daemon novamente, execute o daemon como raiz:

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Para verificar se o processo de `hv_kvp_daemon` está listado com uma nova ID de processo, execute:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Manter os serviços de integração atualizados

Recomendamos que você mantenha os serviços de integração atualizados para obter o melhor desempenho e os recursos mais recentes para suas máquinas virtuais. Isso acontece para a maioria dos convidados do Windows por padrão se eles estiverem configurados para obter atualizações importantes do Windows Update. Os convidados do Linux que usam kernels atuais receberão os componentes de integração mais recentes quando você atualizar o kernel.

**Para máquinas virtuais em execução em hosts Windows 10/Windows Server 2016/2019:**

> [!NOTE]
> O arquivo de imagem vmguest. ISO não está incluído no Hyper-V no Windows 10/Windows Server 2016/2019 porque ele não é mais necessário.

| Convidado  | Mecanismo de atualização | {1&gt;Observações&lt;1} |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Requer o serviço de integração de Troca de Dados.* |
| Windows 7 | Windows Update | Requer o serviço de integração de Troca de Dados.* |
| Windows Vista (SP 2) | Windows Update | Requer o serviço de integração de Troca de Dados.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canal Semestral | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Requer o serviço de integração de Troca de Dados.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Requer o serviço de integração de Troca de Dados.* |
| Windows Server 2008 (SP 2) | Windows Update | Suporte estendido somente no Windows Server 2016 ([Leia mais](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | Não terá suporte no Windows Server 2016 ([Leia mais](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | Não há suporte base ([leia mais](https://support.microsoft.com/lifecycle?p1=15817)). |
| - | | |
| Convidados do Linux | gerenciador de pacotes | O Integration Services para Linux está integrado ao distribuição, mas pode haver atualizações opcionais disponíveis. ******** |

\* se o serviço de integração de troca de dados não puder ser habilitado, os serviços de integração para esses convidados estarão disponíveis no [centro de download](https://support.microsoft.com/kb/3071740) como um arquivo de gabinete (CAB). As instruções para aplicar um CAB estão disponíveis nesta [postagem de blog](https://techcommunity.microsoft.com/t5/virtualization/integration-components-available-for-virtual-machines-not/ba-p/382247).

**Para máquinas virtuais em execução no Windows 8.1/hosts 2012R2 do Windows Server:**

| Convidado  | Mecanismo de atualização | {1&gt;Observações&lt;1} |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows 8 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows 7 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Vista (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows XP (SP 2, SP 3) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canal Semestral | Windows Update | |
| Windows Server 2012 R2 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2012 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2008 R2 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2008 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Home Server 2011 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Small Business Server 2011 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2003 R2 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2003 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| - | | |
| Convidados do Linux | gerenciador de pacotes | O Integration Services para Linux está integrado ao distribuição, mas pode haver atualizações opcionais disponíveis. ** |


**Para máquinas virtuais em execução em hosts Windows 8/Windows Server 2012:**

| Convidado  | Mecanismo de atualização | {1&gt;Observações&lt;1} |
|:---------|:---------|:---------|
| Windows 8.1 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows 8 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows 7 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Vista (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows XP (SP 2, SP 3) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| - | | |
| Windows Server 2012 R2 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2012 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2008 R2 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo.|
| Windows Server 2008 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Home Server 2011 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Small Business Server 2011 | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2003 R2 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| Windows Server 2003 (SP 2) | Disco dos Serviços de Integração | Consulte [as instruções](#install-or-update-integration-services)abaixo. |
| - | | |
| Convidados do Linux | gerenciador de pacotes | O Integration Services para Linux está integrado ao distribuição, mas pode haver atualizações opcionais disponíveis. ** |

Para obter mais detalhes sobre os convidados do Linux, consulte [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Instalar ou atualizar o Integration Services

> [!NOTE]
> Para hosts anteriores ao Windows Server 2016 e Windows 10, você precisará **instalar ou atualizar manualmente** os serviços de integração nos sistemas operacionais convidados. 

Procedimento para instalar ou atualizar os serviços de integração manualmente:

1.  Abra o Gerenciador Hyper-V. No menu ferramentas do Gerenciador do Servidor, clique em **Gerenciador do Hyper-V**.  
  
2.  Conecte-se à máquina virtual. Clique com o botão direito do mouse na máquina virtual e clique em **conectar**.  
  
3.  No menu Ação de Conexão de Máquina Virtual, clique em **Inserir Disco de Instalação dos Serviços de Integração**. Essa ação carrega o disco de instalação na unidade de DVD virtual. Dependendo do sistema operacional convidado, talvez seja necessário iniciar a instalação manualmente.  
  
4.  Após a conclusão da instalação, todos os serviços de integração estarão disponíveis para uso.

> [!NOTE]
> Essas etapas **não podem ser automatizadas** ou executadas em uma sessão do Windows PowerShell para máquinas virtuais **online** .
> Você pode aplicá-los a imagens VHDX **offline** ; consulte [como instalar o Integration Services quando a máquina virtual não está em execução](https://docs.microsoft.com/virtualization/community/team-blog/2013/20130418-how-to-install-integration-services-when-the-virtual-machine-is-not-running).
> Também é possível automatizar a implantação do Integration Services por meio do **Configuration Manager** com as VMs **online**, mas você precisa reiniciar as VMs no final da instalação; consulte [Implantando Integration Services do Hyper-V em VMs usando o config Manager e o DISM](https://docs.microsoft.com/archive/blogs/manageabilityguys/deploying-hyper-v-integration-services-to-vms-using-config-manager-and-dism)
