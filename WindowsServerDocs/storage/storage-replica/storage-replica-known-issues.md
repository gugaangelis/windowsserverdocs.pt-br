---
title: Problemas conhecidos com a Réplica de Armazenamento
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 06/25/2019
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 665d137673c3229f2b06283965c085ae25a2287c
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769614"
---
# <a name="known-issues-with-storage-replica"></a>Problemas conhecidos com a Réplica de Armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Este tópico discute os problemas conhecidos com a réplica de armazenamento no Windows Server.

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Depois de remover a replicação, os discos estão offline e você não pode configurar a replicação novamente

No Windows Server 2016, pode ser impossível provisionar a replicação em um volume que tenha sido replicado anteriormente ou você poderá localizar volumes que não podem ser montados. Isso pode ocorrer quando alguma condição de erro impede a remoção de replicação ou quando você reinstala o sistema operacional em um computador que anteriormente replicava dados.

Para corrigir isto, você deve remover a partição oculta da Réplica de Armazenamento dos discos e retorná-los a um estado gravável usando o cmdlet `Clear-SRMetadata`.

- Para remover todos os slots órfãos de banco de dados de partição da Réplica de Armazenamento e remontar todas as partições, use o parâmetro `-AllPartitions` da seguinte maneira:

    ```PowerShell
    Clear-SRMetadata -AllPartitions
    ```

- Para remover todos os dados de log órfãos da Réplica de Armazenamento, use o parâmetro `-AllLogs` da seguinte maneira:

    ```PowerShell
    Clear-SRMetadata -AllLogs
    ```

- Para remover todos os dados órfãos de configuração do cluster de failover, use o parâmetro `-AllConfiguration` da seguinte maneira:

    ```PowerShell
    Clear-SRMetadata -AllConfiguration
    ```

- Para remover os metadados do grupo de replicação individual, use o parâmetro `-Name` e especifique um grupo de replicação da seguinte maneira:

    ```PowerShell
    Clear-SRMetadata -Name RG01 -Logs -Partition
    ```

O servidor pode precisar ser reiniciado após a limpeza do banco de dados de partição; você pode suprimir isso temporariamente com `-NoRestart`, mas não deve ignorar a reinicialização do servidor, se isso for solicitado pelo cmdlet. Esse cmdlet não remove os volumes de dados nem os dados contidos nesses volumes.

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>Durante a sincronização inicial, confira os avisos 4004 do log de eventos

No Windows Server 2016, ao configurar a replicação, os servidores de origem e de destino podem mostrar vários avisos **StorageReplica\Admin*do log de eventos 4004 cada um durante a sincronização inicial, com um status de "recursos insuficientes do sistema existem para concluir a API". Você provavelmente também verá erros 5014. Eles indicam que os servidores não têm memória física (RAM) suficiente para executar a sincronização inicial e executar cargas de trabalho. Adicione RAM ou reduza a RAM usada de recursos e aplicativos que não sejam de Réplica de Armazenamento.

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>Ao usar clusters de convidado com VHDX compartilhado e um host sem um CSV, as máquinas virtuais param de responder depois de configurar a replicação

No Windows Server 2016, ao usar clusters de convidado do Hyper-V para finalidades de teste ou demonstração da Réplica de Armazenamento, e ao usar o VHDX compartilhado como o armazenamento de cluster de convidado, as máquinas virtuais param de responder após configurar a replicação. Se você reiniciar o host do Hyper-V, as máquinas virtuais começam a responder, mas a configuração de replicação não será concluída e nenhuma replicação ocorrerá.

Esse comportamento ocorre quando você está usando **fltmc.exe anexar svhdxflt*-para ignorar o requisito para o host Hyper-V que está executando um CSV. O uso deste comando não é suportado e é destinado apenas para fins de teste e demonstração.

