---
title: Problemas de espaços diretos do armazenamento
description: Saiba como solucionar problemas de sua implantação de espaços de armazenamento diretos.
keywords: Espaços de Armazenamento
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 44bcf48f3e4a3b4b49ff027d3aa3e5704865e7b5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447885"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Solucionar problemas de espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Use as informações a seguir para solucionar problemas de sua implantação de espaços de armazenamento diretos.

Em geral, começar com as seguintes etapas:

1. Confirme se que a marca/modelo de SSD é certificado para o Windows Server 2016 e Windows Server 2019 usando o catálogo do Windows Server. Confirme com o fornecedor que as unidades têm suporte para espaços de armazenamento diretos.
2. Inspecione o armazenamento para todas as unidades com defeito. Use o software de gerenciamento de armazenamento para verificar o status das unidades. Se qualquer uma das unidades estiverem com defeito, trabalhe com seu fornecedor. 
3. Atualize o armazenamento e o firmware da unidade se necessário.
   Certifique-se de que as atualizações mais recentes do Windows são instaladas em todos os nós. Você pode obter as atualizações mais recentes para o Windows Server 2016 do [histórico de atualizações do Windows 10 e Windows Server 2016](https://aka.ms/update2016) e para o Windows Server 2019 da [histórico de atualizações do Windows 10 e Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Atualizar drivers de adaptador de rede e firmware.
5. Executar a validação de cluster e examine a seção de espaço de armazenamento diretos, certifique-se de que as unidades que serão usadas para o cache são relatadas corretamente e sem erros.

Se você ainda estiver tendo problemas, examine os cenários a seguir.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Recursos de disco virtual estão em estado de redundância não
Os nós de um sistema de espaços de armazenamento diretos reiniciar inesperadamente devido a uma falha ou queda de energia. Em seguida, um ou mais dos discos virtuais podem não ficar online, e você ver a descrição "não há informações de redundância."

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Tamanho| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| OK|  Íntegro| True|  10 TB|  Nó-01.conto...|
|Disk3         |Mirror                 |OK                          |Íntegro       |True            |10 TB | Nó-01.conto...|
|Disk2         |Mirror                 |Nenhuma redundância               |Unhealthy     |True            |10 TB | Nó-01.conto...|
|Disk1         |Mirror                 |{Nenhuma redundância, InService}  |Unhealthy     |True            |10 TB | Nó-01.conto...| 

Além disso, após uma tentativa de colocar o disco virtual online, as informações a seguir são registradas no log do Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

O **nenhuma redundância operacional Status** pode ocorrer se um disco com falha ou se o sistema não puder acessar dados no disco virtual. Esse problema pode ocorrer se uma reinicialização ocorre em um nó durante a manutenção em nós.

Para corrigir esse problema, siga estas etapas:

1. Remova os discos virtuais afetados do CSV. Isso os coloca no grupo de "Armazenamento disponível" do cluster e começar a mostrar como um tipo de recurso de "Disco físico."

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. No nó que possui o grupo de armazenamento disponível, execute o seguinte comando em cada disco que está em um estado sem redundância. Para identificar qual nó o grupo de "Armazenamento disponível" está em que você pode executar o comando a seguir.

   ```powershell
   Get-ClusterGroup
   ```
3. Definir a ação de recuperação de disco e, em seguida, iniciar os discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Um reparo deve iniciar automaticamente. Aguarde até que o reparo concluir. Ele pode entrar em um estado suspenso e inicie novamente. Para monitorar o progresso: 
    - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele for concluído.
    - Execute **Get-VirtualDisk** e verifique se o espaço retorna um HealthStatus de Íntegro.
5. Após o reparo é concluída e os discos virtuais são íntegro, altere os parâmetros do disco Virtual de volta.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Levar os discos offline e depois online novamente para ter o DiskRecoveryAction em vigor:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Adicione os discos virtuais afetados para CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** é uma opção de substituição que permite anexar o volume de espaço no modo de leitura / gravação sem nenhuma verificação. A propriedade permite que você faça o diagnóstico em um volume por que não fica online. É muito semelhante ao modo de manutenção, mas você pode invocá-lo em um recurso em um estado de falha. Ele também permite que você acesse os dados, que podem ser útil em situações como "Sem redundância", onde você pode obter acesso a quaisquer dados que você pode e copiá-lo. A propriedade DiskRecoveryAction foi adicionada na 22 de fevereiro de 2018, atualização, 4077525 KB.


## <a name="detached-status-in-a-cluster"></a>Status desanexado em um cluster 

Quando você executa o **Get-VirtualDisk** cmdlet, o OperationalStatus para um ou mais espaços de armazenamento diretos discos virtuais é desanexado. No entanto, o HealthStatus relatada pelo **Get-PhysicalDisk** cmdlet indica que todos os discos físicos estão em um estado íntegro.

A seguir está um exemplo da saída do **Get-VirtualDisk** cmdlet.

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Tamanho|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 OK|                  Íntegro|       True|            10 TB|  Nó-01.conto...|
|Disk3|         Mirror|                 OK|                  Íntegro|       True|            10 TB|  Nó-01.conto...|
|Disk2|         Mirror|                 “Desanexado”|            Desconhecido|       True|            10 TB|  Nó-01.conto...|
|Disk1|         Mirror|                 “Desanexado”|            Desconhecido|       True|            10 TB|  Nó-01.conto...| 


Além disso, os seguintes eventos podem ser registrados em nós:

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

O **desanexado Status operacional** pode ocorrer se a região suja DRT (rastreamento) em que o log está cheio. Espaços de armazenamento usam DRT (rastreamento) para espaços de espelhados de região suja para certificar-se de que, quando ocorre uma falha de energia, todas as atualizações em andamento para os metadados são registradas para certificar-se de que o espaço de armazenamento pode refazer ou desfazer operações para trazer de volta o espaço de armazenamento em um flexível e o estado consistente quando a energia for restaurada e o sistema volta a funcionar. Se o log DRT estiver cheio, o disco virtual não pode ficar online até que os metadados DRT são sincronizados e liberados. Esse processo requer executar uma verificação completa, que pode levar várias horas para ser concluída.

Para corrigir esse problema, siga estas etapas:
1. Remova os discos virtuais afetados do CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Execute os seguintes comandos em cada disco que não está sendo colocado online. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Execute o seguinte comando em cada nó no qual o volume desanexado está online. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Essa tarefa deve ser iniciada em todos os nós no qual o volume desanexado está online. Um reparo deve iniciar automaticamente. Aguarde até que o reparo concluir. Ele pode entrar em um estado suspenso e inicie novamente. Para monitorar o progresso: 
   - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele for concluído.
   - Execute **Get-VirtualDisk** e verifique se o espaço retorna um HealthStatus de Íntegro.
     - Os "dados integridade verificação para recuperação de pane" é uma tarefa que não aparece como um trabalho de armazenamento, e há um indicador de progresso. Se a tarefa está mostrando como em execução, ele está em execução. Quando ele for concluído, ele mostrará concluído.

       Além disso, você pode exibir o status de uma tarefa agendada em execução usando o seguinte cmdlet: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Assim que o "dados de integridade verificação para recuperação de pane" for concluída, a conclusão do reparo e os discos virtuais são íntegro, altere os parâmetros do disco Virtual de volta. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Adicione os discos virtuais afetados para CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **O valor de DiskRunChkdsk 7** é usado para anexar o volume de espaço e ter a partição no modo somente leitura. Isso permite que os espaços para descobrir por conta própria e auto-recuperação ao disparar um reparo. Reparo executará automaticamente uma vez montado. Ele também permite acessar os dados, que podem ser útil para obter acesso a quaisquer dados que você pode para copiar. Para algumas condições de falha, como um log DRT completo, você precisa executar a verificação de integridade de dados para a tarefa agendada de recuperação de pane.

**Verificação de integridade de dados para a tarefa de recuperação de pane** é usado para sincronizar e limpar um log de região suja completo DRT (rastreamento). Essa tarefa pode levar várias horas para ser concluída. Os "dados integridade verificação para recuperação de pane" é uma tarefa que não aparece como um trabalho de armazenamento, e há um indicador de progresso. Se a tarefa está mostrando como em execução, ele está em execução. Quando ele for concluído, ele mostrará como concluído. Se você cancelar a tarefa ou reiniciar um nó enquanto essa tarefa está em execução, a tarefa será necessário recomeçar desde o início.

Para obter mais informações, consulte [integridade de solução de problemas de espaços de armazenamento diretos e estados operacionais](storage-spaces-states.md).

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>Evento 5120 com STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Para o Windows Server 2016:** Para reduzir a chance de receber esses sintomas ao aplicar a atualização com a correção, é recomendável usar o procedimento de modo de manutenção de armazenamento a seguir para instalar o [18 de outubro de 2018, a atualização cumulativa para o Windows Server 2016](https://support.microsoft.com/help/4462928)ou uma versão posterior quando os nós no momento, instalou uma atualização cumulativa do Windows Server 2016 que foi lançada a partir [8 de maio de 2018](https://support.microsoft.com/help/4103723) para [9 de outubro de 2018](https://support.microsoft.com/help/KB4462917).

Você pode obter o evento 5120 com STATUS_IO_TIMEOUT c00000b5 após a reinicialização de um nó no Windows Server 2016 com a atualização cumulativa que foram lançadas de [de 8 de maio de 2018, KB 4103723](https://support.microsoft.com/help/4103723) para [9 de outubro de 2018 KB 4462917](https://support.microsoft.com/help/4462917)instalado.

Quando você reiniciar o nó, o evento 5120 é registrada no log de eventos do sistema e inclui um dos seguintes códigos de erro:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Quando um evento 5120 for registrado, um despejo de memória dinâmico é gerado para coletar informações de depuração que podem causar os sintomas adicionais ou ter um efeito de desempenho. Gerar o despejo ao vivo cria uma pequena pausa para habilitar a tirar um instantâneo de memória para gravar o arquivo de despejo. Sistemas que têm muita memória e estão sob estresse podem causar a nós descartar fora de associação do cluster e também fazer com que o seguinte 1135 de evento a ser registrada.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Uma alteração foi introduzida no dia 8 de maio de 2018, para o Windows Server 2016, que era uma atualização cumulativa para adicionar o SMB resiliente manipula para as sessões de rede SMB dentro do cluster espaços de armazenamento diretos. Isso foi feito para melhorar a resiliência a falhas de rede transitórios e melhorar como RoCE trata o congestionamento da rede. Esses aprimoramentos também inadvertidamente aumentado tempos limite quando conexões SMB tentam reconectar-se e espera para tempo limite quando um nó é reiniciado. Esses problemas podem afetar um sistema que está sob carga excessiva. Durante o tempo de inatividade não planejado, pausa e/s de até 60 segundos também foram observada enquanto o sistema aguarda para conexões com o tempo limite. Para corrigir esse problema, instale o [18 de outubro de 2018, a atualização cumulativa para o Windows Server 2016](https://support.microsoft.com/help/4462928) ou uma versão posterior.

*Observação* essa atualização alinha os tempos limite CSV com tempos limite de conexão de SMB para corrigir esse problema. Ele não implementa as alterações para desabilitar a geração do despejo ao vivo mencionada na seção de solução alternativa.

### <a name="shutdown-process-flow"></a>Fluxo do processo de desligamento:

1. Execute o cmdlet Get-VirtualDisk e certifique-se de que o valor de HealthStatus é íntegro.
2. Esvaziar o nó executando o seguinte cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Colocar os discos no nó no modo de manutenção de armazenamento executando o seguinte cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Execute o **Get-PhysicalDisk** cmdlet e certifique-se de que o valor de OperationalStatus está no modo de manutenção.
5. Execute o **Restart-Computer** cmdlet para reiniciar o nó.
6. Depois que o nó for reiniciado, remova os discos no nó do modo de manutenção de armazenamento executando o seguinte cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Reiniciar o nó executando o seguinte cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Verificar o status dos trabalhos de ressincronização, executando o seguinte cmdlet:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Desabilitar os despejos de memória em tempo real
Para reduzir o efeito de geração de despejo ao vivo em sistemas que têm muita memória e estão sob estresse, além disso talvez queira desabilitar a geração do despejo de memória em tempo real. Três opções são fornecidas abaixo.

>[!Caution]
>Esse procedimento pode impedir que a coleção de informações de diagnóstico que talvez seja necessário investigar esse problema Microsoft Support. Um agente de suporte talvez precise solicitar que você habilite novamente a geração do despejo de memória em tempo real com base em cenários de solução de problemas específicos.

Há dois métodos para desabilitar os despejos de memória em tempo real, conforme descrito abaixo.

#### <a name="method-1-recommended-in-this-scenario"></a>Método 1 (recomendado neste cenário)
Para desabilitar completamente todos os despejos de memória, incluindo os despejos de memória em tempo real em todo o sistema, siga estas etapas:

1. Crie a seguinte chave do registro: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Na nova **ForceDumpsDisabled** da chave, crie uma propriedade REG_DWORD como GuardedHost e, em seguida, defina seu valor como 0x10000000.
3. Aplica a nova chave de registro para cada nó do cluster.

>[!NOTE]
>Você precisa reiniciar o computador para que a alteração nregistry entrem em vigor.

Depois que essa chave do registro for definida, ao vivo criação de despejo falhará e gerará um erro de "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Método 2
Por padrão, o relatório de erros do Windows permitirá apenas uma LiveDump por tipo de relatório por 7 dias e LiveDump apenas 1 por máquina por 5 dias. Você pode alterar isso definindo as seguintes chaves do registro para permitir que somente um LiveDump na máquina para sempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Observação* você precise reiniciar o computador para que a alteração tenha efeito.

### <a name="method-3"></a>Método 3
Para desabilitar a geração de cluster de despejos de memória dinâmicas (por exemplo, quando um evento 5120 estiver conectado), execute o seguinte cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Esse cmdlet tem efeito imediato em todos os nós de cluster sem a reinicialização do computador.

## <a name="slow-io-performance"></a>Desempenho lento de e/s

Se você estiver vendo o desempenho de e/s lento, verifique se o cache está habilitado em sua configuração de espaços de armazenamento diretos. 

Há duas maneiras de verificar: 


1. Usando o log do cluster. Abra o log do cluster no editor de texto de escolha e pesquise por "[= = = SBL discos = = =]." Isso será uma lista do disco no nó em que o log foi gerado em. 

     Cache habilitado discos exemplo: Observe aqui que o estado é CacheDiskStateInitializedAndBound e há um GUID presente aqui. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: Aqui podemos ver não há nenhum GUID presente e o estado é CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: Quando todos os discos são do mesmo caso de tipo não está habilitado por padrão. Aqui podemos ver não há nenhum GUID presente e o estado é CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usando Get-PhysicalDisk.xml do SDDCDiagnosticInfo
    1. Abra o arquivo XML usando "$d = GetPhysicalDisk.XML Import-Clixml"
    2. Execute "ipmo armazenamento"
    3. Execute "$d". Observe que uso é a seleção automática, não diário você verá uma saída como esta: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Uso| Tamanho|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Íntegro|      Seleção automática de TB 1,82|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Íntegro|Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Íntegro| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Íntegro| Seleção automática |1,82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Como destruir um cluster existente para que você possa usar os mesmos discos novamente

Em um cluster de espaços de armazenamento diretos, uma vez você desabilitar a espaços de armazenamento diretos e usar o processo de limpeza, descrito em [limpar unidades](deploy-storage-spaces-direct.md#step-31-clean-drives), o pool de armazenamento em cluster ainda estiver no estado Offline e o serviço de integridade é removido do cluster.

A próxima etapa é remover o pool de armazenamento fantasma: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Agora, se você executar **Get-PhysicalDisk** em qualquer um de nós, você verá todos os discos que estavam no pool. Por exemplo, em um laboratório com um cluster de 4 nós com 4 discos SAS, 100GB cada apresentada para cada nó. Nesse caso, depois que o espaço de armazenamento diretos é desabilitado, que remove o SBL (camada de barramento de armazenamento), mas deixa o filtro, se você executar **Get-PhysicalDisk**, ele deve relatar 4 discos, exceto o disco do sistema operacional local. Em vez disso, ele relatado 16 em vez disso. Isso é o mesmo para todos os nós no cluster. Quando você executa um **Get-Disk** comando, você verá os discos conectados localmente numerados como 0, 1, 2 e assim por diante, conforme mostrado nesta saída de exemplo:

|Number| Nome Amigável| Número de Série|HealthStatus|OperationalStatus|Tamanho total| Estilo de partição|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Íntegro | Online|  127 GB| GPT|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
|1|Msft Virtu...||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
|2|Msft Virtu...||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
|4|Msft Virtu...||Íntegro| Offline| 100 GB| RAW|
|3|Msft Virtu...||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|
||Msft Virtu... ||Íntegro| Offline| 100 GB| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Mensagem de erro sobre "tipo de mídia sem suporte" ao criar um cluster de espaços de armazenamento diretos usando o Enable-ClusterS2D  

Você poderá ver erros semelhantes a este quando você executa o **Enable-ClusterS2D** cmdlet:

![Mensagem de erro de cenário 6](media/troubleshooting/scenario-error-message.png)

Para corrigir esse problema, verifique se que o adaptador HBA está configurado no modo HBA. Não há HBA deve ser configurado no modo RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect trava em 'Aguardando até SBL discos são exibidos' ou em 27%

Você verá as seguintes informações no relatório de validação:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


O problema é com o cartão de expansor SAS HPE que se encontra entre os discos e a placa HBA. Expansor SAS cria uma ID duplicada entre a primeira unidade conectada no expansor e Expansor em si.  Isso foi resolvido no [HPE inteligente matriz controladores SAS expansor Firmware: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 série tem um NGUID não exclusivo
Você pode encontrar um problema em que um dispositivo da série Intel SSD DC P4600 parece estar relatando semelhante 16 bytes NGUID para vários namespaces, como 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 no exemplo a seguir.


|               ID exclusiva               | DeviceID | MediaType | BusType |               SerialNumber               |      size      | canpool | nome amigável | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui.0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |

Para corrigir esse problema, atualize o firmware nas unidades Intel para a versão mais recente.  Versão do firmware QDV101B1 de maio de 2018 é conhecido para resolver esse problema.

O [maio de 2018 versão da ferramenta Intel SSD Data Center](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) inclui uma atualização de firmware, QDV101B1, para a série Intel SSD DC P4600.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>Físico disco "Íntegro" e o Status operacional é "Remover do Pool" 

Em um cluster do Windows Server 2016 espaços de armazenamento diretos, você poderá ver o HealthStatus de discos físicos de um ou mais como "Íntegro", enquanto o OperationalStatus é "(remoção do Pool, Okey)". 

"Remover do Pool de" é uma intenção definida quando **Remove-PhysicalDisk** for chamado, mas armazenados em funcionamento para manter o estado e permitir a recuperação se a operação de remoção falhar. Você pode alterar manualmente o OperationalStatus para íntegro com um dos seguintes métodos:

- Remover o disco físico do pool e, em seguida, adicioná-lo novamente.
- Execute o [Clear PhysicalDiskHealthData.ps1 script](https://go.microsoft.com/fwlink/?linkid=2034205) para limpar a intenção. (Disponível para download como um. Arquivo TXT. Você precisará salvá-lo como um. Arquivo ps1 antes que você pode executá-lo.)

Aqui estão alguns exemplos que mostram como executar o script:

- Use o **SerialNumber** parâmetro para especificar o disco que você precisa definir para íntegro. Você pode obter o número de série **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**. (Estamos simplesmente usando 0s para o número de série abaixo.)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Use o **UniqueId** parâmetro para especificar o disco (novamente do **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>A cópia de arquivos está lenta

Você pode ver um problema usando o Explorador de arquivos para copiar um VHD grande para o disco virtual - a cópia do arquivo está demorando mais do que o esperado.

Usando o Explorador de arquivos, Robocopy ou Xcopy para copiar um VHD grande para o disco virtual não é um método recomendado para pois isso resultará em mais lento do que o desempenho esperado. O processo de cópia não passa pela pilha de espaços de armazenamento diretos, que fica menor na pilha de armazenamento e, em vez disso, atua como um processo de cópia local.

Se você quiser testar o desempenho de espaços de armazenamento diretos, é recomendável usar VMFleet e Diskspd à carga e teste de estresse os servidores para obter uma linha de base e definir as expectativas de desempenho de espaços de armazenamento diretos.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Espera-se os eventos que você veria no restante de nós durante a reinicialização de um nó. 

É seguro ignorar esses eventos: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Se você estiver executando máquinas virtuais do Azure, você pode ignorar esse evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Desempenho lento ou "Comunicação perdida," "Erro de e/s", "Desanexada", ou erros de "Sem redundância" para implantações que usam dispositivos Intel P3x00 NVMe

Nós identificamos um problema crítico que afeta alguns espaços de armazenamento diretos usuários que estão usando o hardware com base na família Intel P3x00 de NVM Express (NVMe) os dispositivos com versões de firmware antes de "Manutenção versão 8". 

>[!NOTE]
> Os OEMs individuais podem ter dispositivos que se baseiam na família Intel P3x00 de dispositivos NVMe com cadeias de caracteres de versão de firmware exclusivo. Entre em contato com seu OEM para obter mais informações da última versão do firmware.

Se você estiver usando o hardware em sua implantação com base na família Intel P3x00 de dispositivos NVMe, é recomendável que você aplique imediatamente o firmware mais recente (pelo menos 8 da versão de manutenção). Isso [artigo do Microsoft Support](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornece informações adicionais sobre esse problema. 
