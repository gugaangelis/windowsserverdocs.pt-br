---
title: Solução de problemas de um Cluster de Failover usando o Relatório de Erros do Windows
description: Solução de problemas de um cluster de failover usando relatórios do WER, com detalhes específicos sobre como coletar relatórios e diagnosticar problemas comuns.
ms.author: johnmar
author: JohnMarlin-MSFT
ms.date: 03/27/2018
ms.openlocfilehash: 9b4569f4f4d28ad1380cf057cdf96e4b81fbdb2a
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990739"
---
# <a name="troubleshooting-a-failover-cluster-using-windows-error-reporting"></a>Solução de problemas de um Cluster de Failover usando o Relatório de Erros do Windows

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server

O Relatório de Erros do Windows (WER) é uma infra-estrutura de comentários flexível baseada em eventos, criada para ajudar os administradores avançados ou o suporte da camada 3 a coletar informações sobre os problemas de hardware e software que o Windows pode detectar, relatar as informações à Microsoft e fornecer aos usuários todas as soluções disponíveis. Essa [referência](/powershell/module/windowserrorreporting/) fornece descrições e sintaxe para todos os cmdlets WindowsErrorReporting.

As informações sobre solução de problemas apresentadas abaixo serão úteis para solucionar problemas avançados que foram escalonados e que podem exigir que os dados sejam enviados à Microsoft para fins de preparo.

## <a name="enabling-event-channels"></a>Habilitando canais de eventos

Quando o Windows Server é instalado, muitos canais de eventos são habilitados por padrão. Mas, às vezes, ao diagnosticar um problema, queremos poder habilitar alguns desses canais de eventos, pois isso ajudará na separação e no diagnóstico de problemas do sistema.

Você pode habilitar canais de eventos adicionais em cada nó de servidor no cluster, conforme necessário; no entanto, essa abordagem apresenta dois problemas:

1. Você precisa se lembrar de habilitar os mesmos canais de evento em cada novo nó de servidor que você adicionar ao cluster.
2. Ao diagnosticar, pode ser entediante habilitar canais de eventos específicos, reproduzir o erro e repetir esse processo até a causa raiz.

Para evitar esses problemas, você pode habilitar canais de eventos na inicialização do cluster. A lista de canais de eventos habilitados no cluster pode ser configurada usando a propriedade pública **EnabledEventLogs**. Por padrão, os seguintes canais de eventos estão habilitados:

```powershell
PS C:\Windows\system32> (get-cluster).EnabledEventLogs
```

Veja um exemplo da saída:
```
Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic,4,0xFFFFFFFD
Microsoft-Windows-SMBDirect/Debug,4
Microsoft-Windows-SMBServer/Analytic
Microsoft-Windows-Kernel-LiveDump/Analytic
```

A propriedade **EnabledEventLogs** é uma cadeia de caracteres multistring, em que cada cadeia de caracteres está no formato: **Channel-Name, nível de log, keyword-Mask**. A **palavra-chave-Mask** pode ser um número hexadecimal (prefixo 0x), octal (prefixo 0) ou número decimal (sem prefixo). Por exemplo, para adicionar um novo canal de evento à lista e configurar o **nível de log** e a **máscara de palavra-chave** , você pode executar:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,321"
```

Se você quiser definir o **nível de log** , mas manter a **palavra-chave-Mask** em seu valor padrão, poderá usar qualquer um dos seguintes comandos:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,"
```

Se você quiser manter o **nível de log** em seu valor padrão, mas definir a **palavra-chave-Mask** , poderá executar o seguinte comando:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,0xf1"
```

Se desejar manter tanto o nível de **log** quanto a **palavra-chave-Mask** em seus valores padrão, você poderá executar qualquer um dos seguintes comandos:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,"
```

Esses canais de eventos serão habilitados em cada nó de cluster quando o serviço de cluster for iniciado ou sempre que a propriedade **EnabledEventLogs** for alterada.

## <a name="gathering-logs"></a>Coletando logs

Depois de habilitar os canais de eventos, você pode usar o **DumpLogQuery** para coletar logs. A propriedade do tipo de recurso público **DumpLogQuery** é um valor de mutistring. Cada cadeia de caracteres é uma [consulta XPath, conforme descrito aqui](/windows/win32/wes/consuming-events).

Ao solucionar problemas, se você precisar coletar canais de eventos adicionais, poderá modificar a propriedade **DumpLogQuery** adicionando consultas adicionais ou modificando a lista.

