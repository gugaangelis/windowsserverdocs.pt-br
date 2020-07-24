---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: Adicionando servidores ou unidades a Espaços de Armazenamento Diretos
ms.prod: windows-server
ms.author: cosdar
manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: Como adicionar servidores ou unidades a um cluster Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 773bb3a55de27d049d26fa76659d3a4d8057f0fe
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966388"
---
# <a name="adding-servers-or-drives-to-storage-spaces-direct"></a>Adicionando servidores ou unidades a Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve como adicionar servidores ou unidades a Espaços de Armazenamento Diretos.

## <a name="adding-servers"></a><a name="adding-servers"></a>Adicionando servidores

A adição de servidores, geralmente chamada de expansão horizontal, adiciona capacidade de armazenamento e pode melhorar o desempenho e a eficiência do armazenamento. Se sua implantação for hiperconvergente, adicionar servidores também fornecerá mais recursos de computação para sua carga de trabalho.

![Animação da adição de um servidor a um cluster de quatro nós](media/add-nodes/Scaling-Out.gif)

As implantações típicas são simples de serem escaladas horizontalmente adicionando servidores: Existem apenas duas etapas:

1. Execute o [assistente de validação de cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) usando o snap-in Cluster de Failover ou com o cmdlet **Test-Cluster** no PowerShell (executar como Administrador). Inclua o novo servidor *\<NewNode>* que você deseja adicionar.

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   Isso confirma que o novo servidor está executando o Windows Server 2016 Datacenter Edition, que ele ingressou no mesmo domínio do Active Directory Domain Services dos servidores existentes, que ele tem todas as funções e recursos necessários e que a rede dele foi configurada corretamente.

   >[!IMPORTANT]
   > Se você estiver reutilizando unidades que contêm dados antigos ou metadados desnecessários, apague-os usando o **Gerenciamento de Disco** ou o cmdlet **Reset-PhysicalDisk**. Se dados ou metadados antigos forem detectados, as unidades não serão colocadas em pool.

2. Execute o seguinte cmdlet no cluster para concluir a adição do servidor:

```
Add-ClusterNode -Name NewNode 
```

   >[!NOTE]
   > O pooling automático depende de você ter apenas um pool. Se você tiver burlado a configuração padrão para criar vários pools, será necessário adicionar novas unidades ao seu pool preferencial usando **Add-PhysicalDisk**.

### <a name="from-2-to-3-servers-unlocking-three-way-mirroring"></a>De 2 a 3 servidores: desbloqueando o espelhamento de três vias

![adicionando um terceiro servidor a um cluster de dois nós](media/add-nodes/Scaling-2-to-3.png)

Com dois servidores, você só pode criar volumes espelhados bidirecionais (em comparação ao RAID-1 distribuído). Com três servidores, você pode criar volumes espelhados em três vias para melhor tolerância a falhas. É recomendável usar o espelhamento de três vias, sempre que possível.

Volumes espelhados bidirecionais não podem ser atualizados localmente para o espelhamento de três vias. Em vez disso, você pode criar um novo volume e migrar (copiar, usando a [Réplica de Armazenamento](../storage-replica/server-to-server-storage-replication.md)) seus dados para ele e, em seguida, remover o volume antigo.

Para começar a criar volumes espelhados de três vias, você tem várias boas opções. Você pode usar a que você preferir. 

#### <a name="option-1"></a>Opção 1

Especifique **PhysicalDiskRedundancy = 2** em cada novo volume após a criação.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### <a name="option-2"></a>Opção 2

Em vez disso, você pode definir **PhysicalDiskRedundancyDefault = 2** no objeto **ResiliencySetting** chamado **Mirror** do pool. Depois, os novos volumes espelhados usarão automaticamente o espelhamento de *três vias*, mesmo se você não especificá-lo.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### <a name="option-3"></a>Opção 3

