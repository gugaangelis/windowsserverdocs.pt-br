---
title: Atualizar um cluster de espaços de armazenamento diretos para o Windows Server 2019
description: Como atualizar um cluster de espaços de armazenamento diretos para o Windows Server 2019, enquanto mantém as VMs em execução ou enquanto ele estiverem interrompidos.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9db92aa33cde9b2beed11149dae06bb3af2b5a03
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455415"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Atualizar um cluster de espaços de armazenamento diretos para o Windows Server 2019

Este tópico descreve como atualizar um cluster de espaços de armazenamento diretos para o Windows Server 2019. Há quatro abordagens para atualizar um cluster de espaços de armazenamento diretos do Windows Server 2016 para Windows Server 2019, usando o [execução do processo de atualização do sistema operacional do cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) — duas que envolvem manter as VMs em execução e duas que envolvem parar todas as VMs. Cada abordagem tem diferentes pontos fortes e fracos, portanto, selecione que um que melhor atenda às necessidades da sua organização:

- **Atualização in-loco durante a execução de VMs** em cada servidor no cluster — essa opção resulta em nenhum tempo de inatividade da VM, mas você precisará aguardar trabalhos de armazenamento (reparo espelho) ser concluído depois que cada servidor é atualizado.

- **Instalação limpa do sistema operacional durante a execução de VMs** em cada servidor no cluster — essa opção resulta em nenhum tempo de inatividade da VM, mas você precisará aguardar trabalhos de armazenamento (reparo espelho) concluir depois de cada servidor é atualizado, e você terá que definir cada servidor e todos os seus aplicativos e funções novamente.

- **Atualização in-loco enquanto as VMs são interrompidas** em cada servidor no cluster — essa opção resulta em tempo de inatividade da VM, mas você não precisa esperar para trabalhos de armazenamento (reparo espelho), portanto, é mais rápido.

- **Limpar-instalação do sistema operacional enquanto as VMs são interrompidas** em cada servidor no cluster — essa opção resulta em tempo de inatividade da VM, mas você não precisa esperar para armazenamento trabalhos (reparo espelho), portanto, é mais rápido.

## <a name="prerequisites-and-limitations"></a>Pré-requisitos e limitações

Antes de prosseguir com a atualização:

- Verifique se você tem backups utilizáveis, caso haja problemas durante o processo de atualização.

- Verifique se o fornecedor do hardware tem um BIOS, firmware e drivers para os servidores que eles oferecerá suporte no Windows Server 2019.

Existem algumas limitações com o processo de atualização a serem consideradas:

- Para habilitar espaços de armazenamento diretos com compilações de 2019 do Windows Server anteriores ao 176693.292, os clientes talvez seja necessário entrar em contato com o suporte da Microsoft para chaves do registro que habilita a funcionalidade espaços de armazenamento diretos e rede definida pelo Software. Para obter mais informações, consulte Microsoft Knowledge Base [4464776 do artigo](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking).

- Ao atualizar um cluster com volumes ReFS, há algumas limitações:

- A atualização é totalmente suportada em volumes ReFS, no entanto, os volumes atualizados não se beneficiar de ReFS aprimoramentos no Windows Server 2019. Esses benefícios, como melhorar o desempenho de paridade de aceleração de espelho, exigem um volume do Windows Server 2019 ReFS recém-criado. Em outras palavras, você precisaria criar novos volumes usando o `New-Volume` cmdlet ou o Gerenciador do servidor. Aqui estão alguns dos aprimoramentos do ReFS obtém novos volumes:

    - **Bypass de mapa log**: uma melhoria de desempenho no ReFS que só se aplica aos sistemas (espaços de armazenamento diretos) clusterizados e não se aplica a pools de armazenamento autônomo.

    - **A compactação** melhorias na eficiência no Windows Server 2019 resiliente com vários volumes específicos.

- Antes de atualizar um servidor Windows Server 2016 espaços de armazenamento diretos do cluster, é recomendável colocar o servidor para o modo de manutenção do armazenamento. Para obter mais informações, consulte a seção eventos 5120 [solucionar problemas de espaços de armazenamento diretos](troubleshooting-storage-spaces.md). Embora esse problema foi corrigido no Windows Server 2016, é recomendável colocar cada servidor de espaços de armazenamento diretos no modo de manutenção de armazenamento durante a atualização como uma prática recomendada.

