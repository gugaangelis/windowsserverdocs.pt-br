---
title: Espaços de armazenamento e Estados operacionais de Espaços de Armazenamento Diretos e integridade
description: Como localizar e entender os diferentes Estados operacionais e de integridade de Espaços de Armazenamento Diretos e espaços de armazenamento (incluindo discos físicos, pools e discos virtuais) e o que fazer a respeito.
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 83489cb7a8a44de13b5ba245d7ce1cb5ceabc08e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820969"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>Solucionar problemas de espaços de armazenamento e de Espaços de Armazenamento Diretos integridade e Estados operacionais

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semestral), Windows 10, Windows 8.1

Este tópico descreve os Estados de integridade e operacionais dos pools de armazenamento, discos virtuais (que ficam abaixo dos volumes em espaços de armazenamento) e unidades em [espaços de armazenamento diretos](storage-spaces-direct-overview.md) e [espaços de armazenamento](overview.md). Esses Estados podem ser inúteis ao tentar solucionar vários problemas, como por que não é possível excluir um disco virtual devido a uma configuração somente leitura. Ele também discute por que uma unidade não pode ser adicionada a um pool (o CannotPoolReason).

Os espaços de armazenamento têm três objetos principais – *discos físicos* (discos rígidos, SSDs, etc.) que são adicionados a um *pool de armazenamento*, virtualizando o armazenamento para que você possa criar *discos virtuais* de espaço livre no pool, como mostrado aqui. Os metadados do pool são gravados em cada unidade no pool. Os volumes são criados sobre os discos virtuais e armazenam seus arquivos, mas não vamos falar sobre volumes aqui.

![Discos físicos são adicionados a um pool de armazenamento e, em seguida, discos virtuais criados a partir do espaço do pool](media/storage-spaces-states/storage-spaces-object-model.png)

Você pode exibir Estados de integridade e operacionais no Gerenciador do Servidor ou com o PowerShell. Aqui está um exemplo de uma variedade de Estados operacionais e de integridade (principalmente incorretos) em um cluster Espaços de Armazenamento Diretos que não possui a maior parte de seus nós de cluster (clique com o botão direito do mouse nos cabeçalhos de coluna para adicionar o **status operacional**). Esse não é um bom cluster.