Defina **PhysicalDiskRedundancy = 2** no modelo **StorageTier** chamado *Capacity* e, em seguida, crie volumes fazendo referência à camada.

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### <a name="from-3-to-4-servers-unlocking-dual-parity"></a>De 3 a 4 servidores: desbloqueando a paridade dupla

![adicionando um quarto servidor a um cluster de três nós](media/add-nodes/Scaling-3-to-4.png)

Com quatro servidores, você pode usar a paridade dupla, também conhecida comumente como codificação de eliminação (compare com o RAID-6 distribuído). Isso fornece a mesmo tolerância a falhas como espelhamento de três vias, mas com melhor eficiência de armazenamento. Para saber mais, consulte [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md).

Se você está vindo de uma implantação menor, há várias boas opções para começar a criar volumes de paridade dupla. Você pode usar a que você preferir.

#### <a name="option-1"></a>Opção 1

Especifique **PhysicalDiskRedundancy = 2** e **ResiliencySettingName = Parity** em cada novo volume na criação.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### <a name="option-2"></a>Opção 2

Defina **PhysicalDiskRedundancy = 2** no objeto **ResiliencySetting** chamado **Parity** do pool. Depois, os novos volumes de paridade usarão automaticamente a paridade *dupla*, mesmo se você não especificá-la

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

Com quatro servidores, você também pode começar a usar paridade com aceleração de espelho, em que um volume individual é parte espelho, parte paridade.

Para isso, você precisará atualizar seus modelos **StorageTier** para ter duas camadas *Performance* e *Capacity*, uma vez que elas seriam criadas se você tivesse executado primeiro **Enable-ClusterS2D** em quatro servidores. Especificamente, ambas as camadas devem ter **MediaType** de seus dispositivos de capacidade (como SSD ou HDD) e **PhysicalDiskRedundancy = 2**. A camada *Desempenho* deve ser **ResiliencySettingName = Mirror** e a camada *Capacidade* deve ser **ResiliencySettingName = Parity**.

#### <a name="option-3"></a>Opção 3

Você pode achar mais fácil simplesmente remover o modelo de camada existente e criar dois novos. Isso não afetará os volumes pré-existentes que foram criados referenciando o modelo de camada: trata-se apenas de um modelo.

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

É isso! Agora você está pronto para criar volumes de paridade acelerada por espelho referenciando esses modelos de camada.

#### <a name="example"></a>Exemplo

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### <a name="beyond-4-servers-greater-parity-efficiency"></a>Mais de 4 servidores: maior eficiência de paridade

Enquanto você dimensiona mais de quatro servidores, novos volumes podem se beneficiar de uma eficiência de codificação de paridade ainda maior. Por exemplo, entre seis e sete servidores, a eficiência melhora de 50% a 66,7%, pois é possível usar o Reed-Solomon 4+2 (em vez de 2+2). Não há etapas que você precisa seguir para começar a aproveitar essa nova eficiência; a melhor codificação possível é determinada automaticamente sempre que você cria um volume.

No entanto, os volumes pré-existentes *não* serão "convertidos" para a nova codificação mais ampla. Um bom motivo é que fazer isso exigiria um cálculo maciço que afeta literalmente *cada bit* em toda a implantação. Se você quiser que dados pré-existentes sejam codificados com a maior eficiência, você poderá migrá-los para novos volumes.

Para obter mais detalhes, consulte [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md).

### <a name="adding-servers-when-using-chassis-or-rack-fault-tolerance"></a>Adicionando servidores ao usar a tolerância a falhas em chassi ou rack

Se sua implantação usar a tolerância a falhas em chassi ou rack, você deverá especificar o chassi ou o rack dos novos servidores antes de adicioná-los ao cluster. Isso informa aos Espaços de Armazenamento Diretos a melhor maneira de distribuir dados para maximizar a tolerância a falhas.