- Há um problema conhecido com os ambientes de rede definida pelo Software que usam o conjunto de opções. Esse problema envolve migrações ao vivo de VM do Hyper-V do Windows Server 2019 para o Windows Server 2016 (migração ao vivo para um sistema operacional anterior). Para garantir que as migrações ao vivo bem-sucedidas, é recomendável alterar uma configuração de rede VM em VMs que estão sendo migradas ao vivo de 2019 do Windows Server para o Windows Server 2016. Esse problema foi corrigido para Windows Server 2019 no pacote cumulativo de hotfix do D 2019-01, também conhecido como 17763.292 de compilação. Para obter mais informações, consulte Microsoft Knowledge Base [4476976 do artigo](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976).

Por causa dos problemas conhecidos acima, alguns clientes podem considerar a criação de um novo cluster do Windows Server 2019 e copiar dados do cluster antigo, em vez de atualizar seus clusters do Windows Server 2016 usando um dos quatro processos descritos abaixo.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Executar uma atualização in-loco, enquanto as VMs estão em execução

Essa opção resulta em nenhum tempo de inatividade da VM, mas você precisará aguardar trabalhos de armazenamento (reparo espelho) ser concluído depois que cada servidor é atualizado. Embora os servidores individuais serão reiniciados em sequência durante o processo de atualização, os servidores restantes no cluster, bem como todas as VMs, permanecerão em execução.

1. Verifique se todos os servidores no cluster tiveram instalado as atualizações mais recentes do Windows. Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o Microsoft Knowledge Base [4487006 do artigo](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de fevereiro de 2019). O número de build (consulte `ver` comando) deve ser 14393.2828 ou posterior.

2. Se você estiver usando a rede definida pelo Software com o conjunto de opções, abra uma sessão do PowerShell com privilégios elevados e execute o seguinte comando para desabilitar verificações de verificação de migração ao vivo de VM em todas as VMs no cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Execute as seguintes etapas em um servidor de cluster por vez:

   1. Use migração ao vivo de VM do Hyper-V para mover VMs em execução fora do servidor que você está prestes a atualizar.

   2. Pausar o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultas. É recomendável que essa etapa ser cuidadoso — se você já não mora migrar VMs fora do servidor, esse cmdlet fará que para você, portanto, você pode ignorar a etapa anterior se você preferir.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque o servidor no modo de manutenção do armazenamento, executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Execute o seguinte cmdlet para verificar se que o **OperationalStatus** valor estiver **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Executar uma instalação de atualização do Windows Server 2019 no servidor executando **setup.exe** e usando a opção "Manter aplicativos e arquivos pessoais". Após a instalação for concluída, o servidor permanecerá no cluster e o cluster de serviço é iniciado automaticamente.

   6. Verifique se o servidor recém-atualizado tem as atualizações mais recentes do Windows Server 2019. Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). O número de build (consulte `ver` comando) deve ser 17763.292 ou posterior.

   7. Remova o servidor de modo de manutenção do armazenamento usando o seguinte comando do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Retome o servidor usando o seguinte comando do PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Aguarde para trabalhos de reparo de armazenamento para concluir e todos os discos retornar ao estado íntegro. Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Aqui estão os comandos para executar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores foram atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente tenha até quatro semanas para fazê-lo.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o seguinte cmdlet para atualizar o pool de armazenamento. Nesse ponto, novos cmdlets, como `Get-ClusterPerf` será totalmente operacional em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, atualizar os níveis de configuração de VM, interrompendo a cada VM, usando o `Update-VMVersion` cmdlet e, em seguida, reiniciando as máquinas virtuais.

8. Se você estiver usando a rede definida pelo Software com o conjunto de opções e verificações de migração ao vivo de VM desativadas conforme instruído a acima, usam o seguinte cmdlet para habilitar novamente as verificações de VM ao vivo:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Verifique se o cluster atualizado funciona conforme o esperado. As funções devem fazer o failover corretamente e se a migração ao vivo da VM é usada no cluster, VMs devem migrar com êxito ao vivo.

10. Validar o cluster executando a validação de Cluster (`Test-Cluster`) e examinando o relatório de validação de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Executar uma instalação limpa do sistema operacional, enquanto as VMs estão em execução

Essa opção resulta em nenhum tempo de inatividade da VM, mas você precisará aguardar trabalhos de armazenamento (reparo espelho) ser concluído depois que cada servidor é atualizado. Embora os servidores individuais serão reiniciados em sequência durante o processo de atualização, os servidores restantes no cluster, bem como todas as VMs, permanecerão em execução.

1. Verifique se todos os servidores no cluster estão funcionando as atualizações mais recentes. Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o Microsoft Knowledge Base [4487006 do artigo](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de fevereiro de 2019). O número de build (consulte `ver` comando) deve ser 14393.2828 ou posterior.

