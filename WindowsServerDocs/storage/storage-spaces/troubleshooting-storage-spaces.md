---
title: Solução de problemas direto de espaços de armazenamento
description: Saiba como solucionar problemas de sua implantação de espaços de armazenamento diretos.
keywords: Espaços de Armazenamento
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256929"
---
# Solucionar problemas de espaços de armazenamento diretos

Use as seguintes informações para solucionar problemas de sua implantação de espaços de armazenamento diretos.

Em geral, comece com as seguintes etapas:

1. Confirme se que a marca/modelo do SSD é certificado para Windows Server 2016 usando o catálogo do Windows Server. Confirme com o fornecedor que as unidades têm suporte para espaços de armazenamento diretos.
2. Inspecione o armazenamento para todas as unidades com defeito. Use o software de gerenciamento de armazenamento para verificar o status das unidades. Se qualquer uma das unidades estão com defeito, trabalhe com seu fornecedor. 
3. Atualizar o armazenamento e o firmware da unidade se necessário.
   Certifique-se de que as atualizações mais recentes do Windows estão instaladas em todos os nós. Você pode obter as atualizações mais recentes para o Windows Server 2016 de [https://aka.ms/update2016](https://aka.ms/update2016).
4. Atualização de firmware e drivers de adaptador de rede.
5. Executar a validação de cluster e revise a seção de espaço de armazenamento diretos, certifique-se de que as unidades que serão usadas para o cache são relatadas corretamente e sem erros.

Se você ainda estiver tendo problemas, revise os cenários abaixo.

## Recursos de disco virtual estão em estado de redundância não
Os nós de um sistema de espaços de armazenamento diretos reiniciado inesperadamente devido a uma falha de energia ou falha. Em seguida, um ou mais dos discos virtuais podem não ficar online, e você ver a descrição "não há informações de redundância."
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Size| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| OK|  Healthy| True|  10 TB|  Nó-01.conto...|
|Disk3         |Mirror                 |OK                          |Healthy       |True            |10 TB | Nó-01.conto...|
|Disco 2         |Mirror                 |Sem redundância               |Unhealthy     |True            |10 TB | Nó-01.conto...|
|Disk1         |Mirror                 |{Nenhuma redundância, InService}  |Unhealthy     |True            |10 TB | Nó-01.conto...| 

Além disso, depois de uma tentativa de colocar o disco virtual online, as informações a seguir são registradas no log de Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

O **Status operacional de redundância não** pode ocorrer se um disco com falha ou se o sistema não consegue acessar dados no disco virtual. Esse problema pode ocorrer se uma reinicialização ocorre em um nó durante a manutenção em nós.
    
Para corrigir esse problema, siga estas etapas:

1. Remova os discos virtuais afetados CSV. Isso colocá-los no grupo "Armazenamento disponível" no cluster e Iniciar mostrando como um tipo de recurso de "Disco físico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. No nó proprietário do grupo de armazenamento disponível, execute o seguinte comando em cada disco que está em um estado não redundância. Para identificar qual nó é o grupo de "armazenamento disponível" em que você pode executar o comando a seguir.

   ```powershell
   Get-ClusterGroup
   ```
3. Defina a ação de recuperação de disco e inicie os discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Um reparo deve ser iniciado automaticamente. Aguarde o reparo concluir. Ele pode entrar em um estado suspenso e começar novamente. Para monitorar o progresso: 
    - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele é concluído.
    - Execute **Get-VirtualDisk** e verifique se que o espaço retorna um HealthStatus de Íntegro.
5. Após o Reparo concluído e os discos virtuais são íntegro, alterar os parâmetros do disco Virtual de volta.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Tirar os discos offline e, em seguida, online novamente para ter a DiskRecoveryAction entram em vigor:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Adicione os discos virtuais afetados para CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** é uma opção de substituição que permite anexar o volume de espaço no modo de leitura-gravação sem qualquer verificação. A propriedade permite que você faça o diagnóstico em por que um volume não fica online. É muito semelhante ao modo de manutenção, mas você pode chamá-lo em um recurso em um estado de falha. Ele também permite que você acesse os dados, que podem ser útil em situações como "Sem redundância", onde você pode obter acesso para os dados que você pode e copiá-lo. A propriedade DiskRecoveryAction foi adicionada na 22 de fevereiro de 2018, atualização, 4077525 KB.
    
    
## Status desanexados em um cluster 

Quando você executar o cmdlet **Get-VirtualDisk** , o OperationalStatus para um ou mais discos virtuais espaços de armazenamento diretos é separada. No entanto, o HealthStatus relatados pelo cmdlet **Get-PhysicalDisk** indica que todos os discos físicos são em um estado íntegro.

A seguir está um exemplo da saída do cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Size|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 OK|                  Healthy|       True|            10 TB|  Nó-01.conto...|
|Disk3|         Mirror|                 OK|                  Healthy|       True|            10 TB|  Nó-01.conto...|
|Disco 2|         Mirror|                 Desanexado|            Desconhecido|       True|            10 TB|  Nó-01.conto...|
|Disk1|         Mirror|                 Desanexado|            Desconhecido|       True|            10 TB|  Nó-01.conto...| 


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

O **Status operacional desanexados** pode ocorrer se a região suja rastreamento (DRT) o log está cheio. Espaços de armazenamento usa os retângulos sujos região de rastreamento (DRT) para espaços espelhados para certificar-se de que, quando ocorre uma falha de energia, as atualizações em andamento para metadados são registradas para certificar-se de que o espaço de armazenamento pode refazer ou desfazer operações para trazer o espaço de armazenamento de volta para um flexível e estado consistente quando a energia é restaurada e o sistema de volta. Se o log DRT está cheio, o disco virtual não pode ser colocado on-line até que os metadados DRT é sincronizado e liberado. Esse processo requer a execução de uma verificação completa, o que pode levar várias horas para concluir.
    
Para corrigir esse problema, siga estas etapas:
1. Remova os discos virtuais afetados CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Execute os seguintes comandos em cada disco que não estará disponível on-line. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Execute o seguinte comando em cada nó no qual o desanexado volume estiver online. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Essa tarefa deve ser iniciada em todos os nós nos quais o desanexado volume está on-line. Um reparo deve ser iniciado automaticamente. Aguarde o reparo concluir. Ele pode entrar em um estado suspenso e começar novamente. Para monitorar o progresso: 
   - Execute **Get-StorageJob** para monitorar o status do reparo e ver quando ele é concluído.
   -  Execute **Get-VirtualDisk** e verifique se que o espaço retorna um HealthStatus de Íntegro.
    - O "dados integridade de verificação para recuperação de pane" é uma tarefa que não mostra como um trabalho de armazenamento, e não há nenhum indicador de progresso. Se a tarefa está mostrando como execução, ele está em execução. Quando ele for concluído, ele mostrará concluído.
    
       Além disso, você pode exibir o status de uma tarefa agendada em execução usando o seguinte cmdlet: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Assim que o "dados integridade de verificação para recuperação de pane" for concluída, a conclusão do reparo e os discos virtuais são íntegro, alterar os parâmetros do disco Virtual de volta. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Adicione os discos virtuais afetados para CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**O valor de DiskRunChkdsk 7** é usado para anexar o volume de espaço e tem a partição no modo somente leitura. Isso permite que espaços automática descobrir e auto-recuperação pelo reparo. Reparo será executada automaticamente quando montados. Ele também permite que você acesse os dados, que podem ser útil para acessar os dados que você pode para copiar. Para algumas condições de falha, como um log DRT completa, você precisará executar a verificação de integridade de dados para a tarefa agendada de recuperação de pane.
    
**Verificação de integridade de dados para a tarefa de recuperação de pane** é usado para sincronizar e limpar um log de rastreamento (DRT) de região sujos completo. Essa tarefa pode levar várias horas para ser concluído. O "dados integridade de verificação para recuperação de pane" é uma tarefa que não mostra como um trabalho de armazenamento, e não há nenhum indicador de progresso. Se a tarefa está mostrando como execução, ele está em execução. Quando ele for concluído, ele será exibido como concluído. Se você cancelar a tarefa ou reiniciar um nó enquanto essa tarefa está em execução, a tarefa será necessário recomeçar desde o início.

Para obter mais informações, consulte a [integridade de solução de problemas de espaços de armazenamento diretos e estados operacionais](storage-spaces-states.md).
    
## Evento 5120 com STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> Para reduzir a chance de enfrentando esses sintomas ao aplicar a atualização com a correção, é recomendável usar o procedimento de modo de manutenção de armazenamento abaixo para instalar a [atualização cumulativa de 18 de outubro de 2018, para Windows Server 2016](https://support.microsoft.com/help/4462928) ou posterior Quando os nós atualmente tem instalado uma atualização cumulativa do Windows Server 2016 que foi lançada a partir de [8 de maio de 2018](https://support.microsoft.com/help/4103723) a [9 de outubro de 2018](https://support.microsoft.com/help/KB4462917).

Você pode receber o evento 5120 com STATUS_IO_TIMEOUT c00000b5 após a reinicialização de um nó no Windows Server 2016 com atualização cumulativa lançadas do [pode 8 KB 2018 4103723](https://support.microsoft.com/help/4103723) [9 de outubro de 2018 KB 4462917](https://support.microsoft.com/help/4462917) instalado.

Quando você reiniciar o nó, o evento 5120 é registrada no log de eventos do sistema e inclui um dos seguintes códigos de erro:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Quando um evento 5120 estiver conectado, é gerado um despejo de memória dinâmico para coletar informações de depuração que podem causar sintomas adicionais ou ter um efeito de desempenho. Gerar o despejo ao vivo cria uma breve pausa para habilitar tirar um instantâneo de memória para gravar o arquivo de despejo. Sistemas que têm muita memória e estão sob estresse podem causar nós para soltar fora de associação de cluster e também causar a seguir 1135 evento deve ser registrada.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Uma alteração foi introduzida na atualização cumulativa 8 de maio de 2018, adicionar manipula resiliente do SMB para as espaços de armazenamento diretos sessões de rede SMB dentro do cluster. Isso foi feito para melhorar a resiliência a falhas de rede transitório e melhorar como RoCE manipula o congestionamento da rede.

Essas melhorias também inadvertidamente maior tempos limite quando conexões SMB tentam reconectar e aguarda para o tempo limite quando um nó for reiniciado. Esses problemas podem afetar um sistema que está sob carga excessiva. Durante o tempo de inatividade planejado, pausas de e/s de até 60 segundos também foram observadas enquanto o sistema aguarda para conexões de tempo limite.

Para corrigir esse problema, instale a [18 de outubro de 2018, a atualização cumulativa para o Windows Server 2016](https://support.microsoft.com/help/4462928) ou uma versão posterior.

*Observação* Essa atualização alinha os tempos limite CSV com tempos limite de conexão do SMB para corrigir esse problema. Ele não implementa as alterações para desativar a geração de despejo de memória dinâmica mencionada na seção solução alternativa.
    
### Fluxo do processo de desligamento:

1. Execute o cmdlet Get-VirtualDisk e certifique-se de que o valor de HealthStatus é íntegro.
2. Esvaziar o nó executando o seguinte cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Coloque os discos nesse nó no modo de manutenção de armazenamento executando o seguinte cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Execute o cmdlet **Get-PhysicalDisk** e certifique-se de que o valor OperationalStatus está no modo de manutenção.
5. Execute o cmdlet de **Reiniciar o computador** para reiniciar o nó.
6. Depois que o nó for reiniciado, remova os discos nesse nó de modo de manutenção de armazenamento executando o seguinte cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Retome o nó executando o seguinte cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Verificar o status dos trabalhos de ressincronização executando o seguinte cmdlet:

   ```powershell
   Get-StorageJob
   ```

### Desabilitar despejos ao vivo
Para atenuar o efeito de geração de despejo de memória dinâmica em sistemas que têm muita memória e estão sob estresse, além disso convém desativar a geração de despejo de memória dinâmica. Três opções são fornecidas abaixo.

>[!Caution]
>Esse procedimento pode evitar a coleta de informações de diagnósticos que Microsoft Support podem precisar investigar esse problema. Um agente de suporte pode precisar solicitar que você habilitar novamente a geração de despejo de memória dinâmica com base em cenários de solução de problemas específicos.

Existem dois métodos para desabilitar despejos ao vivo, conforme descrito a seguir.

#### Método 1 (recomendado neste cenário)
Para desativar completamente todos os despejos, incluindo despejos dinâmicos em todo o sistema, siga estas etapas:

1. Crie a seguinte chave do registro: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Sob a nova chave **ForceDumpsDisabled** , crie uma propriedade REG_DWORD como GuardedHost e, em seguida, defina seu valor como 0x10000000.
3. Aplica a nova chave do registro para cada nó do cluster.

>[!NOTE]
>Você deve reiniciar o computador para que a alteração de nregistry entre em vigor.

Depois que essa chave do registro estiver definido, ao vivo despejo criação falhará e gerar um erro de "STATUS_NOT_SUPPORTED".

#### Método 2
Por padrão, o relatório de erros do Windows permitirá que apenas um LiveDump por tipo de relatório por 7 dias e apenas 1 LiveDump por computador por 5 dias. Você pode alterar que definindo as seguintes chaves do registro para permitir somente um LiveDump no computador para sempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Observação* Você deve reiniciar o computador para que a alteração entre em vigor.

### Método 3
Para desabilitar a geração de cluster de despejos dinâmicos (por exemplo, quando um evento 5120 for registrado), execute o seguinte cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Esse cmdlet tem um efeito imediato em todos os nós de cluster sem uma reinicialização do computador.
    
## Reduzir o desempenho de e/s

Se você estiver vendo um desempenho de e/s lento, verifique se o cache é habilitado em sua configuração de espaços de armazenamento diretos. 

Há duas maneiras de verificar: 
     
 
1. Usar o log de cluster. Abra o log de cluster no editor de texto de preferência e procure por "[=== SBL discos ===]." Isso será uma lista de disco no log de foi gerado no nó. 

     Exemplo de discos do cache Enabled: Observe aqui que o estado é CacheDiskStateInitializedAndBound e há um GUID presente aqui. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: Aqui, podemos ver não há nenhuma GUID presente e o estado é CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache não habilitado: Quando todos os discos forem do mesmo tipo caso não está habilitado por padrão. Aqui vemos não há nenhuma GUID presente e o estado é CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usando Get-PhysicalDisk.xml do SDDCDiagnosticInfo
    1. Abra o arquivo XML usando "$d = GetPhysicalDisk.XML Import-Clixml"
    2. Execute o "armazenamento ipmo"
    3. Execute "$d". Observe que uso é seleção automática, não diário você verá saída desta forma: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Utilização| Size|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Healthy|      Seleção automática 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Healthy|Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Healthy| Seleção automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Healthy| Seleção automática |1,82 TB|
    
## Como destruir um cluster existente, portanto, você pode usar os mesmos discos novamente

Em um cluster de espaços de armazenamento diretos, depois que você desabilitar espaços de armazenamento diretos e usar o processo de limpeza descrito [unidades limpas](deploy-storage-spaces-direct.md#step-31-clean-drives), o pool de armazenamento clusterizados ainda permanece em um estado Offline e o serviço de integridade é removido do cluster.

A próxima etapa é remover o pool de armazenamento fantasma: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Agora, se você executar **Get-PhysicalDisk** em qualquer um de nós, você verá todos os discos que estavam no pool. Por exemplo, em um laboratório com um cluster de 4 nós com 4 discos SAS, cada 100GB apresentado para cada nó. Nesse caso, depois que o espaço de armazenamento diretos está desabilitado, que remove a SBL (camada do barramento de armazenamento), mas deixa o filtro, se você executar **Get-PhysicalDisk**, ele deve relatar 4 discos excluindo o disco do sistema operacional local. Em vez disso, ele relatado 16 em vez disso. Isso é o mesmo para todos os nós no cluster. Quando você executa um comando **Get-Disk** , você verá os discos conectados localmente numerados como 0, 1, 2 e assim por diante, como visto neste exemplo de saída:

|Número| Nome amigável| Número de Série|HealthStatus|OperationalStatus|Tamanho total| Estilo de partição|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Healthy | Online|  127 GB| GPT|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
|1|Msft Virtu...||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
|2|Msft Virtu...||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
|4|Msft Virtu...||Healthy| Offline| 100 GB| BRUTO|
|3|Msft Virtu...||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|
||Msft Virtu... ||Healthy| Offline| 100 GB| BRUTO|

    
## Mensagem de erro sobre "tipo de mídia sem suporte" quando você cria um cluster de espaços de armazenamento diretos usando Enable-ClusterS2D  

Quando você executa o cmdlet **Enable-ClusterS2D** , você pode ver erros semelhantes a este:

![Mensagem de erro de cenário 6](media/troubleshooting/scenario-error-message.png)

Para corrigir esse problema, verifique se que o adaptador HBA está configurado no modo HBA. Nenhum HBA deve ser configurado no modo RAID.  
    
## Enable-ClusterStorageSpacesDirect trava em 'Aguardar até SBL discos são mostrados' ou em 27%

Você verá as seguintes informações no relatório de validação:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
O problema é com a placa de expansão HPE SAS que se encontra entre os discos e a placa HBA. A expansão SAS cria uma ID duplicada entre a primeira unidade conectada a expansão e a expansão em si.  Isso foi resolvido no [HPE inteligente matriz controladores SAS expansão Firmware: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## Série Intel SSD DC P4600 tem um NGUID não exclusivo
Você pode encontrar um problema em que um dispositivo de série Intel SSD DC P4600 parece estar relatando semelhante 16 bytes NGUID para vários namespaces como 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 no exemplo a seguir.

|UniqueID| DeviceID |MediaType| BusType| SerialNumber| size|canpool| FriendlyName| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| OK|
|EUI.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    True| INTEL| SSDPE2KE016T7|  OK|

Para corrigir esse problema, atualize o firmware nas unidades Intel para a versão mais recente.  Versão do firmware é conhecido QDV101B1 de maio de 2018 para resolver esse problema.

O [lançamento de maio de 2018 da ferramenta Intel SSD dados Center](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) inclui uma atualização de firmware, QDV101B1, para a série Intel SSD DC P4600.

    
## "Íntegro" disco físico e Status operacional é "Remoção do Pool" 

Em um cluster do Windows Server 2016 espaços de armazenamento diretos, você poderá ver o HealthStatus para discos físicos de uma ou mais como "Íntegros", enquanto o OperationalStatus está "(remoção do Pool, Okey)". 

"Remover do Pool de" é uma intenção definida quando **Remove-PhysicalDisk** é chamado, mas armazenados em funcionamento para manter o estado e permitir a recuperação se a operação de remoção falhar. Você pode alterar manualmente a OperationalStatus a Íntegro com um dos seguintes métodos:

- Remover o disco físico do pool e, em seguida, adicioná-lo novamente.
- Execute o [script PhysicalDiskHealthData.ps1 limpar](https://go.microsoft.com/fwlink/?linkid=2034205) para limpar a intenção. (Disponível para download como um. Arquivo TXT. Você precisará salvá-lo como um. Arquivo ps1 antes de executá-lo.)

Aqui estão alguns exemplos mostrando como executar o script:

- Use o parâmetro **SerialNumber** para especificar o disco que você precisa definir a íntegro. Você pode obter o número de série do **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**. (Estamos usando 0s para o número de série abaixo.)
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Use o parâmetro **UniqueId** para especificar o disco (novamente no **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## Cópia de arquivo é lenta

Você pode ver um problema usando o Explorador de arquivos para copiar um VHD grande para o disco virtual - a cópia do arquivo está levando mais tempo do que o esperado.

Usando o Explorador de arquivos, Robocopy ou Xcopy para copiar um VHD grande para o disco virtual não é um método recomendado para que isso resultará em mais lentas do que o desempenho esperado. O processo de cópia não vai pela pilha de espaços de armazenamento diretos, que fica mais baixo na pilha de armazenamento e, em vez disso, age como um processo de cópia local.

Se você quiser testar o desempenho de espaços de armazenamento diretos, recomendamos usar VMFleet e Diskspd para teste de carga e carregamento os servidores para obter uma linha de base e definir as expectativas do desempenho espaços de armazenamento diretos.

## Espera-se eventos que você veria no restante de nós durante a reinicialização de um nó. 

É seguro ignorar esses eventos: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Se você estiver executando VMs do Azure, você pode ignorar esse evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## Desempenho lento ou "Comunicação perdido," "Erro de e/s", "Desconectado," ou "Não redundância" erros para implantações que usam dispositivos Intel P3x00 NVMe

Identificamos os problemas críticos que afeta alguns usuários espaços de armazenamento diretos que estiverem usando o hardware com base na Intel P3x00 família de dispositivos NVM Express (NVMe) com versões de firmware antes do "Lançamento de manutenção 8". 

>[!NOTE]
> Os OEMs individuais podem ter dispositivos baseados na família de dispositivos NVMe com cadeias de caracteres de versão de firmware exclusivo P3x00 Intel. Entre em contato com seu OEM para obter mais informações da versão do firmware mais recente.

Se você estiver usando o hardware na implantação com base na família de dispositivos NVMe Intel P3x00, recomendamos que você aplique imediatamente o firmware mais recente (pelo menos 8 de versão de manutenção). Este [artigo de suporte da Microsoft](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fornece informações adicionais sobre esse problema. 
