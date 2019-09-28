---
title: Atualizar um cluster Espaços de Armazenamento Diretos para o Windows Server 2019
description: Como atualizar um cluster Espaços de Armazenamento Diretos para o Windows Server 2019-ao mesmo tempo em que mantém as VMs em execução ou enquanto elas são interrompidas.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
ms.openlocfilehash: 09a1116b7fc1b1d0f8bc144ba9c4cf68fc45697e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402787"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Atualizar um cluster Espaços de Armazenamento Diretos para o Windows Server 2019

Este tópico descreve como atualizar um cluster Espaços de Armazenamento Diretos para o Windows Server 2019. Há quatro abordagens para atualizar um cluster Espaços de Armazenamento Diretos do Windows Server 2016 para o Windows Server 2019, usando o [processo de atualização sem interrupção do sistema operacional do cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) — dois que envolvem manter as VMs em execução e duas que envolvem a interrupção de todas as VMs. Cada abordagem tem pontos fortes e pontos fracos diferentes, portanto, selecione aquele que melhor atenda às necessidades da sua organização:

- **Atualização in-loco enquanto as VMs estão em execução** em cada servidor do cluster — essa opção não provoca nenhum tempo de inatividade da VM, mas você precisará aguardar que os trabalhos de armazenamento (reparo de espelho) sejam concluídos após a atualização de cada servidor.

- **Instalação limpa do sistema operacional enquanto as VMs estão em execução** em cada servidor do cluster — essa opção não provoca nenhum tempo de inatividade da VM, mas você precisará aguardar que os trabalhos de armazenamento (reparo de espelho) sejam concluídos após a atualização de cada servidor e precisará configurar cada servidor e todos os seus aplicativos e funções novamente.

- **Atualização in-loco enquanto as VMs são interrompidas** em cada servidor no cluster — essa opção incorre em tempo de inatividade da VM, mas você não precisa esperar por trabalhos de armazenamento (reparo de espelho), portanto, é mais rápido.

- **Instalação limpa do sistema operacional enquanto as VMs são interrompidas** em cada servidor no cluster — essa opção incorre em tempo de inatividade da VM, mas você não precisa esperar por trabalhos de armazenamento (reparo de espelho) para que ele seja mais rápido.

## <a name="prerequisites-and-limitations"></a>Pré-requisitos e limitações

Antes de prosseguir com uma atualização:

- Verifique se você tem backups utilizáveis caso haja algum problema durante o processo de atualização.

- Verifique se o seu fornecedor de hardware tem BIOS, firmware e drivers para seus servidores para os quais eles terão suporte no Windows Server 2019.

Há algumas limitações com o processo de atualização a ser considerado:

- Para habilitar Espaços de Armazenamento Diretos com compilações do Windows Server 2019 anteriores a 176693,292, os clientes talvez precisem entrar em contato com o suporte da Microsoft para chaves do registro que habilitam a funcionalidade de rede definida pelo Espaços de Armazenamento Diretos e pelo software. Para obter mais informações, consulte o [artigo 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)da base de dados de conhecimento Microsoft.

- Ao atualizar um cluster com volumes ReFS, há algumas limitações:

- A atualização tem suporte total em volumes ReFS, no entanto, os volumes atualizados não se beneficiarão dos aprimoramentos do ReFS no Windows Server 2019. Esses benefícios, como o aumento do desempenho da paridade acelerada por espelhamento, exigem um volume ReFS do Windows Server 2019 criado recentemente. Em outras palavras, você teria que criar novos volumes usando o `New-Volume` cmdlet ou Gerenciador do servidor. Aqui estão alguns dos aprimoramentos de ReFS que novos volumes obterão:

    - **Log-bypass do mapa**: uma melhoria de desempenho no ReFS que só se aplica a sistemas clusterizados (espaços de armazenamento diretos) e não se aplica a pools de armazenamento autônomos.

    - Melhorias de eficiência de **compactação** no Windows Server 2019 que são específicas para volumes de várias resilientes.

- Antes de atualizar um servidor de cluster Espaços de Armazenamento Diretos do Windows Server 2016, é recomendável colocar o servidor no modo de manutenção de armazenamento. Para obter mais informações, consulte a seção evento 5120 de [solucionar problemas espaços de armazenamento diretos](troubleshooting-storage-spaces.md). Embora esse problema tenha sido corrigido no Windows Server 2016, recomendamos colocar cada servidor de Espaços de Armazenamento Diretos no modo de manutenção de armazenamento durante a atualização como uma prática recomendada.