2. Se você estiver usando a rede definida pelo Software com o conjunto de opções, abra uma sessão do PowerShell com privilégios elevados e execute o seguinte comando para desabilitar verificações de verificação de migração ao vivo de VM em todas as VMs no cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Execute as seguintes etapas em um servidor de cluster por vez:

   1. Use migração ao vivo de VM do Hyper-V para mover VMs em execução fora do servidor que você está prestes a atualizar.

   2. Pausar o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultas. É recomendável que essa etapa ser cuidadoso — se você já não mora migrar VMs fora do servidor, esse cmdlet fará que para você, portanto, você pode ignorar a etapa anterior se você preferir.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque o servidor no modo de manutenção do armazenamento, executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Execute o seguinte cmdlet para verificar se que o **OperationalStatus** valor estiver **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Remova o servidor do cluster executando o seguinte comando do PowerShell:  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Executar uma instalação limpa do Windows Server 2019 no servidor: Formatar a unidade de sistema, execute **setup.exe** e usar o "Nothing" opção. Você terá que configurar a identidade do servidor, funções, recursos e aplicativos após a conclusão da instalação e o servidor for reiniciado.

   5. Instale a função Hyper-V e o recurso de Clustering de Failover no servidor (você pode usar o `Install-WindowsFeature` cmdlet).

   6. Instale os drivers mais recentes armazenamento e rede para o hardware que foram aprovados pelo fabricante do servidor para uso com espaços de armazenamento diretos.

   7. Verifique se o servidor recém-atualizado tem as atualizações mais recentes do Windows Server 2019. Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). O número de build (consulte `ver` comando) deve ser 17763.292 ou posterior.

   8. Ingresse novamente o servidor ao cluster usando o seguinte comando do PowerShell:

       ```PowerShell
       Add-ClusterNode
       ```

   9. Remova o servidor de modo de manutenção de armazenamento usando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Aguarde para trabalhos de reparo de armazenamento para concluir e todos os discos retornar ao estado íntegro. Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Aqui estão os comandos para executar:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores foram atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente tenha até quatro semanas para fazê-lo.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o seguinte cmdlet para atualizar o pool de armazenamento. Nesse ponto, novos cmdlets, como `Get-ClusterPerf` será totalmente operacional em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, atualizar os níveis de configuração de VM parando cada VM e usando o `Update-VMVersion` cmdlet e, em seguida, reiniciando as máquinas virtuais.

8. Se você estiver usando a rede definida pelo Software com o conjunto de opções e verificações de migração ao vivo de VM desativadas conforme instruído a acima, usam o seguinte cmdlet para habilitar novamente as verificações de VM ao vivo:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Verifique se o cluster atualizado funciona conforme o esperado. As funções devem fazer o failover corretamente e se a migração ao vivo da VM é usada no cluster, VMs devem migrar com êxito ao vivo.

10. Validar o cluster executando a validação de Cluster (`Test-Cluster`) e examinando o relatório de validação de cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Realizar uma atualização in-loco, enquanto as VMs são interrompidas

Essa opção resulta em tempo de inatividade da VM, mas pode levar menos tempo do que se você manteve as VMs em execução durante a atualização, porque você não precisa esperar para trabalhos de armazenamento (reparo espelho) ser concluído depois que cada servidor é atualizado. Embora os servidores individuais serão reiniciados em sequência durante o processo de atualização, os servidores restantes no cluster continuam em execução.

1. Verifique se todos os servidores no cluster estão funcionando as atualizações mais recentes. Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o Microsoft Knowledge Base [4487006 do artigo](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de fevereiro de 2019). O número de build (consulte `ver` comando) deve ser 14393.2828 ou posterior.

2. Pare as VMs em execução no cluster.

3. Execute as seguintes etapas em um servidor de cluster por vez:

   1. Pausar o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultas. É recomendável que essa etapa ser cuidadoso.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Coloque o servidor no modo de manutenção do armazenamento, executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Execute o seguinte cmdlet para verificar se que o **OperationalStatus** valor estiver **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Executar uma instalação de atualização do Windows Server 2019 no servidor executando **setup.exe** e usando a opção "Manter aplicativos e arquivos pessoais".  
   Após a instalação for concluída, o servidor permanecerá no cluster e o cluster de serviço é iniciado automaticamente.

   5.  Verifique se o servidor recém-atualizado tem as atualizações mais recentes do Windows Server 2019.  
   Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   O número de build (consulte `ver` comando) deve ser 17763.292 ou posterior.

   6.  Remova o servidor de modo de manutenção de armazenamento usando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Retome o servidor usando o seguinte comando do PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Aguarde para trabalhos de reparo de armazenamento para concluir e todos os discos retornar ao estado íntegro.  
   Isso deve ser relativamente rápido, já que as VMs não estão em execução. Aqui estão os comandos para executar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.
