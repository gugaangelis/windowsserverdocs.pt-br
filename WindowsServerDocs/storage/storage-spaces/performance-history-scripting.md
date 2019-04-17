---
title: Scripts com espaços de armazenamento diretos histórico de desempenho
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843104"
---
# Criando scripts com o PowerShell e espaços de armazenamento diretos histórico de desempenho

> Aplicável a: Windows Server Insider Preview compilação 17692 e posteriores do

No Windows Server 2019, [Espaços de armazenamento diretos](storage-spaces-direct-overview.md) registra e armazena amplo [histórico de desempenho](performance-history.md) para máquinas virtuais, servidores, unidades, volumes, adaptadores de rede e muito mais. Histórico de desempenho é fácil de consulta e o processo no PowerShell, portanto, você pode ir rapidamente de *dados brutos* para *reais respostas* a perguntas como:

1. Houve qualquer picos de CPU na semana passada?
2. Qualquer disco físico está apresentando latência anormal?
3. Quais VMs estão consumindo mais armazenamento IOPS agora?
4. Meu largura de banda de rede estão saturada?
5. Quando esse volume será executado fora do espaço livre?
6. No mês passado, quais VMs usado o máximo de memória?

O `Get-ClusterPerf` cmdlet foi criado para a execução de scripts. Ele aceita a entrada de cmdlets como `Get-VM` ou `Get-PhysicalDisk` pelo pipeline de manipular a associação e você pode Redirecione sua saída no utilitário cmdlets como `Sort-Object`, `Where-Object`, e `Measure-Object` para compor rapidamente consultas avançadas.

**Este tópico fornece e explica 6 scripts de exemplo que perguntas a 6 acima.** Eles apresentam padrões que você pode aplicar para encontrar picos, encontrar médias, plotar linhas de tendência, execute excepcionais detecção e muito mais em uma variedade de dados e períodos. Eles são fornecidos como código inicial livre para copiar, estender e reutilizar.

   > [!NOTE]
   > Para abreviar, os scripts de exemplo omitir coisas como tratamento de erros que você esperava de código do PowerShell de alta qualidade. Eles são projetados principalmente para inspiração e educação em vez de produção usa.

## Exemplo 1: CPU, eu verei você!

Este exemplo usa o `ClusterNode.Cpu.Usage` série do `LastWeek` período de tempo para mostrar o máximo ("marca"), o uso de CPU mínimo e médio para cada servidor no cluster. Ela também faz análise quartil simples para mostrar o uso de CPU horas quantos foi superior a 25%, 50% e 75% nos últimos 8 dias.

### Screenshot

Na captura de tela abaixo, podemos ver que o *servidor 02* tinha um aumento não explicado semana passada:

![Captura de tela do PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### Como funciona

A saída de `Get-ClusterPerf` pipes perfeitamente em integrado `Measure-Object` cmdlet, podemos apenas de especificar o `Value` propriedade. Com o `-Maximum`, `-Minimum`, e `-Average` sinalizadores, `Measure-Object` nos dá três primeiras colunas quase para gratuitos. Para fazer a análise de quartil, podemos pode Redirecione para `Where-Object` e contar quantos valores foram `-Gt` (maior) 25, 50 ou 75. A última etapa é beautify com `Format-Hours` e `Format-Percent` funções auxiliares – certamente opcional.

### Script

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

## Exemplo 2: Tiro, Tiro, excepcionais latência

Este exemplo usa o `PhysicalDisk.Latency.Average` série do `LastHour` período para procurar destaques estatísticas, definidas como unidades com um exceder de latência média por hora + 3σ (três desvios padrão) acima da média de população.

   > [!IMPORTANT]
   > Para abreviar, este script não implementa proteções contra variação baixa, não manipula dados ausentes parciais, não faz distinção por modelo ou firmware, etc. Por favor exercício julgamento boa e não conte com este script para determinar se é necessário substituir o disco rígido. Ele é apresentado aqui apenas para fins educacionais.

### Screenshot

Na captura de tela abaixo, vemos que não há nenhuma destaques:

![Captura de tela do PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### Como funciona

Primeiro, excluímos unidades ociosas ou quase ociosas verificando que `PhysicalDisk.Iops.Total` é consistentemente `-Gt 1`. Para cada ativo HDD, podemos redirecione o `LastHour` cronograma, composta de 360 medições em intervalos de 10 segundos, para `Measure-Object -Average` para obter sua latência média na última hora. Isso configura nossos participantes.

Implementar a [fórmula amplamente conhecidos](http://www.mathsisfun.com/data/standard-deviation.html) para encontrar a média `μ` e desvio padrão `σ` da população. Para cada ativo HDD, podemos comparar sua latência média para a média de população e divida pelo desvio padrão. Podemos manter os valores brutos, para que possamos `Sort-Object` nossos resultados, mas use `Format-Latency` e `Format-StandardDeviation` funções auxiliares para beautify que mostraremos – certamente opcionais.

Se qualquer unidade é mais de + 3σ, nós `Write-Host` em vermelho; Se não, em verde.

### Script

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

## Exemplo 3: Vizinho barulhento? Isso é gravação!

Histórico de desempenho pode responder perguntas sobre *no momento*, também. Novas medidas estão disponíveis em tempo real, a cada 10 segundos. Este exemplo usa o `VHD.Iops.Total` série do `MostRecent` período para identificar o mais ocupado (dizem "mais ruído") máquinas virtuais consumindo mais armazenamento IOPS, em cada host no cluster e mostrar a divisão de leitura/gravação de suas atividades.

### Screenshot

Na captura de tela abaixo, vemos as máquinas virtuais de 10 primeiros por atividade de armazenamento:

![Captura de tela do PowerShell](media/performance-history/Show-TopIopsVMs.png)

### Como funciona

Ao contrário de `Get-PhysicalDisk`, o `Get-VM` cmdlet não está ciente de cluster – retorna apenas VMs no servidor local. Para consultar em cada servidor em paralelo, nós encapsulamos nossa chamada em `Invoke-Command (Get-ClusterNode).Name { ... }`. Para cada VM, obtemos a `VHD.Iops.Total`, `VHD.Iops.Read`, e `VHD.Iops.Write` medições. Não especificando o `-TimeFrame` parâmetro, obtemos o `MostRecent` para cada um único ponto de dados.

   > [!TIP]
   > Essas séries refletem a soma de atividade dessa VM para todos os seus arquivos VHD/VHDX. Este é um exemplo em que o histórico de desempenho é está sendo automaticamente agregado para nós. Para obter a divisão por VHD/VHDX, você pode redirecionar um indivíduo `Get-VHD` em `Get-ClusterPerf` em vez de VM.

Os resultados de cada servidor se reúnem como `$Output`, que podemos `Sort-Object` e em seguida `Select-Object -First 10`. Observe que `Invoke-Command` decora resultados com um `PsComputerName` propriedade indicando onde eles vieram, que podemos imprimir saber onde a VM está sendo executado.

### Script

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

## Exemplo 4: Como dizem, "25 GB é o novo 10-GB"

Este exemplo usa o `NetAdapter.Bandwidth.Total` série do `LastDay` período para procurar sinais de saturação de rede definidas como > 90% da largura de banda máxima teórica. Para cada adaptador de rede no cluster, ele compara o uso de largura de banda observados mais alto no último dia até a velocidade do link estabelecido.

### Screenshot

Na captura de tela abaixo, podemos ver que um *Fabrikam NX-4 Pro n º 2* pico no último dia:

![Captura de tela do PowerShell](media/performance-history/Show-NetworkSaturation.png)

### Como funciona

Podemos Repita nosso `Invoke-Command` truque acima para `Get-NetAdapter` em cada servidor e pipe em `Get-ClusterPerf`. Ao longo do caminho, vamos pegar duas propriedades relevantes: seu `LinkSpeed` cadeia de caracteres como "10 Gbps" e sua bruto `Speed` inteiro como 10000000000. Usamos `Measure-Object` para obter a média e o pico do último dia (lembrete: cada medida no `LastDay` período representa 5 minutos) e multiplique pelo 8 bits por byte para obter uma comparação de igual para igual.

   > [!NOTE]
   > Alguns fornecedores, como Chelsio, incluem atividades de acesso (RDMA) memória remoto direto em seus contadores de desempenho do *Adaptador de rede* , para que ela está incluída no `NetAdapter.Bandwidth.Total` série. Outras, como adaptadores Mellanox, tenham não. Se o fornecedor não, basta adicionar o `NetAdapter.Bandwidth.RDMA.Total` série na sua versão deste script.

### Script

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

## Exemplo 5: Tornar armazenamento moderno novamente!

Para examinar as tendências de macro, histórico de desempenho é retido para até 1 ano. Este exemplo usa o `Volume.Size.Available` série do `LastYear` período de tempo para determinar a taxa de armazenamento está cheio e a estimativa quando ele estará completo.

### Screenshot

Na captura de tela abaixo, vemos que o volume de *Backup* é a adição de cerca de 15 GB por dia:

![Captura de tela do PowerShell](media/performance-history/Show-StorageTrend.png)

Com essa taxa, ela atingirá sua capacidade em outro 42 dias.

### Como funciona

O `LastYear` período tem um ponto de dados por dia. Embora seja necessário somente estritamente dois pontos para ajustar uma linha de tendência, na prática é melhor exigir mais informações como 14 dias. Usamos `Select-Object -Last 14` para configurar uma matriz de pontos *(x, y)* , para *x* no intervalo [1, 14]. Com esses pontos, implementamos o [algoritmo linear mínimos quadrados](http://mathworld.wolfram.com/LeastSquaresFitting.html) simples para encontrar `$A` e `$B` que parametrizar a linha de melhor ajuste *y = ax + b*. Bem-vindo ao ensino em toda novamente.

Dividir o volume `SizeRemaining` propriedade pela tendência (a inclinação `$A`) nos permite a forma bruta estimar quantos dias, na taxa atual de crescimento do armazenamento, até que o volume está cheio. O `Format-Bytes`, `Format-Trend`, e `Format-Days` funções auxiliares beautify a saída.

   > [!IMPORTANT]
   > Essa estimativa é linear e com base apenas nas medições de diárias 14 mais recentes. Existem técnicas mais sofisticadas e precisas. Por favor exercício julgamento boa e não conte com este script para determinar se deve investir em expandindo seu armazenamento. Ele é apresentado aqui apenas para fins educacionais.

### Script

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

## Exemplo 6: Exagero de memória, você pode executar, mas você não pode ocultar

Como o histórico de desempenho é coletada e armazenada centralmente para todo o cluster, você nunca precisa para compor dados de computadores diferentes, não importa como muitas vezes VMs movem entre hosts. Este exemplo usa o `VM.Memory.Assigned` série do `LastMonth` para identificar as máquinas virtuais consumindo mais memória nos últimos 35 dias do período de tempo.

### Screenshot

Na captura de tela abaixo, vemos as máquinas virtuais de 10 primeiros pelo uso da memória mês passado:

![Captura de tela do PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### Como funciona

Podemos Repita nosso `Invoke-Command` truque, introduzido acima, a `Get-VM` em cada servidor. Usamos `Measure-Object -Average` para obter a média mensal para cada VM, em seguida, `Sort-Object` seguido por `Select-Object -First 10` para obter nossa placar de líderes. (Ou talvez seja nosso list? *Quisesse mais* )

### Script

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

Pronto! Esperamos que essas amostras inspiram e o ajudarão a começar. Com espaços de armazenamento diretos histórico de desempenho e a poderosa, scripts amigável `Get-ClusterPerf` cmdlet, você têm a chance de pedir – e responda! – complexo perguntas como gerenciar e monitorar sua infraestrutura do Windows Server 2019.

## Consulte também

- [Introdução ao Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Histórico de desempenho](performance-history.md)
