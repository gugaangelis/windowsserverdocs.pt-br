---
title: Entender e implantar a memória persistente
description: Informações detalhadas sobre o que é a memória persistente e como configurá-la com espaços de armazenamento diretos no Windows Server 2019.
keywords: Espaços de Armazenamento Diretos, memória persistente, pMem, armazenamento, S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 1/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: a9070d2e2ab73c7882f4b2ef585ccb01986695bb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822309"
---
# <a name="understand-and-deploy-persistent-memory"></a>Entender e implantar a memória persistente

> Aplica-se a: Windows Server 2019

A memória persistente (ou PMem) é um novo tipo de tecnologia de memória que oferece uma combinação exclusiva de alta capacidade e persistência acessíveis. Este artigo fornece informações sobre o PMem e as etapas para implantá-lo no Windows Server 2019 usando o Espaços de Armazenamento Diretos.

## <a name="background"></a>Histórico

O PMem é um tipo de memória RAM não volátil (NVDIMM) que mantém seu conteúdo por meio de ciclos de energia. O conteúdo da memória permanece mesmo quando a energia do sistema fica inativa no caso de uma perda de energia inesperada, um desligamento iniciado pelo usuário, uma falha do sistema e assim por diante. Essa característica exclusiva significa que você também pode usar a PMem como armazenamento. É por isso que você pode ouvir as pessoas se referirem à PMem como "memória de classe de armazenamento".

Para ver alguns desses benefícios, vejamos a demonstração a seguir do Microsoft Ignite 2018.

