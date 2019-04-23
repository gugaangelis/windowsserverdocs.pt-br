---
title: Delimitar a alocação de volumes em espaços de armazenamento diretos
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889047"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimitar a alocação de volumes em espaços de armazenamento diretos
> Aplica-se a: Windows Server Insider Preview, build 17093 e posterior

Windows Server Insider Preview apresenta uma opção para delimitar manualmente a alocação de volumes em espaços de armazenamento diretos. Fazer para que pode aumentar significativamente a tolerância a falhas em determinadas condições, mas impõe algumas considerações de gerenciamento e a complexidade. Este tópico explica como funciona e fornece exemplos do PowerShell.

   > [!IMPORTANT]
   > Esse recurso é novo no Windows Server Insider Preview, build 17093 e posterior. Ele não está disponível no Windows Server 2016. Nós o convidamos a profissionais de TI para unir a [programa do Windows Server Insider](https://aka.ms/serverinsider) para nos fornecer comentários sobre o que podemos fazer para tornar o Windows Server funcionam melhor para sua organização.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Ícone de marca de seleção verde.](media/delimit-volume-allocation/supported.png) Considere usar essa opção se:

- O cluster tem seis ou mais servidores; e
- O cluster usa somente [espelho triplo](storage-spaces-fault-tolerance.md#mirroring) resiliência

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Ícone de X vermelho.](media/delimit-volume-allocation/unsupported.png) Não use esta opção se:

- O cluster tiver menos de seis servidores; ou
- O cluster usa [paridade](storage-spaces-fault-tolerance.md#parity) ou [acelerada de espelho paridade](storage-spaces-fault-tolerance.md#mirror-accelerated-parity) resiliência

## <a name="understand"></a>Entender

### <a name="review-regular-allocation"></a>Revisão: alocação de regular

Com o espelhamento de três vias regular, o volume é dividido em várias pequenas "repara" que é copiadas três vezes e distribuídas uniformemente entre cada unidade em todos os servidores no cluster. Para obter mais detalhes, leia [este blog de aprofundamento](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagrama mostrando o volume que está sendo dividido em três pilhas de sessões e distribuído uniformemente entre todos os servidores.](media/delimit-volume-allocation/regular-allocation.png)

Essa alocação padrão maximiza paralelo lê e grava, levando a melhor desempenho e é atraente na sua simplicidade: todos os servidores são igualmente ocupados, cada unidade é igualmente completa e todos os volumes permanecem online ou offline juntos. Todos os volumes é garantido para resistir a até duas falhas simultâneas, como [esses exemplos](storage-spaces-fault-tolerance.md#examples) ilustrar.

No entanto, com essa alocação volumes não podem sobreviver a três falhas simultâneas. Se três servidores falharem ao mesmo tempo, ou se as unidades em três servidores falharem ao mesmo tempo, os volumes ficar inacessíveis porque pelo menos algum repara (com uma probabilidade muito alta) alocadas para os três unidades de exatas ou servidores que falharam.

No exemplo a seguir, os servidores de 1, 3 e 5 falharem ao mesmo tempo. Embora muitas sessões tem cópias de sobreviventes, alguns não:

![Diagrama mostrando o três de seis servidores realçados em vermelho e o volume total é vermelho.](media/delimit-volume-allocation/regular-does-not-survive.png)

O volume fica offline e ficará inacessível até que os servidores são recuperados.

### <a name="new-delimited-allocation"></a>Novo: delimitados alocação

Com alocação delimitada, você deve especificar um subconjunto de servidores para usar (três mínima para espelho triplo). O volume é dividido em sessões que são copiados três vezes, como antes, mas em vez de alocar em todos os servidores do **as sessões são alocados somente ao subconjunto de servidores que você especificar**.

![Diagrama mostrando o volume que está sendo dividido em três pilhas de sessões e distribuído somente para três das seis servidores.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Vantagens

Com essa alocação, o volume é provável resistir a falhas simultâneas de três: na verdade, sua probabilidade de sobrevivência aumenta de 0% (com alocação regular) para 95% (com alocação delimitada) nesse caso! Intuitivamente, isso ocorre porque ele não depende de servidores, 4, 5 ou 6 para que ele não é afetado por suas falhas.

No exemplo acima, os servidores de 1, 3 e 5 falharem ao mesmo tempo. Como alocação delimitada garantiu que esse servidor 2 contém uma cópia de cada sessão, cada sessão tem uma cópia sobrevivente e o volume permanece online e acessível:

![Diagrama mostrando o três de seis servidores realçados em vermelho, ainda que o volume total é verde.](media/delimit-volume-allocation/delimited-does-survive.png)

Probabilidade de sobrevivência depende do número de servidores e outros fatores – consulte [análise](#analysis) para obter detalhes.

#### <a name="disadvantages"></a>Desvantagens

Alocação delimitada impõe algumas considerações de gerenciamento e a complexidade:

1. O administrador é responsável para delimitar a alocação de cada volume para equilibrar o uso do armazenamento entre servidores e mantêm: alta probabilidade de sobrevivência, conforme descrito na [práticas recomendadas](#best-practices) seção.

2. Com alocação delimitada, reservar o equivalente a **unidade de capacidade de uma por servidor (com nenhum máximo)**. Isso é mais do que o [publicados recomendação](plan-volumes.md#choosing-the-size-of-volumes) para alocação regular, que permite o máximo de capacidade de quatro unidades de total.

3. Se um servidor falhar e precisa ser substituído, conforme descrito em [remover um servidor e suas unidades](remove-servers.md#remove-a-server-and-its-drives), o administrador é responsável por atualizar a delimitação dos volumes afetados pela adição do novo servidor e removendo o exemplo de um – com falha abaixo.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Você pode usar o `New-Volume` cmdlet para criar volumes em espaços de armazenamento diretos.

Por exemplo, para criar um volume de espelho de três vias regular:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Crie um volume e delimitar sua alocação

Para criar um volume de espelho triplo e delimitar sua alocação:

1. Primeiro atribuir os servidores no seu cluster para a variável `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > Espaços de armazenamento diretos, o termo 'Unidade de escala de armazenamento' se refere a todo o armazenamento bruto anexado a um servidor, incluindo unidades de conexão direta e direta compartimentos externos com as unidades. Nesse contexto, é o mesmo que 'server'.

2. Especifique quais servidores para usar com o novo `-StorageFaultDomainsToUse` parâmetro e indexando em `$Servers`. Por exemplo, para delimitar a alocação para o primeiro, segundo e terceiro servidores (índices de 0, 1 e 2):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Veja uma alocação delimitada

Para ver como *MyVolume* é alocado, use o `Get-VirtualDiskFootprintBySSU.ps1` script na [apêndice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Observe que apenas servidor1, Servidor2 e Server3 contém repara dos *MyVolume*.

### <a name="change-a-delimited-allocation"></a>Alterar uma alocação delimitada

Use a nova `Add-StorageFaultDomain` e `Remove-StorageFaultDomain` cmdlets para alterar como a alocação é delimitada.

Por exemplo, para mover *MyVolume* a tecla TAB um servidor:

1. Especificar que o servidor quarto **pode** armazenar repara de *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Especificar que o primeiro servidor **não é possível** armazenar repara dos *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Redistribuir o pool de armazenamento para que a alteração entre em vigor:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagrama mostrando que a repara migrar en masse de servidores 1, 2 e 3 para servidores de 2, 3 e 4.](media/delimit-volume-allocation/move.gif)

Você pode monitorar o progresso do rebalanceamento com `Get-StorageJob`.

Quando ele for concluído, verifique *MyVolume* foi movido, executando `Get-VirtualDiskFootprintBySSU.ps1` novamente.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Observe que não contém Server1 repara da *MyVolume* mais – em vez disso, Server04 faz.

## <a name="best-practices"></a>Práticas recomendadas

Aqui estão as práticas recomendadas a seguir ao usar delimitados alocação de volume:

### <a name="choose-three-servers"></a>Escolha três servidores

Delimite cada volume de três vias para três servidores, não mais.

### <a name="balance-storage"></a>Armazenamento de saldo

Equilibre a quantidade de armazenamento é alocado para cada servidor, contabilidade do tamanho do volume.

### <a name="every-delimited-allocation-unique"></a>Cada alocação delimitada exclusiva

Para maximizar a tolerância a falhas, tornar a alocação de cada volume exclusivo, o que significa que não compartilha *todos os* seus servidores com o outro volume (alguma sobreposição é okey). Com os servidores de N, há combinações exclusivas de "N escolher 3" – aqui está o que isso significa que para alguns tamanhos de cluster comuns:

| Número de servidores (N) | Número de exclusivo delimitado por alocações (N escolha 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Considere esta revisão úteis de [Matemática combinatória e escolha notação](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Aqui está um exemplo que maximiza a tolerância a falhas – todos os volumes tem uma alocação delimitada exclusiva:

![alocação exclusiva](media/delimit-volume-allocation/unique-allocation.png)

Por outro lado, no exemplo a seguir, os primeiros três volumes usam a mesma alocação delimitada (para servidores de 1, 2 e 3) e os últimos três volumes usam a mesma alocação delimitada (para servidores, 4, 5 e 6). Isso não maximizar a tolerância a falhas: se três servidores falharem, vários volumes poderia ficar offline e inacessíveis ao mesmo tempo.

![alocação não-exclusiva](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Análise

Esta seção deriva a probabilidade de matemática que um volume permanece online e acessível (ou de forma equivalente, a fração esperada de armazenamento geral que permanece online e acessível) como uma função do número de falhas e o tamanho do cluster.

   > [!NOTE]
   > Esta seção é opcional de leitura. Se você estiver interessado em ver a matemática, continue lendo! Mas, se não, não se preocupe: [Uso do PowerShell](#usage-in-powershell) e [práticas recomendadas](#best-practices) é tudo o que você precisa implementar alocação delimitada com êxito.

### <a name="up-to-two-failures-is-always-okay"></a>Até duas falhas são sempre okey

Todos os volumes de espelho triplo podem sobreviver a falhas de até dois ao mesmo tempo, como [esses exemplos](storage-spaces-fault-tolerance.md#examples) ilustrar, independentemente de sua alocação. Se duas unidades falharem, dois servidores falharem ou um de cada, cada volume de três vias permanece online e acessível, mesmo com alocação regular.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Falha de mais da metade do cluster nunca é okey

Por outro lado, em casos extremos que mais da metade das unidades no cluster ou servidores ao mesmo tempo falham [quorum for perdido](understand-quorum.md) e todos os volumes de espelho triplo fica offline e torna-se inacessíveis, independentemente de sua alocação.

### <a name="what-about-in-between"></a>E entre?

Se três ou mais falhas ocorrem ao mesmo tempo, mas pelo menos metade dos servidores e unidades ainda estão em volumes com alocação delimitado podem permanecer online e acessível, dependendo de quais servidores têm falhas. Vamos executar os números para determinar a probabilidade de precisa.

Para simplificar, suponha que volumes são independentemente e distribuídos de forma idêntica (IID) acordo com as melhores práticas acima e que combinações exclusivos suficientes estão disponíveis para alocação de cada volume para ser exclusivo. A probabilidade de qualquer volume sobrevive também é a fração esperada de armazenamento geral que sobrevive a linearidade de expectativa. 

Considerando **N** servidores dos quais **F** apresentam falhas, um volume alocado a **3** deles ficar offline se-e-somente-se todos os **3** estão entre o  **F** com falhas. Há **(N escolha F)** maneiras para **F** falhas ocorra, dos quais **(F escolhe 3)** resultar no volume off-line e se torne inacessível. A probabilidade pode ser expresso como:

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

Em todos os outros casos, o volume permanece online e acessível:

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

As tabelas a seguir avaliar a probabilidade para alguns tamanhos de cluster comuns e até 5 falhas, revelando a alocação delimitada aumenta a tolerância a falhas em comparação comparada alocação regular em todos os casos considerados.

### <a name="with-6-servers"></a>Com os servidores de 6

| Alocação                           | Probabilidade de sobreviventes 1 Falha | Probabilidade de sobrevivência a falhas de 2 | Probabilidade de sobrevivência a falhas de 3 | Probabilidade de sobrevivência a falhas de 4 | Probabilidade de sobrevivência a falhas de 5 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regulares e distribuídos em todos os servidores de 6 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado por 3 somente a servidores          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > Depois de mais de 3 falhas em 6 total de servidores, o cluster perde quorum.

### <a name="with-8-servers"></a>Com 8 servidores

| Alocação                           | Probabilidade de sobreviventes 1 Falha | Probabilidade de sobrevivência a falhas de 2 | Probabilidade de sobrevivência a falhas de 3 | Probabilidade de sobrevivência a falhas de 4 | Probabilidade de sobrevivência a falhas de 5 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regulares e distribuídos em todos os servidores de 8 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado por 3 somente a servidores          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > Depois de mais de 4 falhas em 8 servidores total, o cluster perde quorum.

### <a name="with-12-servers"></a>Com 12 servidores

| Alocação                            | Probabilidade de sobreviventes 1 Falha | Probabilidade de sobrevivência a falhas de 2 | Probabilidade de sobrevivência a falhas de 3 | Probabilidade de sobrevivência a falhas de 4 | Probabilidade de sobrevivência a falhas de 5 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regulares e distribuídos em todos os servidores de 12 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado por 3 somente a servidores           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>Com 16 servidores

| Alocação                            | Probabilidade de sobreviventes 1 Falha | Probabilidade de sobrevivência a falhas de 2 | Probabilidade de sobrevivência a falhas de 3 | Probabilidade de sobrevivência a falhas de 4 | Probabilidade de sobrevivência a falhas de 5 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regulares e distribuídos em todos os 16 servidores | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado por 3 somente a servidores           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="can-i-delimit-some-volumes-but-not-others"></a>Posso delimitar alguns volumes, mas não para outros?

Sim. Você pode escolher por volume se deseja ou não delimitar a alocação.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>Alocação delimitada altera como a substituição da unidade funciona?

Não, é o mesmo que a alocação regular.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Tolerância a falhas em espaços de armazenamento diretos](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Apêndice

Este script ajuda você a ver como os volumes são alocados.

Para usá-lo, conforme descrito acima, copie/cole e salve como `Get-VirtualDiskFootprintBySSU.ps1`.

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