A causa da lentidão é um problema de interoperabilidade pretendido entre o novo recurso de QoS de armazenamento no Windows Server 2016 e o filtro de VHDX compartilhado anexado manualmente. Para resolver esse problema, desabilite o driver de filtro de QoS de armazenamento e reinicie o host do Hyper-V:

```
SC config storqosflt start= disabled
```

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>Não é possível configurar a replicação ao usar armazenamento diferente e de Novo Volume

Ao usar o cmdlet `New-Volume` com conjuntos diferentes de armazenamento nos servidores de origem e de destino, como duas SANs diferentes ou dois JBODs com discos diferentes, não é possível configurar a replicação subsequentemente usando `New-SRPartnership`. O erro mostrado pode incluir:

```
Data partition sizes are different in those two groups
```

Use o cmdlet `New-Partition**` para criar volumes e formatá-los em vez de `New-Volume`, porque o último cmdlet pode arredondar o tamanho do volume em matrizes de armazenamento diferentes. Se você já tiver criado um volume NTFS, poderá usar o `Resize-Partition` para aumentar ou reduzir um dos volumes para coincidir com o outro (não pode ser feito com volumes de ReFS). Se estiver usando **diskmgmt*-ou **Gerenciador do servidor**, nenhum arredondamento ocorrerá.

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>Executar Test-SRTopology falha com erros relacionados ao nome

Ao tentar usar `Test-SRTopology`, você recebe um dos seguintes erros:

**EXEMPLO DE ERRO 1:**

```
WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs
WARNING: System.Exception
WARNING: at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs
At line:1 char:1
+ Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception
+ FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand
```

**EXEMPLO DE ERRO 2:**

```
WARNING: Invalid value entered for source computer name
```

**EXEMPLO DE ERRO 3:**

```
The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1
```

Esse cmdlet limitou a descrição de erros no Windows Server 2016 e retornará o mesmo resultado para vários problemas comuns. O erro pode ocorrer pelos seguintes motivos:

- Você efetuou logon no computador de origem como um usuário local, não um usuário de domínio.

- O computador de destino não está em execução ou não está acessível pela rede.

- Você especificou um nome incorreto para o computador de destino.

- Você especificou um endereço IP para o servidor de destino.

- O firewall do computador de destino está bloqueando o acesso às chamadas do PowerShell e/ou CIM.

- O computador de destino não está executando o serviço do WMI.

- Você não usou CREDSSP ao executar o cmdlet `Test-SRTopology` remotamente de um computador de gerenciamento.

- O volume de origem ou de destino especificado é um disco local em um nó de cluster, não um disco clusterizado.

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>Configurar nova parceria de Réplica de Armazenamento retorna um erro - "Falha ao provisionar partição"

Ao tentar criar uma nova parceria de replicação com `New-SRPartnership`, o seguinte erro é exibido:

```
New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
At line: 1 char: 1
+ New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
+ Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
+ FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership
```

Isso é causado pela seleção de um volume de dados que está na mesma partição que a unidade do sistema (ou seja, a unidade **C:*-drive com sua pasta do Windows). Por exemplo, em uma unidade que contém os **C:*-e **D:*-volumes criados a partir da mesma partição. Isso não é suportado na Réplica de Armazenamento; você deve escolher um volume diferente para replicar.

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>A tentativa de aumentar um volume replicado falha devido a uma atualização ausente.

Ao tentar aumentar ou ampliar um volume replicado, você recebe o seguinte erro:

```powershell
Resize-Partition -DriveLetter d -Size 44GB
Resize-Partition : The operation failed with return code 8
At line:1 char:1
+ Resize-Partition -DriveLetter d -Size 44GB
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
[Resize-Partition], CimException
+ FullyQualifiedErrorId : StorageWMI 8,Resize-Partition
```

Se estiver usando o snap-in do MMC de Gerenciamento de Disco, você receberá este erro:

```
Element not found
```

Isso ocorre mesmo se habilitar corretamente o redimensionamento de volume no servidor de origem usando `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE`.