[demonstração do ![Microsoft Ignite 2018 pMem](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualquer sistema de armazenamento que fornece tolerância a falhas necessariamente faz cópias distribuídas de gravações. Essas operações devem atravessar a rede e ampliar o tráfego de gravação de back-end. Por esse motivo, os números de benchmark de maior IOPS absolutos normalmente são obtidos apenas com a medição de leituras, especialmente se o sistema de armazenamento tiver otimizações de senso comum para ler a partir da cópia local sempre que possível. Espaços de Armazenamento Diretos é otimizado para fazer isso.

**Quando medido usando apenas operações de leitura, o cluster fornece 13.798.674 IOPS.**

![captura de tela de registro de IOPS de 13.7 m](media/deploy-pmem/iops-record.png)

Se você assistir ao vídeo de maneira mais minuciosa, observará que o que é ainda mais Jaw é a latência. Mesmo em mais de 13,7 M IOPS, o sistema de arquivos no Windows está relatando latência consistentemente menor que 40 μs! (Esse é o símbolo de microssegundos, um milionésimo de um segundo.) Essa velocidade é uma ordem de magnitude mais rápida do que os fornecedores típicos de tudo-flash que, de forma orgulho, anunciam hoje.

Juntos, Espaços de Armazenamento Diretos no Windows Server 2019 e no Intel® Optane™ memória persistente de DC oferecem desempenho inovador. Esse benchmark de HCI líder do setor de 13.7 M IOPS, acompanhado por latência previsível e extremamente baixa, é mais do que o dobro de nosso benchmark líder do setor de IOPS de 6.7 M. Além disso, desta vez precisávamos de apenas 12 nós de servidor&mdash;25% menos de dois anos atrás.

![Ganhos de IOPS](media/deploy-pmem/iops-gains.png)

O hardware de teste era um cluster de 12 servidores que foi configurado para usar o espelhamento triplo e volumes ReFS delimitados **, 12** x Intel® S2600WFT **, 384 GiB** de memória, 2 x 28-Core "CASCADELAKE", **1,5 TB** Intel® Optane™ memória persistente de DC como cache, **32 TB** NVME (4 x 8 TB Intel® DC P4510) como capacidade, **2** x Mellanox ConnectX-4 25 Gbps.

A tabela a seguir mostra os números de desempenho completos.  

| Comparação                   | Desempenho         |
|-----------------------------|---------------------|
| leitura aleatória de 4K 100%         | IOPS de 13,8 milhões   |
| leitura/gravação aleatória de 4K 90/10% | IOPS de 9.450.000   |
| leitura sequencial de 2 MB         | taxa de transferência de 549 GB/s |

### <a name="supported-hardware"></a>Hardware com suporte

A tabela a seguir mostra o hardware de memória persistente com suporte para o Windows Server 2019 e o Windows Server 2016.  

| Tecnologia de memória persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** no modo persistente                                  | Com suporte                | Com suporte                |
| **Intel Optane™ memória persistente de DC** no modo direto do aplicativo             | Sem suporte            | Com suporte                |
| **Intel Optane™ memória persistente de DC** no modo de memória | Com suporte            | Com suporte                |

> [!NOTE]  
> O Intel Optane dá suporte aos modos de *memória* (volátil) e *direto do aplicativo* (persistente).
   
> [!NOTE]  
> Quando você reinicia um sistema que tem vários módulos PMem do Intel® Optane™ no modo de aplicativo direto que são divididos em vários namespaces, você pode perder o acesso a alguns ou todos os discos de armazenamento lógicos relacionados. Esse problema ocorre em versões do Windows Server 2019 anteriores à versão 1903.
>   
> Essa perda de acesso ocorre porque um módulo PMem é não treinado ou, de outra forma, falha quando o sistema é iniciado. Nesse caso, todos os namespaces de armazenamento em qualquer módulo PMem no sistema falham, incluindo namespaces que não são mapeados fisicamente para o módulo com falha.
>   
> Para restaurar o acesso a todos os namespaces, substitua o módulo com falha.
>   
> Se um módulo falhar no Windows Server 2019 versão 1903 ou em versões mais recentes, você perderá o acesso somente a namespaces que são mapeados fisicamente para o módulo afetado. Outros namespaces não são afetados.

Agora, vamos nos aprofundar em como você configura a memória persistente.

## <a name="interleaved-sets"></a>Conjuntos intercalados

### <a name="understanding-interleaved-sets"></a>Noções básicas sobre conjuntos intercalados

Lembre-se de que um NVDIMM reside em um slot de DIMM (memória) padrão, que coloca os dados mais próximos do processador. Essa configuração reduz a latência e melhora o desempenho de busca. Para aumentar ainda mais a taxa de transferência, dois ou mais NVDIMMs criam um conjunto intercalado de n vias para distribuir operações de leitura/gravação. As configurações mais comuns são intercalação bidirecional ou de quatro vias. Um conjunto intercalado também faz com que vários dispositivos de memória persistente apareçam como um único disco lógico para o Windows Server. Você pode usar o cmdlet **Get-PmemDisk** do Windows PowerShell para examinar a configuração desses discos lógicos, da seguinte maneira:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que o disco PMem lógico #2 usa os dispositivos físicos Id20 e Id120, e o disco PMem lógico #3 usa os dispositivos físicos Id1020 e Id1120.  

Para recuperar mais informações sobre o conjunto intercalado usado por uma unidade lógica, execute o cmdlet **Get-PmemPhysicalDevice** :

```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleaved-sets"></a>Configurando conjuntos intercalados

Para configurar um conjunto intercalado, comece revisando todas as regiões de memória persistente que não estão atribuídas a um disco PMem lógico no sistema. Para fazer isso, execute o seguinte cmdlet do PowerShell:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Para ver todas as informações do dispositivo PMem no sistema, incluindo tipo de dispositivo, local, integridade e status operacional, e assim por diante, execute o seguinte cmdlet no servidor local:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Como temos uma região PMem não usada disponível, podemos criar novos discos de memória persistente. Podemos usar a região não usada para criar vários discos de memória persistente executando os seguintes cmdlets:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Depois que isso for feito, podemos ver os resultados executando:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale a pena observar que podemos executar **Get-PhysicalDisk | Em que o PmemDisk de MediaType-EQ** em vez de **Get-** para obter os mesmos resultados. O disco PMem recém-criado corresponde a um para um com unidades que aparecem no PowerShell e no centro de administração do Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Usando memória persistente para cache ou capacidade

O Espaços de Armazenamento Diretos no Windows Server 2019 dá suporte ao uso de memória persistente como um cache ou uma unidade de capacidade. Para obter mais informações sobre como configurar unidades de cache e capacidade, consulte [noções básicas sobre o cache em espaços de armazenamento diretos](understand-the-cache.md).

## <a name="creating-a-dax-volume"></a>Criando um volume DAX

### <a name="understanding-dax"></a>Compreendendo o DAX

Há dois métodos para acessar a memória persistente. São eles:

1. **Acesso direto (DAX)** , que opera como memória para obter a menor latência. O aplicativo modifica diretamente a memória persistente, ignorando a pilha. Observe que você só pode usar DAX em combinação com NTFS.
1. **Bloquear o acesso**, que funciona como armazenamento para compatibilidade de aplicativos. Neste configuração, os dados fluem pela pilha. Você pode usar essa configuração em combinação com NTFS e ReFS.

A figura a seguir mostra um exemplo de uma configuração DAX:

![Pilha DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurando DAX

Precisamos usar os cmdlets do PowerShell para criar um volume DAX em um disco de memória persistente. Usando a opção **-IsDax** , podemos Formatar um volume para que seja habilitado para Dax.

```PowerShell
Format-Volume -IsDax:$true
```

O trecho de código a seguir ajuda a criar um volume DAX em um disco de memória persistente.

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>Monitorando a integridade

Quando você usa a memória persistente, há algumas diferenças na experiência de monitoramento:

- A memória persistente não cria contadores de desempenho de disco físico, portanto, você não verá que ele aparece em gráficos no centro de administração do Windows.
- A memória persistente não cria dados Storport 505, portanto, você não obterá detecção de exceção proativa.

Além disso, a experiência de monitoramento é a mesma para qualquer outro disco físico. Você pode consultar a integridade de um disco de memória persistente executando os seguintes cmdlets:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus** mostra se o disco PMem está íntegro.  

O valor **UnsafeshutdownCount** controla o número de desligamentos que podem causar perda de dados nesse disco lógico. É a soma das contagens de desligamento não seguras de todos os dispositivos PMem subjacentes deste disco. Para obter mais informações sobre o status de integridade, use o cmdlet **Get-PmemPhysicalDevice** para encontrar informações como **OperationalStatus**.

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Esse cmdlet mostra qual dispositivo de memória persistente não está íntegro. O dispositivo não íntegro (**DeviceID** 20) corresponde ao caso no exemplo anterior. O **PhysicalLocation** no BIOS pode ajudar a identificar qual dispositivo de memória persistente está em estado de falha.

## <a name="replacing-persistent-memory"></a>Substituindo a memória persistente

Este artigo descreve como exibir o status de integridade da sua memória persistente. Se você precisar substituir um módulo com falha, será necessário provisionar novamente o disco PMem (consulte as etapas descritas anteriormente).

Ao solucionar problemas, talvez seja necessário usar **Remove-PmemDisk**. Esse cmdlet Remove um disco de memória persistente específico. Podemos remover todos os discos PMem atuais executando os seguintes cmdlets:

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

> [!IMPORTANT]  
> A remoção de um disco de memória persistente causa a perda de dados nesse disco.

Outro cmdlet que você pode precisar é **Initialize-PmemPhysicalDevice**. Esse cmdlet inicializa as áreas de armazenamento de rótulo nos dispositivos de memória persistente física e pode limpar as informações de armazenamento de rótulo corrompidas nos dispositivos PMem.

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

> [!IMPORTANT]  
> **Initialize-PmemPhysicalDevice** causa a perda de dados na memória persistente. Use-o como um último recurso para corrigir problemas persistentes relacionados à memória.

## <a name="see-also"></a>Veja também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Gerenciamento de integridade de memória de classe de armazenamento (NVDIMM-N) no Windows](storage-class-memory-health.md)
- [Noções básicas sobre o cache](understand-the-cache.md)
