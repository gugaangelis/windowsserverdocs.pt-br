---
title: Atualização sem interrupção do sistema operacional do cluster
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: f56c036768de7c1afcf3327135a7ff7d7a690a8b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440146"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016

Atualização sem interrupção do cluster do sistema operacional permite que um administrador atualizar o sistema operacional de nós do cluster sem interromper o Hyper-V ou as cargas de trabalho do servidor de arquivos de escalabilidade horizontal. Usando esse recurso, as penalidades de tempo de inatividade em SLAs (Contratos de Nível de Serviço) podem ser evitadas.

Atualização sem interrupção do cluster do sistema operacional oferece os seguintes benefícios:

- Clusters de failover que executam cargas de trabalho do servidor de arquivos de escalabilidade horizontal (SOFS) e de máquina virtual Hyper-V podem ser atualizados do Windows Server 2012 R2 (em execução em todos os nós no cluster) para o Windows Server 2016 (em execução em todos os nós de cluster do cluster) sem tempo de inatividade. Outras cargas de trabalho de cluster, como o SQL Server estará indisponíveis durante o tempo (normalmente menos de cinco minutos) para o failover para o Windows Server 2016.  
- Ele não exige nenhum hardware adicional. No entanto, você pode adicionar mais nós de cluster temporariamente para clusters pequenos para melhorar a disponibilidade do cluster durante o Cluster do sistema operacional atualização sem interrupção do processam.  
- O cluster não precisa ser interrompida ou reiniciada.  
- Um novo cluster não é necessário. O cluster existente será atualizado. Além disso, os objetos de cluster existentes armazenados no Active Directory são usados.  
- O processo de atualização é reversível até que o decide cliente o "ponto-de-não-return", quando os nós de cluster está executando o Windows Server 2016, e quando o cmdlet Update-ClusterFunctionalLevel PowerShell é executado.  
- O cluster pode dar suporte a operações de aplicação de patch e manutenção durante a execução no modo misto do sistema operacional.  
- Ele dá suporte a automação por meio do PowerShell e o WMI.  
- A propriedade pública de cluster **ClusterFunctionalLevel** propriedade indica o estado do cluster em nós de cluster do Windows Server 2016. Essa propriedade pode ser consultada usando o cmdlet do PowerShell de um nó de cluster do Windows Server 2016 que pertence a um cluster de failover:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Um valor de **8** indica que o cluster está em execução no nível funcional do Windows Server 2012 R2. Um valor de **9** indica que o cluster está em execução no nível funcional do Windows Server 2016.  

Este guia descreve os diversos estágios do processo de atualização sem interrupção do Cluster do sistema operacional, etapas de instalação, limitações de recursos e perguntas frequentes (FAQs) e é aplicável para os seguintes cenários de atualização sem interrupção do Cluster do sistema operacional no Windows Server 2016:  
- Clusters do Hyper-V  
- Clusters de servidor de arquivos de escalabilidade horizontal  

Não há suporte no Windows Server 2016 para o cenário a seguir:  
-  Atualização do sistema operacional sem interrupção de clusters de convidados usando um disco rígido virtual (arquivo. vhdx) como armazenamento compartilhado de cluster  

Atualização sem interrupção do cluster do sistema operacional é totalmente suportado pelo System Center Virtual Machine Manager (SCVMM) 2016. Se você estiver usando o SCVMM 2016, consulte [executar uma atualização sem interrupção de um cluster de host do Hyper-V para Windows Server 2016 no VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) para obter orientação sobre como atualizar os clusters e automatizar as etapas descritas neste documento.  

## <a name="requirements"></a>Requisitos  
Complete os seguintes requisitos antes de começar o processo de atualização sem interrupção do Cluster do sistema operacional:

- Comece com um Cluster de Failover executando o Windows Server 2012 R2, Windows Server 2016 ou Windows Server (canal semestral).
- Atualizar um cluster de espaços de armazenamento diretos para o Windows Server, versão 1709 não tem suporte.
- Se a carga de trabalho do cluster for VMs Hyper-V ou servidor de arquivos de escalabilidade horizontal, você pode esperar a atualização de tempo de inatividade zero.
- Verifique se os nós do Hyper-V tem CPUs que dão suporte a endereçamento de tabela (SLAT de segundo nível) usando um dos seguintes métodos:  
        -Examine o [tem SLAT compatível? WP8 SDK dica 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) artigo que descreve dois métodos para verificar se uma CPU oferece suporte a SLATs  
        -Baixe o [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) tool para determinar se uma CPU suporta SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transição de cluster durante a atualização sem interrupção do Cluster do sistema operacional

