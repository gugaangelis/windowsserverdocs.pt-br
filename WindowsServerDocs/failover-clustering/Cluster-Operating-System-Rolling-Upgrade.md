---
title: "Atualização do sistema operacional sem interrupção do cluster"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Atualização do sistema operacional sem interrupção do cluster

> Aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Atualização de rolamento de sistema operacional do cluster permite que um administrador atualizar o sistema operacional de nós de cluster sem interromper o Hyper-V ou as cargas de trabalho do servidor de arquivos Scale-Out. Usando esse recurso, as penalidades de tempo de inatividade contra contratos de nível de serviço (SLA) podem ser evitadas.

Atualização de rolamento de sistema operacional do cluster fornece os seguintes benefícios:

- Clusters de failover executam máquina virtual Hyper-V e cargas de trabalho do servidor de arquivos de recusa de escala (SOFS) podem ser atualizados do Windows Server 2012 R2 (em execução em todos os nós do cluster) para Windows Server 2016 (em execução em todos os nós de cluster do cluster) sem tempo de inatividade. Outras cargas de trabalho de cluster, como o SQL Server, estarão disponíveis durante o tempo (normalmente menos de cinco minutos) de failover para o Windows Server 2016.  
- Ele não requer qualquer hardware adicional. Embora, você pode adicionar outros nós de cluster temporariamente para clusters pequenos para melhorar a disponibilidade do cluster durante o sistema operacional sem interrupção de atualização de Cluster processam.  
- O cluster não precisa ser interrompido ou reiniciado.  
- Um novo cluster não é necessário. O cluster existente é atualizado. Além disso, os objetos de cluster existentes armazenados no Active Directory são usados.  
- O processo de atualização é reversível até que podem o cliente o "ponto-de-não-retorno", quando todos os nós de cluster está executando o Windows Server 2016 e quando o cmdlet do PowerShell Update-ClusterFunctionalLevel é executado.  
- O cluster pode dar suporte a operações de manutenção e aplicação de patch durante a execução no modo misto do sistema operacional.  
- Ele dá suporte à automação por meio do PowerShell e WMI.  
- A propriedade pública de cluster **ClusterFunctionalLevel** propriedade indica o estado do cluster no Windows Server 2016 nós de cluster. Esta propriedade pode ser consultada usando o cmdlet do PowerShell de um nó de cluster do Windows Server 2016 ao qual pertence a um cluster de failover:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    Um valor de **8** indica se o cluster está em execução no nível funcional do Windows Server 2012 R2. Um valor de **9** indica se o cluster está em execução no nível funcional do Windows Server 2016.  

Este guia descreve os diversos estágios do processo de atualização de rolamento de sistema operacional do Cluster, etapas de instalação, limitações de recurso e perguntas frequentes (perguntas frequentes) e é aplicável para os seguintes cenários de atualização de rolamento de sistema operacional do Cluster no Windows Server 2016:  
- Clusters Hyper-V  
- Clusters de servidor de arquivos fora de escala  

O seguinte cenário não é suportado no Windows Server 2016:  
-  Sistema operacional sem interrupção de atualização de cluster de convidado clusters usando o disco rígido virtual (.vhdx file) como armazenamento compartilhado  

