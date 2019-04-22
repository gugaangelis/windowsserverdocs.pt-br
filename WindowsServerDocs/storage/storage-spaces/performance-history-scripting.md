---
title: Scripts com o histórico de desempenho de espaços de armazenamento diretos
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816317"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Scripts com o histórico de desempenho do PowerShell e espaços de armazenamento diretos

> Aplica-se a: Build do Windows Server Insider Preview 17692 e posterior

No Windows Server de 2019, [espaços de armazenamento diretos](storage-spaces-direct-overview.md) registros e repositórios de amplo [histórico de desempenho](performance-history.md) para máquinas virtuais, servidores, discos, volumes, adaptadores de rede e muito mais. Histórico de desempenho é fácil consultar e processar no PowerShell para que você possa rapidamente da *dados brutos* à *respostas reais* a perguntas como:

1. Houve quaisquer picos de CPU na semana passada?
2. Qualquer disco físico está apresentando latência anormal?
3. No momento, quais VMs estão consumindo mais IOPS de armazenamento?
4. Minha largura de banda de rede está saturada?
5. Quando esse volume será executado sem espaço livre?
6. No mês passado, quais VMs usadas mais memória?

O `Get-ClusterPerf` cmdlet baseia-se para o script. Ele aceita a entrada de cmdlets como `Get-VM` ou `Get-PhysicalDisk` pelo pipeline para lidar com associação e você pode direcionar sua saída em cmdlets de utilitário como `Sort-Object`, `Where-Object`, e `Measure-Object` rapidamente compor consultas avançadas.

**Este tópico fornece e explica os scripts de exemplo 6 que responda às 6 perguntas acima.** Além disso, apresentam padrões que você pode aplicar para encontrar picos, médias de localizar, criar gráficos de linhas de tendência, executar excepcionais detecção e muito mais, através de uma variedade de dados e períodos de tempo. Eles são fornecidos como código inicial gratuito para copiar, estender e reutilizar.

   > [!NOTE]
   > Para resumir, os scripts de exemplo omitir coisas como o tratamento de erros que você poderia esperar do código do PowerShell de alta qualidade. Elas são destinadas principalmente inspiração e educação em vez de produção use.

## <a name="sample-1-cpu-i-see-you"></a>Exemplo 1: CPU, estou vendo você!

Este exemplo usa o `ClusterNode.Cpu.Usage` séries do `LastWeek` período de tempo para mostrar o máximo ("high watermark"), o uso de CPU mínimo e médio para todos os servidores no cluster. Ele também faz uma análise quartil simples para mostrar quantos de CPU de horas de uso foi mais de 25%, 50% e 75% nos últimos 8 dias.

### <a name="screenshot"></a>Screenshot

Captura de tela abaixo, podemos ver que *Server-02* tivesse um pico inexplicáveis última semana:

![Captura de tela do PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Como funciona

A saída do `Get-ClusterPerf` pipes perfeitamente em interna `Measure-Object` cmdlet, simplesmente especificamos o `Value` propriedade. Com sua `-Maximum`, `-Minimum`, e `-Average` sinalizadores, `Measure-Object` fornece as três primeiras colunas quase para gratuitamente. Para fazer a análise quartil, é possível direcionar para `Where-Object` e conte quantos valores foram `-Gt` (maior que) 25, 50 ou 75. A última etapa é beautify com `Format-Hours` e `Format-Percent` funções auxiliares – certamente opcional.

### <a name="script"></a>Script

Aqui está o script:

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## <a name="sample-2-fire-fire-latency-outlier"></a>Exemplo 2: Incêndio, incêndio, exceções de latência

Este exemplo usa o `PhysicalDisk.Latency.Average` séries do `LastHour` procurar exceções estatísticas, no período de tempo definido como unidades com um excedendo de latência média por hora + 3σ (três desvios padrão) acima da média de população.

   > [!IMPORTANT]
   > Para resumir, esse script não implementa proteções contra variação baixa, não lida com dados ausentes parciais, não distingue pelo modelo ou firmware, etc. Tenha bom senso e não contam com esse script apenas para determinar se é necessário substituir um disco rígido. Ela é apresentada aqui somente para fins educacionais.

### <a name="screenshot"></a>Screenshot

Na captura de tela abaixo, vemos que há não há exceções:

![Captura de tela do PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Como funciona

Primeiro, podemos excluir unidades ociosas ou quase ociosas, verificando se `PhysicalDisk.Iops.Total` estiver consistentemente `-Gt 1`. Para cada ativo HDD, canalizamos seus `LastHour` período de tempo, composto de medições de 360 intervalos de 10 segundos, para `Measure-Object -Average` para obter sua média latência na última hora. Isso configura o nosso população.

Implementamos o [amplamente conhecido fórmula](http://www.mathsisfun.com/data/standard-deviation.html) para localizar a média `μ` e o desvio padrão `σ` da população. Para cada ativo HDD, podemos comparar sua latência média a média da população e divida pelo desvio padrão. Podemos manter os valores brutos, então, podemos `Sort-Object` nossos resultados, mas use `Format-Latency` e `Format-StandardDeviation` funções auxiliares para beautify o que vamos mostrar – certamente opcionais.

Se nenhuma unidade for mais de + 3σ, podemos `Write-Host` em vermelho; se não estiver, em verde.

### <a name="script"></a>Script

Aqui está o script:

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## <a name="sample-3-noisy-neighbor-thats-write"></a>Exemplo 3: Vizinho barulhento? Isso é gravação!

Histórico de desempenho pode responder a perguntas sobre *agora*também. Novas medidas estão disponíveis em tempo real, a cada 10 segundos. Este exemplo usa o `VHD.Iops.Total` séries do `MostRecent` período de tempo para identificar mais ocupado (dizem "mais") máquinas de virtuais consumindo mais IOPS de armazenamento, em cada host no cluster e mostrar a divisão de leitura/gravação dos seus atividade.

### <a name="screenshot"></a>Screenshot

Captura de tela abaixo, podemos ver as máquinas virtuais de 10 primeiros pela atividade de armazenamento:

![Captura de tela do PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Como funciona

Diferentemente `Get-PhysicalDisk`, o `Get-VM` cmdlet não está com suporte a cluster – ele retorna apenas as VMs no servidor local. Para consultar de todos os servidores em paralelo, podemos encapsular nossa chamada no `Invoke-Command (Get-ClusterNode).Name { ... }`. Para cada VM, obtemos o `VHD.Iops.Total`, `VHD.Iops.Read`, e `VHD.Iops.Write` medidas. Não especificando o `-TimeFrame` parâmetro, obtemos o `MostRecent` único ponto de dados para cada um.

   > [!TIP]
   > Essas séries refletem a soma da atividade desta VM para todos os seus arquivos VHD/VHDX. Este é um exemplo em que o histórico de desempenho está sendo agregado automaticamente para nós. Para obter a divisão por VHD/VHDX, você pode direcionar um indivíduo `Get-VHD` em `Get-ClusterPerf` em vez da VM.

Os resultados de todos os servidores se reúnem como `$Output`, que podemos `Sort-Object` e, em seguida, `Select-Object -First 10`. Observe que `Invoke-Command` decora resultados com um `PsComputerName` propriedade que indica onde eles vieram, que podemos imprimir saber onde a VM está em execução.

### <a name="script"></a>Script

Aqui está o script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Exemplo 4: Como dizem, "25 GB é o novo 10-gig"

Este exemplo usa o `NetAdapter.Bandwidth.Total` séries do `LastDay` definido pelo período de tempo para procurar por sinais de saturação da rede, como > 90% da largura de banda máxima teórica. Para cada adaptador de rede do cluster, ele compara o uso de largura de banda observados mais alto no último dia até a velocidade do link indicado.

### <a name="screenshot"></a>Screenshot

Captura de tela abaixo, podemos ver que um *NX Fabrikam-4 Pro n º 2* pico no último dia:

![Captura de tela do PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Como funciona

Repetimos nossos `Invoke-Command` truque acima para `Get-NetAdapter` em cada servidor e pipe em `Get-ClusterPerf`. Ao longo do caminho, capturamos duas propriedades relevantes: seus `LinkSpeed` cadeia de caracteres como "10 Gbps" e seu brutos `Speed` inteiro como 10000000000. Usamos `Measure-Object` para obter a média e o horário de pico do último dia (lembrete: cada medida no `LastDay` período de tempo representa 5 minutos) e multiplicar por 8 bits por byte para obter uma comparação de igual para igual.

   > [!NOTE]
   > Alguns fornecedores, como Chelsio, incluem atividades de acesso (RDMA) de memória direta remota em seus *adaptador de rede* contadores de desempenho, portanto, ele está incluído no `NetAdapter.Bandwidth.Total` série. Outros, como Mellanox, podem não ter. Se o fornecedor não, basta adicionar o `NetAdapter.Bandwidth.RDMA.Total` série na sua versão do script.

### <a name="script"></a>Script

Aqui está o script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## <a name="sample-5-make-storage-trendy-again"></a>Exemplo 5: Torne o armazenamento moderno novamente!

Para examinar as tendências de macro, o histórico de desempenho é retido por até 1 ano. Este exemplo usa o `Volume.Size.Available` séries do `LastYear` período de tempo para determinar a taxa de armazenamento está ficando cheia e a estimativa quando ela estará completa.

### <a name="screenshot"></a>Screenshot

Captura de tela abaixo, vemos o *Backup* volume é a adição de cerca de 15 GB por dia:

![Captura de tela do PowerShell](media/performance-history/Show-StorageTrend.png)

Com essa taxa, ela será atingir sua capacidade em outro 42 dias.

### <a name="how-it-works"></a>Como funciona

O `LastYear` período de tempo tem um ponto de dados por dia. Embora você precise apenas estritamente dois pontos de acordo com uma linha de tendência, na prática é melhor exigir a obter mais informações, como 14 dias. Usamos `Select-Object -Last 14` para configurar uma matriz de *(x, y)* pontos, para *x* no intervalo [1, 14]. Com esses pontos, implementamos o simples [algoritmo linear mínimos quadrados](http://mathworld.wolfram.com/LeastSquaresFitting.html) para localizar `$A` e `$B` que parametrizar a linha de melhor ajuste *y = ax + b*. Bem-vindo ao ensino tudo novamente.

Dividindo o volume `SizeRemaining` propriedade pela tendência (a inclinação `$A`) nos permite a forma bruta, estimar o número de dias, na taxa atual de crescimento do armazenamento, até que o volume está cheio. O `Format-Bytes`, `Format-Trend`, e `Format-Days` funções auxiliares beautify a saída.

   > [!IMPORTANT]
   > Essa estimativa é linear e com base apenas nas medidas diárias 14 mais recentes. Existem mais técnicas sofisticadas e precisas. Tenha bom senso e não contam com esse script apenas para determinar se vai investir no seu armazenamento de expansão. Ela é apresentada aqui somente para fins educacionais.

### <a name="script"></a>Script

Aqui está o script:

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Exemplo 6: Sobrecarga para a memória, você pode executar, mas você não pode ocultar

Como é de histórico de desempenho coletados e armazenados centralmente para o cluster inteiro, você nunca precisa unir dados em máquinas diferentes, não importa como muitas vezes VMs movem entre hosts. Este exemplo usa o `VM.Memory.Assigned` séries do `LastMonth` período de tempo para identificar as máquinas virtuais consumindo mais memória ao longo dos últimos 35 dias.

### <a name="screenshot"></a>Screenshot

Na captura de tela abaixo, podemos ver as máquinas virtuais de 10 principais por último mês do uso de memória:

![Captura de tela do PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Como funciona

Repetimos nossos `Invoke-Command` truque, apresentada acima, a `Get-VM` em todos os servidores. Usamos `Measure-Object -Average` para obter a média mensal para cada VM, em seguida, `Sort-Object` seguido de `Select-Object -First 10` obter nosso placar de líderes. (Ou talvez seja nossa *queria mais* lista?)

### <a name="script"></a>Script

Aqui está o script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

É só isso! Esperamos que esses exemplos inspiram e ajudam a começar. Com o histórico de desempenho de espaços de armazenamento diretos e a poderosa, amigável para scripts `Get-ClusterPerf` cmdlet, você tem a capacidade fazer – e responder! – complexo perguntas como você gerenciar e monitorar sua infraestrutura do Windows Server 2019.

## <a name="see-also"></a>Consulte também

- [Introdução ao Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Histórico de desempenho](performance-history.md)
