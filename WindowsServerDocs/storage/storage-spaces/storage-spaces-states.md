---
title: Integridade de espaços diretos de armazenamento e estados operacionais
description: Como localizar e compreender a integridade de diferente e os estados operacionais de espaços de armazenamento diretos e espaços de armazenamento (incluindo discos físicos, pools e discos virtuais) e o que fazer sobre eles.
keywords: Espaços de armazenamento, desanexados, disco virtual, o disco físico, degradado
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849137"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>Solucionar problemas de integridade de espaços de armazenamento diretos e estados operacionais

> Aplica-se a: 2019 do Windows Server, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semestral), Windows 10, Windows 8.1

Este tópico descreve a integridade e estados operacionais dos pools de armazenamento, discos virtuais (que ficam abaixo de volumes em espaços de armazenamento) e unidades no [espaços de armazenamento diretos](storage-spaces-direct-overview.md) e [espaços de armazenamento](overview.md). Esses estados podem ser inestimáveis, ao tentar solucionar vários problemas, como por que você não pode excluir um disco virtual devido a uma configuração somente leitura. Ele também discute por que uma unidade não pode ser adicionada a um pool (o CannotPoolReason).

Espaços de armazenamento tem três objetos principais - *discos físicos* (discos rígidos, SSDs, etc.) que são adicionados a um *pool de armazenamento*, virtualizar o armazenamento para que você possa criar *dediscosvirtuais* do espaço livre no pool, conforme mostrado aqui. Metadados do pool é gravado para cada unidade no pool. Volumes são criados sobre os discos virtuais e armazenam seus arquivos, mas não vamos falar sobre volumes aqui.

![Discos físicos são adicionados a um pool de armazenamento e, em seguida, discos virtuais criados a partir do espaço de pool](media/storage-spaces-states/storage-spaces-object-model.png)

Você pode exibir a integridade e estados operacionais no Gerenciador do servidor ou com o PowerShell. Aqui está um exemplo de uma variedade de integridade (principalmente ruim) e estados operacionais em um cluster de espaços de armazenamento diretos que está faltando a maioria de seus nós de cluster (clique com botão direito para adicionar os cabeçalhos de coluna **Status operacional**). Isso não é um cluster feliz.