Esse problema foi corrigido na atualização cumulativa para Windows 10, versão 1607 (atualização de aniversário) e Windows Server 2016:9 de dezembro de 2016 (KB3201845).

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>A tentativa de aumentar um volume replicado falha devido a um passo ausente.

Se tentar redimensionar um volume replicado no servidor de origem sem primeiro configurar `-AllowResizeVolume $TRUE`, você receberá os seguintes erros:

```powershell
Resize-Partition -DriveLetter I -Size 8GB
Resize-Partition : Failed

Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
At line:1 char:1
+ Resize-Partition -DriveLetter I -Size 8GB
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
     + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

Storage Replica Event log error 10307:

Attempted to resize a partition that is protected by Storage Replica.

DeviceName: \Device\Harddisk1\DR1
PartitionNumber: 7
PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.
```

```powershell
Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true
```

Antes de aumentar a partição de dados de origem, verifique se a partição de dados de destino tem espaço suficiente para aumentar para um tamanho igual. A redução da partição de dados protegida pela réplica de armazenamento está bloqueada.

Erro de snap-in de Gerenciamento de Disco

```
An unexpected error has occurred
```

Após o redimensionamento do volume, lembre-se de desabilitar o redimensionamento com `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE`. Esse parâmetro impede que administradores tentem redimensionar volumes antes de garantir que haja espaço suficiente no volume de destino, geralmente porque não perceberam a presença da Réplica de Armazenamento.

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>A tentativa de mover um recurso PDR entre sites em um cluster estendido assíncrono falha

Ao tentar mover uma função anexada a recurso Disco Físico, como um servidor de arquivos para uso geral, para mover o armazenamento associado em um cluster estendido assíncrono, você recebe um erro.

Se estiver usando o snap-in Gerenciador de Cluster de Failover:

```
Error
The operation has failed.
The action 'Move' did not complete.
Error Code: 0x80071398
The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
```

Se estiver usando o cmdlet do PowerShell do Cluster:

```powershell
Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
At line:1 char:1
+ Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
+ FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand
```

Isso ocorre por causa de um comportamento planejado no Windows Server 2016. Use `Set-SRPartnership` para mover esses discos PDR em um cluster estendido assíncrono.

Esse comportamento foi alterado no Windows Server, versão 1709, para permitir failovers manuais e automatizados com replicação assíncrona, com base nos comentários do cliente.

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>Tentativa de adicionar discos a um cluster assimétrico de dois nós retorna "Nenhum disco adequado para discos de cluster encontrado"

Ao tentar provisionar um cluster com apenas dois nós, antes de adicionar a replicação estendida da Réplica de Armazenamento, você tenta adicionar os discos no segundo local aos Discos Disponíveis. Você recebe o seguinte erro:

```
No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests.
```

Isso não ocorrerá se você tiver pelo menos três nós no cluster. Esse problema ocorre devido a uma alteração de código padrão no Windows Server 2016 para comportamentos de clustering de armazenamento assimétrico.

Para adicionar o armazenamento, você pode executar o comando a seguir no nó no segundo local:

```
Get-ClusterAvailableDisk -All | Add-ClusterDisk
```

Isso não funcionará com o armazenamento local do nó. Você pode usar o armazenamento réplica para replicar um cluster estendido entre dois nós totais, **cada um usando seu próprio conjunto de armazenamento compartilhado.**

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>O limitador de largura de banda do SMB não consegue restringir a largura de banda da réplica de armazenamento

Ao especificar um limite de largura de banda para a réplica de armazenamento, o limite é ignorado e a largura de banda total é usada. Por exemplo:

```
Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB
```

Isso ocorre devido ao um problema de interoperabilidade entre a réplica de armazenamento e o SMB. Esse problema foi corrigido primeiro na atualização cumulativa de julho de 2017 do Windows Server 2016 e no Windows Server, versão 1709.

