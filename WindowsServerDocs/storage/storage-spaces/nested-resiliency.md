---
title: Resiliência aninhada para Espaços de Armazenamento Diretos
ms.author: jgerend
manager: dansimpspaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: 91d8cce64088855d2e8a0c89c1084077a252e26a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935952"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resiliência aninhada para Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019

A resiliência aninhada é um novo recurso de [espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server 2019 que permite que um cluster de dois servidores resista a várias falhas de hardware ao mesmo tempo sem perda de disponibilidade de armazenamento, de modo que os usuários, aplicativos e máquinas virtuais continuem a ser executados sem interrupções. Este tópico explica como ele funciona, fornece instruções passo a passo para começar e responde às perguntas mais frequentes.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="green-checkmark-icon-consider-nested-resiliency-if"></a>![Ícone de marca de seleção verde.](media/nested-resiliency/supported.png) Considere a resiliência aninhada se:

- O cluster executa o Windows Server 2019; e
- O cluster tem exatamente 2 nós de servidor

### <a name="red-x-icon-you-cant-use-nested-resiliency-if"></a>![Ícone de X vermelho.](media/nested-resiliency/unsupported.png) Você não poderá usar resiliência aninhada se:

- O cluster executa o Windows Server 2016; or
- O cluster tem três ou mais nós de servidor

## <a name="why-nested-resiliency"></a>Por que resiliência aninhada

Os volumes que usam resiliência aninhada podem **permanecer online e acessíveis mesmo que ocorram várias falhas de hardware ao mesmo tempo**, ao contrário da resiliência de [espelhamento bidirecional de duas vias](storage-spaces-fault-tolerance.md) . Por exemplo, se duas unidades falharem ao mesmo tempo ou se um servidor ficar inativo e uma unidade falhar, os volumes que usam resiliência aninhada permanecerão online e acessíveis. Para a infraestrutura hiperconvergente, isso aumenta o tempo de atividade para aplicativos e máquinas virtuais; para cargas de trabalho de servidor de arquivos, isso significa que os usuários aproveitam o acesso ininterrupto aos seus arquivos.

![Disponibilidade de armazenamento](media/nested-resiliency/storage-availability.png)

A desvantagem é que a resiliência aninhada tem **eficiência de capacidade menor do que o espelhamento bidirecional clássico**, o que significa que você obtém um pouco menos de espaço utilizável. Para obter detalhes, consulte a seção [eficiência da capacidade](#capacity-efficiency) abaixo.

## <a name="how-it-works"></a>Como ele funciona

### <a name="inspiration-raid-51"></a>Inspiração: RAID 5 + 1

O RAID 5 + 1 é uma forma estabelecida de resiliência de armazenamento distribuído que fornece um plano de fundo útil para entender a resiliência aninhada. No RAID 5 + 1, em cada servidor, a resiliência local é fornecida por RAID-5, ou *paridade única*, para proteger contra a perda de qualquer unidade única. Em seguida, a resiliência adicional é fornecida por RAID-1 ou *espelhamento bidirecional*, entre os dois servidores para proteger contra a perda de qualquer um dos servidores.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Duas novas opções de resiliência

Espaços de Armazenamento Diretos no Windows Server 2019 oferece duas novas opções de resiliência implementadas no software, sem a necessidade de hardware RAID especializado:

- **Espelho bidirecional aninhado.** Em cada servidor, a resiliência local é fornecida pelo espelhamento bidirecional e, em seguida, a resiliência é fornecida pelo espelhamento bidirecional entre os dois servidores. Ele é essencialmente um espelho de quatro vias, com duas cópias em cada servidor. O espelhamento bidirecional aninhado fornece desempenho absoluto: gravações vão para todas as cópias e as leituras são provenientes de qualquer cópia.

  ![Espelho bidirecional aninhado](media/nested-resiliency/nested-two-way-mirror.png)

- **Paridade com aceleração de espelhamento aninhada.** Combine espelhamento bidirecional aninhado, acima, com paridade aninhada. Dentro de cada servidor, a resiliência local para a maioria dos dados é fornecida por uma [aritmética de paridade de bit](storage-spaces-fault-tolerance.md#parity)único, exceto por novas gravações recentes que usam o espelhamento bidirecional. Em seguida, a resiliência adicional para todos os dados é fornecida pelo espelhamento bidirecional entre os servidores. Para obter mais informações sobre como funciona a paridade acelerada por espelhamento, consulte [paridade acelerada por espelhamento](../refs/mirror-accelerated-parity.md).

  ![Espelhamento aninhado-paridade acelerada](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Eficiência da capacidade

A eficiência da capacidade é a taxa de espaço utilizável para a [superfície do volume](plan-volumes.md#choosing-the-size-of-volumes). Ele descreve a sobrecarga de capacidade que pode ser atribuída à resiliência e depende da opção de resiliência que você escolher. Como um exemplo simples, o armazenamento de dados sem resiliência é de 100% de capacidade eficiente (1 TB de dados ocupa 1 TB de capacidade de armazenamento físico), enquanto o espelhamento bidirecional é de 50% eficiente (1 TB de dados ocupa 2 TB de capacidade de armazenamento físico).

- O **espelho bidirecional aninhado** grava quatro cópias de tudo, o que significa armazenar 1 TB de dados, você precisa de 4 TB de capacidade de armazenamento físico. Embora sua simplicidade seja atraente, a eficiência da capacidade do espelho bidirecional aninhado de 25% é a mais baixa de qualquer opção de resiliência em Espaços de Armazenamento Diretos.

- A **paridade com aceleração de espelhamento aninhada** atinge mais eficiência de capacidade, cerca de 35% a 40%, que depende de dois fatores: o número de unidades de capacidade em cada servidor e a combinação de espelhamento e paridade que você especifica para o volume. Esta tabela fornece uma pesquisa para configurações comuns:

  | Unidades de capacidade por servidor | espelho de 10% | espelho de 20% | espelho de 30% |
  |----------------------------|------------|------------|------------|
  | 4                          | 35,7%      | 34,1%      | 32,6%      |
  | 5                          | 37,7%      | 35,7%      | 33,9%      |
  | 6                          | 39,1%      | 36,8%      | 34,7%      |
  | 7+                         | 40,0%      | 37,5%      | 35,3%      |

  > [!NOTE]
  > **Se você estiver curioso, aqui está um exemplo da matemática completa.** Suponha que tenhamos seis unidades de capacidade em cada um dos dois servidores e desejamos criar um volume de 1 100 GB composto de 10 GB de espelho e 90 GB de paridade. O espelho bidirecional local do servidor é de 50,0% eficiente, o que significa que os 10 GB de dados de espelho levam 20 GB para serem armazenados em cada servidor. Espelhado em ambos os servidores, seu espaço total é de 40 GB. A paridade única local do servidor, nesse caso, é 5/6 = 83,3% eficiente, o que significa que o 90 GB de dados de paridade leva 108 GB para armazenar em cada servidor. Espelhado em ambos os servidores, seu espaço total é de 216 GB. A superfície total é, portanto, [(10 GB/50,0%) + (90 GB/83,3%)] × 2 = 256 GB, para a eficiência da capacidade geral de 39,1%.

Observe que a eficiência da capacidade do espelhamento bidirecional clássico (cerca de 50%) e paridade com aceleração de espelhamento aninhado (até 40%) Não são muito diferentes. Dependendo dos seus requisitos, a eficiência de capacidade ligeiramente menor pode ser um aumento significativo na disponibilidade do armazenamento. Você escolhe resiliência por volume, para que possa misturar volumes de resiliência aninhados e volumes de espelho bidirecionais clássicos no mesmo cluster.

![Relações](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Uso no PowerShell

Você pode usar cmdlets de armazenamento conhecidos no PowerShell para criar volumes com resiliência aninhada.

### <a name="step-1-create-storage-tier-templates"></a>Etapa 1: criar modelos de camada de armazenamento

Primeiro, crie novos modelos de camada de armazenamento usando o `New-StorageTier` cmdlet. Você só precisa fazer isso uma vez e, em seguida, todos os novos volumes criados podem fazer referência a esse modelo. Especifique a `-MediaType` de suas unidades de capacidade e, opcionalmente, a `-FriendlyName` de sua escolha. Não modifique os outros parâmetros.

Se suas unidades de capacidade forem unidades de disco rígido (HDD), inicie o PowerShell como administrador e execute:

```PowerShell
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk
```

Se suas unidades de capacidade forem unidades de estado sólido (SSD), defina `-MediaType` como `SSD` em vez disso. Não modifique os outros parâmetros.

> [!TIP]
> Verifique se as camadas foram criadas com êxito com `Get-StorageTier` .

### <a name="step-2-create-volumes"></a>Etapa 2: criar volumes

Em seguida, crie novos volumes usando o `New-Volume` cmdlet.

#### <a name="nested-two-way-mirror"></a>Espelho bidirecional aninhado

Para usar o espelhamento bidirecional aninhado, faça referência ao `NestedMirror` modelo de camada e especifique o tamanho. Por exemplo:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Espelhamento aninhado-paridade acelerada

Para usar a paridade com aceleração de espelho aninhado, referencie os `NestedMirror` modelos de camada e e `NestedParity` especifique dois tamanhos, um para cada parte do volume (espelho primeiro, paridade segundo). Por exemplo, para criar um volume de 1 500 GB que seja de 20% de espelho bidirecional aninhado e 80% de paridade aninhada, execute:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Etapa 3: continuar no centro de administração do Windows

Os volumes que usam resiliência aninhada aparecem no [centro de administração do Windows](../../manage/windows-admin-center/overview.md) com rotulagem clara, como na captura de tela abaixo. Depois que eles forem criados, você poderá gerenciá-los e monitorá-los usando o centro de administração do Windows, assim como qualquer outro volume no Espaços de Armazenamento Diretos.

![Gerenciamento de volume no centro de administração do Windows](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Opcional: estender para unidades de cache

Com suas configurações padrão, a resiliência aninhada protege contra a perda de várias unidades de capacidade ao mesmo tempo, ou um servidor e uma unidade de capacidade ao mesmo tempo. Para estender essa proteção às [unidades de cache](understand-the-cache.md) há uma consideração adicional: como as unidades de cache geralmente fornecem cache de leitura *e gravação* para *várias* unidades de capacidade, a única maneira de garantir que você possa tolerar a perda de uma unidade de cache quando o outro servidor está inativo é simplesmente não armazenar em cache gravações, mas isso afeta o desempenho.

Para resolver esse cenário, o Espaços de Armazenamento Diretos oferece a opção de desabilitar automaticamente o cache de gravação quando um servidor em um cluster de dois servidores estiver inoperante e reabilitar o cache de gravação depois que o servidor for revertido. Para permitir reinicializações rotineiras sem impacto no desempenho, o cache de gravação não é desabilitado até que o servidor fique inoperante por 30 minutos. Quando o cache de gravação está desabilitado, o conteúdo do cache de gravação é gravado em dispositivos de capacidade. Depois disso, o servidor pode tolerar um dispositivo de cache com falha no servidor online, embora as leituras do cache possam ser atrasadas ou falhem se um dispositivo de cache falhar.

Para definir esse comportamento (opcional), inicie o PowerShell como administrador e execute:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Uma vez definido como **true**, o comportamento do cache é:

| Situação                       | Comportamento do cache                           | Pode tolerar a perda da unidade de cache? |
|---------------------------------|------------------------------------------|--------------------------------|
| Ambos os servidores                 | Leituras e gravações de cache, desempenho completo | Sim                            |
| Servidor inativo, primeiros 30 minutos   | Leituras e gravações de cache, desempenho completo | Não (temporariamente)               |
| Após os primeiros 30 minutos          | Somente leituras de cache, desempenho afetado   | Sim (depois que o cache tiver sido gravado em unidades de capacidade)                           |

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>Posso converter um volume existente entre o espelho bidirecional e a resiliência aninhada?

Não, os volumes não podem ser convertidos entre os tipos de resiliência. Para novas implantações no Windows Server 2019, decida antecipadamente qual tipo de resiliência melhor se adapta às suas necessidades. Se estiver atualizando do Windows Server 2016, você poderá criar novos volumes com resiliência aninhada, migrar seus dados e, em seguida, excluir os volumes mais antigos.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>Posso usar resiliência aninhada com vários tipos de unidades de capacidade?

Sim, basta especificar o `-MediaType` de cada camada adequadamente durante a [etapa 1](#step-1-create-storage-tier-templates) acima. Por exemplo, com NVMe, SSD e HDD no mesmo cluster, o NVMe fornece o cache enquanto os dois últimos fornecem capacidade: defina a `NestedMirror` camada como `-MediaType SSD` e a `NestedParity` camada como `-MediaType HDD` . Nesse caso, observe que a eficiência da capacidade de paridade depende do número de unidades de HDD apenas, e você precisa de pelo menos 4 delas por servidor.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>Posso usar resiliência aninhada com três ou mais servidores?

Não, use somente resiliência aninhada se o cluster tiver exatamente dois servidores.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>Quantas unidades eu preciso para usar a resiliência aninhada?

O número mínimo de unidades necessárias para Espaços de Armazenamento Diretos é de 4 unidades de capacidade por nó de servidor, mais 2 unidades de cache por nó de servidor (se houver). Isso não é alterado no Windows Server 2016. Não há nenhum requisito adicional para resiliência aninhada e a recomendação para a capacidade de reserva também é inalterada.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>A resiliência aninhada altera como funciona a substituição da unidade?

Não.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>A resiliência aninhada altera como funciona a substituição de nó de servidor?

Não. Para substituir um nó de servidor e suas unidades, siga esta ordem:

1. Desativar as unidades no servidor de saída
2. Adicionar o novo servidor, com suas unidades, ao cluster
3. O pool de armazenamento será Rebalanceado
4. Remover o servidor de saída e suas unidades

Para obter detalhes, consulte o tópico [remover servidores](remove-servers.md) .

## <a name="additional-references"></a>Referências adicionais

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Entender a tolerância a falhas no Espaços de Armazenamento Diretos](storage-spaces-fault-tolerance.md)
- [Planejar volumes em Espaços de Armazenamento Diretos](plan-volumes.md)
- [Criar volumes em Espaços de Armazenamento Diretos](create-volumes.md)
