---
title: Resiliência aninhada para espaços de armazenamento diretos
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879567"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>Resiliência aninhada para espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019

Resiliência aninhada é um novo recurso do [espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server 2019 que permite que um cluster de dois servidores para dar suporte a várias falhas de hardware ao mesmo tempo sem perda de disponibilidade de armazenamento, para que os usuários, aplicativos, e as máquinas virtuais continuarão sendo executados sem interrupção. Este tópico explica como ele funciona, fornece instruções passo a passo para começar a usar e respostas a perguntas mais frequentes.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![Ícone de marca de seleção verde.](media/nested-resiliency/supported.png) Considere aninhada resiliência se:

- O cluster esteja executando Windows Server de 2019; e
- O cluster tiver exatamente 2 nós de servidor

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![Ícone de X vermelho.](media/nested-resiliency/unsupported.png) Você não poderá usar resiliência aninhada se:

- O cluster esteja executando o Windows Server 2016; ou
- O cluster tiver 3 ou mais nós de servidor

## <a name="why-nested-resiliency"></a>Por que resiliência aninhada

Volumes que usam a resiliência aninhada podem **permanecer online e acessível, mesmo se acontecem de várias falhas de hardware ao mesmo tempo**, ao contrário do clássico [espelhamento bidirecional](storage-spaces-fault-tolerance.md) resiliência. Por exemplo, se duas unidades falharem ao mesmo tempo, ou se um servidor falhar e um disco falhar, volumes que usam a resiliência aninhada fique online e acessível. Para infraestrutura hiperconvergente, isso aumenta o tempo de atividade para aplicativos e máquinas virtuais; para cargas de trabalho de servidor de arquivo, isso significa que os usuários desfrutam de acesso ininterrupto aos seus arquivos.

![Disponibilidade de armazenamento](media/nested-resiliency/storage-availability.png)

A desvantagem é que resiliência aninhada tem **reduzir a eficiência da capacidade que o espelhamento bidirecional clássico**, que significa que você obtenha um pouco menos espaço utilizável. Para obter detalhes, consulte o [eficiência da capacidade](#capacity-efficiency) seção abaixo.

## <a name="how-it-works"></a>Como funciona

### <a name="inspiration-raid-51"></a>Inspiração: RAID 5 + 1

RAID 5 + 1 é um formato estabelecido da resiliência de armazenamento distribuído que fornece informações úteis para garantir a resiliência aninhada Noções básicas sobre. Em RAID 5 + 1, dentro de cada servidor, a resiliência local é fornecido pelo RAID-5, ou *único paridade*, para proteger contra a perda de qualquer unidade única. Em seguida, é ainda mais a resiliência é fornecida pelo RAID-1, ou *espelhamento bidirecional*, entre os dois servidores para proteger contra a perda de qualquer um dos servidores.

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>Duas novas opções de resiliência

Espaços de armazenamento diretos no Windows Server 2019 oferece duas novas opções de resiliência, implementadas no software, sem a necessidade de hardware especializado de RAID:

- **Espelhamento bidirecional aninhado.** Dentro de cada servidor, a resiliência local é fornecida pelo espelhamento bidirecional e, em seguida, a resiliência adicional é fornecida pelo espelhamento bidirecional entre os dois servidores. Ele é essencialmente um espelho de quatro vias, com duas cópias de cada servidor. Espelhamento bidirecional aninhada fornece desempenho de ponta: todas as cópias acessem gravações e leituras vir de qualquer cópia.

  ![Espelhamento bidirecional aninhado](media/nested-resiliency/nested-two-way-mirror.png)

- **Paridade de aceleração de espelho aninhada.** Combinar aninhados espelhamento bidirecional de acima, com a paridade aninhada. Dentro de cada servidor, a resiliência local para a maioria dos dados é fornecida por única [paridade de bit a bit aritmética](storage-spaces-fault-tolerance.md#parity), exceto as novas gravações recentes que usam espelhamento bidirecional. Em seguida, ainda mais resiliência para todos os dados é fornecida pelo espelhamento bidirecional entre os servidores. Para obter mais informações sobre como aceleradas espelho paridade funciona, consulte [paridade acelerada de espelho](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity).

  ![Paridade de aceleração de espelho aninhada](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>Eficiência de capacidade

Eficiência de capacidade é a proporção de espaço utilizável para [espaço de volume](plan-volumes.md#choosing-the-size-of-volumes). Ele descreve a sobrecarga de capacidade atribuível a resiliência e depende da opção de resiliência que você escolher. Como um exemplo simples, o armazenamento de dados sem resiliência é 100% da capacidade eficiente (1 TB de dados leva até 1 TB de capacidade de armazenamento físico), enquanto o espelhamento bidirecional é 50% eficiente (1 TB de dados leva até 2 TB de capacidade de armazenamento físico).

- **Espelhamento bidirecional aninhado** grava quatro cópias de tudo, ou seja armazenar a 1 TB de dados, você precisa de 4 TB de capacidade de armazenamento físico. Embora sua simplicidade é atraente, eficiência da capacidade do espelho bidirecional aninhada de 25% é o mais baixo de qualquer opção de resiliência em espaços de armazenamento diretos.

- **Aninhado acelerada de espelho paridade** alcança a maior eficiência de capacidade, aproximadamente 35% a 40%, que depende de dois fatores: o número de capacidade de unidades em cada servidor e a combinação de espelho e paridade especificados para o volume. Esta tabela fornece uma pesquisa para as configurações comuns:

  | Unidades de capacidade por servidor | espelho de 10% | espelho de 20% | espelho de 30% |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **Se você estiver curioso, aqui está um exemplo de como a matemática completo.** Suponha que temos seis unidades de capacidade em cada um dos dois servidores, e queremos criar um volume de 100 GB, composto de 10 GB de espelho e de 90 GB de paridade. Local do servidor de espelho bidirecional é 50,0% eficiente, que significa que 10 GB de dados espelho usa 20 GB para armazenar em cada servidor. Total do seu espaço espelhado para ambos os servidores, é de 40 GB. Servidor de local paridade única, nesse caso, é 5/6 = 83.3% de eficiência, o que significa que os 90 GB de dados de paridade leva 108 GB para armazenar em cada servidor. Total do seu espaço espelhado para ambos os servidores, é 216 GB. O volume total é, portanto, [(10 GB/50,0%) + (90 GB / 83.3%)] × 2 = 256 GB, 39.1% geral eficiência da capacidade.

Observe que a eficiência da capacidade do clássico (cerca de 50%) de espelhamento bidirecional e aninhados paridade acelerada de espelho (até 40%) não são muito diferentes. Dependendo dos seus requisitos, a eficiência da capacidade ligeiramente menor talvez vale a pena o aumento significativo na disponibilidade de armazenamento. Escolha o resiliência por volume, portanto, você pode misturar os volumes de resiliência aninhados e clássico de espelho bidirecional dentro do mesmo cluster.

![Compensação](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>Uso no PowerShell

Você pode usar os cmdlets de armazenamento familiarizado no PowerShell para criar volumes com resiliência aninhada.

### <a name="step-1-create-storage-tier-templates"></a>Etapa 1: Criar modelos de camada de armazenamento

Primeiro, crie novos modelos de camadas de armazenamento usando o `New-StorageTier` cmdlet. Você só precisará fazer isso vez e, em seguida, cada novo volume em que você cria pode fazer referência a esses modelo. Especifique o `-MediaType` de suas unidades de capacidade e, opcionalmente, o `-FriendlyName` de sua escolha. Não modifique os outros parâmetros.

Se suas unidades de capacidade são unidades de disco rígido (HDD), inicie o PowerShell como administrador e execute:

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

Se suas unidades de capacidade são unidades de estado sólido (SSD), defina as `-MediaType` para `SSD` em vez disso. Não modifique os outros parâmetros.

> [!TIP]
> Verifique se as camadas criadas com êxito com `Get-StorageTier`.

### <a name="step-2-create-volumes"></a>Etapa 2: Criar volumes

Em seguida, criar novos volumes usando o `New-Volume` cmdlet.

#### <a name="nested-two-way-mirror"></a>Espelhamento bidirecional aninhado

Para usar espelhamento bidirecional aninhado, fazer referência a `NestedMirror` camada de modelo e especifique o tamanho. Por exemplo: 

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>Paridade de aceleração de espelho aninhada

Para usar a paridade de aceleração de espelho aninhada, fazer referência a `NestedMirror` e `NestedParity` modelos de camada e especifique dois tamanhos, uma para cada parte do volume (espelhar o paridade da primeiro, segundo). Por exemplo criar um volume de 500 GB que é espelhamento bidirecional aninhada de 20% e 80% aninhados paridade, execute:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>Etapa 3: Continuar no Windows Admin Center

Volumes que usam a resiliência aninhada aparecem no [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) com os rótulos não criptografado, como na captura de tela abaixo. Depois que elas forem criadas, você pode gerenciar e monitorá-los usando Windows Admin Center, assim como qualquer outro volume em espaços de armazenamento diretos.

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>Opcional: Estender para unidades de cache

Com suas configurações padrão, a resiliência aninhada protege contra a perda de várias unidades de capacidade ao mesmo tempo, ou um servidor e uma unidade de capacidade ao mesmo tempo. Para expandir essa proteção para [unidades de cache](understand-the-cache.md) tem uma consideração adicional: porque as unidades de cache geralmente fornecem leitura *e gravar* cache para *vários* unidades de capacidade , a única maneira de garantir que você pode tolerar perda de uma unidade de cache quando o outro servidor está inativo é simplesmente não gravações em cache, mas que afeta o desempenho.

Para abordar esse cenário, espaços de armazenamento diretos oferece a opção de desabilitar automaticamente a gravação em cache quando um servidor em um cluster de dois servidores é pressionada e, em seguida, habilite novamente depois que o servidor é fazer backup de cache de gravação. Para permitir que a rotina reinicializações sem afetar o desempenho, gravar o cache não é desabilitado até que o servidor tiver sido interrompido por 30 minutos.

Para definir esse comportamento (opcional), inicie o PowerShell como administrador e execute:

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

Uma vez definido como `True`, o comportamento de cache é:

| Situação                       | Comportamento do cache                           | Pode tolerar perda de unidade de cache? |
|---------------------------------|------------------------------------------|--------------------------------|
| Ambos os servidores de backup                 | Cache de leituras e gravações, o desempenho total | Sim                            |
| Servidor inoperante, primeiros 30 minutos   | Cache de leituras e gravações, o desempenho total | Não (temporariamente)               |
| Após os primeiros 30 minutos          | Desempenho de cache lê apenas, afetado   | Sim                            |

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>É possível converter um volume existente entre espelho bidirecional e resiliência aninhada?

Não, volumes não podem ser convertidos entre tipos de resiliência. Para novas implantações no Windows Server 2019, decida antecipadamente qual tipo de resiliência melhor atenda às suas necessidades. Se você estiver atualizando do Windows Server 2016, pode criar novos volumes com resiliência aninhada, migre seus dados e, em seguida, exclua os volumes antigos.

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>Pode usar resiliência aninhada com vários tipos de unidades de capacidade?

Sim, basta especificar o `-MediaType` de cada camada adequadamente durante [etapa 1](#step-1-create-storage-tier-templates) acima. Por exemplo, com NVMe, SSD e HDD no mesmo cluster, o NVMe fornece cache enquanto a capacidade de fornecer os dois últimos: definir a `NestedMirror` camadas para `-MediaType SSD` e o `NestedParity` camadas para `-MediaType HDD`. Nesse caso, observe que eficiência da capacidade de paridade depende do número de unidades de disco rígido somente, e você precisa de pelo menos 4 deles por servidor.

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>Pode usar resiliência aninhada com 3 ou mais servidores?

Não, use apenas aninhada resiliência se seu cluster tem exatamente 2 servidores.

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>Quantas unidades precisa usar resiliência aninhada?

O número mínimo de unidades necessárias para espaços de armazenamento diretos é 4 unidades de capacidade por nó de servidor, além de cache de 2 discos por nó de servidor (se houver). Isso permanece inalterado em Windows Server 2016. Não há nenhum requisito adicional para garantir a resiliência aninhada, e a recomendação para capacidade reserva é alterada muito.

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>Resiliência aninhada altera como a substituição da unidade funciona?

Nenhum.

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>Resiliência aninhada altera como a substituição de nó do servidor funciona?

Nenhum. Para substituir um nó de servidor e suas unidades, siga esta ordem:

1. Desativar as unidades no servidor de saída
2. Adicione o novo servidor, com suas unidades para o cluster
3. O pool de armazenamento irá rebalancear
4. Remover o servidor de saída e suas unidades

Para obter detalhes, consulte o [remover servidores](remove-servers.md) tópico.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Entender a tolerância a falhas em espaços de armazenamento diretos](storage-spaces-fault-tolerance.md)
- [Plano de volumes em espaços de armazenamento diretos](plan-volumes.md)
- [Criar volumes em espaços de armazenamento diretos](create-volumes.md)