## <a name="event-1241-warning-repeated-during-initial-sync"></a>Aviso de evento 1241 repetido durante sincronização inicial

Quando uma parceria de replicação especificada é assíncrona, o computador de origem registra o evento de aviso 1241 várias vezes no canal de administração de réplica de armazenamento. Por exemplo:

```
Log Name:      Microsoft-Windows-StorageReplica/Admin
Source:        Microsoft-Windows-StorageReplica
Date:          3/21/2017 3:10:41 PM
Event ID:      1241
Task Category: (1)
Level:         Warning
Keywords:      (1)
User:          SYSTEM
Computer:      sr-srv05.corp.contoso.com
Description:
The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

LocalReplicationGroupName: rg01
LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
LocalReplicaName: f:\
LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
RemoteReplicationGroupName: rg02
RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
TargetRPO: 30
```

Orientação: normalmente, isso ocorre devido a um dos seguintes motivos:

- O destino assíncrono está desconectado no momento. O RPO pode ser disponibilizado depois que a conexão é restaurada.

- O destino assíncrono não pode manter o ritmo com a origem, de modo que o registro de log de destino mais recente não esteja mais presente no log de origem. O destino iniciará o bloco de cópia. O RPO deve ficar disponível após a conclusão da cópia do bloco.

Esse é o comportamento esperado durante a sincronização inicial e pode ser ignorado com segurança. Esse comportamento pode mudar em uma versão posterior. Se você esse comportamento ocorrer durante uma replicação assíncrona contínua, investigue a parceria para determinar por que a duplicação atrasa além do RPO configurado (30 segundos por padrão).

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>Aviso de evento 4004 repetido após a reinicialização de um nó replicado

Em casos raros e, geralmente, em circunstâncias irreproduzíveis, a reinicialização de um servidor que está em uma parceria resulta na falha da replicação e no registro do evento de aviso 4004 pelo nó reinicializado com um erro de acesso negado.

```
Log Name:      Microsoft-Windows-StorageReplica/Admin
Source:        Microsoft-Windows-StorageReplica
Date:          3/21/2017 11:43:25 AM
Event ID:      4004
Task Category: (7)
Level:         Warning
Keywords:      (256)
User:          SYSTEM
Computer:      server.contoso.com
Description:
Failed to establish a connection to a remote computer.

RemoteComputerName: server
LocalReplicationGroupName: rg01
LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
RemoteReplicationGroupName: rg02
RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
ReplicaSetId: {00000000-0000-0000-0000-000000000000}
RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
Status: {Access Denied}
A process has requested access to an object, but has not been granted those access rights.

Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.
```

Observe o `Status: "{Access Denied}"` e a mensagem `A process has requested access to an object, but has not been granted those access rights.` esse é um problema conhecido na réplica de armazenamento e foi corrigido na atualização de qualidade de 12 de setembro de 2017 – KB4038782 (Build do sistema operacional 14393,1715)https://support.microsoft.com/help/4038782/windows-10-update-kb4038782

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>Erro "Falha ao colocar online o recurso 'Disco de Cluster x'". com um cluster estendido

Durante a tentativa de colocar um disco de cluster online após um failover bem-sucedido, onde você está tentando tornar o site de origem principal novamente, você receberá um erro no Gerenciador de Cluster de Failover. Por exemplo:

```
Error
The operation has failed.
Failed to bring the resource 'Cluster Disk 2' online.

Error Code: 0x80071397
The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.
```

Se você tentar mover o disco ou o CSV manualmente, receberá um erro adicional. Por exemplo:

```
Error
The operation has failed.
The action 'Move' did not complete.

Error Code: 0x8007138d
A cluster node is not available for this operation
```

Esse problema é causado por um ou mais discos não inicializados sendo anexados a um ou mais nós de cluster. Para resolver o problema, inicialize todos os armazenamentos anexados usando DiskMgmt.msc, DISKPART.EXE ou o cmdlet Initialize-Disk do PowerShell.

