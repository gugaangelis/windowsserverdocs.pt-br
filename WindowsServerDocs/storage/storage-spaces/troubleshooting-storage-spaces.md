---
title: Solução de problemas Espaços de Armazenamento Diretos
description: Saiba como solucionar problemas de implantação de Espaços de Armazenamento Diretos.
ms.prod: windows-server
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: ca3a1ec8462f96c1f6a018d1148b7824cdf8cc20
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769464"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Solucionar problemas Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Use as informações a seguir para solucionar problemas de implantação do Espaços de Armazenamento Diretos.

Em geral, comece com as seguintes etapas:

1. Confirme se a marca/modelo do SSD é certificado para o Windows Server 2016 e o Windows Server 2019 usando o catálogo do Windows Server. Confirme com o fornecedor que as unidades têm suporte para Espaços de Armazenamento Diretos.
2. Inspecione o armazenamento em busca de quaisquer unidades defeituosas. Use o software de gerenciamento de armazenamento para verificar o status das unidades. Se qualquer uma das unidades for defeituosa, trabalhe com seu fornecedor.
3. Atualize o armazenamento e a unidade de firmware, se necessário.
   Verifique se as atualizações mais recentes do Windows estão instaladas em todos os nós. Você pode obter as atualizações mais recentes para o Windows Server 2016 do histórico de atualização do Windows [10 e do Windows server 2016](https://aka.ms/update2016) e para o windows Server 2019 do histórico de atualização do Windows [10 e do Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Atualize os drivers do adaptador de rede e o firmware.
5. Execute a validação de cluster e examine a seção espaço de armazenamento direto, verifique se as unidades que serão usadas para o cache são relatadas corretamente e não há erros.

Se você ainda tiver problemas, examine os cenários a seguir.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Os recursos de disco virtual não estão em estado de redundância
Os nós de um Espaços de Armazenamento Diretos sistema são reiniciados inesperadamente devido a uma falha ou energia. Em seguida, um ou mais discos virtuais podem não ficar online e você verá a descrição "não há informações de redundância suficientes".

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Tamanho| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Espelho| OK|  Íntegros| Verdadeiro|  10 TB|  Nó-01. cont...|
|Disk3         |Espelho                 |OK                          |Íntegros       |Verdadeiro            |10 TB | Nó-01. cont...|
|Disk2         |Espelho                 |Sem redundância               |Unhealthy     |Verdadeiro            |10 TB | Nó-01. cont...|
|Disk1         |Espelho                 |{Sem redundância, InService}  |Unhealthy     |Verdadeiro            |10 TB | Nó-01. cont...|

Além disso, após uma tentativa de colocar o disco virtual online, as informações a seguir são registradas no log de cluster (DiskRecoveryAction).

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable.
```

O **status operacional sem redundância** poderá ocorrer se um disco falhar ou se o sistema não puder acessar dados no disco virtual. Esse problema pode ocorrer se uma reinicialização ocorrer em um nó durante a manutenção nos nós.

Para corrigir esse problema, siga estas etapas:

1. Remova os discos virtuais afetados do CSV. Isso os colocará no grupo "armazenamento disponível" no cluster e começará a ser exibido como um ResourceType de "disco físico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ```
2. No nó que possui o grupo de armazenamento disponível, execute o comando a seguir em cada disco que esteja em um estado sem redundância. Para identificar em qual nó o grupo "armazenamento disponível" está, você pode executar o comando a seguir.

   ```powershell
   Get-ClusterGroup
   ```
3. Defina a ação de recuperação de disco e inicie os discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Um reparo deve ser iniciado automaticamente. Aguarde a conclusão do reparo. Ele pode entrar em um estado suspenso e começar novamente. Para monitorar o progresso:
    - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele for concluído.
    - Execute **Get-VirtualDisk** e verifique se o espaço retorna um HealthStatus de íntegro.
5. Depois que o reparo for concluído e os discos virtuais estiverem íntegros, altere os parâmetros do disco virtual de volta.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Coloque os discos offline e, em seguida, online novamente para que os DiskRecoveryAction entrem em vigor:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ```
7. Adicione os discos virtuais afetados de volta ao CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** é uma opção de substituição que permite anexar o volume de espaço no modo de leitura/gravação sem nenhuma verificação. A propriedade permite que você faça diagnósticos no porquê de um volume não ficar online. É muito semelhante ao modo de manutenção, mas você pode chamá-lo em um recurso em um estado de falha. Ele também permite que você acesse os dados, o que pode ser útil em situações como "sem redundância", em que você pode obter acesso a quaisquer dados que você possa e copiá-los. A propriedade DiskRecoveryAction foi adicionada em 22 de fevereiro de 2018, atualização, KB 4077525.


## <a name="detached-status-in-a-cluster"></a>Status desanexado em um cluster

Quando você executa o cmdlet **Get-VirtualDisk** , o OperationalStatus para um ou mais discos virtuais espaços de armazenamento diretos é desanexado. No entanto, o HealthStatus relatado pelo cmdlet **Get-PhysicalDisk** indica que todos os discos físicos estão em um estado íntegro.

Veja a seguir um exemplo da saída do cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Tamanho|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Espelho|                 OK|                  Íntegros|       Verdadeiro|            10 TB|  Nó-01. cont...|
|Disk3|         Espelho|                 OK|                  Íntegros|       Verdadeiro|            10 TB|  Nó-01. cont...|
|Disk2|         Espelho|                 Desanexado|            Unknown (desconhecido)|       Verdadeiro|            10 TB|  Nó-01. cont...|
|Disk1|         Espelho|                 Desanexado|            Unknown (desconhecido)|       Verdadeiro|            10 TB|  Nó-01. cont...|


Além disso, os seguintes eventos podem ser registrados nos nós:

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver
Event ID: 311
Level: Error
User: SYSTEM
Computer: Node#.contoso.local
Description: Virtual disk {GUID} requires a data integrity scan.

Data on the disk is out-of-sync and a data integrity scan is required.

To start the scan, run the following command:
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask

Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS
Event ID: 134
Level: Error
User: SYSTEM
Computer: Node#.contoso.local
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS
Event ID: 5
Level: Error
User: SYSTEM
Computer: Node#.contoso.local
Description: ReFS failed to mount the volume.
Context: 0xffffbb89f53f4180
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000}
DeviceName:
Volume Name:
```

O **status operacional desanexado** pode ocorrer se o log de controle de região sujo (DRT) estiver cheio. Os espaços de armazenamento usam DRT (controle de região suja) para espaços espelhados para garantir que, quando ocorre uma falha de energia, quaisquer atualizações em andamento para metadados são registradas para garantir que o espaço de armazenamento possa refazer ou desfazer operações para colocar o espaço de armazenamento em um estado flexível e consistente quando a energia for restaurada e o sistema voltar a funcionar. Se o log DRT estiver cheio, o disco virtual não poderá ser colocado online até que os metadados DRT sejam sincronizados e liberados. Esse processo requer a execução de uma verificação completa, o que pode levar várias horas para ser concluído.

Para corrigir esse problema, siga estas etapas:
1. Remova os discos virtuais afetados do CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ```
2. Execute os comandos a seguir em todos os discos que não estão disponíveis online.

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ```
3. Execute o comando a seguir em cada nó no qual o volume desanexado esteja online.

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask
   ```
   Essa tarefa deve ser iniciada em todos os nós nos quais o volume desanexado está online. Um reparo deve ser iniciado automaticamente. Aguarde a conclusão do reparo. Ele pode entrar em um estado suspenso e começar novamente. Para monitorar o progresso:
   - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele for concluído.
   - Execute **Get-VirtualDisk** e verifique se o espaço retorna um HealthStatus de íntegro.
     - A "verificação de integridade de dados para recuperação de falhas" é uma tarefa que não é mostrada como um trabalho de armazenamento e não há nenhum indicador de progresso. Se a tarefa estiver sendo exibida como em execução, ela estará em execução. Quando ele for concluído, ele mostrará concluído.

       Além disso, você pode exibir o status de uma tarefa de agendamento em execução usando o seguinte cmdlet:
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ```
4. Assim que a "verificação de integridade de dados para recuperação de falha" for concluída, o reparo será concluído e os discos virtuais estarão íntegros, altere os parâmetros de disco virtual de volta.

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0
   ```

5. Coloque os discos offline e, em seguida, online novamente para que os DiskRecoveryAction entrem em vigor:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ```

6. Adicione os discos virtuais afetados de volta ao CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```
   O **valor de DiskRunChkdsk 7** é usado para anexar o volume de espaço e ter a partição no modo somente leitura. Isso permite que os espaços para autodescoberta e autoatendimento disparem um reparo. O reparo será executado automaticamente depois de montado. Ele também permite que você acesse os dados, o que pode ser útil para obter acesso a quaisquer dados que você possa copiar. Para algumas condições de falha, como um log de DRT completo, você precisa executar a verificação de integridade de dados para a tarefa agendada de recuperação de falha.

**Verificação de integridade de dados para a tarefa de recuperação de falha** é usada para sincronizar e limpar um log de controle de região sujo (DRT) completo. Essa tarefa pode levar várias horas para ser concluída. A "verificação de integridade de dados para recuperação de falhas" é uma tarefa que não é mostrada como um trabalho de armazenamento e não há nenhum indicador de progresso. Se a tarefa estiver sendo exibida como em execução, ela estará em execução. Quando ele for concluído, ele será exibido como concluído. Se você cancelar a tarefa ou reiniciar um nó enquanto essa tarefa estiver em execução, a tarefa precisará ser reiniciada desde o início.

Para obter mais informações, consulte [solução de problemas espaços de armazenamento diretos integridade e Estados operacionais](storage-spaces-states.md).

## <a name="event-5120-with-status_io_timeout-c00000b5"></a>Evento 5120 com STATUS_IO_TIMEOUT c00000b5

> [!Important]
> **Para o Windows Server 2016:** Para reduzir a chance de ocorrer esses sintomas ao aplicar a atualização com a correção, é recomendável usar o procedimento do modo de manutenção de armazenamento abaixo para instalar a [atualização cumulativa de 18 de outubro de 2018 para o Windows server 2016](https://support.microsoft.com/help/4462928) ou uma versão posterior quando os nós tiverem instalado uma atualização cumulativa do windows Server 2016, que foi lançada de [8 de maio de 2018](https://support.microsoft.com/help/4103723) a 9 de outubro de [2018](https://support.microsoft.com/help/KB4462917).

Você pode obter o evento 5120 com STATUS_IO_TIMEOUT c00000b5 depois de reiniciar um nó no Windows Server 2016 com a atualização cumulativa que foi lançada a partir de [8 de maio de 2018 kb 4103723](https://support.microsoft.com/help/4103723) para [9 de outubro de 2018 KB 4462917](https://support.microsoft.com/help/4462917) instalado.

Quando você reinicia o nó, o evento 5120 é registrado no log de eventos do sistema e inclui um dos seguintes códigos de erro:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished.

Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.
```

Quando um evento 5120 é registrado, um despejo ao vivo é gerado para coletar informações de depuração que podem causar sintomas adicionais ou ter um efeito de desempenho. A geração do despejo ao vivo cria uma breve pausa para habilitar a criação de um instantâneo da memória para gravar o arquivo de despejo. Os sistemas que têm muita memória e que estão sob estresse podem fazer com que os nós removam a associação do cluster e também façam com que o evento 1135 a seguir seja registrado.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Uma alteração introduzida em 8 de maio de 2018 para o Windows Server 2016, que era uma atualização cumulativa para adicionar identificadores flexíveis de SMB para as sessões de rede SMB de Espaços de Armazenamento Diretos dentro do cluster. Isso foi feito para melhorar a resiliência a falhas de rede transitórias e melhorar a forma como o RoCE lida com o congestionamento da rede. Esses aprimoramentos também aumentaram inadvertidamente o tempo limite quando as conexões SMB tentam se reconectar e aguardam o tempo limite quando um nó é reiniciado. Esses problemas podem afetar um sistema que está sob estresse. Durante o tempo de inatividade não planejado, as pausas de e/s de até 60 segundos também foram observadas enquanto o sistema aguarda as conexões expirarem. Para corrigir esse problema, instale a [atualização cumulativa de 18 de outubro de 2018 para o Windows Server 2016](https://support.microsoft.com/help/4462928) ou uma versão posterior.

*Observação* Essa atualização alinha os tempos limite de CSV com tempos limite de conexão SMB para corrigir esse problema. Ele não implementa as alterações para desabilitar a geração de despejo dinâmico mencionado na seção de solução alternativa.

### <a name="shutdown-process-flow"></a>Desligar o fluxo do processo:

1. Execute o cmdlet Get-VirtualDisk e verifique se o valor de HealthStatus está íntegro.
2. Dissipe o nó executando o seguinte cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Coloque os discos nesse nó no modo de manutenção de armazenamento executando o seguinte cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Execute o cmdlet **Get-PhysicalDisk** e verifique se o valor de OperationalStatus está no modo de manutenção.
5. Execute o cmdlet **Restart-Computer** para reiniciar o nó.
6. Após a reinicialização do nó, remova os discos nesse nó do modo de manutenção de armazenamento executando o seguinte cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Retome o nó executando o seguinte cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Verifique o status dos trabalhos de ressincronização executando o seguinte cmdlet:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Desabilitando despejos ao vivo
Para atenuar o efeito da geração de despejos dinâmicos em sistemas que têm muita memória e estão sob estresse, você também pode querer desabilitar a geração de despejo dinâmico. Três opções são fornecidas abaixo.

>[!Caution]
>Esse procedimento pode impedir a coleta de informações de diagnóstico que Suporte da Microsoft pode precisar investigar esse problema. Um agente de suporte pode solicitar que você habilite novamente a geração de despejo dinâmico com base em cenários de solução de problemas específicos.

Há dois métodos para desabilitar despejos em tempo real, conforme descrito abaixo.

#### <a name="method-1-recommended-in-this-scenario"></a>Método 1 (recomendado neste cenário)
Para desabilitar completamente todos os despejos, incluindo despejos dinâmicos em todo o sistema, siga estas etapas:

1. Crie a seguinte chave do registro: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Na nova chave **ForceDumpsDisabled** , crie uma propriedade REG_DWORD como GuardedHost e, em seguida, defina seu valor como 0x10000000.
3. Aplique a nova chave do registro a cada nó do cluster.

>[!NOTE]
>Você precisa reiniciar o computador para que a alteração na entre em vigor.

Depois que essa chave do registro for definida, a criação de despejo dinâmico falhará e gerará um erro "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Método 2
Por padrão, Relatório de Erros do Windows permitirá apenas um LiveDump por tipo de relatório por 7 dias e apenas 1 LiveDump por computador por 5 dias. Você pode alterar isso definindo as seguintes chaves do registro para permitir apenas um LiveDump no computador para sempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Observação* Você precisa reiniciar o computador para que a alteração entre em vigor.

### <a name="method-3"></a>Método 3
Para desabilitar a geração de clusters de despejos ao vivo (por exemplo, quando um evento 5120 é registrado), execute o seguinte cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Esse cmdlet tem um efeito imediato em todos os nós de cluster sem a reinicialização do computador.

## <a name="slow-io-performance"></a>Desempenho de e/s lento

Se você estiver vendo o desempenho de e/s lento, verifique se o cache está habilitado em sua configuração de Espaços de Armazenamento Diretos.

Há duas maneiras de verificar:


1. Usando o log do cluster. Abra o log do cluster no editor de texto de sua escolha e pesquise por "[= = = discos SBL = = =]." Essa será uma lista do disco no nó em que o log foi gerado.

     Discos habilitados para cache exemplo: Observe aqui que o estado é CacheDiskStateInitializedAndBound e há um GUID presente aqui.

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: aqui, podemos ver que não há nenhum GUID presente e o estado é CacheDiskStateNonHybrid.
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: quando todos os discos do mesmo tipo de caso não estão habilitados por padrão. Aqui, podemos ver que não há nenhum GUID presente e o estado é CacheDiskStateIneligibleDataPartition.
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```
2. Usando Get-PhysicalDisk.xml do SDDCDiagnosticInfo
    1. Abra o arquivo XML usando "$d = Import-Clixml GetPhysicalDisk.XML"
    2. Executar "armazenamento ipmo"
    3. Execute "$d". Observe que o uso é seleção automática, não diário. você verá uma saída como esta:

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Uso| Tamanho|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| Falso|   OK|                Íntegros|      Seleção automática de 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  Falso|    OK|                Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  Falso|  OK|                Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| Falso| OK|    Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| Falso|OK|       Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| Falso| OK|      Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| Falso| OK|      Íntegros|Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| Falso| OK|  Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| Falso| OK| Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| Falso| OK|Íntegros| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| Falso| OK| Íntegros| Seleção automática |1,82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Como destruir um cluster existente para que você possa usar os mesmos discos novamente

Em um cluster Espaços de Armazenamento Diretos, quando você desabilita Espaços de Armazenamento Diretos e usa o processo de limpeza descrito em [unidades limpas](deploy-storage-spaces-direct.md#step-31-clean-drives), o pool de armazenamento clusterizado permanece em um estado offline e o serviço de integridade é removido do cluster.

A próxima etapa é remover o pool de armazenamento fantasma:
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Agora, se você executar **Get-PhysicalDisk** em qualquer um dos nós, verá todos os discos que estavam no pool. Por exemplo, em um laboratório com um cluster de 4 nós com 4 discos SAS, 100 GB cada um deles é apresentado a cada nó. Nesse caso, depois que o espaço de armazenamento direto estiver desabilitado, o que remove o SBL (camada de barramento de armazenamento), mas deixa o filtro, se você executar **Get-PhysicalDisk**, ele deverá relatar 4 discos, excluindo o disco do sistema operacional local. Em vez disso, ele relatou 16 em vez disso. Isso é o mesmo para todos os nós no cluster. Ao executar um comando **Get-Disk** , você verá os discos anexados localmente numerados como 0, 1, 2 e assim por diante, como visto neste exemplo de saída:

|Número| Nome amigável| Número de Série|HealthStatus|OperationalStatus|Tamanho total| Estilo de partição|
|-|-|-|-|-|-|-|-|
|0|Virtu MSFT...  ||Íntegros | Online|  127 GB| GPT|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
|1|Virtu MSFT...||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
|2|Virtu MSFT...||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
|4|Virtu MSFT...||Íntegros| Offline| 100 GB| RAW|
|3|Virtu MSFT...||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|
||Virtu MSFT... ||Íntegros| Offline| 100 GB| RAW|

## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Mensagem de erro sobre "tipo de mídia sem suporte" quando você cria um cluster de Espaços de Armazenamento Diretos usando enable-ClusterS2D

Você poderá ver erros semelhantes a este ao executar o cmdlet **Enable-ClusterS2D** :

![Cenário 6 mensagem de erro](media/troubleshooting/scenario-error-message.png)

Para corrigir esse problema, verifique se o adaptador HBA está configurado no modo HBA. Nenhum HBA deve ser configurado no modo RAID.

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect trava em ' aguardando até que os discos SBL sejam exibidos ' ou em 27%

Você verá as seguintes informações no relatório de validação:

`<identifier>`O disco conectado ao nó `<nodename>` retornou uma associação de porta SCSI e o dispositivo de compartimento correspondente não pôde ser encontrado. O hardware não é compatível com o Espaços de Armazenamento Diretos (S2D), entre em contato com o fornecedor do hardware para verificar o suporte para o SES (serviços de compartimento SCSI).

O problema é com a placa de expansão SAS do HPE que está entre os discos e a placa do HBA. O expansor SAS cria uma ID duplicada entre a primeira unidade conectada ao expansor e o próprio expansor.  Isso foi resolvido em [HPE Smart Array controladores SAS Expander firmware: 4, 2](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>A série Intel SSD DC P4600 tem um NGUID não exclusivo
Você pode ver um problema em que um dispositivo Intel SSD DC série P4600 parece estar relatando um NGUID de 16 bytes semelhante para vários namespaces, como 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 no exemplo abaixo.


|               uniqueid               | deviceid | MediaType | BusType |               SerialNumber               |      tamanho      | canpool | FriendlyName | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  Falso  |     HGST     |  HUH721010AL4200  |
| EUI. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  Verdadeiro   |    PROCESSADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  Verdadeiro   |    PROCESSADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  Verdadeiro   |    PROCESSADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  Verdadeiro   |    PROCESSADOR     |   SSDPE2KE016T7   |

Para corrigir esse problema, atualize o firmware nas unidades Intel para a versão mais recente.  A versão de firmware QDV101B1 de maio de 2018 é conhecida por resolver esse problema.

A [versão de maio de 2018 da ferramenta Data Center Intel SSD](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) inclui uma atualização de firmware, QDV101B1, para a série de P4600 do controlador de domínio SSD Intel.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>O disco físico "íntegro" e o status operacional é "removendo do pool"

Em um cluster Espaços de Armazenamento Diretos do Windows Server 2016, você pode ver o HealthStatus de um ou mais discos físicos como "íntegros", enquanto o OperationalStatus é "(removendo do pool, OK)".

A "remoção do pool" é uma intenção definida quando **Remove-PhysicalDisk** é chamado, mas armazenado em integridade para manter o estado e permitir a recuperação se a operação de remoção falhar. Você pode alterar manualmente o OperationalStatus para íntegro com um dos seguintes métodos:

- Remova o disco físico do pool e, em seguida, adicione-o novamente.
- Execute o [scriptClear-PhysicalDiskHealthData.ps1](https://go.microsoft.com/fwlink/?linkid=2034205) para desmarcar a intenção. (Disponível para download como um. Arquivo TXT. Você precisará salvá-lo como um. Arquivo PS1 antes de poder executá-lo.)

Aqui estão alguns exemplos que mostram como executar o script:

- Use o parâmetro **SerialNumber** para especificar o disco que você precisa definir como íntegro. Você pode obter o número de série do **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDisk**. (Estamos apenas usando 0s para o número de série abaixo.)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Use o parâmetro **UniqueId** para especificar o disco (novamente do **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDisk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>A cópia do arquivo está lenta

Você pode ter visto um problema usando o explorador de arquivos para copiar um VHD grande para o disco virtual-a cópia do arquivo está demorando mais do que o esperado.

Usar o explorador de arquivos, Robocopy ou xcopy para copiar um VHD grande para o disco virtual não é um método recomendado para, pois isso resultará em um desempenho mais lento do que o esperado. O processo de cópia não passa pela pilha de Espaços de Armazenamento Diretos, que fica mais abaixo na pilha de armazenamento e, em vez disso, atua como um processo de cópia local.

Se você quiser testar Espaços de Armazenamento Diretos desempenho, é recomendável usar VMFleet e Diskspd para carregar e fazer testes de estresse nos servidores para obter uma linha base e definir as expectativas do desempenho do Espaços de Armazenamento Diretos.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Eventos esperados que você veria no restante dos nós durante a reinicialização de um nó.

É seguro ignorar esses eventos:

```
Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed.

Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed.
```

Se você estiver executando VMs do Azure, poderá ignorar este evento:`Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur.`

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Desempenho lento ou erros de "comunicação perdida", "erro de e/s", "desanexados" ou "sem redundância" para implantações que usam dispositivos Intel P3x00 NVMe

Identificamos um problema crítico que afeta alguns Espaços de Armazenamento Diretos usuários que estão usando hardware com base na família de dispositivos P3x00 do NVM Express (NVMe) com versões de firmware antes da "versão de manutenção 8".

>[!NOTE]
> OEMs individuais podem ter dispositivos baseados na família Intel P3x00 de dispositivos NVMe com cadeias de caracteres de versão de firmware exclusivo. Entre em contato com seu OEM para obter mais informações sobre a versão mais recente do firmware.

Se você estiver usando hardware em sua implantação com base na família Intel P3x00 de dispositivos NVMe, recomendamos que você aplique imediatamente o firmware mais recente disponível (pelo menos a versão de manutenção 8). Este [artigo suporte da Microsoft](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornece informações adicionais sobre esse problema.