![Gerenciador de servidores mostrando os resultados de dois nós ausentes em um cluster de espaços de armazenamento diretos - muita falta de discos físicos e os discos virtuais em um estado não íntegro](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Estados de pool de armazenamento

Cada pool de armazenamento tem um status de integridade - **Íntegro**, **aviso**, ou **desconhecido**/**não íntegro**, bem como uma ou mais estados operacionais.

Para descobrir qual estado é de um pool, use os seguintes comandos do PowerShell:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Aqui está um exemplo de saída mostrando um pool de armazenamento no estado de integridade desconhecido com o status operacional somente leitura:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

As seções a seguir listam a integridade e estados operacionais.

### <a name="pool-health-state-healthy"></a>Estado de integridade do pool: Íntegro

|estado operacional    |Descrição|
|---------            |---------  |
|OK|O pool de armazenamento está íntegro.|

### <a name="pool-health-state-warning"></a>Estado de integridade do pool: Aviso

Quando o pool de armazenamento está no **aviso** estado de integridade, isso significa que o pool está acessível, mas um ou mais discos com falhas ou ausentes. Como resultado, o pool de armazenamento pode ter reduzido em resiliência.

|estado operacional    |Descrição|
|---------            |---------  |
|Degradada|Existem unidades com falha ou ausentes no pool de armazenamento. Essa condição ocorre apenas com unidades de metadados do pool de hospedagem. <br><br>**Ação**: Verificar o estado de suas unidades e substitua todas as unidades com falha antes que haja falhas adicionais.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Estado de integridade do pool: Desconhecido ou não está íntegro

Quando um pool de armazenamento está no **desconhecido** ou **não íntegro** estado de integridade, isso significa que o pool de armazenamento é somente leitura e não pode ser modificado até que o pool é retornado para o **aviso**ou **Okey** estados de integridade.

|estado operacional    |Motivo de somente leitura |Descrição|
|---------            |---------       |--------   |
|Somente leitura|Incompleto|Isso pode ocorrer se o pool de armazenamento perde sua [quorum](understand-quorum.md), que significa que a maioria das unidades no pool falharam ou estão offline por algum motivo. Quando um pool perde seu quorum, espaços de armazenamento define automaticamente a configuração do pool para somente leitura até que as unidades suficientes ficam disponíveis novamente.<br><br>**Ação:** <br>1. Reconectar-se a todas as unidades ausentes e, em seguida, se você estiver usando espaços de armazenamento diretos, coloque todos os servidores online. <br>2. Defina o pool de volta para leitura e gravação abrindo uma sessão do PowerShell com permissões administrativas e, em seguida, digitando:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Política|Um administrador definir o pool de armazenamento para somente leitura.<br><br>**Ação:** Para definir um pool de armazenamento em cluster para leitura e gravação de acesso no Gerenciador de Cluster de Failover, vá para **Pools**, clique com botão direito no pool e, em seguida, selecione **colocar Online**.<br><br>Para outros servidores e PCs, abra uma sessão do PowerShell com permissões administrativas e, em seguida, digite:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Iniciando|Espaços de armazenamento está iniciando ou aguardando para unidades de conexão em pool. Isso deve ser um estado temporário. Depois de iniciado completamente, o pool deve fazer a transição para um estado operacional diferente.<br><br>**Ação:** Se o pool de permanecer a *iniciando* de estado, certifique-se de que todas as unidades no pool estão conectadas corretamente.|

Consulte também [modificando um Pool de armazenamento que tem uma configuração de somente leitura](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>Estados de disco virtual

Nos espaços de armazenamento, os volumes são colocados em discos virtuais (espaços de armazenamento) que são formados de espaço livre em um pool. Cada disco virtual tem um status de integridade - **Íntegro**, **aviso**, **Unhealthy**, ou **desconhecido** , bem como um ou mais estados operacionais.

Para descobrir quais discos virtuais de estado estão em, use os seguintes comandos do PowerShell:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Aqui está um exemplo de saída, mostrando um disco virtual desconectado e um disco virtual degradado/incompletas:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

As seções a seguir listam a integridade e estados operacionais.

### <a name="virtual-disk-health-state-healthy"></a>Estado de integridade do disco virtual: Íntegro

|estado operacional    |Descrição|
|---------            |---------          |
|OK    |O disco virtual está íntegro.|
|Abaixo do ideal    |Dados não for escritos uniformemente entre unidades. <br><br>**Ação**: Otimizar o uso da unidade no pool de armazenamento, executando o [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet.|

### <a name="virtual-disk-health-state-warning"></a>Estado de integridade do disco virtual: Aviso

Quando o disco virtual está em um **aviso** estado de integridade, isso significa que uma ou mais cópias de seus dados não estão disponíveis, mas os espaços de armazenamento ainda pode ler a pelo menos uma cópia dos seus dados.

|estado operacional    |Descrição|
|---------            |---------          |
|No serviço            |Windows é reparar o disco virtual, como após a adição ou remoção de uma unidade. Quando o reparo for concluído, o disco virtual deve retornar para o estado de integridade Okey.|
|Incompleto           |A resiliência do disco virtual é reduzida porque uma ou mais unidades de falharam ou estão ausentes. No entanto, as unidades ausentes contêm cópias atualizadas dos seus dados.<br><br> **Ação**: <br>1. Reconectar-se a todas as unidades ausentes, substitua todas as unidades com falha e se você estiver usando espaços de armazenamento diretos, colocar online todos os servidores que estão offline. <br>2. Se você não estiver usando espaços de armazenamento diretos, em seguida reparar o disco virtual usando o [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br> Espaços de armazenamento diretos é iniciado automaticamente um reparo se for necessário após a reconexão ou substituir uma unidade.|
|Degradada             |A resiliência do disco virtual é reduzida porque uma ou mais unidades de falharam ou estão ausentes e há desatualizadas cópias de seus dados nessas unidades. <br><br>**Ação**: <br> 1. Reconectar-se a todas as unidades ausentes, substitua todas as unidades com falha e se você estiver usando espaços de armazenamento diretos, colocar online todos os servidores que estão offline. <br> 2. Se você não estiver usando espaços de armazenamento diretos, em seguida reparar o disco virtual usando o [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet. <br>Espaços de armazenamento diretos é iniciado automaticamente um reparo se for necessário após a reconexão ou substituir uma unidade.|

### <a name="virtual-disk-health-state-unhealthy"></a>Estado de integridade do disco virtual: Unhealthy

Quando um disco virtual está em um **Unhealthy** estado de integridade, alguns ou todos os dados no disco virtual está inacessíveis no momento.

|estado operacional    |Descrição|
|---------            |--------   |
|Nenhuma redundância|O disco virtual perdeu dados porque muitas unidades falhou.<br><br>**Ação**: Substitua as unidades com falha e, em seguida, restaurar seus dados de backup.|

### <a name="virtual-disk-health-state-informationunknown"></a>Estado de integridade do disco virtual: Informações/desconhecido

O disco virtual também pode estar na **informações** estado de integridade (conforme mostrado no item do painel de controle de espaços de armazenamento) ou **desconhecido** integridade de estado (conforme mostrado no PowerShell) se um administrador levou o disco virtual offline ou o disco virtual se tornam destacado.

|estado operacional    |Motivo desanexado |Descrição|
|---------            |---------       |--------   |
|“Desanexado”             |Pela política            |Um administrador colocou o disco virtual offline, ou definir o disco virtual para exigir o anexo manual, nesse caso, você precisará anexar o disco virtual manualmente sempre que o Windows é reiniciado.,<br><br>**Ação**: Coloque o disco virtual online novamente. Para fazer isso, quando o disco virtual está em um pool de armazenamento em cluster, selecione no Gerenciador de Cluster de Failover **armazenamento** > **Pools** > **discos virtuais** , selecione o disco virtual que mostra a **Offline** status e, em seguida, selecione **colocar Online**. <br><br>Para colocar online um disco virtual quando não estiver em um cluster, abra uma sessão do PowerShell como administrador e, em seguida, tente usar o comando a seguir:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Para anexar automaticamente todos os discos virtuais não clusterizados após a reinicialização do Windows, abra uma sessão do PowerShell como administrador e, em seguida, use o seguinte comando:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Maioria de discos não íntegro |Muitas unidades usadas por esse disco virtual falharam, estão ausentes ou tem dados obsoletos.   <br><br>**Ação**: <br> 1. Reconectar-se a todas as unidades ausentes e, se você estiver usando espaços de armazenamento diretos, colocar online todos os servidores que estão offline. <br> 2. Depois que todas as unidades e servidores estiverem online, substitua todas as unidades com falha. Ver [serviço de integridade](../../failover-clustering/health-service-overview.md) para obter detalhes. <br>Espaços de armazenamento diretos é iniciado automaticamente um reparo se for necessário após a reconexão ou substituir uma unidade.<br>3. Se você não estiver usando espaços de armazenamento diretos, em seguida reparar o disco virtual usando o [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.  <br><br>Se tiver falhado mais discos que você tiver cópias de seus dados e o disco virtual não foi reparadas falhas entre as palavras, todos os dados no disco virtual for permanentemente perdido. Nesse caso uma pena, excluir o disco virtual, crie um novo disco virtual e, em seguida, restaurar de um backup.|
|                     |Incompleto |Não há unidades estão presentes para ler o disco virtual.    <br><br>**Ação**: <br> 1. Reconectar-se a todas as unidades ausentes e, se você estiver usando espaços de armazenamento diretos, colocar online todos os servidores que estão offline. <br> 2. Depois que todas as unidades e servidores estiverem online, substitua todas as unidades com falha. Ver [serviço de integridade](../../failover-clustering/health-service-overview.md) para obter detalhes. <br>Espaços de armazenamento diretos é iniciado automaticamente um reparo se for necessário após a reconexão ou substituir uma unidade.<br>3. Se você não estiver usando espaços de armazenamento diretos, em seguida reparar o disco virtual usando o [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br><br>Se tiver falhado mais discos que você tiver cópias de seus dados e o disco virtual não foi reparadas falhas entre as palavras, todos os dados no disco virtual for permanentemente perdido. Nesse caso uma pena, excluir o disco virtual, crie um novo disco virtual e, em seguida, restaurar de um backup.|
| |Limite de tempo|Anexar o disco virtual levou muito tempo <br><br> **Ação:** Isso não deve ocorrer com frequência, portanto, você pode tentar ver se a condição for atendida no tempo. Ou você pode tentar desconectar o disco virtual com o [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet, em seguida, usando o [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) para se reconectar a ele. |

## <a name="drive-physical-disk-states"></a>Estados da unidade (disco físico)

As seções a seguir descrevem os estados de integridade, uma unidade pode ser no. Unidades em um pool são representadas no PowerShell como *disco físico* objetos.

### <a name="drive-health-state-healthy"></a>Estado de integridade da unidade: Íntegro

|estado operacional    |Descrição|
|---------            |---------          |
|OK|A unidade está íntegra.|
|No serviço|A unidade é executar algumas operações de manutenção interna. Quando a ação for concluída, a unidade deve retornar para o *Okey* estado de integridade.|

### <a name="drive-health-state-warning"></a>Estado de integridade da unidade: Aviso

Uma unidade em que a lata de estado de aviso ler e gravar dados com êxito, mas tem um problema.

|estado operacional    |Descrição|
|---------            |---------          |
|Perda de comunicação|A unidade está ausente. Se você estiver usando espaços de armazenamento diretos, isso pode ser porque um servidor está inativo.<br><br>**Ação**: Se você estiver usando espaços de armazenamento diretos, coloque todos os servidores online novamente. Se isso não resolver a ele, reconectar-se a unidade, substituí-lo ou tente obter as informações de diagnóstico detalhadas sobre essa unidade, seguindo as etapas na solução de problemas usando o relatório de erros do Windows > [atingiu o disco físico](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Remover do pool|Espaços de armazenamento está no processo de remoção da unidade de seu pool de armazenamento. <br><br> Este é um estado temporário. Após a remoção for concluída, se a unidade estiver anexada ao sistema, a unidade faz a transição para outro estado operacional (geralmente Okey) em um pool primordial.|
|Modo de manutenção inicial|Espaços de armazenamento está no processo de colocar a unidade no modo de manutenção, depois que um administrador colocar a unidade no modo de manutenção. Este é um estado temporário – em breve, a unidade deve estar na *no modo de manutenção* estado.|
|No modo de manutenção|Um administrador colocado a unidade no modo de manutenção, interrompendo leituras e gravações da unidade. Geralmente, isso é feito antes de atualizar o firmware da unidade, ou durante o teste de falhas.<br><br>**Ação**: Para tirar a unidade do modo de manutenção, use o [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet.|
|Parar modo de manutenção|Um administrador levou a unidade do modo de manutenção e espaços de armazenamento está no processo de colocar a unidade online novamente. Este é um estado temporário – a unidade assim que deve ser o ideal é que em outro estado - *Íntegro*.|
|Falha preditiva|A unidade informou que está próximo com falha.<br><br>**Ação**: Substitua a unidade.|
|Erro de e/s|Ocorreu um erro temporário ao acessar a unidade.<br><br>**Ação**: <br>1. Se a unidade não fazer a transição para o **Okey** de estado, você pode tentar usar o [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet para apagar a unidade. <br> 2. Use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar a resiliência dos discos virtuais afetados. <br>3. Se isso continuar acontecendo, substitua a unidade.|
|Erro transitório|Ocorreu um erro temporário com a unidade. Isso geralmente significa que a unidade não estava respondendo, mas ele também pode significar que os espaços de armazenamento partição protetora inadequadamente foi removida da unidade. <br><br>**Ação**: <br>1. Se a unidade não fazer a transição para o **Okey** de estado, você pode tentar usar o [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet para apagar a unidade. <br> 2. Use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar a resiliência dos discos virtuais afetados. <br>3. Se isso continuar acontecendo, substituir a unidade, ou tente obter as informações de diagnóstico detalhadas sobre essa unidade, seguindo as etapas na solução de problemas usando o relatório de erros do Windows > [disco físico não pôde ser colocado on-line](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latência Anormal|A unidade está executando lentamente, conforme medido pelo serviço de integridade em espaços de armazenamento diretos.<br><br>**Ação**: Se isso continuar acontecendo, substitua o disco para que ela não reduz o desempenho dos espaços de armazenamento como um todo.

### <a name="drive-health-state-unhealthy"></a>Estado de integridade da unidade: Unhealthy

Uma unidade no estado não íntegro atualmente não pode ser gravada ou acessada.

|estado operacional    |Descrição|
|---------            |---------          |
|Não pode ser usado|Essa unidade não pode ser usada por espaços de armazenamento. Para obter mais informações, consulte [requisitos de hardware de espaços de armazenamento diretos](storage-spaces-direct-hardware-requirements.md); se você não estiver usando espaços de armazenamento diretos, consulte [visão geral dos espaços de armazenamento](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|Divisão|A unidade tem se tornam separada do pool.<br><br>**Ação**: Redefina a unidade, apagar todos os dados da unidade e adicioná-lo de volta para o pool como uma unidade vazia. Para fazer isso, abra uma sessão do PowerShell como administrador, execute as [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet e, em seguida, execute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Para obter informações de diagnóstico detalhadas sobre essa unidade, siga as etapas na solução de problemas usando o relatório de erros do Windows > [disco físico não pôde ser colocado on-line](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadados obsoletos|Espaços de armazenamento encontrou metadados antigos na unidade.<br><br>**Ação**: Isso deve ser um estado temporário. Se a unidade não fazer a transição para Okey, você pode executar [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) iniciar uma operação de reparo nos discos virtuais afetados. Se isso não resolver o problema, você pode redefinir a unidade com o [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet, apagando todos os dados da unidade e, em seguida, execute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadados não reconhecidos|Espaços de armazenamento encontrada metadados não reconhecidos na unidade, que normalmente significa que a unidade tem metadados de um pool diferente nele.<br><br>**Ação**: Para apagar a unidade e adicioná-lo ao pool atual, redefina a unidade. Para redefinir a unidade, abra uma sessão do PowerShell como administrador, execute as [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet e, em seguida, execute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Falha na mídia|A unidade de falha e não será usada por espaços de armazenamento mais.<br><br>**Ação**: Substitua a unidade. <br><br>Para obter informações de diagnóstico detalhadas sobre essa unidade, siga as etapas na solução de problemas usando o relatório de erros do Windows > [disco físico não pôde ser colocado on-line](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Falha de hardware do dispositivo|Houve uma falha de hardware nessa unidade. <br><br>**Ação**: Substitua a unidade.|
|Atualização do firmware|Windows está atualizando o firmware da unidade. Esse é um estado temporário que, geralmente, dura menos de um minuto e durante o qual o tempo de outras unidades no pool de lidar com todas as leituras e gravações. Para obter mais informações, consulte [atualizar o firmware da unidade](../update-firmware.md).|
|Iniciando|A unidade está se preparando para a operação. Isso deve ser um estado temporário - uma vez concluído, que a unidade deve fazer a transição para um estado operacional diferente.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Motivos de que uma unidade não pode ser colocado em pool

Algumas unidades apenas não estão prontas para estar em um pool de armazenamento. Você pode descobrir por que uma unidade não é elegível ao pooling examinando o `CannotPoolReason` propriedade de um disco físico. Aqui está um exemplo de script do PowerShell para exibir a propriedade CannotPoolReason:

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

Aqui está um exemplo de saída:

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

A tabela a seguir fornece mais detalhes sobre cada um dos motivos.

|Motivo|Descrição|
|---|---|
|Em um pool|A unidade já pertence a um pool de armazenamento. <br><br>Unidades podem pertencer a apenas um único pool de armazenamento por vez. Para usar essa unidade em outro pool de armazenamento, primeiro remova a unidade do seu pool existente, que informa à espaços de armazenamento para mover os dados na unidade para outras unidades no pool. Ou redefina a unidade se a unidade foi desconectada do seu pool sem notificar os espaços de armazenamento. <br><br>Para remover com segurança uma unidade de um pool de armazenamento, use [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk), ou vá para o Gerenciador de servidores > **serviços de arquivo e armazenamento** > **Pools de armazenamento**, > **Discos físicos**, a unidade com o botão direito e, em seguida, selecione **Remover disco**.<br><br>Para redefinir uma unidade, use [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Não está íntegro|A unidade não está em um estado íntegro e talvez precise ser substituído.|
|Mídia removível|A unidade é classificada como uma unidade removível. <br><br>Espaços de armazenamento não dá suporte a mídia que é reconhecidas pelo Windows como removível, como unidades de disco Blu-Ray. Embora muitos fixos unidades estão em slots removíveis, em geral, mídia que estão *classificados* pelo Windows como removível não são adequadas para uso com espaços de armazenamento.|
|Em uso por cluster|Atualmente, a unidade é usada por um Cluster de Failover.|
|Offline|A unidade está offline. <br><br>Para colocar offline todos os drives on-line e definido como leitura/gravação, abra uma sessão do PowerShell como administrador e use os scripts a seguir:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacidade insuficiente|Isso normalmente ocorre quando há partições ocupando o espaço livre na unidade. <br><br>**Ação**: Exclua todos os volumes no disco, apagando todos os dados na unidade. Uma maneira de fazer isso é usar o [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) cmdlet do PowerShell.|
|Verificação em andamento|O [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document) está verificando se a unidade ou o firmware da unidade é aprovado para uso pelo administrador do servidor.|
|Falha na verificação|O [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document) não foi possível verificar para ver se a unidade ou o firmware da unidade é aprovado para uso pelo administrador do servidor.|
|Firmware não estão em conformidade|O firmware da unidade física não está na lista de revisões de firmware aprovados especificados pelo administrador do servidor usando o [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware não está em conformidade|A unidade não está na lista de modelos de armazenamento aprovadas especificados pelo administrador do servidor usando o [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Consulte também

- [Espaços de armazenamento diretos](storage-spaces-direct-overview.md)
- [Requisitos de hardware de espaços diretos de armazenamento](storage-spaces-direct-hardware-requirements.md)
- [Noções básicas sobre cluster e pool de quorum](understand-quorum.md)