Para fazer isso, primeiro teste sua consulta XPATH usando o cmdlet [Get-WinEvent](/powershell/module/Microsoft.PowerShell.Diagnostics/Get-WinEvent?view=powershell-5.1) do PowerShell:

```powershell
get-WinEvent -FilterXML "<QueryList><Query><Select Path='Microsoft-Windows-GroupPolicy/Operational'>*[System[TimeCreated[timediff(@SystemTime) &gt;= 600000]]]</Select></Query></QueryList>"
```

Em seguida, acrescente sua consulta à propriedade **DumpLogQuery** do recurso:

```powershell
(Get-ClusterResourceType -Name "Physical Disk".DumpLogQuery += "<QueryList><Query><Select Path='Microsoft-Windows-GroupPolicy/Operational'>*[System[TimeCreated[timediff(@SystemTime) &gt;= 600000]]]</Select></Query></QueryList>"
```

E se você quiser obter uma lista de consultas a serem usadas, execute:
```powershell
(Get-ClusterResourceType -Name "Physical Disk").DumpLogQuery
```

## <a name="gathering-windows-error-reporting-reports"></a>Coletando relatórios de Relatório de Erros do Windows

Relatório de Erros do Windows relatórios são armazenados em **%ProgramData%\Microsoft\Windows\WER**

Dentro da pasta **WER** , a pasta **ReportsQueue** contém relatórios que estão aguardando para serem carregados no Watson.

```powershell
PS C:\Windows\system32> dir c:\ProgramData\Microsoft\Windows\WER\ReportQueue
```

Veja um exemplo da saída:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

Directory of C:\ProgramData\Microsoft\Windows\WER\ReportQueue

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_02d10a3f
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_0588dd06
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_10d55ef5
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13258c8c
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13a8c4ac
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_13dcf4d3
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1721a0b0
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1839758a
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_1d4131cb
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_23551d79
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_2468ad4c
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_255d4d61
<date>  <time>    <DIR>          Critical_Physical Disk_1cbd8ffecbc8a1a0e7819e4262e3ece2909a157a_00000000_cab_08289734
<date>  <time>    <DIR>          Critical_Physical Disk_64acaf7e4590828ae8a3ac3c8b31da9a789586d4_00000000_cab_1d94712e
<date>  <time>    <DIR>          Critical_Physical Disk_ae39f5243a104f21ac5b04a39efeac4c126754_00000000_003359cb
<date>  <time>    <DIR>          Critical_Physical Disk_ae39f5243a104f21ac5b04a39efeac4c126754_00000000_cab_1b293b17
<date>  <time>    <DIR>          Critical_Physical Disk_b46b8883d892cfa8a26263afca228b17df8133d_00000000_cab_08abc39c
<date>  <time>    <DIR>          Kernel_166_1234dacd2d1a219a3696b6e64a736408fc785cc_00000000_cab_19c8a127
               0 File(s)              0 bytes
              20 Dir(s)  23,291,658,240 bytes free
```

Dentro da pasta **WER** , a pasta **ReportsArchive** contém relatórios que já foram carregados no Watson. Os dados nesses relatórios são excluídos, mas o arquivo **Report. WER** persiste.

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive
```

Veja um exemplo da saída:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

Directory of c:\ProgramData\Microsoft\Windows\WER\ReportArchive

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>    <DIR>          Critical_powershell.exe_7dd54f49935ce48b2dd99d1c64df29a5cfb73db_00000000_cab_096cc802
               0 File(s)              0 bytes
               3 Dir(s)  23,291,658,240 bytes free

```

Relatório de Erros do Windows fornece muitas configurações para personalizar a experiência de relatório de problemas. Para obter mais informações, consulte a [documentação](/windows/win32/wer/wer-settings)do relatório de erros do Windows.


## <a name="troubleshooting-using-windows-error-reporting-reports"></a>Solução de problemas usando relatórios de Relatório de Erros do Windows

### <a name="physical-disk-failed-to-come-online"></a>O disco físico não pôde ser colocado online

Para diagnosticar esse problema, navegue até a pasta de relatório do WER:

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive\Critical_PhysicalDisk_b46b8883d892cfa8a26263afca228b17df8133d_00000000_cab_08abc39c
```