![Gerenciador do Servidor mostrando os resultados de dois nós ausentes em um cluster Espaços de Armazenamento Diretos-muitos discos físicos ausentes e discos virtuais em um estado não íntegro](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Estados do pool de armazenamento

Cada pool de armazenamento tem um status de integridade- **íntegro**, **aviso**ou **desconhecido**/não **íntegro**, bem como um ou mais Estados operacionais.

Para descobrir em que estado um pool se encontra, use os seguintes comandos do PowerShell:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Aqui está um exemplo de saída mostrando um pool de armazenamento no estado de integridade desconhecido com o status operacional somente leitura:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

As seções a seguir listam os Estados de integridade e operacionais.

### <a name="pool-health-state-healthy"></a>Estado de integridade do pool: íntegro

| Estado operacional    | Descrição |
| ---------            | ---------  |
| OK | O pool de armazenamento está íntegro. |

### <a name="pool-health-state-warning"></a>Estado de integridade do pool: aviso

Quando o pool de armazenamento está no estado de integridade de **aviso** , isso significa que o pool está acessível, mas uma ou mais unidades falharam ou estão ausentes. Como resultado, o pool de armazenamento pode ter uma resiliência reduzida.

|Estado operacional    |Descrição|
|---------            |---------  |
|Degradada|Há unidades com falha ou ausentes no pool de armazenamento. Essa condição ocorre apenas com unidades que hospedam metadados de pool. <br><br>**Ação**: Verifique o estado das unidades e substitua as unidades com falha antes de haver falhas adicionais.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Estado de integridade do pool: desconhecido ou não íntegro

Quando um pool de armazenamento está no estado de integridade **desconhecido** ou não **íntegro** , isso significa que o pool de armazenamento é somente leitura e não pode ser modificado até que o pool seja retornado para os Estados de integridade de **aviso** ou **OK** .

|Estado operacional    |Motivo de somente leitura |Descrição|
|---------            |---------       |--------   |
|Somente leitura|Incompleto|Isso pode ocorrer se o pool de armazenamento perder seu [Quorum](understand-quorum.md), o que significa que a maioria das unidades no pool falhou ou está offline por algum motivo. Quando um pool perde seu quorum, os espaços de armazenamento definem automaticamente a configuração do pool como somente leitura até que as unidades suficientes fiquem disponíveis novamente.<br><br>**Action** <br>1. Reconecte as unidades ausentes e, se você estiver usando Espaços de Armazenamento Diretos, coloque todos os servidores online. <br>2. defina o pool de volta para leitura/gravação abrindo uma sessão do PowerShell com permissões administrativas e, em seguida, digitando:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Política|Um administrador define o pool de armazenamento como somente leitura.<br><br>**Ação:** Para definir um pool de armazenamento clusterizado para acesso de leitura/gravação em Gerenciador de Cluster de Failover, vá para **pools**, clique com o botão direito do mouse no pool e selecione **colocar online**.<br><br>Para outros servidores e computadores, abra uma sessão do PowerShell com permissões administrativas e, em seguida, digite:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Iniciando|Os espaços de armazenamento estão sendo iniciados ou aguardando que as unidades sejam conectadas no pool. Esse deve ser um estado temporário. Uma vez completamente iniciado, o pool deve fazer a transição para um estado operacional diferente.<br><br>**Ação:** Se o pool permanecer no estado de *inicialização* , verifique se todas as unidades no pool estão conectadas corretamente.|

Consulte também [modificando um pool de armazenamento que tem uma configuração somente leitura](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>Estados de disco virtual

Em espaços de armazenamento, os volumes são colocados em discos virtuais (espaços de armazenamento) que são gravados sem espaço livre em um pool. Cada disco virtual tem um status de integridade- **íntegro**, **aviso**, não **íntegro**ou **desconhecido** , bem como um ou mais Estados operacionais.

Para descobrir em quais Estados os discos virtuais estão, use os seguintes comandos do PowerShell:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Aqui está um exemplo de saída mostrando um disco virtual desanexado e um disco virtual degradado/incompleto:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

As seções a seguir listam os Estados de integridade e operacionais.

### <a name="virtual-disk-health-state-healthy"></a>Estado de integridade do disco virtual: íntegro

|Estado operacional    |Descrição|
|---------            |---------          |
|OK    |O disco virtual está íntegro.|
|Ideal    |Os dados não são gravados uniformemente entre as unidades. <br><br>**Ação**: Otimize o uso da unidade no pool de armazenamento executando o cmdlet [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) .|

### <a name="virtual-disk-health-state-warning"></a>Estado de integridade do disco virtual: aviso

Quando o disco virtual está em um estado de integridade de **aviso** , isso significa que uma ou mais cópias de seus dados estão indisponíveis, mas os espaços de armazenamento ainda podem ler pelo menos uma cópia dos dados.

|Estado operacional    |Descrição|
|---------            |---------          |
|Em serviço            |O Windows está reparando o disco virtual, como depois de adicionar ou remover uma unidade. Quando o reparo for concluído, o disco virtual deverá retornar para o estado de integridade OK.|
|Incompleto           |A resiliência do disco virtual é reduzida porque uma ou mais unidades falharam ou estão ausentes. No entanto, as unidades ausentes contêm cópias atualizadas dos dados.<br><br> **Ação**: <br>1. Reconecte as unidades ausentes, substitua todas as unidades com falha e, se você estiver usando Espaços de Armazenamento Diretos, coloque todos os servidores que estiverem offline. <br>2. se você não estiver usando Espaços de Armazenamento Diretos, em seguida, repare o disco virtual usando o cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br> Espaços de Armazenamento Diretos iniciar automaticamente um reparo, se necessário, após a reconexão ou substituição de uma unidade.|
|Degradada             |A resiliência do disco virtual é reduzida porque uma ou mais unidades falharam ou estão ausentes, e há cópias desatualizadas dos dados nessas unidades. <br><br>**Ação**: <br> 1. Reconecte as unidades ausentes, substitua todas as unidades com falha e, se você estiver usando Espaços de Armazenamento Diretos, coloque todos os servidores que estiverem offline. <br> 2. se você não estiver usando Espaços de Armazenamento Diretos, em seguida, repare o disco virtual usando o cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) . <br>Espaços de Armazenamento Diretos iniciar automaticamente um reparo, se necessário, após a reconexão ou substituição de uma unidade.|

### <a name="virtual-disk-health-state-unhealthy"></a>Estado de integridade do disco virtual: não íntegro

Quando um disco virtual está em um estado de integridade não **íntegro** , alguns ou todos os dados no disco virtual estão inacessíveis no momento.

|Estado operacional    |Descrição|
|---------            |--------   |
|Sem redundância|O disco virtual perdeu dados porque muitas unidades falharam.<br><br>**Ação**: substitua as unidades com falha e restaure os dados do backup.|

### <a name="virtual-disk-health-state-informationunknown"></a>Estado de integridade do disco virtual: informações/desconhecido

O disco virtual também pode estar no estado de integridade **das informações** (conforme mostrado no item do painel de controle espaços de armazenamento) ou estado de integridade **desconhecido** (como mostrado no PowerShell) se um administrador colocou o disco virtual offline ou se o disco virtual foi desanexado.

|Estado operacional    |Motivo desanexado |Descrição|
|---------            |---------       |--------   |
|“Desanexado”             |Por política            |Um administrador colocou o disco virtual offline ou definiu o disco virtual para exigir o anexo manual. nesse caso, você precisará anexar manualmente o disco virtual sempre que o Windows for reiniciado.<br><br>**Ação**: Coloque o disco virtual novamente online. Para fazer isso, quando o disco virtual estiver em um pool de armazenamento clusterizado, em Gerenciador de Cluster de Failover selecione **armazenamento** > **pools** > **discos virtuais**, selecione o disco virtual que mostra o status **offline** e, em seguida, selecione **colocar online**. <br><br>Para colocar um disco virtual online novamente quando não estiver em um cluster, abra uma sessão do PowerShell como administrador e tente usar o seguinte comando:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Para anexar automaticamente todos os discos virtuais não clusterizados após a reinicialização do Windows, abra uma sessão do PowerShell como administrador e, em seguida, use o seguinte comando:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Maioria dos discos não íntegros |Um número excessivo de unidades usadas por este disco virtual falhou, está ausente ou tem dados obsoletos.   <br><br>**Ação**: <br> 1. Reconecte as unidades ausentes e, se você estiver usando Espaços de Armazenamento Diretos, coloque todos os servidores offline. <br> 2. depois que todas as unidades e servidores estiverem online, substitua todas as unidades com falha. Consulte [serviço de integridade](../../failover-clustering/health-service-overview.md) para obter detalhes. <br>Espaços de Armazenamento Diretos iniciar automaticamente um reparo, se necessário, após a reconexão ou substituição de uma unidade.<br>3. se você não estiver usando Espaços de Armazenamento Diretos, em seguida, repare o disco virtual usando o cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .  <br><br>Se houver falha em mais discos do que você tem cópias de seus dados e o disco virtual não tiver sido reparado entre falhas, todos os dados no disco virtual serão permanentemente perdidos. Nesse caso, exclua o disco virtual, crie um novo disco virtual e, em seguida, restaure de um backup.|
|                     |Incompleto |Não há unidades suficientes presentes para ler o disco virtual.    <br><br>**Ação**: <br> 1. Reconecte as unidades ausentes e, se você estiver usando Espaços de Armazenamento Diretos, coloque todos os servidores offline. <br> 2. depois que todas as unidades e servidores estiverem online, substitua todas as unidades com falha. Consulte [serviço de integridade](../../failover-clustering/health-service-overview.md) para obter detalhes. <br>Espaços de Armazenamento Diretos iniciar automaticamente um reparo, se necessário, após a reconexão ou substituição de uma unidade.<br>3. se você não estiver usando Espaços de Armazenamento Diretos, em seguida, repare o disco virtual usando o cmdlet [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br><br>Se houver falha em mais discos do que você tem cópias de seus dados e o disco virtual não tiver sido reparado entre falhas, todos os dados no disco virtual serão permanentemente perdidos. Nesse caso, exclua o disco virtual, crie um novo disco virtual e, em seguida, restaure de um backup.|
| |Limite de tempo|Anexar o disco virtual demorou muito tempo <br><br> **Ação:** Isso não deveria acontecer com frequência, portanto, você pode tentar ver se a condição passou no tempo. Ou você pode tentar desconectar o disco virtual com o cmdlet [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) e, em seguida, usar o cmdlet [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) para reconectá-lo. |

## <a name="drive-physical-disk-states"></a>Estados da unidade (disco físico)

As seções a seguir descrevem os Estados de integridade em que uma unidade pode estar. As unidades em um pool são representadas no PowerShell como objetos de *disco físico* .

### <a name="drive-health-state-healthy"></a>Estado de integridade da unidade: íntegro

|Estado operacional    |Descrição|
|---------            |---------          |
|OK|A unidade está íntegra.|
|Em serviço|A unidade está executando algumas operações internas de manutenção. Quando a ação for concluída, a unidade deverá retornar ao estado de integridade *OK* .|

### <a name="drive-health-state-warning"></a>Estado de integridade da unidade: aviso

Uma unidade no estado de aviso pode ler e gravar dados com êxito, mas tem um problema.

|Estado operacional    |Descrição|
|---------            |---------          |
|Comunicação perdida|A unidade está ausente. Se você estiver usando Espaços de Armazenamento Diretos, isso pode ocorrer porque um servidor está inoperante.<br><br>**Ação**: se você estiver usando espaços de armazenamento diretos, coloque todos os servidores novamente online. Se isso não corrigir, reconecte a unidade, substitua-a ou tente obter informações de diagnóstico detalhadas sobre essa unidade seguindo as etapas em solução de problemas usando Relatório de Erros do Windows > [disco físico esgotou o tempo limite](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Removendo do pool|Os espaços de armazenamento estão no processo de remover a unidade de seu pool de armazenamento. <br><br> Esse é um estado temporário. Após a conclusão da remoção, se a unidade ainda estiver anexada ao sistema, a unidade passará para outro estado operacional (normalmente OK) em um pool primordial.|
|Iniciando o modo de manutenção|Os espaços de armazenamento estão no processo de colocar a unidade no modo de manutenção depois que um administrador coloca a unidade no modo de manutenção. Esse é um estado temporário-a unidade deve estar em breve no estado do *modo de manutenção* .|
|No modo de manutenção|Um administrador colocou a unidade no modo de manutenção, interrompendo leituras e gravações da unidade. Isso geralmente é feito antes da atualização do firmware da unidade ou do teste de falhas.<br><br>**Ação**: para retirar a unidade do modo de manutenção, use o cmdlet [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) .|
|Parando o modo de manutenção|Um administrador tirou a unidade do modo de manutenção e os espaços de armazenamento estão no processo de colocar a unidade online novamente. Esse é um estado temporário-a unidade deve estar em breve em outro Estado – idealmente *íntegro*.|
|Falha preditiva|A unidade relatou que está perto de falhar.<br><br>**Ação**: substitua a unidade.|
|Erro de e/s|Ocorreu um erro temporário ao acessar a unidade.<br><br>**Ação**: <br>1. se a unidade não fizer a transição de volta para o estado **OK** , você poderá tentar usar o cmdlet [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) para apagar a unidade. <br> 2. use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar a resiliência dos discos virtuais afetados. <br>3. se isso continuar acontecendo, substitua a unidade.|
|Erro transitório|Ocorreu um erro temporário com a unidade. Isso geralmente significa que a unidade não estava respondendo, mas também pode significar que a partição de proteção de espaços de armazenamento foi removida incorretamente da unidade. <br><br>**Ação**: <br>1. se a unidade não fizer a transição de volta para o estado **OK** , você poderá tentar usar o cmdlet [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) para apagar a unidade. <br> 2. use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar a resiliência dos discos virtuais afetados. <br>3. se isso continuar acontecendo, substitua a unidade ou tente obter informações de diagnóstico detalhadas sobre esta unidade seguindo as etapas em solução de problemas usando Relatório de Erros do Windows > [disco físico falhou em ficar online](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latência anormal|A unidade está sendo executada lentamente, conforme medido pelo Serviço de Integridade em Espaços de Armazenamento Diretos.<br><br>**Ação**: se isso continuar acontecendo, substitua a unidade para que ela não reduza o desempenho dos espaços de armazenamento como um todo.

### <a name="drive-health-state-unhealthy"></a>Estado de integridade da unidade: não íntegro

Uma unidade no estado não íntegro não pode ser gravada ou acessada no momento.

|Estado operacional    |Descrição|
|---------            |---------          |
|Não utilizável|Esta unidade não pode ser usada por espaços de armazenamento. Para obter mais informações, consulte [espaços de armazenamento diretos requisitos de hardware](storage-spaces-direct-hardware-requirements.md); Se você não estiver usando Espaços de Armazenamento Diretos, consulte [visão geral de espaços de armazenamento](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|Split|A unidade se tornou separada do pool.<br><br>**Ação**: redefina a unidade, apagando todos os dados da unidade e adicionando-o de volta ao pool como uma unidade vazia. Para fazer isso, abra uma sessão do PowerShell como administrador, execute o cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) e execute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Para obter informações de diagnóstico detalhadas sobre esta unidade, siga as etapas em solução de problemas usando Relatório de Erros do Windows > [disco físico não pôde ser colocado online](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadados obsoletos|Os espaços de armazenamento encontraram metadados antigos na unidade.<br><br>**Ação**: deve ser um estado temporário. Se a unidade não fizer a transição de volta para OK, você poderá executar o [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) para iniciar uma operação de reparo em discos virtuais afetados. Se isso não resolver o problema, você poderá redefinir a unidade com o cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , apagar todos os dados da unidade e executar [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadados não reconhecidos|Os espaços de armazenamento encontraram metadados não reconhecidos na unidade, o que geralmente significa que a unidade tem metadados de um pool diferente.<br><br>**Ação**: para apagar a unidade e adicioná-la ao pool atual, redefina a unidade. Para redefinir a unidade, abra uma sessão do PowerShell como administrador, execute o cmdlet [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) e execute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Mídia com falha|A unidade falhou e não será mais usada por espaços de armazenamento.<br><br>**Ação**: substitua a unidade. <br><br>Para obter informações de diagnóstico detalhadas sobre esta unidade, siga as etapas em solução de problemas usando Relatório de Erros do Windows > [disco físico não pôde ser colocado online](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Falha de hardware do dispositivo|Houve uma falha de hardware nesta unidade. <br><br>**Ação**: substitua a unidade.|
|Atualização do firmware|O Windows está atualizando o firmware na unidade. Esse é um estado temporário que geralmente dura menos de um minuto e durante o qual o tempo que outras unidades no pool lidam com todas as leituras e gravações. Para obter mais informações, consulte [atualizar firmware da unidade](../update-firmware.md).|
|Iniciando|A unidade está se preparando para a operação. Isso deve ser um estado temporário-uma vez concluído, a unidade deve fazer a transição para um estado operacional diferente.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Motivos pelos quais uma unidade não pode ser agrupada

Algumas unidades não estão prontas para estarem em um pool de armazenamento. Você pode descobrir por que uma unidade não está qualificada para pooling examinando a propriedade `CannotPoolReason` de um disco físico. Aqui está um exemplo de script do PowerShell para exibir a propriedade CannotPoolReason:

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

A tabela a seguir fornece um pouco mais de detalhes sobre cada um dos motivos.

|Reason|Descrição|
|---|---|
|Em um pool|A unidade já pertence a um pool de armazenamento. <br><br>As unidades podem pertencer apenas a um único pool de armazenamento de cada vez. Para usar essa unidade em outro pool de armazenamento, primeiro remova a unidade de seu pool existente, o que informa aos espaços de armazenamento para mover os dados na unidade para outras unidades no pool. Ou redefina a unidade se a unidade tiver sido desconectada de seu pool sem notificar os espaços de armazenamento. <br><br>Para remover uma unidade de um pool de armazenamento com segurança, use [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)ou vá para Gerenciador do servidor > **serviços de arquivo e armazenamento** > **pools de armazenamento**, > **discos físicos**, clique com o botão direito do mouse na unidade e selecione **remover disco**.<br><br>Para redefinir uma unidade, use [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Não íntegro|A unidade não está em um estado íntegro e talvez precise ser substituída.|
|Mídia removível|A unidade é classificada como uma unidade removível. <br><br>Os espaços de armazenamento não dão suporte a mídia reconhecida pelo Windows como removível, como unidades Blu-Ray. Embora muitas unidades fixas estejam em slots removíveis, em geral, a mídia *classificada* pelo Windows como removível não é adequada para uso com espaços de armazenamento.|
|Em uso pelo cluster|A unidade é usada atualmente por um cluster de failover.|
|Offline|A unidade está offline. <br><br>Para colocar todas as unidades offline e definir como leitura/gravação, abra uma sessão do PowerShell como administrador e use os seguintes scripts:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacidade insuficiente|Isso normalmente ocorre quando há partições que ocupam espaço livre na unidade. <br><br>**Ação**: exclua todos os volumes na unidade, apagando todos os dados na unidade. Uma maneira de fazer isso é usar o cmdlet [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) do PowerShell.|
|Verificação em andamento|A [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document) está verificando se a unidade ou o firmware na unidade foi aprovado para uso pelo administrador do servidor.|
|Falha na verificação|O [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document) não pôde verificar se a unidade ou o firmware na unidade foi aprovado para uso pelo administrador do servidor.|
|Firmware não compatível|O firmware na unidade física não está na lista de revisões de firmware aprovadas especificadas pelo administrador do servidor usando o [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware incompatível|A unidade não está na lista de modelos de armazenamento aprovados especificados pelo administrador do servidor usando o [serviço de integridade](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Consulte também

- [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Requisitos de hardware Espaços de Armazenamento Diretos](storage-spaces-direct-hardware-requirements.md)
- [Noções básicas de quórum de cluster e de pool](understand-quorum.md)