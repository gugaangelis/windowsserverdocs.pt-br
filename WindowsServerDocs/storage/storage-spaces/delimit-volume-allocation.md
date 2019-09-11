---
title: Delimitar a alocação de volumes no Espaços de Armazenamento Diretos
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: faf9547833764e9075e86515d1f486a5a3f61ff8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872083"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimitar a alocação de volumes no Espaços de Armazenamento Diretos
> Aplica-se a: Windows Server 2019

O Windows Server 2019 apresenta uma opção para delimitar manualmente a alocação de volumes no Espaços de Armazenamento Diretos. Isso pode aumentar significativamente a tolerância a falhas sob determinadas condições, mas impõe algumas considerações e complexidade de gerenciamento adicionais. Este tópico explica como ele funciona e fornece exemplos no PowerShell.

   > [!IMPORTANT]
   > Esse recurso é novo no Windows Server 2019. Ele não está disponível no Windows Server 2016. 

## <a name="prerequisites"></a>Pré-requisitos

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Ícone de marca de seleção verde.](media/delimit-volume-allocation/supported.png) Considere o uso desta opção se:

- O cluster tem seis ou mais servidores; e
- O cluster usa somente a resiliência [de espelhamento de três vias](storage-spaces-fault-tolerance.md#mirroring)

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Ícone de X vermelho.](media/delimit-volume-allocation/unsupported.png) Não use esta opção se:

- O cluster tem menos de seis servidores; or
- Seu cluster usa [paridade](storage-spaces-fault-tolerance.md#parity) ou resiliência [de paridade acelerada por espelho](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)

## <a name="understand"></a>Entender

### <a name="review-regular-allocation"></a>Revisão: alocação regular

Com o espelhamento de três vias regular, o volume é dividido em muitas "Slabs" pequenas que são copiadas três vezes e distribuídas uniformemente em cada unidade em cada servidor no cluster. Para obter mais detalhes, leia [este blog aprofundado](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagrama que mostra o volume que está sendo dividido em três pilhas de Slabs e distribuída uniformemente em todos os servidores.](media/delimit-volume-allocation/regular-allocation.png)

Essa alocação padrão maximiza leituras e gravações paralelas, levando a um melhor desempenho e é atraente em sua simplicidade: cada servidor está igualmente ocupado, cada unidade está igualmente cheia e todos os volumes permanecem online ou ficam offline juntos. Cada volume tem a garantia de sobreviver a duas falhas simultâneas, como mostram [esses exemplos](storage-spaces-fault-tolerance.md#examples) .

No entanto, com essa alocação, os volumes não podem sobreviver a três falhas simultâneas. Se três servidores falharem ao mesmo tempo ou se as unidades em três servidores falharem ao mesmo tempo, os volumes se tornarão inacessíveis porque pelo menos alguns Slabs eram (com probabilidade muito alta) alocados às três unidades ou servidores exatos que falharam.

No exemplo a seguir, os servidores 1, 3 e 5 falham ao mesmo tempo. Embora muitos Slabs tenham cópias sobreviventes, alguns não:

![Diagrama mostrando três de seis servidores realçados em vermelho e o volume geral é vermelho.](media/delimit-volume-allocation/regular-does-not-survive.png)

O volume fica offline e se torna inacessível até que os servidores sejam recuperados.

### <a name="new-delimited-allocation"></a>Novo: alocação delimitada

Com a alocação delimitada, você especifica um subconjunto de servidores a serem usados (mínimo três para espelhamento triplo). O volume é dividido em Slabs que são copiados três vezes, como antes, mas em vez de alocar em cada servidor, **os Slabs são alocados somente para o subconjunto de servidores que você especificar**.

![Diagrama que mostra o volume que está sendo dividido em três pilhas de Slabs e é distribuído somente para três de seis servidores.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Vantagens

Com essa alocação, o volume provavelmente sobreviver a três falhas simultâneas: na verdade, sua probabilidade de sobrevivência aumenta de 0% (com alocação regular) para 95% (com alocação delimitada) nesse caso! Intuitivamente, isso ocorre porque ele não depende dos servidores 4, 5 ou 6, portanto, ele não é afetado por suas falhas.

No exemplo acima, os servidores 1, 3 e 5 falham ao mesmo tempo. Como a alocação delimitada garantiu que o servidor 2 contém uma cópia de cada laje, cada laje tem uma cópia sobrevivente e o volume permanece online e acessível:

![Diagrama mostrando três de seis servidores realçados em vermelho, mas o volume geral está verde.](media/delimit-volume-allocation/delimited-does-survive.png)

A probabilidade de sobrevivência depende do número de servidores e outros fatores – consulte [análise](#analysis) para obter detalhes.

#### <a name="disadvantages"></a>Desvantagens

A alocação delimitada impõe algumas considerações e complexidade de gerenciamento adicionais:

1. O administrador é responsável por delimitar a alocação de cada volume para balancear a utilização de armazenamento entre os servidores e defender a alta probabilidade de sobrevivência, conforme descrito na seção [práticas recomendadas](#best-practices) .

2. Com a alocação delimitada, Reserve o equivalente de **uma unidade de capacidade por servidor (sem máximo)** . Isso é mais do que a [recomendação publicada](plan-volumes.md#choosing-the-size-of-volumes) para alocação regular, que maximizar em quatro unidades de capacidade total.

3. Se um servidor falhar e precisar ser substituído, conforme descrito em [remover um servidor e suas unidades](remove-servers.md#remove-a-server-and-its-drives), o administrador será responsável por atualizar a deslimitação dos volumes afetados adicionando o novo servidor e removendo o que falhou um – exemplo abaixo.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Você pode usar o `New-Volume` cmdlet para criar volumes em espaços de armazenamento diretos.

Por exemplo, para criar um volume de espelhamento de três vias regular:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Criar um volume e delimitar sua alocação

Para criar um volume de espelho de três vias e delimitar sua alocação:

1. Primeiro, atribua os servidores no cluster à variável `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > No Espaços de Armazenamento Diretos, o termo "unidade de escala de armazenamento" refere-se a todo o armazenamento bruto conectado a um servidor, incluindo unidades conectadas diretamente e compartimentos externos conectados diretamente a unidades. Nesse contexto, ele é o mesmo que ' Server '.

2. Especifique quais servidores usar com o novo `-StorageFaultDomainsToUse` parâmetro e indexação em. `$Servers` Por exemplo, para delimitar a alocação para o primeiro, o segundo e o terceiro servidores (índices 0, 1 e 2):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Ver uma alocação delimitada

Para ver como *myvolume* é alocado, use o `Get-VirtualDiskFootprintBySSU.ps1` script no [Apêndice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Observe que somente Server1, server2 e Server3 contêm Slabs de *myvolume*.

### <a name="change-a-delimited-allocation"></a>Alterar uma alocação delimitada

Use os novos `Add-StorageFaultDomain` cmdlets e `Remove-StorageFaultDomain` para alterar como a alocação é delimitada.

Por exemplo, para mover *myvolume* por um servidor:

1. Especifique que o quarto servidor **pode** armazenar Slabs de *myvolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Especifique que o primeiro servidor **não pode** armazenar Slabs de *myvolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Equilibre o pool de armazenamento para que a alteração entre em vigor:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagrama que mostra a Slabs migrar en-Mass dos servidores 1, 2 e 3 para os servidores 2, 3 e 4.](media/delimit-volume-allocation/move.gif)

Você pode monitorar o progresso do rebalanceamento com `Get-StorageJob`.

Após a conclusão, verifique se *myvolume* foi movido executando `Get-VirtualDiskFootprintBySSU.ps1` novamente.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Observe que Server1 não contém mais Slabs de *myvolume* – em vez disso, Server04.

## <a name="best-practices"></a>Práticas recomendadas

Aqui estão as práticas recomendadas a serem seguidas ao usar a alocação de volume delimitada:

### <a name="choose-three-servers"></a>Escolher três servidores

Delimite cada volume de espelho de três vias a três servidores, e não mais.

### <a name="balance-storage"></a>Balancear o armazenamento

Equilibre a quantidade de armazenamento alocada para cada servidor, a contabilização do tamanho do volume.

### <a name="every-delimited-allocation-unique"></a>Cada alocação delimitada exclusiva

Para maximizar a tolerância a falhas, torne a alocação de cada volume exclusiva, o que significa que ele não compartilha *todos os* seus servidores com outro volume (alguma sobreposição está ok). Com N servidores, há "N escolher 3" combinações exclusivas – aqui está o que significa para alguns tamanhos de cluster comuns:

| Número de servidores (N) | Número de alocações delimitadas exclusivas (N escolha 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Considere essa revisão útil do [combinatorics e escolha a notação](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Veja um exemplo que maximiza a tolerância a falhas – cada volume tem uma alocação delimitada exclusiva:

![alocação exclusiva](media/delimit-volume-allocation/unique-allocation.png)

Por outro lado, no próximo exemplo, os três primeiros volumes usam a mesma alocação delimitada (para os servidores 1, 2 e 3) e os últimos três volumes usam a mesma alocação delimitada (para os servidores 4, 5 e 6). Isso não maximiza a tolerância a falhas: se três servidores falharem, vários volumes poderão ficar offline e ficar inacessíveis ao mesmo tempo.

![alocação não exclusiva](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analisa

Esta seção deriva a probabilidade matemática de que um volume permaneça online e acessível (ou, de forma equivalente, a fração esperada do armazenamento geral que permanece online e acessível) como uma função do número de falhas e do tamanho do cluster.

   > [!NOTE]
   > Esta seção é leitura opcional. Se você estiver pronto para ver a matemática, continue lendo! Mas se não, não se preocupe: O [uso no PowerShell e nas](#usage-in-powershell) [práticas recomendadas](#best-practices) é tudo o que você precisa para implementar a alocação delimitada com êxito.

### <a name="up-to-two-failures-is-always-okay"></a>Até duas falhas é sempre Ok

Cada volume de espelho de três vias pode sobreviver a duas falhas ao mesmo tempo, como [esses exemplos](storage-spaces-fault-tolerance.md#examples) ilustram, independentemente de sua alocação. Se duas unidades falharem, ou se dois servidores falharem, ou um de cada, cada volume de espelho de três vias permanecerá online e acessível, mesmo com a alocação regular.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Mais da metade o cluster está falhando nunca muito bem

Por outro lado, no caso extremo de que mais da metade de servidores ou unidades no cluster falham ao mesmo tempo, o [quorum é perdido](understand-quorum.md) e cada volume de espelho de três vias fica offline e se torna inacessível, independentemente de sua alocação.

### <a name="what-about-in-between"></a>E o que há entre?

Se três ou mais falhas ocorrerem ao mesmo tempo, mas pelo menos metade de servidores e unidades ainda estiverem ativos, os volumes com alocação delimitada poderão permanecer online e acessíveis, dependendo de quais servidores têm falhas. Vamos executar os números para determinar as chances exatas.

Para simplificar, suponha que os volumes sejam independentes e distribuídos de forma idêntica (IID) de acordo com as práticas recomendadas acima, e que as combinações exclusivas suficientes estejam disponíveis para que cada alocação de volume seja exclusiva. A probabilidade de que qualquer volume determinado sobreviver também é a fração esperada do armazenamento geral que sobrevive pela linearidade da expectativa. 

Considerando os **N** servidores dos quais o **F** tem falhas, um volume alocado para **três** deles ficará offline se-e-somente-se todos os **3** estiverem entre o **F** com falhas. Há **(N escolha F)** maneiras de ocorrerem falhas de **f** , das quais **(F escolha 3)** resultam no volume ficar offline e ficando inacessível. A probabilidade pode ser expressa como:

![P_offline = Fc3/NcF](media/delimit-volume-allocation/probability-volume-offline.png)

Em todos os outros casos, o volume permanece online e acessível:

![P_online = 1 – (Fc3/NcF)](media/delimit-volume-allocation/probability-volume-online.png)

As tabelas a seguir avaliam a probabilidade de alguns tamanhos de cluster comuns e até 5 falhas, revelando que a alocação delimitada aumenta a tolerância a falhas em comparação com a alocação regular em todos os casos considerados.

### <a name="with-6-servers"></a>Com 6 servidores

| Alocação                           | Probabilidade de sobrevivente 1 falha | Probabilidade de sobreviventes 2 falhas | Probabilidade de sobreviventes 3 falhas | Probabilidade de sobreviver 4 falhas | Probabilidade de sobreviventes 5 falhas |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, espalhada entre todos os 6 servidores | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| Delimitado a 3 servidores somente          | 100%                               | 100%                                | 95,0%                               | 0                                  | 0                                  |

   > [!NOTE]
   > Após mais de três falhas de 6 servidores no total, o cluster perde quorum.

### <a name="with-8-servers"></a>Com 8 servidores

| Alocação                           | Probabilidade de sobrevivente 1 falha | Probabilidade de sobreviventes 2 falhas | Probabilidade de sobreviventes 3 falhas | Probabilidade de sobreviver 4 falhas | Probabilidade de sobreviventes 5 falhas |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, espalhada entre todos os 8 servidores | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| Delimitado a 3 servidores somente          | 100%                               | 100%                                | 98,2%                               | 94,3%                               | 0                                  |

   > [!NOTE]
   > Após mais de 4 falhas de 8 servidores no total, o cluster perde quorum.

### <a name="with-12-servers"></a>Com 12 servidores

| Alocação                            | Probabilidade de sobrevivente 1 falha | Probabilidade de sobreviventes 2 falhas | Probabilidade de sobreviventes 3 falhas | Probabilidade de sobreviver 4 falhas | Probabilidade de sobreviventes 5 falhas |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, espalhada entre todos os 12 servidores | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| Delimitado a 3 servidores somente           | 100%                               | 100%                                | 99,5%                               | 99,2%                               | 98,7%                               |

### <a name="with-16-servers"></a>Com 16 servidores

| Alocação                            | Probabilidade de sobrevivente 1 falha | Probabilidade de sobreviventes 2 falhas | Probabilidade de sobreviventes 3 falhas | Probabilidade de sobreviver 4 falhas | Probabilidade de sobreviventes 5 falhas |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, espalhada por todos os 16 servidores | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| Delimitado a 3 servidores somente           | 100%                               | 100%                                | 99,8%                               | 99,8%                               | 99,8%                               |

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="can-i-delimit-some-volumes-but-not-others"></a>Posso delimitar alguns volumes, mas não outros?

Sim. Você pode escolher por volume se deseja ou não delimitar a alocação.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>A alocação delimitada muda a forma como a substituição da unidade funciona?

Não, é o mesmo que com a alocação regular.

## <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Tolerância a falhas no Espaços de Armazenamento Diretos](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Anexo

Esse script ajuda a ver como os volumes são alocados.

Para usá-lo conforme descrito acima, copie/cole e salve `Get-VirtualDiskFootprintBySSU.ps1`como.

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