Atualização de rolamento de sistema operacional do cluster é totalmente compatível pelo sistema Center Virtual Machine Manager (SCVMM) 2016. Se você estiver usando SCVMM 2016, consulte [clusters de atualizar o Windows Server 2012 R2 para o Windows Server 2016 no VMM](https://technet.microsoft.com/library/mt445417.aspx) para obter orientação sobre como atualizar os clusters e automatizar as etapas descritas neste documento.  

## <a name="requirements"></a>Requisitos  
Conclua os requisitos a seguir antes de começar o processo de atualização de rolamento de sistema operacional do Cluster:

- Comece com um Cluster de Failover executando o Windows Server 2012 R2, Windows Server 2016 ou Windows Server (anual por canal).
- Atualizando um cluster de espaços de armazenamento direto para o Windows Server, versão 1709 não é suportado.
- Se a carga de trabalho do cluster for VMs do Hyper-V ou servidor de arquivos Scale-Out, você pode esperar a atualização de inatividade.
- Verifique se os nós do Hyper-V tem CPUs que dão suporte a segundo nível endereçamento tabela (SLAT) usando um dos métodos a seguir;  
        -Revisar o [tem SLAT compatíveis? WP8 SDK dica 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) artigo que descreve os dois métodos para verificar se uma CPU dá suporte a SLATs  
        -Baixar o [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) ferramenta para determinar se uma CPU dá suporte a SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Estados de transição de cluster durante a atualização de rolamento de sistema operacional do Cluster

Esta seção descreve os vários estados de transição do Windows Server 2012 R2 cluster que está sendo atualizado para o Windows Server 2016 usando a atualização de rolamento de sistema operacional do Cluster.  

Para manter as cargas de trabalho de cluster em execução durante o processo de atualização de rolamento de sistema operacional do Cluster, mudando de uma carga de trabalho de cluster de um nó Windows Server 2012 R2 para o Windows Server 2016 nó funciona como se ambos os nós estavam executando o sistema operacional Windows Server 2012 R2. Quando nós do Windows Server 2016 são adicionadas ao cluster, eles operam em um modo de compatibilidade do Windows Server 2012 R2. Um novo modo de cluster conceitual, chamado de "Modo misto do sistema operacional", permite que os nós de versões diferentes de existir no mesmo do cluster (veja a Figura 1).  

![Ilustração mostrando os três estágios de uma atualização sem interrupção de cluster SO: todos os nós Windows Server 2012 R2, o modo misto do sistema operacional e todos os nós Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figura 1: As transições de estado do sistema operacional do Cluster**  

Um cluster do Windows Server 2012 R2 entra no modo misto do sistema operacional quando um nó Windows Server 2016 é adicionado ao cluster. O processo é totalmente reversível - Windows Server 2016 nós podem ser removidos do cluster e Windows Server 2012 R2 nós podem ser adicionados ao cluster nesse modo. O "ponto de onde não há retorno" ocorre quando o cmdlet do PowerShell Update-ClusterFunctionalLevel é executado no cluster. Em ordem para este cmdlet para ter sucesso, todos os nós devem ser Windows Server 2016 e todos os nós devem estar online.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>Estados de transição de um cluster de quatro nós durante a execução de rolamento atualização do sistema operacional

Esta seção ilustra e descreve os quatro diferentes estágios de um cluster com armazenamento compartilhado cujos nós forem atualizados do Windows Server 2012 R2 para o Windows Server 2016.  

"Estágio 1" é o estado inicial - começamos com um cluster do Windows Server 2012 R2.  

![Ilustração que mostra o estado inicial: todos os nós Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figura 2: Inicial estado: Cluster de Failover do Windows Server 2012 R2 (etapa 1)**  

Na "etapa 2", dois nós foram pausadas, drenagem, removidos, reformatadas e instalados com o Windows Server 2016.  

![Ilustração mostrando o cluster no modo misto do sistema operacional: fora do cluster de nó 4 do exemplo, dois nós estão executando o Windows Server 2016 e dois nós estão executando o Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figura 3: Intermediário estado: o modo misto do sistema operacional: Windows Server 2012 R2 e Windows Server 2016 Failover cluster (estágio 2)**  

Na "etapa 3", todos os nós do cluster foram atualizados para o Windows Server 2016 e está pronto para ser atualizado com o cmdlet do PowerShell Update-ClusterFunctionalLevel no cluster.  

> [!NOTE]  
> Nesse estágio, o processo pode ser revertido totalmente e Windows Server 2012 R2 nós podem ser adicionados a esse cluster.  

![Ilustração que mostra que o cluster foi totalmente atualizado para o Windows Server 2016 e está pronto para o cmdlet Update-ClusterFunctionalLevel trazer o nível funcional do cluster até Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figura 4: Intermediário estado: todos os nós atualizou para o Windows Server 2016, pronto para Update-ClusterFunctionalLevel (estágio 3)**  

Depois que o Update-ClusterFunctionalLevelcmdlet for executado, o cluster entra "Etapa 4", onde os novos recursos de cluster do Windows Server 2016 podem ser usados.  

![Ilustração que mostra que a atualização do sistema operacional sem interrupção de cluster foi concluído com êxito; todos os nós foram atualizados para o Windows Server 2016 e o cluster está em execução no nível funcional do Windows Server 2016 cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figura 5: Final estado: Cluster de Failover do Windows Server 2016 (etapa 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Sistema operacional de cluster Implantando o processo de atualização

Esta seção descreve o fluxo de trabalho para realizar a atualização de rolamento de sistema operacional do Cluster.  

![Ilustração que mostra o fluxo de trabalho para atualizar um cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figura 6: Cluster SO Implantando o fluxo de trabalho do processo de atualização**  

Atualização de sistema operacional sem interrupção de cluster inclui as seguintes etapas:  

1. Prepare o cluster para a atualização do sistema operacional da seguinte maneira:  
    1. Atualização de rolamento de sistema operacional do cluster requer removendo um nó de cada vez do cluster. Verifique se você tem capacidade suficiente no cluster manter SLAs HA quando um de nós de cluster é removido do cluster para uma atualização do sistema operacional. Em outras palavras, exigem a capacidade de cargas de trabalho de failover para outro nó quando um nó é removido do cluster durante o processo de atualização de rolamento de sistema operacional do Cluster? O cluster tem a capacidade de executar as cargas de trabalho necessárias quando um nó é removido do cluster para Cluster SO rolamento atualizar?  
    2. Para cargas de trabalho do Hyper-V, verifique se todos os hosts do Windows Server 2016 Hyper-V tem CPU dar suporte a tabela de endereço de segundo nível (SLAT). Apenas os computadores compatíveis com SLAT podem usar a função Hyper-V no Windows Server 2016.  
    3. Verifique se todos os backups carga de trabalho concluíram e considere a possibilidade de fazer o backup de cluster. Pare de operações de backup durante a adição de nós de cluster.  
    4. Verifique se todos os nós de cluster estão online/execução/para cima usando o [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)cmdlet (ver Figura 7).  

        ![Screencap mostrando os resultados da execução o cmdlet Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figura 7: Determinando o status de nó usando o cmdlet Get-ClusterNode**  

    5. Se você estiver executando atualizações ciente do Cluster (CAU), verifique se CAU está sendo executado usando o **ciente do Cluster Atualizando** interface do usuário, ou o [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)cmdlet (ver Figura 8). Parar de usar CAU o [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)cmdlet (veja a Figura 9) para impedir que todos os nós de ser pausado e drenagem por CAU durante o processo de atualização de rolamento de sistema operacional do Cluster.  

        ![Screencap mostrando a saída do cmdlet Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figura 8: Usando o [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)cmdlet para determinar se as atualizações de reconhecimento de Cluster está em execução no cluster**  

        ![Screencap mostrando a saída do cmdlet Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figura 9: Desabilitando a função de atualizações de reconhecimento de Cluster usando o [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)cmdlet**  

2. Para cada nó no cluster, execute o seguinte:  
    1. Usando o Gerenciador de Cluster de UI, selecione um nó e use o **pausar | Drain** opção de menu para o nó de consumo (veja a Figura 10) ou usar o [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)cmdlet (veja a Figura 11).  

        ![Screencap mostrando como funções com a UI do Gerenciador de Cluster de consumo](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figura 10: Funções de descarga de um nó usando o Gerenciador de Cluster de Failover**  

        ![Screencap mostrando a saída do cmdlet Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figura 11: Drenagem funções de um nó usando o [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)cmdlet**  

    2.  Usando o Gerenciador de Cluster de UI, **remover** o nó pausado do cluster ou use o [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)cmdlet.  

        ![Screencap mostrando a saída do cmdlet Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figura 12: Remover um nó de cluster usando [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)cmdlet**  

    3.  Reformatar a unidade do sistema e executar uma "instalação limpa do sistema operacional" do Windows Server 2016 no nó usando o **personalizado: instalar o Windows somente (Avançado)** opção de instalação (ver Figura 13) em setup.exe. Evite selecionar o **atualizar: instalar o Windows e manter arquivos, configurações e aplicativos** opção desde que a atualização de rolamento de sistema operacional do Cluster não incentivar a atualização in-loco.  

        ![Screencap do Assistente de instalação do Windows Server 2016 mostrando a opção de instalação personalizada selecionada](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figura 13: Opções disponíveis para o Windows Server 2016**  

    4.  Adicione o nó para o domínio do Active Directory apropriado.  
    5.  Adicione os usuários apropriados ao grupo Administradores.  
    6.  Usando o cmdlet UI Gerenciador do servidor ou Install-WindowsFeature PowerShell, instale qualquer funções de servidor que você precisa, como o Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  Usando o cmdlet UI Gerenciador do servidor ou Install-WindowsFeature PowerShell, instale o recurso de cluster de Failover.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Instale recursos adicionais necessários para as cargas de trabalho do cluster.  
    9. Verifique as configurações de conectividade de rede e armazenamento usando o Gerenciador de Cluster de Failover UI.  
    10. Se o Firewall do Windows for usado, verifique se as configurações do Firewall estão corretas para o cluster. Por exemplo, clusters de Cluster ciente atualizando (CAU) habilitada podem exigir configuração do Firewall.  
    11. Para cargas de trabalho do Hyper-V, usar a interface do usuário do Gerenciador do Hyper-V para iniciar a caixa de diálogo Gerenciador de comutador Virtual (veja a Figura 14).  

        Verifique se o nome da switches Virtual usado são idênticas para todos os nós de host do Hyper-V no cluster.  

        ![Screencap mostrando o local da caixa de diálogo Gerenciador do Hyper-V Virtual Switch](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figura 14: Gerenciador de comutador Virtual**  

    12. Em um nó Windows Server 2016 (não usar um nó Windows Server 2012 R2), use o Gerenciador de Cluster de Failover (veja a Figura 15) para se conectar ao cluster.  

        ![Screencap mostrando a caixa de diálogo Selecione cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figura 15: Adicionando um nó de cluster usando o Gerenciador de Cluster de Failover**  

    13. Use qualquer a UI Gerenciador de Cluster de Failover ou os [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)cmdlet (veja a Figura 16) para adicionar o nó de cluster.  

        ![Screencap mostrando a saída do cmdlet Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figura 16: Adicionando um nó para o cluster usando [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)cmdlet**  

        > [!NOTE]  
        > Quando o primeiro nó Windows Server 2016 ingressa no cluster, o cluster entra no modo de "Misto-OS" e os principais recursos de cluster são movidos para o nó Windows Server 2016. Um modo "Misto-OS" é um cluster totalmente funcional onde os novos nós é executado em um modo de compatibilidade com os nós antigos. Modo de "Misto do sistema operacional" é um modo transitório para o cluster. Ele não deve ser permanente e os clientes devem atualizar todos os nós de cluster em quatro semanas.  

    14. Após o Windows Server 2016 nó é adicionado com êxito ao cluster, você pode (opcionalmente) mover alguns da carga de trabalho do cluster ao nó recém-adicionado para rebalancear a carga de trabalho no cluster da seguinte maneira:

        ![Screencap mostrando a saída do cmdlet Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figura 17: Mover uma carga de trabalho (papel VM do cluster) do cluster usando [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)cmdlet**  

        1. Use **migração dinâmica** do Gerenciador de Cluster de Failover para máquinas virtuais ou o [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)cmdlet (veja a Figura 17) para executar uma migração ao vivo de máquinas virtuais.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Use **mover** do Gerenciador de Cluster de Failover ou os [`Move-ClusterGroup`](https://technet.microsoft.com/library/ee461002.aspx)cmdlet para outras cargas de trabalho do cluster.  

3. Quando cada nó foi atualizado para o Windows Server 2016 e adicionada de volta para o cluster, ou quando os demais nós do Windows Server 2012 R2 tiverem sido excluídos, faça o seguinte:  

    > [!IMPORTANT]  
    > -   Depois de atualizar o nível funcional do cluster, você não poderá voltar para o nível funcional do Windows Server 2012 R2 e Windows Server 2012 R2 nós não podem ser adicionados ao cluster.
    > -   Até que o [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet é executado, o processo é totalmente reversível e Windows Server 2012 R2 nós podem ser adicionados a esse cluster e Windows Server 2016 nós podem ser removidas.  
    > -   Após a [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet é executado, novos recursos estarão disponíveis.  

    1.  Usando o Gerenciador de Cluster de Failover UI ou o [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)cmdlet, verifique se todas as funções de cluster são executados no cluster conforme o esperado. No exemplo a seguir, o armazenamento disponível não está sendo usado, em vez disso, CSV é usada, portanto, o armazenamento disponível exibe um **off-line** status (veja a Figura 18).  

        ![Screencap mostrando a saída do cmdlet Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figura 18: Verificando que todos os grupos (funções de cluster) do cluster estão em execução usando o [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)cmdlet**  

    2.  Verifique se todos os nós de cluster estão online e em execução usando o [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)cmdlet.  
    3.  Execute o [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet - sem erros devem ser retornados (veja a Figura 19).  

        ![Screencap mostrando a saída do cmdlet Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figura 19: Atualizando o nível funcional de um cluster usando o PowerShell**  

    4.  Após a [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet é executado, novos recursos estão disponíveis.  

4. Windows Server 2016 - retomar backups e atualizações de cluster normal:  

    1. Se você estava executando o CAU, reiniciá-lo usando o CAU UI ou usar o [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)cmdlet (veja a Figura 20).  

        ![Screencap mostrando a saída do Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figura 20: Habilitar atualizações de reconhecimento de Cluster função usando o [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)cmdlet**  

    2. Retomar operações de backup.  

5. Ativar e usar os recursos do Windows Server 2016 em máquinas virtuais do Hyper-V.  

    1. Depois que o cluster foi atualizado para o nível funcional do Windows Server 2016, várias cargas de trabalho como VMs do Hyper-V terá novos recursos. Para obter uma lista de novos recursos do Hyper-V. Consulte [migrar e atualizar máquinas virtuais](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. Em cada nó de host do Hyper-V no cluster, use o [`Get-VMHostSupportedVersion`](https://technet.microsoft.com/library/mt653838.aspx)cmdlet para exibir as versões de configuração de VM do Hyper-V que são compatíveis com o host.  

        ![Screencap mostrando a saída do cmdlet Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figura 21: Exibindo as versões de configuração de VM do Hyper-V com suporte pelo host**  

   3.  Em cada nó de host do Hyper-V no cluster, versões de configuração da VM do Hyper-V podem ser atualizadas pelo agendamento de uma janela de manutenção breve com os usuários, fazer backup, desativando máquinas virtuais e executando o [`Update-VMVersion`](https://technet.microsoft.com/library/mt484146.aspx)cmdlet (veja a Figura 22). Isso irá atualizar a versão de máquina virtual e habilitar recursos novos do Hyper-V, eliminando a necessidade de atualizações futuras do componente de integração do Hyper-V (IC). Este cmdlet pode ser executado a partir do nó do Hyper-V que hospeda a VM ou o `-ComputerName`parâmetro pode ser usado para atualizar a versão de VM remotamente. Neste exemplo, aqui estamos atualizar a versão de configuração do VM1 do 5.0 para 7.0 para tirar proveito dos novos recursos de Hyper-V muitos associados a esta versão de configuração VM, como pontos de verificação de produção (aplicativo consistente backups) e arquivo de configuração de VM binário.  

        ![Screencap mostrando o cmdlet Update-VMVersion em ação](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **Figura 22: Atualizando uma versão VM usando o cmdlet do PowerShell Update-VMVersion**  

4.  Pools de armazenamento podem ser atualizados usando o [StoragePool atualização](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) cmdlet do PowerShell - esta é uma operação online.  

Embora nós se destinam a cenários de nuvem privada, especificamente Hyper-V e clusters de servidor de arquivos fora de escala, que podem ser atualizados sem tempo de inatividade, o processo de atualização de rolamento de sistema operacional do Cluster podem ser usados para qualquer função de cluster.  

## <a name="restrictions--limitations"></a>Restrições / limitações  
- Esse recurso funciona apenas para o Windows Server 2012 R2 para versões do Windows Server 2016 apenas. Esse recurso não pode atualizar as versões anteriores do Windows Server, como Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para Windows Server 2016.  
- Cada nó Windows Server 2016 deve ser reformatada/nova instalação somente. "In-loco" ou "Atualizar" tipo de instalação não é recomendado.  
- Um nó Windows Server 2016 deve ser usado para adicionar nós do Windows Server 2016 ao cluster.  
- Ao gerenciar um cluster de modo misto do sistema operacional, sempre execute as tarefas de gerenciamento de um nó de nível superior que está executando o Windows Server 2016. Nós de nível inferior Windows Server 2012 R2 não podem usar ferramentas de gerenciamento ou de interface do usuário contra o Windows Server 2016.  
- Recomendamos que os clientes a percorrer o processo de atualização do cluster rapidamente, pois alguns recursos de cluster não são otimizados para o modo misto do sistema operacional.  
- Evite criar ou redimensionamento armazenamento no Windows Server 2016 nós enquanto o cluster está em execução no modo misto do sistema operacional por causa de possíveis incompatibilidades em failover de um nó Windows Server 2016 a nós do Windows Server 2012 R2 de nível inferior.  

## <a name="frequently-asked-questions"></a>Perguntas frequentes  
**Quanto tempo pode cluster de failover executar no modo misto do sistema operacional?**  
    Recomendamos que os clientes para concluir a atualização em quatro semanas. Há várias dessas otimizações no Windows Server 2016. Hyper-V foi atualizado com êxito e o servidor de arquivos escalonadas clusters com inatividade em menos de quatro horas total.  

**Você serão portados esse recurso de volta para o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008?**  
    Não temos planos para portar esse recurso para versões anteriores. Atualização de rolamento de sistema operacional do cluster é nossa visão para atualizar o Windows Server 2012 R2 clusters a Windows Server 2016 e muito mais.  

**O Windows Server 2012 R2 cluster precisa tem todas as atualizações de software instaladas antes de iniciar o processo de atualização de rolamento de sistema operacional do Cluster?**  
    Sim, antes de iniciar o processo de atualização de rolamento de sistema operacional do Cluster, verifique se que todos os nós de cluster são atualizados com as últimas atualizações de software.  

**Posso executar o [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet enquanto nós estão desativados ou pausada?**  
    Não. Todos os nós de cluster devem estar em e na associação ativa para o [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)cmdlet para trabalhar.  

**Atualização de rolamento de sistema operacional do Cluster funciona para qualquer carga de trabalho do cluster? Isso funciona para SQL Server?**  
    Sim, sem interrupção de atualização de Cluster SO funciona para qualquer carga de trabalho do cluster. No entanto, é somente inatividade para Hyper-V e clusters de servidor de arquivos fora de escala. A maioria das outras cargas de trabalho gera uma pausa (normalmente alguns minutos) quando eles failover e failover é necessário pelo menos uma vez durante o processo de atualização de rolamento de sistema operacional do Cluster.  

**Eu automatizar esse processo usando o PowerShell?**  
    Sim, nós criamos Cluster SO rolamento atualizar para ser automatizado usando o PowerShell.  

**Para um cluster grande com carga de trabalho extra e capacidade de failover, posso atualizar vários nós simultaneamente?**  
    Sim. Quando um nó é removido do cluster para atualizar o sistema operacional, cluster terá um menos nó de failover, portanto, terá uma capacidade de failover reduzido. Para grandes clusters com suficiente carga de trabalho e a capacidade de failover, vários nós podem ser atualizados simultaneamente. Você pode temporariamente adicionar nós de cluster ao cluster para fornecer a carga de trabalho aprimorada e capacidade de failover durante o processo de atualização de rolamento de sistema operacional do Cluster.  

**E se eu detectar um problema no meu cluster após [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)foi executado com êxito?**  
    Se você tiver backup do banco de dados do cluster com um backup do estado do sistema antes de executar [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx), você deve ser capaz de executar uma autoritativas restaurar em um nó de cluster do Windows Server 2012 R2 e restaurar o banco de dados de cluster original e a configuração.  

**Pode usar atualização in-loco para cada nó em vez de usar a instalação limpa do sistema operacional, reformatando a unidade do sistema?**  
    Não recomendamos o uso de atualização in-loco do Windows Server, mas estamos cientes de que ele funcione em alguns casos em que os drivers padrão são usadas. Leia atentamente todas as mensagens de aviso exibida durante a atualização in-loco de um nó de cluster.  

**Se estou usando replicação do Hyper-V para uma VM Hyper-V no meu cluster Hyper-V, replicação permanecerá intacta durante e após o processo de atualização de rolamento de sistema operacional do Cluster?**  
    Sim, réplica do Hyper-V permanece intacta durante e após o processo de atualização de rolamento de sistema operacional do Cluster.  

**Pode usar o sistema Center 2016 Virtual Machine Manager (SCVMM) para automatizar o processo de atualização de rolamento de sistema operacional do Cluster?**  
    Sim, você pode automatizar o processo de atualização de rolamento de sistema operacional do Cluster usando VMM no System Center 2016.  

## <a name="see-also"></a>Consulte também  
-   [Notas de versão: Problemas importantes no Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Quais são as novidades no Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [O que há de novo no Failover Clustering no Windows Server](whats-new-in-failover-clustering.md)  