1. Crie um domínio de falha temporário para o nó abrindo uma sessão do PowerShell com privilégios elevados e, em seguida, usando o comando a seguir, em que *\<NewNode>* é o nome do novo nó de cluster:

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. Mova esse domínio de falha temporário para o chassi ou rack onde o novo servidor está localizado no mundo real, conforme especificado por *\<ParentName>* :

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   Para obter mais informações, consulte [Fault domain awareness in Windows Server 2016](../../failover-clustering/fault-domains.md) (Reconhecimento de domínio de falha no Windows Server 2016).

3. Adicione o servidor ao cluster conforme descrito em [Adicionando servidores](#adding-servers). Quando o novo servidor ingressa no cluster, ele é automaticamente associado (usando seu nome) ao domínio de falha do espaço reservado.

## <a name="adding-drives"></a><a name="adding-drives"></a>Adicionando unidades

A adição de unidades, também conhecida como expansão vertical, adiciona capacidade de armazenamento e pode melhorar o desempenho. Se você tiver slots disponíveis, você poderá adicionar unidades a cada servidor para expandir a capacidade de armazenamento sem adicionar servidores. Você pode adicionar unidades de cache ou de capacidade independentemente a qualquer momento.

   >[!IMPORTANT]
   > É altamente recomendável que todos os servidores tenham configurações de armazenamento idênticas.

![Animação mostrando a adição de unidades a um sistema](media/add-nodes/Scale-Up.gif)

Para escalar verticalmente, conecte as unidades e verifique se o Windows as detectará. Elas devem aparecer na saída do cmdlet **Get-PhysicalDisk** no PowerShell com a propriedade **CanPool** definida como **True**. Se elas forem mostradas como **CanPool = False**, você poderá ver o motivo verificando a propriedade **CannotPoolReason**.

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

Dentro de um curto período, as unidades qualificadas serão automaticamente reivindicadas por Espaços de Armazenamento Diretos, adicionadas ao pool de armazenamento e os volumes serão automaticamente [redistribuídos uniformemente em todas as unidades](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959). Neste ponto, você concluiu e está pronto para [estender os volumes](resize-volumes.md) ou [criar novos](create-volumes.md).

Se as unidades não aparecerem, verifique manualmente se há alterações de hardware. Isso pode ser feito usando o **Gerenciador de Dispositivos** no menu **Ação**. Se eles contiverem dados antigos ou metadados, considere reformatá-los. Isso pode ser feito usando o **Gerenciamento de Disco** ou com o cmdlet **Reset-PhysicalDisk**.

   >[!NOTE]
   > O pooling automático depende de você ter apenas um pool. Se você tiver burlado a configuração padrão para criar vários pools, será necessário adicionar novas unidades ao seu pool preferencial usando **Add-PhysicalDisk**.

## <a name="optimizing-drive-usage-after-adding-drives-or-servers"></a>Otimizando o uso da unidade depois de adicionar unidades ou servidores

Ao longo do tempo, à medida que as unidades são adicionadas ou removidas, a distribuição de dados entre as unidades no pool pode se tornar desigual. Em alguns casos, isso pode resultar em algumas unidades ficando cheias enquanto outras unidades no pool têm um consumo muito menor.

Para ajudar a manter a alocação de unidade mesmo em todo o pool, Espaços de Armazenamento Diretos otimiza automaticamente o uso da unidade depois de adicionar unidades ou servidores ao pool (esse é um processo manual para sistemas de espaços de armazenamento que usam compartimentos SAS compartilhados). A otimização inicia 15 minutos depois que você adiciona uma nova unidade ao pool. A otimização de pool é executada como uma operação em segundo plano de baixa prioridade, de modo que pode levar horas ou dias para ser concluída, especialmente se você estiver usando discos rígidos grandes.

A otimização usa dois trabalhos – um chamado *Optimize* e outro chamado *rebalance* -e você pode monitorar seu progresso com o seguinte comando:

```powershell
Get-StorageJob
```

Você pode otimizar manualmente um pool de armazenamento com o cmdlet [Optimize-StoragePool](/powershell/module/storage/optimize-storagepool?view=win10-ps) . Este é um exemplo:

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
