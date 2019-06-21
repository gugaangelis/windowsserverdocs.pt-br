---
title: Compreender e implantar memória persistente
description: Informações detalhadas sobre a qual memória persistente é e como configurá-lo com espaços de armazenamento diretos no Windows Server 2019.
keywords: Espaços de armazenamento diretos, memória persistente, pmem, armazenamento, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280042"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Compreender e implantar memória persistente

>Aplica-se a: Windows Server 2019

Memória persistente (ou PMem) é um novo tipo de tecnologia de memória que oferece uma combinação exclusiva de grande capacidade acessível e persistência. Este tópico fornece informações sobre PMem e as etapas para implantá-lo com o Windows Server 2019 com espaços de armazenamento diretos.

## <a name="background"></a>Histórico

PMem é um tipo de não-volátil DRAM (NVDIMM) que tem a velocidade de DRAM, mas retém o conteúdo de memória por meio de ciclos de alimentação (o conteúdo da memória permanece mesmo quando a alimentação do sistema falhar em caso de uma queda de energia inesperada, o desligamento iniciado pelo usuário, a falha do sistema, etc.). Por isso, a retomar de onde você parou é significativamente mais rápida, pois o conteúdo de sua memória RAM não precisa ser recarregado. Outra característica exclusiva é PMem bytes endereçável, que significa que você também pode usá-lo como o armazenamento (motivo pelo qual você pode ouvir PMem que está sendo chamado de memória de classe de armazenamento).


Para ver alguns desses benefícios, vamos examinar esta demonstração do Microsoft Ignite 2018:

[![Demonstração do Microsoft Ignite 2018 Pmem](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Qualquer sistema de armazenamento que fornece tolerância a falhas necessariamente faz cópias distribuídas de gravações, que devem atravessar a rede e incorre em amplificação de gravação de back-end. Por esse motivo, os números de parâmetro de comparação IOPS maiores absolutos normalmente são atingidos com leituras apenas, especialmente se o sistema de armazenamento tem otimizações de senso comum para ler a partir da cópia local sempre que possível, faz quais espaços de armazenamento diretos.

**Com 100% de leituras, o cluster oferece 13,798,674 IOPS.**

![captura de tela Registro do IOPS 13.7m](media/deploy-pmem/iops-record.png)

Se você observar com atenção o vídeo, você observará ainda mais do thatwhat jaw soltar é a latência: até mesmo em mais de 13.7 IOPS de M, o sistema de arquivos no Windows está relatando latência que é consistente com menos de 40 microssegundos! (Que é o símbolo para microssegundos, um milionésimo de segundo). Isso é uma ordem de magnitude mais rapidamente do que o que os fornecedores de todos os flash típicos Orgulhosamente anunciam hoje mesmo.

Juntas, espaços de armazenamento diretos no Windows Server 2019 e memória persistente do controlador de domínio do Intel® Optane™ oferecer desempenho inovador. Nesse benchmark HCI líderes do setor de mais de M 13.7 IOPS, com latência extremamente baixa e previsível, é mais do que o dobro nosso anterior benchmark de líderes do setor de M 6,7 IOPS. O que é mais, desta vez precisávamos apenas 12 nós de servidor, 25% menos de dois anos atrás.

![Ganhos IOPS](media/deploy-pmem/iops-gains.png)

O hardware usado era um cluster de servidor 12 usando o espelhamento de três vias e delimitado por volumes de ReFS **12** x S2600WFT do Intel®, **GiB 384** memória, 2 x 28-core "CascadeLake" **1,5 TB**Memória persistente do controlador de domínio do Intel® Optane™ como cache, **32 TB** NVMe (4 x 8 P4510 TB Intel® DC), como capacidade **2** x Mellanox ConnectX-4 25 Gbps

A tabela a seguir tem os números de desempenho completo: 

| Parâmetro de comparação                   | Desempenho         |
|-----------------------------|---------------------|
| Leitura aleatória de 100% de 4K         | 13.8 milhão de IOPS   |
| 4 K 90/10% leitura/gravação aleatórias | 9.45 milhão de IOPS   |
| 2MB sequencial de leitura         | Taxa de transferência 549 GB/s |

### <a name="supported-hardware"></a>Hardware com suporte

A tabela a seguir mostra o hardware com suporte de memória persistente para 2019 do Windows Server e Windows Server 2016. Observe que Optane Intel especificamente dá suporte ao modo de memória e o modo de aplicativo direct. Windows Server 2019 dá suporte a operações de modo misto.

| Tecnologia de memória persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **O NVDIMM-N** no modo de aplicativo Direct                                       | Com suporte                | Com suporte                |
| **Memória persistente do Intel Optane™ DC** no modo de aplicativo Direct             | Sem suporte            | Com suporte                |
| **Memória persistente do Intel Optane™ DC** no modo de dois de nível de memória (2LM) | Sem suporte            | Com suporte                |

Agora, vamos nos aprofundar em como você configura a memória persistente.

## <a name="interleave-sets"></a>Conjuntos de intercalação

### <a name="understanding-interleave-sets"></a>Noções básicas sobre conjuntos de intercalação

Lembre-se de que o NVDIMM-N reside em um slot DIMM (memória) padrão, colocando dados mais próximos do processador (portanto, reduzindo a latência e buscando o melhor desempenho). Para criar a partir disso, um conjunto de intercalação é quando dois ou mais NVDIMMs cria uma intercalação de maneira N definida para fornecer operações de leitura/gravação de faixas para maior taxa de transferência. As configurações mais comuns são a maneira de 2 ou 4 vias de intercalação.

Conjuntos intercalados com frequência podem ser criados na BIOS de uma plataforma para fazer com que vários dispositivos de memória persistentes são exibidos como um único disco lógico para o Windows Server. Cada disco lógico de memória persistente contém um conjunto intercalado de dispositivos físicos, executando:

```PowerShell
Get-PmemDisk
```

Um exemplo de saída é mostrado abaixo:

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que o disco lógico pmem #2 tiver dispositivos físicos de Id20 e Id120 e disco lógico pmem #3 tem dispositivos físicos de Id1020 e Id1120. Também podemos pode alimentar um disco específico pmem para Get-PmemPhysicalDevice para obter todos os seus NVDIMMs físicos na intercalação definido como mostrado abaixo.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

Um exemplo de saída é mostrado abaixo:

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

Isso mostra todas as regiões de memória persistente não atribuídas a um lógica de memória persistente no disco do sistema.

Para ver todas as informações de dispositivos de memória persistente no sistema, incluindo o tipo de dispositivo, localização, integridade e o status operacional, etc. você pode executar o cmdlet a seguir no servidor local:

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

Como temos região pmem não utilizado disponível, podemos criar novos discos de memória persistente. Podemos criar vários discos de memória persistente usando as regiões não utilizadas por:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Após fazer isso, podemos ver os resultados executando:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Vale a pena observar que poderíamos ter executar **Get-PhysicalDisk | Onde MediaType - Eq SCM** em vez de **Get-PmemDisk** para obter os mesmos resultados. O disco recém-criado memória persistente corresponde 1:1 para unidades que aparecem no PowerShell e Windows Admin Center.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Usando memória persistente para o cache ou a capacidade

Espaços de armazenamento diretos no Windows Server 2019 aceita o uso de memória persistente como um cache ou a capacidade de unidade. Consulte este [documentação](understand-the-cache.md) para obter mais detalhes sobre como configurar unidades de cache e a capacidade.

## <a name="creating-a-dax-volume"></a>Criação de um Volume DAX

### <a name="understanding-dax"></a>Noções básicas sobre DAX

Há dois métodos para acessar a memória persistente. São eles:

1. **Acesso (DAX) direto**, que funciona como a memória para obter a menor latência. O aplicativo modifica diretamente a memória persistente, ignorando a pilha. Observe que isso só pode ser usado com o NTFS.
2. **Bloquear o acesso**, que opera como o armazenamento para fins de compatibilidade do aplicativo. Os dados fluem por meio da pilha nessa configuração, e isso pode ser usado com NTFS e ReFS.

Um exemplo disso pode ser visto abaixo:

![Pilha DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configurando o DAX

Precisamos usar cmdlets do PowerShell para criar um volume DAX em uma memória persistente. Usando o **IsDax -** switch, podemos pode formatar um volume para ser habilitado de DAX.

```PowerShell
Format-Volume -IsDax:$true
```

O trecho de código a seguir ajudará você a criar um volume DAX em um disco de memória persistente.

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

## <a name="monitoring-health"></a>Monitoramento de integridade

Quando você usa a memória persistente, existem algumas diferenças na experiência de monitoramento:

1. Memória persistente não cria o disco físico, contadores de desempenho, portanto, você não verá se são exibidos em gráficos no Windows Admin Center.
2. Memória persistente não cria dados 505 Storport, portanto, você não obterá a detecção de exceções proativo.

Além disso, a experiência de monitoramento é o mesmo que qualquer outro disco físico. Você pode consultar a integridade de um disco de memória persistente, executando:

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

**HealthStatus** mostra se o disco de memória persistente está íntegro ou não. O **UnsafeshutdownCount** rastreia o número de desligamentos que podem causar perda de dados nesse disco lógico. É a soma das contagens unsafe desligamento de todos os dispositivos de memória persistente subjacente desse disco. Também pode usar os comandos abaixo para informações de integridade de consulta. O **OperationalStatus** e **OperationalDetails** fornecem mais informações sobre o status de integridade.

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

Isso mostra qual dispositivo de memória persistente não está íntegro. O dispositivo não íntegro (**DeviceId**) 20 corresponde o caso no exemplo acima. O **PhysicalLocation** de BIOS pode ajudar a identificar qual memória persistente dispositivo estiver no estado com falha.

## <a name="replacing-persistent-memory"></a>Substituição de memória persistente

Acima, descrevemos como exibir o status de integridade de sua memória persistente. Se você precisar substituir um módulo com falha, você precisará provisionar novamente o disco de memória persistente (consulte as etapas, descritas acima).

Ao solucionar o problema, você talvez precise usar **PmemDisk remover**, que remove um disco de memória persistente específica. Podemos remover todos os discos persistentes atuais por:

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

É outro cmdlet que talvez seja necessário **PmemPhysicalDevice Initialize**, que irá inicializar a área de armazenamento de rótulo nos dispositivos físicos de memória persistente. Isso pode ser usado para limpar as informações de armazenamento de rótulo corrompido nos dispositivos de memória persistente.

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

É importante observar que esse comando deve ser usado como um último recurso para memória persistente de corrigir os problemas relacionados. Isso resultará em perda de dados na memória persistente.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Gerenciamento de integridade de memória (NVDIMM-N) de classe de armazenamento no Windows](storage-class-memory-health.md)
- [Noções básicas sobre o cache](understand-the-cache.md)