Esta seção descreve os diversos estados de transição do cluster do Windows Server 2012 R2 que está sendo atualizado para o Windows Server 2016 usando a atualização sem interrupção do Cluster do sistema operacional.  

Para manter as cargas de trabalho de cluster em execução durante o processo de atualização sem interrupção do Cluster do sistema operacional, movendo uma carga de trabalho de cluster de um nó do Windows Server 2012 R2 para Windows Server 2016 nó funciona como se os dois nós estavam executando o sistema operacional Windows Server 2012 R2. Quando nós do Windows Server 2016 forem adicionados ao cluster, eles operam em um modo de compatibilidade do Windows Server 2012 R2. Um novo modo de cluster conceitual, chamado "Modo misto do sistema operacional", permite que os nós de diferentes versões de existir no mesmo cluster (veja a Figura 1).  

![Ilustração mostrando três estágios de uma atualização sem interrupção do sistema operacional do cluster: todos os nós do Windows Server 2012 R2, o modo misto do sistema operacional e todos os nós do Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: Transições de estado do sistema operacional de cluster**  

Um cluster do Windows Server 2012 R2 entra no modo misto do sistema operacional quando um nó do Windows Server 2016 é adicionado ao cluster. O processo é totalmente reversível – nós do Windows Server 2016 podem ser removidos do cluster e nós do Windows Server 2012 R2 podem ser adicionados ao cluster neste modo. O "ponto não haverá mais retorno" ocorre quando o cmdlet Update-ClusterFunctionalLevel PowerShell é executado no cluster. Em ordem para esse cmdlet seja bem-sucedida, todos os nós devem ser Windows Server 2016 e todos os nós devem estar online.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transição de um cluster de quatro nós durante a execução de atualização sem interrupção do sistema operacional

Esta seção ilustra e descreve os quatro estágios diferentes de um cluster com armazenamento compartilhado cujos nós são atualizados do Windows Server 2012 R2 para o Windows Server 2016.  

"Estágio de 1" é o estado inicial – vamos começar com um cluster do Windows Server 2012 R2.  

![Ilustração mostrando o estado inicial: todos os nós do Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Estado inicial: Cluster de Failover do Windows Server 2012 R2 (etapa 1)**  

No "estágio 2", dois nós foram em pausa, descarregadas, removidos, reformatadas e instalados com o Windows Server 2016.  

![Ilustração mostrando o cluster no modo misto do sistema operacional: fora do cluster de 4 nós de exemplo, dois nós estão executando o Windows Server 2016 e dois nós executando o Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Estado intermediário: Modo misto do sistema operacional: Windows Server 2012 R2 e Windows Server 2016 Failover cluster (etapa 2)**  

Em "etapa 3", todos os nós no cluster foram atualizados para o Windows Server 2016 e o cluster está pronto para ser atualizado com o cmdlet do PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> Nesse estágio, o processo pode ser totalmente revertido e nós do Windows Server 2012 R2 podem ser adicionados a esse cluster.  

![Ilustração mostrando que o cluster foi totalmente atualizado para o Windows Server 2016 e está pronto para o cmdlet Update-ClusterFunctionalLevel trazer o nível funcional do cluster até o Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Estado intermediário: Todos os nós atualizados para o Windows Server 2016, pronto para Update-ClusterFunctionalLevel (estágio 3)**  

Após a execução de atualização-ClusterFunctionalLevelcmdlet, o cluster entra no "Estágio 4", onde os novos recursos de cluster do Windows Server 2016 podem ser usados.  

![Ilustração mostrando que o cluster sem interrupção do sistema operacional foi concluído com êxito; todos os nós foram atualizados para o Windows Server 2016 e o cluster está em execução no nível funcional do cluster do Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Estado final: Cluster de Failover do Windows Server 2016 (etapa 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>O processo de atualização sem interrupção do sistema de operacional do cluster

Esta seção descreve o fluxo de trabalho para executar a atualização sem interrupção do Cluster do sistema operacional.  

![Ilustração mostrando o fluxo de trabalho para atualizar um cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: Sistema operacional do cluster sem interrupção de fluxo de trabalho do processo de atualização**  

Atualização sem interrupção do sistema operacional de cluster inclui as seguintes etapas:  

1. Prepare o cluster para atualização do sistema operacional, da seguinte maneira:  
    1. Atualização sem interrupção do cluster do sistema operacional requer a remoção de um nó por vez do cluster. Verifique se você tem capacidade suficiente no cluster para manter os SLAs de alta disponibilidade quando um de nós do cluster é removido do cluster para uma atualização do sistema operacional. Em outras palavras, exigem a capacidade de cargas de trabalho de failover para outro nó quando um nó é removido do cluster durante o processo de atualização sem interrupção do Cluster do sistema operacional? O cluster tem a capacidade para executar as cargas de trabalho necessárias quando um nó é removido do cluster para atualização sem interrupção do Cluster do sistema operacional?  
    2. Para cargas de trabalho do Hyper-V, verifique se o todos os hosts Windows Server 2016 Hyper-V tem CPU dar suporte a tabela de endereços de segundo nível (SLAT). Somente as máquinas compatíveis com SLAT podem usar a função Hyper-V no Windows Server 2016.  
    3. Verifique se quaisquer backups de carga de trabalho concluiram e considerar fazer o backup do cluster. Interrompa as operações de backup durante a adição de nós no cluster.  
    4. Verifique se todos os nós de cluster estão online/execução/backup usando o [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet (veja a Figura 7).  

        ![Captura mostrando os resultados da execução do cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinando o status do nó usando o cmdlet Get-ClusterNode**  

    5. Se você estiver executando as atualizações com suporte do Cluster (CAU), verifique se o CAU está sendo executado usando o **atualizando ciente do Cluster** interface do usuário, ou o [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet (consulte a Figura 8). Parar de usar a CAU as [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet (veja a Figura 9) para impedir que todos os nós seja pausado e descarregadas pela CAU durante o processo de atualização sem interrupção do Cluster do sistema operacional.  

        ![Captura mostrando a saída do cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Usando o [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet para determinar se as atualizações com suporte de Cluster está em execução no cluster**  

        ![Captura mostrando a saída do cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: Desabilitar a função as atualizações com suporte de Cluster usando o [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet**  

2. Para cada nó no cluster, faça o seguinte:  
    1. Usando o Gerenciador de Cluster de UI, selecione um nó e use o **pausar | Drenar** opção de menu para esvaziar o nó (veja a Figura 10) ou usar o [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet (veja a Figura 11).  

        ![Captura mostrando como drenar funções com a UI do Gerenciador de Cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Funções de descarga de um nó usando o Gerenciador de Cluster de Failover**  

        ![Captura mostrando a saída do cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Drenagem de funções de um nó usando o [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet**  

    2.  Usando o Gerenciador de Cluster de UI, **Evict** o nó em pausa no cluster, ou use o [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet.  

        ![Captura mostrando a saída do cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Remover um nó de cluster usando [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  Reformatar a unidade do sistema e executar uma "instalação limpa do sistema operacional" do Windows Server 2016 no nó usando o **personalizado: Instalar somente o Windows (Avançado)** opção de instalação (consulte a Figura 13) no setup.exe. Evite escolher o **atualizar: Instalar Windows e manter os arquivos, configurações e aplicativos** opção, pois a atualização sem interrupção do Cluster do sistema operacional não incentivar a atualização in-loco.  

        ![Captura do Assistente de instalação do Windows Server 2016, mostrando a opção de instalação personalizada selecionada](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opções de instalação disponíveis para o Windows Server 2016**  

    4.  Adicione o nó ao domínio do Active Directory apropriado.  
    5.  Adicione os usuários apropriados ao grupo Administradores.  
    6.  Usando o Gerenciador do servidor UI ou Install-WindowsFeature cmdlet do PowerShell, instale quaisquer funções de servidor que você precisa, como o Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Usando o Gerenciador do servidor UI ou Install-WindowsFeature cmdlet do PowerShell, instale o recurso de cluster de Failover.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Instale os recursos adicionais necessários para suas cargas de trabalho do cluster.  
    9. Verifique as configurações de conectividade de rede e armazenamento usando a UI do Gerenciador de Cluster de Failover.  
    10. Se o Firewall do Windows for usado, verifique se as configurações de Firewall estão corretas para o cluster. Por exemplo, clusters de Cluster com suporte a CAU (atualização) habilitada podem exigir a configuração de Firewall.  
    11. Para cargas de trabalho do Hyper-V, use a interface de usuário do Gerenciador do Hyper-V para iniciar a caixa de diálogo Gerenciador de comutador Virtual (veja a Figura 14).  

        Verifique se o nome da Switch(s) Virtual usado são idênticos para todos os nós de host do Hyper-V no cluster.  

        ![Captura mostrando o local da caixa de diálogo Gerenciador de comutador Virtual Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Gerenciador de comutador virtual**  

    12. Em um nó do Windows Server 2016 (não usar um nó do Windows Server 2012 R2), use o Gerenciador de Cluster de Failover (consulte a Figura 15) para se conectar ao cluster.  

        ![Captura mostrando a caixa de diálogo Selecionar cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Adicionando um nó ao cluster usando o Gerenciador de Cluster de Failover**  

    13. Use qualquer um da UI Gerenciador de Cluster de Failover ou o [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet (veja a Figura 16) para adicionar o nó ao cluster.  

        ![Captura mostrando a saída do cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: Adicionando um nó ao cluster usando [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > Quando o primeiro nó do Windows Server 2016 ingressa no cluster, o cluster entra no modo de "Sistema operacional misto" e os recursos principais de cluster são movidos para o nó do Windows Server 2016. Um modo "OS misto" é um cluster totalmente funcional em que os novos nós executam em um modo de compatibilidade com os nós antigos. "OS misto" é um modo transitório para o cluster. Ele não deve ser permanente e os clientes devem atualizar todos os nós do cluster nas quatro semanas.  

    14. Após o Windows Server 2016 nó for adicionado com êxito para o cluster, você pode (opcionalmente) mover alguns da carga de trabalho do cluster para o nó recém-adicionado para redistribuir a carga de trabalho no cluster da seguinte maneira:

        ![Captura mostrando a saída do cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Movendo uma carga de trabalho (função VM do cluster) do cluster usando [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. Use **migração ao vivo** do Gerenciador de Cluster de Failover para máquinas virtuais ou o [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet (consulte a Figura 17) para executar uma migração ao vivo das máquinas virtuais.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Use **mova** do Gerenciador de Cluster de Failover ou o [ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) cmdlet para outras cargas de trabalho do cluster.  

3. Quando todos os nós foi atualizado para o Windows Server 2016 e adicionado de volta ao cluster, ou quando os nós restantes do Windows Server 2012 R2 tiverem sido excluídos, faça o seguinte:  

    > [!IMPORTANT]  
    > -   Depois de atualizar o nível funcional do cluster, você não poderá voltar para o nível funcional do Windows Server 2012 R2 e Windows Server 2012 R2 nós não podem ser adicionados ao cluster.
    > -   Até que o [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet é executado, o processo é totalmente reversível e nós do Windows Server 2012 R2 podem ser adicionados a este cluster e nós do Windows Server 2016 podem ser removidos.  
    > -   Após o [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet é executado, novos recursos estarão disponíveis.  

    1.  Usando a UI do Gerenciador de Cluster de Failover ou o [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet, verifique se todas as funções de cluster estão em execução no cluster conforme o esperado. No exemplo a seguir, o armazenamento disponível não está sendo usado, em vez disso, o CSV é usado, portanto, o armazenamento disponível exibe uma **Offline** status (consulte a Figura 18).  

        ![Captura mostrando a saída do cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: Verificando se os grupos (funções de cluster) de cluster estão em execução usando o [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet**  

    2.  Verifique se todos os nós de cluster estão online e em execução usando o [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet.  
    3.  Execute o [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet - não há erros devem ser retornados (consulte a Figura 19).  

        ![Captura mostrando a saída do cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: Atualizando o nível funcional de um cluster usando o PowerShell**  

    4.  Após o [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet é executado, novos recursos estão disponíveis.  

4. Windows Server 2016 - retomar backups e atualizações de cluster normais:  

    1. Se anteriormente você estava executando o CAU, reiniciá-lo usando a UI do CAU ou use o [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet (veja a Figura 20).  

        ![Captura mostrando a saída do Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Habilitar as atualizações com suporte de Cluster de função usando o [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet**  

    2. Retomar operações de backup.  

5. Habilitar e usar os recursos do Windows Server 2016 em máquinas virtuais do Hyper-V.  

    1. Depois que o cluster tiver sido atualizado para o nível funcional do Windows Server 2016, muitas cargas de trabalho, como VMs do Hyper-V terá novos recursos. Para obter uma lista dos novos recursos do Hyper-V. consulte [máquinas de virtuais de atualização e migração](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. Em cada nó de host do Hyper-V no cluster, use o [ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) cmdlet para exibir as versões de configuração de VM do Hyper-V são compatíveis com o host.  

        ![Captura mostrando a saída do cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Exibindo as versões de configuração de VM do Hyper-V com suporte pelo host**  

   3. Em cada nó de host do Hyper-V no cluster, versões de configuração da VM do Hyper-V podem ser atualizadas pelo agendamento de uma janela de manutenção breve com usuários, fazendo backup, desativar as máquinas virtuais e executando o [ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) cmdlet (consulte Figura 22). Isso irá atualizar a versão da máquina virtual e habilitar novos recursos do Hyper-V, eliminando a necessidade de atualizações futuras do componente de integração do Hyper-V (IC). Esse cmdlet pode ser executado do nó do Hyper-V que hospeda a VM, ou o `-ComputerName` parâmetro pode ser usado para atualizar a versão da VM remotamente. Neste exemplo, aqui vamos atualizar a versão da configuração de VM1 do 5.0 para 7.0 para tirar proveito dos muitos novos recursos de Hyper-V associada com esta versão de configuração de VM, como pontos de verificação de produção (backups consistentes com o aplicativo) e a VM binário arquivo de configuração.  

       ![Captura mostrando o cmdlet Update-VMVersion em ação](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **Figura 22: Atualizando uma versão VM usando o cmdlet Update-VMVersion PowerShell**  

6. Pools de armazenamento podem ser atualizados usando o [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) cmdlet do PowerShell - esta é uma operação online.  

Embora estamos visando cenários de nuvem privada, especificamente o Hyper-V e clusters de servidor de arquivos de escalabilidade horizontal, que podem ser atualizados sem tempo de inatividade, o processo de atualização sem interrupção do Cluster do sistema operacional podem ser usados para qualquer função de cluster.  

## <a name="restrictions--limitations"></a>Restrições / limitações  
- Esse recurso funciona somente para o Windows Server 2012 R2 para apenas a versões do Windows Server 2016. Esse recurso não pode atualizar versões anteriores do Windows Server, como o Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para Windows Server 2016.  
- Cada nó do Windows Server 2016 deve ser apenas reformatado/nova instalação. "In-loco" ou "Atualizar" tipo de instalação não é recomendado.  
- Um nó do Windows Server 2016 deve ser usado para adicionar nós do Windows Server 2016 para o cluster.  
- Ao gerenciar um cluster de modo misto do sistema operacional, sempre execute as tarefas de gerenciamento de um nó de nível superior que está executando o Windows Server 2016. Nós de nível inferior do Windows Server 2012 R2 não é possível usar ferramentas de interface do usuário ou de gerenciamento no Windows Server 2016.  
- Incentivamos os clientes a percorrer o processo de atualização de cluster rapidamente, porque alguns recursos de cluster não são otimizados para o modo misto do sistema operacional.  
- Evitar a criação ou redimensionamento de armazenamento no Windows Server 2016 nós enquanto o cluster está em execução no modo misto do sistema operacional devido a possíveis incompatibilidades no failover de um nó do Windows Server 2016 a nós do Windows Server 2012 R2 de nível inferior.  

## <a name="frequently-asked-questions"></a>Perguntas frequentes  
**Quanto tempo o cluster de failover executar no modo misto do sistema operacional?**  
    Encorajamos os clientes para concluir a atualização nas quatro semanas. Há muitas otimizações no Windows Server 2016. Hyper-V foi atualizado com êxito e Scale-out File Server clusters com zero tempo de inatividade em menos de quatro horas total.  

**Você será compatibilizar esse recurso de volta para o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008?**  
    Não temos planos para esse recurso para as versões anteriores da porta. Atualização sem interrupção do cluster do sistema operacional é a nossa visão para atualizar clusters do Windows Server 2012 R2 para Windows Server 2016 e muito mais.  

**O cluster do Windows Server 2012 R2 precisa ter todas as atualizações de software instaladas antes de iniciar o processo de atualização sem interrupção do Cluster do sistema operacional?**  
    Sim, antes de iniciar o processo de atualização sem interrupção do Cluster do sistema operacional, verifique se que todos os nós de cluster são atualizados com as últimas atualizações de software.  

**É possível executar o [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet enquanto nós desativados ou em pausa?**  
    Não. Todos os nós de cluster devem estar em e na associação ativa para o [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet funcione.  

**Atualização sem interrupção do Cluster do sistema operacional funciona para qualquer carga de trabalho do cluster? Ele funciona para o SQL Server?**  
    Sim, a atualização sem interrupção do Cluster do sistema operacional funciona para qualquer carga de trabalho do cluster. No entanto, é apenas sem tempo de inatividade para Hyper-V e clusters de servidor de arquivos de escalabilidade horizontal. A maioria das outras cargas de trabalho incorrerá em algum tempo de inatividade (normalmente alguns minutos) quando eles failover e o failover é necessário pelo menos uma vez durante o processo de atualização sem interrupção do Cluster do sistema operacional.  

**É possível automatizar esse processo usando o PowerShell?**  
    Sim, nós criamos atualização SO do Cluster sem interrupção para ser automatizada usando o PowerShell.  

**Para um cluster grande que tem a carga de trabalho extra e a capacidade de failover, pode atualizar vários nós simultaneamente?**  
    Sim. Quando um nó é removido do cluster para atualizar o sistema operacional, o cluster terá uma menor nó para o failover, portanto, terá uma capacidade reduzida de failover. Para grandes clusters com suficiente capacidade de failover e a carga de trabalho, vários nós podem ser atualizados ao mesmo tempo. Você pode adicionar nós do cluster temporariamente para o cluster para fornecer a carga de trabalho aprimorada e capacidade de failover durante o processo de atualização sem interrupção do Cluster do sistema operacional.  

**E se eu descobrir um problema no meu cluster após [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) foi executado com êxito?**  
    Se você tiver feito backup o banco de dados do cluster com um backup de estado do sistema antes da execução [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), você deve ser capaz de realizar um autoritativo restaure em um nó de cluster do Windows Server 2012 R2 e do cluster original banco de dados e configuração.  

**Pode usar o atualização in-loco para cada nó em vez de usar a instalação do SO limpar reformatando a unidade do sistema?**  
    Não recomendamos o uso de atualização in-loco do Windows Server, mas estamos cientes de que ele funciona em alguns casos em que os drivers padrão são usadas. Leia atentamente todas as mensagens de aviso são exibidas durante a atualização in-loco de um nó de cluster.  

**Se eu estiver usando replicação de Hyper-V para uma VM do Hyper-V no meu cluster do Hyper-V, replicação permanecerão intacta durante e após o processo de atualização sem interrupção do Cluster do sistema operacional?**  
    Sim, a réplica do Hyper-V permanece intacta durante e após o processo de atualização sem interrupção do Cluster do sistema operacional.  

**Pode usar o System Center 2016 Virtual Machine Manager (SCVMM) para automatizar o processo de atualização sem interrupção do Cluster do sistema operacional?**  
    Sim, você pode automatizar o processo de atualização sem interrupção do Cluster do sistema operacional usando o VMM no System Center 2016.  

## <a name="see-also"></a>Consulte também  
-   [Notas de versão: Problemas importantes no Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Novidades no Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [O que há de novo no Clustering de Failover no Windows Server](whats-new-in-failover-clustering.md)  
