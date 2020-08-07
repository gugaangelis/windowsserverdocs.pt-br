---
title: Criando scripts com o histórico de desempenho Espaços de Armazenamento Diretos
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: a0e04034c79a82bb245b611eca291acca0e40f9f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935777"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Criando scripts com o PowerShell e Espaços de Armazenamento Diretos histórico de desempenho

> Aplica-se a: Windows Server 2019

No Windows Server 2019, o [espaços de armazenamento diretos](storage-spaces-direct-overview.md) registra e armazena o [histórico de desempenho](performance-history.md) extensivo para máquinas virtuais, servidores, unidades, volumes, adaptadores de rede e muito mais. O histórico de desempenho é fácil de consultar e processar no PowerShell para que você possa ir rapidamente de *dados brutos* para *respostas reais* a perguntas como:

1. Houve picos de CPU na semana passada?
2. Algum disco físico está apresentando latência anormal?
3. Quais VMs estão consumindo a maior parte do IOPS de armazenamento agora?
4. Minha largura de banda de rede está saturada?
5. Quando esse volume ficar sem espaço livre?
6. No último mês, quais VMs usaram mais memória?

O `Get-ClusterPerf` cmdlet é criado para scripts. Ele aceita a entrada de cmdlets como `Get-VM` ou `Get-PhysicalDisk` pelo pipeline para lidar com a associação, e você pode canalizar sua saída em cmdlets de utilitário como `Sort-Object` , `Where-Object` e `Measure-Object` para compor rapidamente consultas poderosas.

**Este tópico fornece e explica 6 scripts de exemplo que respondem às 6 perguntas acima.** Eles apresentam padrões que você pode aplicar para localizar picos, encontrar médias, plotar linhas de tendência, executar detecção de exceção e muito mais, em uma variedade de dados e períodos de tempo. Eles são fornecidos como um código inicial gratuito para você copiar, estender e reutilizar.

   > [!NOTE]
   > Para resumir, os scripts de exemplo omitem coisas como o tratamento de erros que você pode esperar do código do PowerShell de alta qualidade. Elas são destinadas principalmente para inspiração e educação em vez de uso em produção.

## <a name="sample-1-cpu-i-see-you"></a>Exemplo 1: CPU, vejo você!

Este exemplo usa a `ClusterNode.Cpu.Usage` série do `LastWeek` período de tempo para mostrar o máximo ("marca d' água alta"), o mínimo e o uso médio da CPU para cada servidor no cluster. Ele também faz uma análise simples de quartil para mostrar quantas horas o uso da CPU foi superior a 25%, 50% e 75% nos últimos 8 dias.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos que o *servidor-02* teve um pico não explicado na última semana:

![Captura de tela do PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Como ele funciona

A saída de `Get-ClusterPerf` pipes é bem no cmdlet interno `Measure-Object` , apenas especificamos a `Value` propriedade. Com seus `-Maximum` `-Minimum` sinalizadores,, e `-Average` , `Measure-Object` nos dá as três primeiras colunas quase gratuitas. Para fazer a análise de quartil, podemos canalizar `Where-Object` e contar quantos valores foram `-Gt` (maiores que) 25, 50 ou 75. A última etapa é Beautify com `Format-Hours` `Format-Percent` funções auxiliares e, certamente, opcional.

### <a name="script"></a>script

Este é o script:

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

## <a name="sample-2-fire-fire-latency-outlier"></a>Exemplo 2: incêndio, incêndio, exceção de latência

Este exemplo usa a `PhysicalDisk.Latency.Average` série do `LastHour` período de tempo para procurar por exceções estatísticas, definidas como unidades com uma latência média de hora excedente +3 σ (três desvios padrão) acima da média da população.

   > [!IMPORTANT]
   > Para resumir, esse script não implementa proteções contra a baixa variação, não trata dados parciais ausentes, não distingue por modelo ou firmware, etc. Faça um bom Judgement e não confie nesse script sozinho para determinar se um disco rígido deve ser substituído. Ele é apresentado aqui apenas para fins educacionais.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos que não há exceções:

![Captura de tela do PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Como ele funciona

Primeiro, excluímos unidades ociosas ou quase ociosas verificando se isso `PhysicalDisk.Iops.Total` é consistente `-Gt 1` . Para cada HDD ativo, canalizamos seu `LastHour` período de tempo, composto de 360 medições em intervalos de 10 segundos, para `Measure-Object -Average` obter sua latência média na última hora. Isso configura nossa população.

Implementamos a [fórmula amplamente conhecida](http://www.mathsisfun.com/data/standard-deviation.html) para encontrar a média `μ` e o desvio padrão `σ` da população. Para cada HDD ativo, comparamos sua latência média com a média da população e dividemos pelo desvio padrão. Mantemos os valores brutos, portanto, podemos nos `Sort-Object` resultados, mas as funções de uso `Format-Latency` e `Format-StandardDeviation` auxiliares Beautify o que mostraremos – certamente opcional.

Se qualquer unidade for maior do que +3 σ, `Write-Host` em vermelho; se não, em verde.

### <a name="script"></a>script

Este é o script:

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>Exemplo 3: vizinho barulhento? Isso é escrito!

O histórico de desempenho também pode responder a perguntas sobre *o momento.* Novas medições estão disponíveis em tempo real, a cada 10 segundos. Este exemplo usa a `VHD.Iops.Total` série do `MostRecent` período de tempo para identificar o mais ocupado (algumas podem dizer "noisiest") máquinas virtuais que consomem a maior parte do IOPS de armazenamento, em cada host no cluster e mostram a divisão de leitura/gravação de sua atividade.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos as 10 principais máquinas virtuais por atividade de armazenamento:

![Captura de tela do PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Como ele funciona

Ao contrário `Get-PhysicalDisk` do, o `Get-VM` cmdlet não reconhece o cluster – ele retorna apenas as VMs no servidor local. Para consultar de cada servidor em paralelo, Encapsulamos nossa chamada em `Invoke-Command (Get-ClusterNode).Name { ... }` . Para cada VM, obtemos as `VHD.Iops.Total` `VHD.Iops.Read` medidas, e `VHD.Iops.Write` . Ao não especificar o `-TimeFrame` parâmetro, obtemos o `MostRecent` único ponto de dados para cada um.

   > [!TIP]
   > Essas séries refletem a soma da atividade dessa VM a todos os seus arquivos VHD/VHDX. Este é um exemplo em que o histórico de desempenho está sendo agregado automaticamente para nós. Para obter a divisão por VHD/VHDX, você poderia canalizar um indivíduo `Get-VHD` em `Get-ClusterPerf` vez da VM.

Os resultados de cada servidor vêm juntos como `$Output` , que podemos `Sort-Object` e depois `Select-Object -First 10` . Observe que `Invoke-Command` decora os resultados com uma `PsComputerName` propriedade que indica de onde eles vieram, que podemos imprimir para saber onde a VM está em execução.

### <a name="script"></a>script

Este é o script:

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Exemplo 4: como dizem, "25 GB é o novo" de 10 GB "

Este exemplo usa a `NetAdapter.Bandwidth.Total` série do `LastDay` período de tempo para procurar sinais de saturação de rede, definidos como >90% da largura de banda máxima teórica. Para cada adaptador de rede no cluster, ele compara o uso mais alto de largura de banda observado no último dia para sua velocidade de link declarada.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos que uma *Fabrikam NX-4 Pro #2* foi efetuada com pico no último dia:

![Captura de tela do PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Como ele funciona

Repetimos nosso `Invoke-Command` truque de acima para `Get-NetAdapter` em cada servidor e pipe para o `Get-ClusterPerf` . Ao longo do caminho, pegamos duas propriedades relevantes: sua `LinkSpeed` cadeia de caracteres como "10 Gbps" e seu inteiro bruto, `Speed` como 10000000000. Usamos `Measure-Object` para obter a média e o pico do último dia (lembrete: cada medida no `LastDay` período representa 5 minutos) e multiplicada por 8 bits por byte para obter uma comparação de maçãs para maçãs.

   > [!NOTE]
   > Alguns fornecedores, como o Chelsio, incluem atividade de RDMA (acesso remoto direto à memória) em seus contadores de desempenho do *adaptador de rede* , portanto, ele está incluído na `NetAdapter.Bandwidth.Total` série. Outros, como o Mellanox, talvez não. Se seu fornecedor não, basta adicionar a `NetAdapter.Bandwidth.RDMA.Total` série à sua versão do script.

### <a name="script"></a>script

Este é o script:

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

## <a name="sample-5-make-storage-trendy-again"></a>Exemplo 5: tornar a tendência de armazenamento novamente!

Para examinar as tendências de macro, o histórico de desempenho é retido por até 1 ano. Este exemplo usa a `Volume.Size.Available` série do `LastYear` período de tempo para determinar a taxa em que o armazenamento está se enchendo e Estimando quando ele estará cheio.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos que o volume de *backup* está adicionando cerca de 15 GB por dia:

![Captura de tela do PowerShell](media/performance-history/Show-StorageTrend.png)

A essa taxa, ela atingirá sua capacidade em mais de 42 dias.

### <a name="how-it-works"></a>Como ele funciona

O `LastYear` período de tempo tem um ponto de dados por dia. Embora você precise estritamente apenas de dois pontos para se ajustar a uma linha de tendência, na prática, é melhor exigir mais, como 14 dias. Usamos `Select-Object -Last 14` para configurar uma matriz de pontos *(x, y)* , para *x* no intervalo [1, 14]. Com esses pontos, implementamos o [algoritmo de quadrados mínimos lineares](http://mathworld.wolfram.com/LeastSquaresFitting.html) simples para localizar `$A` e `$B` parametrizamos a linha de melhor ajuste *y = ax + b*. Bem-vindo ao ensino médio.

Dividir a propriedade do volume `SizeRemaining` pela tendência (a inclinação `$A` ) nos permite estimar de forma bruta quantos dias, na taxa atual de crescimento do armazenamento, até que o volume esteja cheio. As `Format-Bytes` `Format-Trend` `Format-Days` funções auxiliares, e Beautify a saída.

   > [!IMPORTANT]
   > Essa estimativa é linear e baseada apenas nas últimas 14 medidas diárias. Existem técnicas mais sofisticadas e precisas. Faça um bom Judgement e não confie nesse script sozinho para determinar se deve investir na expansão do armazenamento. Ele é apresentado aqui apenas para fins educacionais.

### <a name="script"></a>script

Este é o script:

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Exemplo 6: conexagero de memória, você pode executar, mas não pode ocultar

Como o histórico de desempenho é coletado e armazenado centralmente para todo o cluster, você nunca precisa reunir dados de computadores diferentes, independentemente de quantas vezes as VMs se movem entre os hosts. Este exemplo usa a `VM.Memory.Assigned` série do `LastMonth` período de tempo para identificar as máquinas virtuais que consomem mais memória nos últimos 35 dias.

### <a name="screenshot"></a>Captura de tela

Na captura de tela abaixo, vemos as 10 principais máquinas virtuais por uso de memória no mês passado:

![Captura de tela do PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Como ele funciona

Repetimos nosso `Invoke-Command` truque, apresentado acima, para `Get-VM` em cada servidor. Usamos `Measure-Object -Average` para obter a média mensal de cada VM e, em seguida, `Sort-Object` seguimos `Select-Object -First 10` para obter nosso placar. (Ou talvez seja nossa lista *mais desejada* ?)

### <a name="script"></a>script

Este é o script:

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

É isso! Espero que esses exemplos inspiram você e o ajudem a começar. Com o histórico de desempenho Espaços de Armazenamento Diretos e o cmdlet poderoso e amigável `Get-ClusterPerf` para scripts, você é capacitado a perguntar – e responder! – perguntas complexas ao gerenciar e monitorar sua infraestrutura do Windows Server 2019.

## <a name="additional-references"></a>Referências adicionais

- [Introdução ao Windows PowerShell](/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Histórico de desempenho](performance-history.md)