- Há um problema conhecido com ambientes de rede definidos pelo software que usam opções SET. Esse problema envolve migrações dinâmicas de VM do Hyper-V do Windows Server 2019 para o Windows Server 2016 (migração ao vivo para um sistema operacional anterior). Para garantir migrações dinâmicas bem-sucedidas, é recomendável alterar uma configuração de rede VM em VMs que estão sendo migradas ao vivo do Windows Server 2019 para o Windows Server 2016. Esse problema é corrigido para o Windows Server 2019 no pacote de rollup do hotfix 2019-01D, também conhecido como Build 17763,292. Para obter mais informações, consulte o [artigo 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)da base de dados de conhecimento Microsoft.

Devido aos problemas conhecidos acima, alguns clientes podem considerar a criação de um novo cluster do Windows Server 2019 e a cópia de dados do cluster antigo, em vez de atualizar seus clusters do Windows Server 2016 usando um dos quatro processos descritos abaixo.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Executando uma atualização in-loco enquanto as VMs estão em execução

Essa opção não provoca nenhum tempo de inatividade da VM, mas você precisará aguardar a conclusão dos trabalhos de armazenamento (reparo de espelho) após a atualização de cada servidor. Embora os servidores individuais sejam reiniciados sequencialmente durante o processo de atualização, os servidores restantes no cluster, bem como todas as VMs, permanecerão em execução.

1. Verifique se todos os servidores no cluster instalaram as atualizações mais recentes do Windows. Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o [artigo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) da base de dados de conhecimento Microsoft (19 de fevereiro de 2019). O número de Build ( `ver` consulte o comando) deve ser 14393,2828 ou superior.

2. Se você estiver usando a rede definida pelo software com opções SET, abra uma sessão do PowerShell com privilégios elevados e execute o seguinte comando para desabilitar as verificações de verificação da migração dinâmica da VM em todas as VMs no cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Execute as seguintes etapas em um servidor de cluster de cada vez:

   1. Use a migração dinâmica de VM do Hyper-V para mover VMs em execução do servidor que você está prestes a atualizar.

   2. Pause o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultos. É recomendável que essa etapa seja cuidadosa — se você não migrar VMs ao vivo do servidor, esse cmdlet fará isso para você, portanto, você poderá ignorar a etapa anterior se preferir.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque o servidor no modo de manutenção de armazenamento executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Execute o seguinte cmdlet para verificar se o valor de **OperationalStatus** está **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Execute uma instalação de atualização do Windows Server 2019 no servidor executando **Setup. exe** e usando a opção "manter arquivos pessoais e aplicativos". Após a conclusão da instalação, o servidor permanece no cluster e o serviço de cluster é iniciado automaticamente.

   6. Verifique se o servidor recentemente atualizado tem as atualizações mais recentes do Windows Server 2019. Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). O número de Build ( `ver` consulte o comando) deve ser 17763,292 ou superior.

   7. Remova o servidor do modo de manutenção de armazenamento usando o seguinte comando do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Retome o servidor usando o seguinte comando do PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Aguarde a conclusão dos trabalhos de reparo de armazenamento e para que todos os discos retornem a um estado íntegro. Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Estes são os comandos a serem executados:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores tiverem sido atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente você tenha até quatro semanas para fazer isso.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o cmdlet a seguir para atualizar o pool de armazenamento. Neste ponto, novos cmdlets como `Get-ClusterPerf` estarão totalmente operacionais em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, atualize os níveis de configuração da VM interrompendo `Update-VMVersion` cada VM, usando o cmdlet e, em seguida, iniciando as VMs novamente.

8. Se você estiver usando a rede definida pelo software com opções SET e desabilitada as verificações de migração dinâmica ao vivo conforme instruído acima, use o seguinte cmdlet para reabilitar as verificações de verificação dinâmicas de VM:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Verifique se o cluster atualizado funciona conforme o esperado. As funções devem fazer failover corretamente e se a migração dinâmica da VM for usada no cluster, as VMs devem migrar ao vivo com êxito.

10. Valide o cluster executando a validação de cluster`Test-Cluster`() e examinando o relatório de validação de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Executando uma instalação limpa do sistema operacional enquanto as VMs estão em execução

Essa opção não provoca nenhum tempo de inatividade da VM, mas você precisará aguardar a conclusão dos trabalhos de armazenamento (reparo de espelho) após a atualização de cada servidor. Embora os servidores individuais sejam reiniciados sequencialmente durante o processo de atualização, os servidores restantes no cluster, bem como todas as VMs, permanecerão em execução.

1. Verifique se todos os servidores no cluster estão executando as atualizações mais recentes. Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o [artigo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) da base de dados de conhecimento Microsoft (19 de fevereiro de 2019). O número de Build ( `ver` consulte o comando) deve ser 14393,2828 ou superior.