Veja um exemplo da saída:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_1.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_10.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_11.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_12.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_13.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_14.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_15.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_16.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_17.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_18.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_19.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_2.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_20.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_21.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_22.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_23.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_24.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_25.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_26.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_27.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_28.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_29.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_3.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_30.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_31.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_32.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_33.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_34.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_35.evtx
<date>  <time>         2,166,784 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_36.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_37.evtx
<date>  <time>            33,194 Report.wer
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_38.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_39.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_4.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_40.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_41.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_5.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_6.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_7.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_8.evtx
<date>  <time>            69,632 CLUSWER_RHS_ERROR_8d06c544-47a4-4396-96ec-af644f45c70a_9.evtx
<date>  <time>             7,382 WERC263.tmp.WERInternalMetadata.xml
<date>  <time>            59,202 WERC36D.tmp.csv
<date>  <time>            13,340 WERC38D.tmp.txt
```

Em seguida, comece a triagem do arquivo **Report. WER** — isso informará o que falhou.

```
EventType=Failover_clustering_resource_error
<skip>
Sig[0].Name=ResourceType
Sig[0].Value=Physical Disk
Sig[1].Name=CallType
Sig[1].Value=ONLINERESOURCE
Sig[2].Name=RHSCallResult
Sig[2].Value=5018
Sig[3].Name=ApplicationCallResult
Sig[3].Value=999
Sig[4].Name=DumpPolicy
Sig[4].Value=5225058577
DynamicSig[1].Name=OS Version
DynamicSig[1].Value=10.0.17051.2.0.0.400.8
DynamicSig[2].Name=Locale ID
DynamicSig[2].Value=1033
DynamicSig[27].Name=ResourceName
DynamicSig[27].Value=Cluster Disk 10
DynamicSig[28].Name=ReportId
DynamicSig[28].Value=8d06c544-47a4-4396-96ec-af644f45c70a
DynamicSig[29].Name=FailureTime
DynamicSig[29].Value=2017//12//12-22:38:05.485
```

Como o recurso falhou em ficar online, nenhum despejo foi coletado, mas o relatório de Relatório de Erros do Windows coletava logs. Se você abrir todos os arquivos **. evtx** usando o Microsoft Message Analyzer, verá todas as informações coletadas usando as consultas a seguir por meio do canal do sistema, do canal de aplicativo, dos canais de diagnóstico de cluster de failover e de alguns outros canais genéricos.

```powershell
PS C:\Windows\system32> (Get-ClusterResourceType -Name "Physical Disk").DumpLogQuery
```

Veja um exemplo da saída:
```
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Kernel-PnP/Configuration">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-ReFS/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Ntfs/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Ntfs/WHC">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Health">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Storport/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-ClassPnP/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-ClassPnP/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-ScmBus/Certification">*[System[TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-ScmBus/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-PmemDisk/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-NvdimmN/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-INvdimm/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-PersistentMemory-VirtualNvdimm/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Disk/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Disk/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-ScmDisk0101/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Partition/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Volume/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-VolumeSnapshot-Driver/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-FailoverClustering-Clusport/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-FailoverClustering-ClusBflt/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageSpaces-Driver/Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageManagement/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-StorageSpaces-Driver/Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Storage-Tiering/Admin">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Hyper-V-VmSwitch-Operational">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
<QueryList><Query Id="0"><Select Path="Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic">*[System[TimeCreated[timediff(@SystemTime) &lt;= 600000]]]</Select></Query></QueryList>
```

O analisador de mensagem permite que você capture, exiba e analise o tráfego de mensagens de protocolo. Ele também permite que você rastreie e avalie eventos do sistema e outras mensagens de componentes do Windows. Você pode baixar [o Microsoft Message Analyzer aqui](https://www.microsoft.com/download/details.aspx?id=44226). Ao carregar os logs no analisador de mensagem, você verá os seguintes provedores e mensagens dos canais de log.

![Carregando logs no analisador de mensagem](media/troubleshooting-using-WER-reports/loading-logs-into-message-analyzer.png)

Você também pode agrupar por provedores para obter a seguinte exibição:

![Logs agrupados por provedores](media/troubleshooting-using-WER-reports/logs-grouped-by-providers.png)

Para identificar por que o disco falhou, navegue até os eventos em **FailoverClustering/Diagnostic** e **FailoverClustering/DiagnosticVerbose**. Em seguida, execute a seguinte consulta: **EventLog. EventData ["LogString"] contém "disco de cluster 10"**.  Isso fornecerá a você a seguinte saída:

![Saída da consulta de log em execução](media/troubleshooting-using-WER-reports/output-of-running-log-query.png)


### <a name="physical-disk-timed-out"></a>O disco físico atingiu o tempo limite

Para diagnosticar esse problema, navegue até a pasta de relatório do WER. A pasta contém arquivos de log e arquivos de despejo para **RHS**, **clussvc.exe**e do processo que hospeda o serviço "**smphost**", como mostrado abaixo:

```powershell
PS C:\Windows\system32> dir C:\ProgramData\Microsoft\Windows\WER\ReportArchive\Critical_PhysicalDisk_64acaf7e4590828ae8a3ac3c8b31da9a789586d4_00000000_cab_1d94712e
```

Veja um exemplo da saída:
```
Volume in drive C is INSTALLTO
Volume Serial Number is 4031-E397

<date>  <time>    <DIR>          .
<date>  <time>    <DIR>          ..
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_1.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_10.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_11.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_12.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_13.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_14.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_15.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_16.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_17.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_18.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_19.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_2.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_20.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_21.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_22.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_23.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_24.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_25.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_26.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_27.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_28.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_29.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_3.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_30.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_31.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_32.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_33.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_34.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_35.evtx
<date>  <time>         2,166,784 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_36.evtx
<date>  <time>         1,118,208 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_37.evtx
<date>  <time>        28,340,500 memory.hdmp
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_38.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_39.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_4.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_40.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_41.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_5.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_6.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_7.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_8.evtx
<date>  <time>            69,632 CLUSWER_RHS_HANG_75e60318-50c9-41e4-94d9-fb0f589cd224_9.evtx
<date>  <time>         4,466,943 minidump.0f14.mdmp
<date>  <time>         1,735,776 minidump.2200.mdmp
<date>  <time>            33,890 Report.wer
<date>  <time>            49,267 WER69FA.tmp.mdmp
<date>  <time>             5,706 WER70A2.tmp.WERInternalMetadata.xml
<date>  <time>            63,206 WER70E0.tmp.csv
<date>  <time>            13,340 WER7100.tmp.txt
```

Em seguida, comece a triagem do arquivo **Report. WER** — isso informará qual chamada ou recurso está suspenso.

```
EventType=Failover_clustering_resource_timeout_2
<skip>
Sig[0].Name=ResourceType
Sig[0].Value=Physical Disk
Sig[1].Name=CallType
Sig[1].Value=ONLINERESOURCE
Sig[2].Name=DumpPolicy
Sig[2].Value=5225058577
Sig[3].Name=ControlCode
Sig[3].Value=18
DynamicSig[1].Name=OS Version
DynamicSig[1].Value=10.0.17051.2.0.0.400.8
DynamicSig[2].Name=Locale ID
DynamicSig[2].Value=1033
DynamicSig[26].Name=ResourceName
DynamicSig[26].Value=Cluster Disk 10
DynamicSig[27].Name=ReportId
DynamicSig[27].Value=75e60318-50c9-41e4-94d9-fb0f589cd224
DynamicSig[29].Name=HangThreadId
DynamicSig[29].Value=10008
```

A lista de serviços e processos que coletamos em um despejo é controlada pela seguinte propriedade: **PS C:\Windows\system32> (Get-ClusterResourceType-Name "disco físico"). DumpServicesSmphost**

Para identificar por que o travamento ocorreu, abra os arquivos dum. Em seguida, execute a seguinte consulta: **EventLog. EventData ["LogString"] contém "disco de cluster 10"** . isso fornecerá a você a seguinte saída:

![Saída da consulta de log de execução 2](media/troubleshooting-using-WER-reports/output-of-running-log-query-2.png)

Podemos examinar isso com o thread do arquivo **Memory. hdmp** :

```
# 21  Id: 1d98.2718 Suspend: 0 Teb: 0000000b`f1f7b000 Unfrozen
# Child-SP          RetAddr           Call Site
00 0000000b`f3c7ec38 00007ff8`455d25ca ntdll!ZwDelayExecution+0x14
01 0000000b`f3c7ec40 00007ff8`2ef19710 KERNELBASE!SleepEx+0x9a
02 0000000b`f3c7ece0 00007ff8`3bdf7fbf clusres!ResHardDiskOnlineOrTurnOffMMThread+0x2b0
03 0000000b`f3c7f960 00007ff8`391eed34 resutils!ClusWorkerStart+0x5f
04 0000000b`f3c7f9d0 00000000`00000000 vfbasics+0xed34
```