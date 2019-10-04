---
title: Entender e implantar a memória persistente
description: Informações detalhadas sobre o que é a memória persistente e como configurá-la com espaços de armazenamento diretos no Windows Server 2019.
keywords: Espaços de Armazenamento Diretos, memória persistente, pMem, armazenamento, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 549cc6dbeec3d414e886f6ebf32315ae13627812
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940804"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Entender e implantar a memória persistente

>Aplica-se a: Windows Server 2019

A memória persistente (ou PMem) é um novo tipo de tecnologia de memória que oferece uma combinação exclusiva de alta capacidade e persistência acessíveis. Este tópico fornece informações sobre o PMem e as etapas para implantá-lo com o Windows Server 2019 com o Espaços de Armazenamento Diretos.

## <a name="background"></a>Informações preliminares

O PMem é um tipo de memória RAM não volátil (NVDIMM) que mantém seu conteúdo por meio de ciclos de energia. O conteúdo da memória permanece mesmo quando a energia do sistema fica inativa em caso de perda de energia inesperada, desligamento iniciado pelo usuário, falha do sistema, etc. Essa característica exclusiva significa que você também pode usar o PMem como armazenamento – e é por isso que você pode ouvir o PMem ser chamado de "memória de classe de armazenamento".

Para ver alguns desses benefícios, vamos examinar a demonstração do Microsoft Ignite 2018:

[![Demonstração do pMem do Microsoft Ignite 2018](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualquer sistema de armazenamento que fornece tolerância a falhas necessariamente faz cópias distribuídas de gravações, que devem atravessar a rede e incorrer em amplificação de gravação de back-end. Por esse motivo, os números de benchmark de maior IOPS absolutos normalmente são obtidos somente com leituras, especialmente se o sistema de armazenamento tiver otimizações de senso comum para ler a partir da cópia local sempre que possível, o que Espaços de Armazenamento Diretos.

**Com 100% de leituras, o cluster oferece um IOPS de 13.798.674.**

![captura de tela de registro de IOPS de 13.7 m](media/deploy-pmem/iops-record.png)

Se você assistir ao vídeo de maneira mais minuciosa, perceberá que o thatwhat de Jaw é a latência: mesmo em mais de 13,7 milhões de IOPS, o sistema de arquivos no Windows está relatando a latência que é consistentemente menor que 40 μs! (Esse é o símbolo de microssegundos, um milionésimo de um segundo.) Essa é uma ordem de magnitude mais rápida do que os fornecedores típicos de todos os flash de forma orgulhada hoje.

Juntos, Espaços de Armazenamento Diretos no Windows Server 2019 e no Intel® Optane™ memória persistente de DC oferecem desempenho inovador. Esse benchmark de HCI líder do setor de mais de 13.7 M IOPS, com latência previsível e extremamente baixa, é mais do que dobrar nosso benchmark líder do setor de IOPS de 6.7 M. Além disso, desta vez precisávamos apenas 12 nós de servidor, 25% menos de dois anos atrás.

![Ganhos de IOPS](media/deploy-pmem/iops-gains.png)

O hardware usado era um cluster de 12 servidores usando espelhamento triplo e volumes ReFS delimitados, **12** x Intel® S2600WFT, **384 GiB** Memory, 2 x 28-Core "CASCADELAKE", **1,5 TB** Intel® Optane™ memória persistente de DC como cache, **32 TB** NVMe ( 4 x 8 TB Intel® DC P4510) como capacidade, **2** x Mellanox ConnectX-4 25 Gbps

A tabela a seguir tem os números de desempenho completos: 

| Comparação                   | Desempenho         |
|-----------------------------|---------------------|
| Leitura aleatória de 4K 100%         | IOPS de 13,8 milhões   |
| Leitura/gravação aleatória de 4K 90/10% | IOPS de 9.450.000   |
| Leitura sequencial de 2MB         | Taxa de transferência de 549 GB/s |

### <a name="supported-hardware"></a>Hardware com suporte

A tabela a seguir mostra o hardware de memória persistente com suporte para o Windows Server 2019 e o Windows Server 2016. Observe que o Intel Optane dá suporte à memória (ou seja, volátil) e ao aplicativo direto (ou seja, modo persistente).

| Tecnologia de memória persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** no modo persistente                                  | Suportado                | Suportado                |
| **Intel Optane™ memória persistente de DC** no modo direto do aplicativo             | Sem Suporte            | Suportado                |
| **Intel Optane™ memória persistente de DC** no modo de memória | Suportado            | Suportado                |

Agora, vamos nos aprofundar em como você configura a memória persistente.

## <a name="interleave-sets"></a>Conjuntos de intercalação

### <a name="understanding-interleave-sets"></a>Noções básicas sobre conjuntos de intercalação

Lembre-se de que um NVDIMM reside em um slot de DIMM (memória) padrão, colocando os dados mais próximos do processador (portanto, reduzindo a latência e buscando melhor desempenho). Para se basear nisso, um conjunto de intercalação é quando dois ou mais NVDIMMs criam uma intercalação de N vias definida para fornecer operações de leitura/gravação de listras para maior taxa de transferência. As configurações mais comuns são intercalações de 2 vias ou de 4 vias.

Conjuntos intercalados geralmente podem ser criados no BIOS de uma plataforma para fazer com que vários dispositivos de memória persistentes apareçam como um único disco lógico para o Windows Server. Cada disco lógico de memória persistente contém um conjunto intercalado de dispositivos físicos executando:

```PowerShell
Get-PmemDisk
```

Uma saída de exemplo é mostrada abaixo:

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que o disco pMem lógico #2 tem dispositivos físicos de Id20 e Id120 e disco pMem lógico #3 tem dispositivos físicos de Id1020 e Id1120. Também é possível alimentar um disco pMem específico para Get-PmemPhysicalDevice para obter todos os seus NVDIMMs físicos no conjunto de intercalação, como mostrado abaixo.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

Uma saída de exemplo é mostrada abaixo:

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configurando conjuntos de intercalação

Para configurar um conjunto de intercalação, execute o seguinte cmdlet do PowerShell:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Isso mostra todas as regiões de memória persistente não atribuídas a um disco de memória persistente lógica no sistema.

Para ver todas as informações de dispositivos de memória persistentes no sistema, incluindo tipo de dispositivo, local, integridade e status operacional, etc., você pode executar o seguinte cmdlet no servidor local:

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

Como temos a região pMem não utilizada, podemos criar novos discos de memória persistente. Podemos criar vários discos de memória persistente usando as regiões não utilizadas por:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Após isso é feito, podemos ver os resultados executando:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale a pena observar que poderíamos ter executado **Get-PhysicalDisk | Em que o PmemDisk de MediaType-EQ** em vez de **Get-** para obter os mesmos resultados. O disco de memória persistente recém-criado corresponde a 1:1 a unidades que aparecem no PowerShell e no centro de administração do Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Usando memória persistente para cache ou capacidade

O Espaços de Armazenamento Diretos no Windows Server 2019 dá suporte ao uso de memória persistente como uma unidade de cache ou capacidade. Consulte esta [documentação](understand-the-cache.md) para obter mais detalhes sobre como configurar unidades de cache e capacidade.

## <a name="creating-a-dax-volume"></a>Criando um volume DAX

### <a name="understanding-dax"></a>Compreendendo o DAX

Há dois métodos para acessar a memória persistente. São eles:

1. **Acesso direto (DAX)** , que opera como memória para obter a menor latência. O aplicativo modifica diretamente a memória persistente, ignorando a pilha. Observe que isso só pode ser usado com NTFS.
2. **Bloquear o acesso**, que funciona como armazenamento para compatibilidade de aplicativos. Os dados fluem pela pilha nessa configuração, e isso pode ser usado com NTFS e ReFS.

Um exemplo disso pode ser visto abaixo:

![Pilha DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurando DAX

Precisaremos usar os cmdlets do PowerShell para criar um volume DAX em uma memória persistente. Usando a opção **-IsDax** , podemos Formatar um volume para que seja habilitado para Dax.

```PowerShell
Format-Volume -IsDax:$true
```

O trecho de código a seguir o ajudará a criar um volume DAX em um disco de memória persistente.

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

1. A memória persistente não cria contadores de desempenho de disco físico, portanto, você não verá se aparecem em gráficos no centro de administração do Windows.
2. A memória persistente não cria dados Storport 505, portanto, você não obterá detecção de exceção proativa.

Além disso, a experiência de monitoramento é a mesma que qualquer outro disco físico. Você pode consultar a integridade de um disco de memória persistente executando:

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

**HealthStatus** mostra se o disco de memória persistente está íntegro ou não. O **UnsafeshutdownCount** rastreia o número de desligamentos que podem causar perda de dados nesse disco lógico. É a soma das contagens de desligamento não seguras de todos os dispositivos de memória persistente subjacentes deste disco. Também podemos usar os comandos abaixo para consultar informações de integridade. O **OperationalStatus** e o **OperationalDetails** fornecem mais informações sobre o status de integridade.

Para consultar a integridade do dispositivo de memória persistente:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Isso mostra qual dispositivo de memória persistente não está íntegro. O dispositivo não íntegro (**DeviceID**) 20 corresponde ao caso no exemplo acima. O **PhysicalLocation** do BIOS pode ajudar a identificar qual dispositivo de memória persistente está em estado de falha.

## <a name="replacing-persistent-memory"></a>Substituindo a memória persistente

Acima, descrevemos como exibir o status de integridade da sua memória persistente. Se você precisar substituir um módulo com falha, será necessário reprovisionar o disco de memória persistente (consulte as etapas descritas acima).

Ao solucionar problemas, talvez seja necessário usar **Remove-PmemDisk**, que remove um disco de memória persistente específico. Podemos remover todos os discos persistentes atuais:

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

É importante observar que a remoção de um disco de memória persistente resultará em perda de dados nesse disco.

Outro cmdlet que você pode precisar é **Initialize-PmemPhysicalDevice**, que inicializará a área de armazenamento de rótulo nos dispositivos de memória persistente física. Isso pode ser usado para limpar as informações de armazenamento de rótulo corrompidas nos dispositivos de memória persistente.

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

É importante observar que esse comando deve ser usado como último recurso para corrigir problemas relacionados à memória persistente. Isso resultará em perda de dados na memória persistente.

## <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Gerenciamento de integridade de memória de classe de armazenamento (NVDIMM-N) no Windows](storage-class-memory-health.md)
- [Noções básicas sobre o cache](understand-the-cache.md)