2. Se você estiver usando a rede definida pelo software com opções SET, abra uma sessão do PowerShell com privilégios elevados e execute o seguinte comando para desabilitar as verificações de verificação da migração dinâmica da VM em todas as VMs no cluster:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Execute as seguintes etapas em um servidor de cluster de cada vez:

   1. Use a migração dinâmica de VM do Hyper-V para mover VMs em execução do servidor que você está prestes a atualizar.

   2. Pause o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultos. É recomendável que essa etapa seja cuidadosa — se você não migrar VMs ao vivo do servidor, esse cmdlet fará isso para você, portanto, você poderá ignorar a etapa anterior se preferir.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Coloque o servidor no modo de manutenção de armazenamento executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Execute o seguinte cmdlet para verificar se o valor de **OperationalStatus** está **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Remova o servidor do cluster executando o seguinte comando do PowerShell:  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Execute uma instalação limpa do Windows Server 2019 no servidor: formate a unidade do sistema, execute **Setup. exe** e use a opção "Nothing". Você precisará configurar a identidade do servidor, as funções, os recursos e os aplicativos após a conclusão da instalação e a reinicialização do servidor.

   5. Instale a função Hyper-V e o recurso de clustering de failover no servidor (você pode usar `Install-WindowsFeature` o cmdlet).

   6. Instale os drivers de armazenamento e de rede mais recentes para o hardware que foram aprovados pelo fabricante do servidor para uso com o Espaços de Armazenamento Diretos.

   7. Verifique se o servidor recentemente atualizado tem as atualizações mais recentes do Windows Server 2019. Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). O número de Build ( `ver` consulte o comando) deve ser 17763,292 ou superior.

   8. Reingresse o servidor no cluster usando o seguinte comando do PowerShell:

       ```PowerShell
       Add-ClusterNode
       ```

   9. Remova o servidor do modo de manutenção de armazenamento usando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Aguarde a conclusão dos trabalhos de reparo de armazenamento e para que todos os discos retornem a um estado íntegro. Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Estes são os comandos a serem executados:

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores tiverem sido atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente você tenha até quatro semanas para fazer isso.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o cmdlet a seguir para atualizar o pool de armazenamento. Neste ponto, novos cmdlets como `Get-ClusterPerf` estarão totalmente operacionais em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Opcionalmente, atualize os níveis de configuração da VM interrompendo `Update-VMVersion` cada VM e usando o cmdlet e, em seguida, iniciando as VMs novamente.

8. Se você estiver usando a rede definida pelo software com opções SET e desabilitada as verificações de migração dinâmica ao vivo conforme instruído acima, use o seguinte cmdlet para reabilitar as verificações de verificação dinâmicas de VM:

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Verifique se o cluster atualizado funciona conforme o esperado. As funções devem fazer failover corretamente e se a migração dinâmica da VM for usada no cluster, as VMs devem migrar ao vivo com êxito.

10. Valide o cluster executando a validação de cluster`Test-Cluster`() e examinando o relatório de validação de cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Executando uma atualização in-loco enquanto as VMs são interrompidas

Essa opção incorre em tempo de inatividade da VM, mas pode demorar menos do que se você mantiver as VMs em execução durante a atualização, pois não precisa esperar que os trabalhos de armazenamento (reparo de espelho) sejam concluídos após a atualização de cada servidor. Embora os servidores individuais sejam reiniciados sequencialmente durante o processo de atualização, os servidores restantes no cluster permanecem em execução.

1. Verifique se todos os servidores no cluster estão executando as atualizações mais recentes. Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). No mínimo, instale o [artigo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) da base de dados de conhecimento Microsoft (19 de fevereiro de 2019). O número de Build ( `ver` consulte o comando) deve ser 14393,2828 ou superior.

2. Pare as VMs em execução no cluster.

3. Execute as seguintes etapas em um servidor de cluster de cada vez:

   1. Pause o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultos. É recomendável que essa etapa seja cuidadosa.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Coloque o servidor no modo de manutenção de armazenamento executando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Execute o seguinte cmdlet para verificar se o valor de **OperationalStatus** está **no modo de manutenção**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Execute uma instalação de atualização do Windows Server 2019 no servidor executando **Setup. exe** e usando a opção "manter arquivos pessoais e aplicativos".  
   Após a conclusão da instalação, o servidor permanece no cluster e o serviço de cluster é iniciado automaticamente.

   5.  Verifique se o servidor recentemente atualizado tem as atualizações mais recentes do Windows Server 2019.  
   Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   O número de Build ( `ver` consulte o comando) deve ser 17763,292 ou superior.

   6.  Remova o servidor do modo de manutenção de armazenamento usando os seguintes comandos do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Retome o servidor usando o seguinte comando do PowerShell:

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Aguarde a conclusão dos trabalhos de reparo de armazenamento e para que todos os discos retornem a um estado íntegro.  
   Isso deve ser relativamente rápido, pois as VMs não estão em execução. Estes são os comandos a serem executados:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.