Estamos trabalhando para fornecer uma atualização que resolva definitivamente esse problema. Se você estiver interessado em nos ajudar e tiver um contrato do Suporte Microsoft Premier, envie um email para SRFEED@microsoft.com, para que possamos trabalhar com você no preenchimento de uma nova solicitação.

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>Erro GPT durante a tentativa de criar uma nova parceria de SR

Ao executar New-SRPartnership, ocorre uma falha com o erro:

```
Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
\\?\Volume{GUID}\ is not a valid GPT style layout.
At line:1 char:1
+ New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership], CimException
+ FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership
```

Na GUI do Gerenciador de Cluster de Failover, não há nenhuma opção de configuração da replicação do disco.

Ao executar Test-SRTopology, ocorre uma falha com:

```
WARNING: Object reference not set to an instance of an object.
WARNING: System.NullReferenceException
WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
    at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
Test-SRTopology : Object reference not set to an instance of an object.
At line:1 char:1
+ Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
+ FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand
```

Isso é ocasionado porque o nível funcional do cluster ainda está sendo definido para o Windows Server 2012 R2 (por exemplo, FL 8). Espera-se que a réplica de armazenamento retorne um erro específico aqui, mas em vez disso, ela retorna um mapeamento de erro incorreto.

```
Run Get-Cluster | fl - on each node.
```

Se `ClusterFunctionalLevel = 9` , essa é a versão ClusterFunctionalLevel do Windows 2016 necessária para implementar a réplica de armazenamento neste nó.
Se ClusterFunctionalLevel não for 9, o ClusterFunctionalLevel precisará ser atualizado para implementar a réplica de armazenamento neste nó.

Para resolver o problema, aumente o nível funcional do cluster executando o cmdlet do PowerShell: [Update-ClusterFunctionalLevel](/powershell/module/failoverclusters/update-clusterfunctionallevel).

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>Pequena partição desconhecida listada em DISKMGMT para cada volume replicado

Ao executar o snap-in de Gerenciamento de Disco (DISKMGMT.MSC), você percebe um ou mais volumes listados sem rótulo ou letra de unidade e 1 MB de tamanho. Você poderá excluir o volume desconhecido ou receber:

```
An Unexpected Error has Occurred
```

Este comportamento ocorre por design. Este não é um volume, mas uma partição. A Réplica de Armazenamento cria uma partição de 512 KB como um slot de banco de dados para operações de replicação (a ferramenta DiskMgmt.msc herdada arredonda para o MB mais próximo). Ter uma partição assim para cada volume replicado é normal e desejável. Quando não estiver mais em uso, você poderá excluir a partição de 512 KB; não é possível excluir aquelas em uso. A partição nunca aumentará ou diminuirá. Se você estiver recriando a replicação, recomendamos deixar a partição como Réplica de Armazenamento para solicitar as utilizadas.

Para ver detalhes, use a ferramenta DISKPART ou o cmdlet Get-Partition. Essas partições terão um tipo GPT de `558d43c5-a1ac-43c0-aac8-d1472b2923d1`.

## <a name="a-storage-replica-node-hangs-when-creating-snapshots"></a>Um nó de réplica de armazenamento trava ao criar instantâneos

Ao criar um instantâneo do VSS (por meio de backup, VSSADMIN, etc), um nó de réplica de armazenamento trava e você deve forçar uma reinicialização do nó a ser recuperado. Não há erro, apenas um desligamento forçado do servidor.

Esse problema ocorre quando você cria um instantâneo do VSS do volume de log. A causa subjacente é um aspecto de design herdado do VSS, não da réplica de armazenamento. O comportamento resultante quando você faz o instantâneo do volume de log da réplica de armazenamento é um mecanismo de queing de e/s do VSS trava o servidor.