5. Depois que todos os servidores foram atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente tenha até quatro semanas para fazê-lo.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o seguinte cmdlet para atualizar o pool de armazenamento.  
   Nesse ponto, novos cmdlets, como `Get-ClusterPerf` será totalmente operacional em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Iniciar as máquinas virtuais no cluster e verifique se está funcionando corretamente.

8. Opcionalmente, atualizar os níveis de configuração de VM parando cada VM e usando o `Update-VMVersion` cmdlet e, em seguida, reiniciando as máquinas virtuais.

9. Verifique se o cluster atualizado funciona conforme o esperado.  
   As funções devem fazer o failover corretamente e se a migração ao vivo da VM é usada no cluster, VMs devem migrar com êxito ao vivo.

10. Validar o cluster executando a validação de Cluster (`Test-Cluster`) e examinando o relatório de validação de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>A execução de uma instalação limpa do sistema operacional enquanto as VMs são interrompidas

Essa opção resulta em tempo de inatividade da VM, mas pode levar menos tempo do que se você manteve as VMs em execução durante a atualização, porque você não precisa esperar para trabalhos de armazenamento (reparo espelho) ser concluído depois que cada servidor é atualizado. Embora os servidores individuais serão reiniciados em sequência durante o processo de atualização, os servidores restantes no cluster continuam em execução.

1. Verifique se todos os servidores no cluster estão funcionando as atualizações mais recentes.  
   Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   No mínimo, instale o Microsoft Knowledge Base [4487006 do artigo](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (19 de fevereiro de 2019). O número de build (consulte `ver` comando) deve ser 14393.2828 ou posterior.

2. Pare as VMs em execução no cluster.

3. Execute as seguintes etapas em um servidor de cluster por vez:

   2. Pausar o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultas.  
      É recomendável que essa etapa ser cuidadoso.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Coloque o servidor no modo de manutenção do armazenamento, executando os seguintes comandos do PowerShell:

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Execute o seguinte cmdlet para verificar se que o **OperationalStatus** valor estiver **no modo de manutenção**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Remova o servidor do cluster executando o seguinte comando do PowerShell:  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Executar uma instalação limpa do Windows Server 2019 no servidor: Formatar a unidade de sistema, execute **setup.exe** e usar o "Nothing" opção.  
      Você terá que configurar a identidade do servidor, funções, recursos e aplicativos após a conclusão da instalação e o servidor for reiniciado.

   7. Instale a função Hyper-V e o recurso de Clustering de Failover no servidor (você pode usar o `Install-WindowsFeature` cmdlet).

   8. Instale os drivers mais recentes armazenamento e rede para o hardware que foram aprovados pelo fabricante do servidor para uso com espaços de armazenamento diretos.

   9. Verifique se o servidor recém-atualizado tem as atualizações mais recentes do Windows Server 2019.  
      Para obter mais informações, consulte [histórico de atualizações do Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      O número de build (consulte `ver` comando) deve ser 17763.292 ou posterior.

   10. Ingresse novamente o servidor ao cluster usando o seguinte comando do PowerShell:

      ```PowerShell
      Add-ClusterNode
      ```

   11. Remova o servidor de modo de manutenção do armazenamento usando o seguinte comando do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Aguarde para trabalhos de reparo de armazenamento para concluir e todos os discos retornar ao estado íntegro.  
       Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Aqui estão os comandos para executar:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores foram atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente tenha até quatro semanas para fazê-lo.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o seguinte cmdlet para atualizar o pool de armazenamento.  
   Nesse ponto, novos cmdlets, como `Get-ClusterPerf` será totalmente operacional em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Iniciar as máquinas virtuais no cluster e verifique se está funcionando corretamente.

8. Opcionalmente, atualizar os níveis de configuração de VM parando cada VM e usando o `Update-VMVersion` cmdlet e, em seguida, reiniciando as máquinas virtuais.

9. Verifique se o cluster atualizado funciona conforme o esperado.  
   As funções devem fazer o failover corretamente e se a migração ao vivo da VM é usada no cluster, VMs devem migrar com êxito ao vivo.

10. Validar o cluster executando a validação de Cluster (`Test-Cluster`) e examinando o relatório de validação de cluster.