5. Depois que todos os servidores tiverem sido atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente você tenha até quatro semanas para fazer isso.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o cmdlet a seguir para atualizar o pool de armazenamento.  
   Neste ponto, novos cmdlets como `Get-ClusterPerf` estarão totalmente operacionais em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie as VMs no cluster e verifique se elas estão funcionando corretamente.

8. Opcionalmente, atualize os níveis de configuração da VM interrompendo `Update-VMVersion` cada VM e usando o cmdlet e, em seguida, iniciando as VMs novamente.

9. Verifique se o cluster atualizado funciona conforme o esperado.  
   As funções devem fazer failover corretamente e se a migração dinâmica da VM for usada no cluster, as VMs devem migrar ao vivo com êxito.

10. Valide o cluster executando a validação de cluster`Test-Cluster`() e examinando o relatório de validação de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Executando uma instalação limpa do sistema operacional enquanto as VMs são interrompidas

Essa opção incorre em tempo de inatividade da VM, mas pode demorar menos do que se você mantiver as VMs em execução durante a atualização, pois não precisa esperar que os trabalhos de armazenamento (reparo de espelho) sejam concluídos após a atualização de cada servidor. Embora os servidores individuais sejam reiniciados sequencialmente durante o processo de atualização, os servidores restantes no cluster permanecem em execução.

1. Verifique se todos os servidores no cluster estão executando as atualizações mais recentes.  
   Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   No mínimo, instale o [artigo 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) da base de dados de conhecimento Microsoft (19 de fevereiro de 2019). O número de Build ( `ver` consulte o comando) deve ser 14393,2828 ou superior.

2. Pare as VMs em execução no cluster.

3. Execute as seguintes etapas em um servidor de cluster de cada vez:

   2. Pause o servidor de cluster usando o seguinte comando do PowerShell — Observe que alguns grupos internos estão ocultos.  
      É recomendável que essa etapa seja cuidadosa.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Coloque o servidor no modo de manutenção de armazenamento executando os seguintes comandos do PowerShell:

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Execute o seguinte cmdlet para verificar se o valor de **OperationalStatus** está **no modo de manutenção**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Remova o servidor do cluster executando o seguinte comando do PowerShell:  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Execute uma instalação limpa do Windows Server 2019 no servidor: formate a unidade do sistema, execute **Setup. exe** e use a opção "Nothing".  
      Você precisará configurar a identidade do servidor, as funções, os recursos e os aplicativos após a conclusão da instalação e a reinicialização do servidor.

   7. Instale a função Hyper-V e o recurso de clustering de failover no servidor (você pode usar `Install-WindowsFeature` o cmdlet).

   8. Instale os drivers de armazenamento e de rede mais recentes para o hardware que foram aprovados pelo fabricante do servidor para uso com o Espaços de Armazenamento Diretos.

   9. Verifique se o servidor recentemente atualizado tem as atualizações mais recentes do Windows Server 2019.  
      Para obter mais informações, consulte histórico de atualização do Windows [10 e do Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      O número de Build ( `ver` consulte o comando) deve ser 17763,292 ou superior.

   10. Reingresse o servidor no cluster usando o seguinte comando do PowerShell:

      ```PowerShell
      Add-ClusterNode
      ```

   11. Remova o servidor do modo de manutenção de armazenamento usando o seguinte comando do PowerShell:

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Aguarde a conclusão dos trabalhos de reparo de armazenamento e para que todos os discos retornem a um estado íntegro.  
       Isso pode levar um tempo considerável, dependendo do número de VMs em execução durante a atualização do servidor. Estes são os comandos a serem executados:

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Atualize o próximo servidor no cluster.

5. Depois que todos os servidores tiverem sido atualizados para o Windows Server 2019, use o seguinte cmdlet do PowerShell para atualizar o nível funcional do cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   É recomendável atualizar o nível funcional do cluster assim que possível, embora tecnicamente você tenha até quatro semanas para fazer isso.

6. Depois que o nível funcional do cluster tiver sido atualizado, use o cmdlet a seguir para atualizar o pool de armazenamento.  
   Neste ponto, novos cmdlets como `Get-ClusterPerf` estarão totalmente operacionais em qualquer servidor no cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Inicie as VMs no cluster e verifique se elas estão funcionando corretamente.

8. Opcionalmente, atualize os níveis de configuração da VM interrompendo `Update-VMVersion` cada VM e usando o cmdlet e, em seguida, iniciando as VMs novamente.

9. Verifique se o cluster atualizado funciona conforme o esperado.  
   As funções devem fazer failover corretamente e se a migração dinâmica da VM for usada no cluster, as VMs devem migrar ao vivo com êxito.

10. Valide o cluster executando a validação de cluster`Test-Cluster`() e examinando o relatório de validação de cluster.