Para evitar esse comportamento, não faça instantâneos dos volumes de log da réplica de armazenamento. Não é necessário fazer instantâneos dos volumes de log da réplica de armazenamento, pois esses logs não podem ser restaurados. Além disso, o volume de log nunca deve conter outras cargas de trabalho, portanto, nenhum instantâneo é necessário em geral.

## <a name="high-io-latency-increase-when-using-storage-spaces-direct-with-storage-replica"></a>Alta latência de e/s aumenta ao usar Espaços de Armazenamento Diretos com a réplica de armazenamento

Ao usar Espaços de Armazenamento Diretos com um cache NVME ou SSD, você verá um aumento maior que o esperado em latência ao configurar a replicação de réplica de armazenamento entre clusters Espaços de Armazenamento Diretos. A alteração na latência é proporcionalmente muito maior do que você vê ao usar o NVME e SSD em uma configuração de desempenho + capacidade e nenhuma camada de HDD nem camada de capacidade.

Esse problema ocorre devido a limitações de arquitetura dentro do mecanismo de log da réplica de armazenamento combinado com a latência extremamente baixa de NVME quando comparada à mídia mais lenta. Ao usar o cache de Espaços de Armazenamento Diretos, todas as e/s de logs de réplica de armazenamento, juntamente com todas as e/s de leitura/gravação recentes de aplicativos, ocorrerão no cache e nunca nas camadas de desempenho ou capacidade. Isso significa que toda a atividade de réplica de armazenamento ocorre na mesma mídia de velocidade-essa configuração tem suporte, mas não é recomendada (consulte https://aka.ms/srfaq para obter recomendações de log).

Ao usar Espaços de Armazenamento Diretos com HDDs, você não pode desabilitar ou evitar o cache. Como alternativa, se estiver usando apenas SSD e NVME, você pode configurar apenas as camadas de desempenho e capacidade. Se estiver usando essa configuração e posicionar os logs do SR no nível de desempenho somente com os volumes de dados em que eles estão sendo atendidos apenas na camada de capacidade, você evitará o problema de alta latência descrito acima. O mesmo pode ser feito com uma combinação de SSDs mais rápidos e lentos e sem NVME.

Essa solução alternativa não é ideal e alguns clientes podem não conseguir usá-la. A equipe de réplica de armazenamento está trabalhando em otimizações e um mecanismo de log atualizado para o futuro para reduzir esses afunilamentos artificiais. Esse log v 1.1 primeiro se tornou disponível no Windows Server 2019 e seu desempenho aprimorado é descrito no no [blog de armazenamento do servidor](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).

## <a name="error-could-not-find-file-when-running-test-srtopology-between-two-clusters"></a>Erro "não foi possível encontrar o arquivo" ao executar Test-SRTopology entre dois clusters

Ao executar Test-SRTopology entre dois clusters e seus caminhos CSV, ele falha com o erro:

```powershell
PS C:\Windows\system32> Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName NedClusterB -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 1 -ResultPath C:\Temp

Validating data and log volumes...
Measuring Storage Replica recovery and initial synchronization performance...
WARNING: Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
WARNING: System.IO.FileNotFoundException
WARNING:    at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 bufferSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost) at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share, Int32 bufferSize, FileOptions options) at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.GenerateWriteIOWorkload(String Path, UInt32 IoSizeInBytes, UInt32 Parallel IoCount, UInt32 Duration)at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.<>c__DisplayClass75_0.<PerformRecoveryTest>b__0()at System.Threading.Tasks.Task.Execute()
Test-SRTopology : Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
At line:1 char:1
+ Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName  ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], FileNotFoundException
+ FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand
```

Isso é causado por um defeito de código conhecido no Windows Server 2016. Esse problema foi corrigido primeiro no Windows Server, versão 1709 e as ferramentas do RSAT associadas. Para uma resolução de nível inferior, entre em contato com Suporte da Microsoft e solicite uma atualização do backport. Não há solução alternativa.

## <a name="error-specified-volume-could-not-be-found-when-running-test-srtopology-between-two-clusters"></a>Erro "o volume especificado não pôde ser encontrado" ao executar Test-SRTopology entre dois clusters

Ao executar Test-SRTopology entre dois clusters e seus caminhos CSV, ele falha com o erro:

```
PS C:\> Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName RRN44-14-13 -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 30 -ResultPath c:\report

Test-SRTopology : The specified volume C:\ClusterStorage\Volume1 cannot be found on computer RRN44-14-09. If this is a cluster node, the volume must be part of a role or CSV; volumes in Available Storage are not accessible
At line:1 char:1
+ Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], Exception
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand
```

Ao especificar o CSV do nó de origem como o volume de origem, você deve selecionar o nó que possui o CSV. Você pode mover o CSV para o nó especificado ou alterar o nome do nó especificado em `-SourceComputerName` . Esse erro recebeu uma mensagem aprimorada no Windows Server 2019.

## <a name="unable-to-access-the-data-drive-in-storage-replica-after-unexpected-reboot-when-bitlocker-is-enabled"></a>Não é possível acessar a unidade de dados na réplica de armazenamento após a reinicialização inesperada quando o BitLocker está habilitado

Se o BitLocker estiver habilitado em ambas as unidades (unidade de log e unidade de dados) e em ambas as unidades de réplica de armazenamento, se o servidor primário for reinicializado, você não poderá acessar a unidade primária mesmo depois de desbloquear a unidade de log do BitLocker.

Este é um comportamento esperado. Para recuperar os dados ou acessar a unidade, você precisa desbloquear a unidade de log primeiro e, em seguida, abrir diskmgmt. msc para localizar a unidade de dados. Transforme a unidade de dados offline e online novamente. Localize o ícone do BitLocker na unidade e desbloqueie a unidade.

## <a name="issue-unlocking-the-data-drive-on-secondary-server-after-breaking-the-storage-replica-partnership"></a>Problema ao desbloquear a unidade de dados no servidor secundário após a interrupção da parceria de réplica de armazenamento

Depois de desabilitar a parceria SR e remover a réplica de armazenamento, ela será esperada se você não conseguir desbloquear a unidade de dados do servidor secundário com sua respectiva senha ou chave.

Você precisa usar a chave ou a senha da unidade de dados do servidor primário para desbloquear a unidade de dados do servidor secundário.

## <a name="test-failover-doesnt-mount-when-using-asynchronous-replication"></a>O failover de teste não é montado ao usar a replicação assíncrona

Ao executar o Mount-SRDestination para colocar um volume de destino online como parte do recurso de failover de teste, ele falha com o erro:

```
Mount-SRDestination: Unable to mount SR group <TEST>, detailed reason: The group or resource is not in the correct state to perform the supported operation.
    At line:1 char:1
    + Mount-SRDestination -ComputerName SRV1 -Name TEST -TemporaryP . . .
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (MSFT WvrAdminTasks : root/Microsoft/...(MSFT WvrAdminTasks : root/Microsoft/. T_WvrAdminTasks) (Mount-SRDestination], CimException
        + FullyQua1ifiedErrorId : Windows System Error 5823, Mount-SRDestination.
```

Se estiver usando um tipo de parceria síncrona, o failover de teste funcionará normalmente.

Isso é causado por um defeito de código conhecido no Windows Server, versão 1709. Para resolver esse problema, instale a [atualização de 18 de outubro de 2018](https://support.microsoft.com/help/4462932/windows-10-update-kb4462932). Esse problema não está presente no Windows Server 2019 e no Windows Server, versão 1809 e mais recente.

## <a name="additional-references"></a>Referências adicionais

- [Réplica de armazenamento](storage-replica-overview.md)
- [Estender a replicação do cluster usando o armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)
- [Cluster para replicação de armazenamento de cluster](cluster-to-cluster-storage-replication.md)
- [Réplica de armazenamento: perguntas frequentes](storage-replica-frequently-asked-questions.md)
- [Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)
