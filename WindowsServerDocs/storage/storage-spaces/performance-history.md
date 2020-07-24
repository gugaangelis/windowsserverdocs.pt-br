---
title: Histórico de desempenho para Espaços de Armazenamento Diretos
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: ce984d3a88f46b77773c524e5b75135930e1bb03
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961788"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Histórico de desempenho para Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019

O histórico de desempenho é um novo recurso que oferece aos administradores de [espaços de armazenamento diretos](storage-spaces-direct-overview.md) acesso fácil a medidas históricas de computação, memória, rede e armazenamento em servidores de host, unidades, volumes, máquinas virtuais e muito mais. O histórico de desempenho é coletado automaticamente e armazenado no cluster por até um ano.

   > [!IMPORTANT]
   > Esse recurso é novo no Windows Server 2019. Ele não está disponível no Windows Server 2016.

## <a name="get-started"></a>Introdução

O histórico de desempenho é coletado por padrão com Espaços de Armazenamento Diretos no Windows Server 2019. Você não precisa instalar, configurar nem iniciar nada. Uma conexão com a Internet não é necessária, o System Center não é necessário e um banco de dados externo não é necessário.

Para ver o histórico de desempenho do cluster graficamente, use o [centro de administração do Windows](../../manage/windows-admin-center/overview.md):

![Histórico de desempenho no centro de administração do Windows](media/performance-history/perf-history-in-wac.png)

Para consultá-lo e processá-lo programaticamente, use o novo `Get-ClusterPerf` cmdlet. Consulte [uso no PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>O que é coletado

O histórico de desempenho é coletado para 7 tipos de objetos:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tem muitas séries: por exemplo, `ClusterNode.Cpu.Usage` é coletado para cada servidor.

Para obter detalhes sobre o que é coletado para cada tipo de objeto e como interpretá-los, consulte estes subtópicos:

| Objeto             | Série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [O que é coletado para unidades](performance-history-for-drives.md)                     |
| Adaptadores de rede   | [O que é coletado para adaptadores de rede](performance-history-for-network-adapters.md) |
| Servidores            | [O que é coletado para servidores](performance-history-for-servers.md)                   |
| Discos rígidos virtuais | [O que é coletado para discos rígidos virtuais](performance-history-for-vhds.md)           |
| Máquinas virtuais   | [O que é coletado para máquinas virtuais](performance-history-for-vms.md)              |
| Volumes            | [O que é coletado para volumes](performance-history-for-volumes.md)                   |
| Clusters           | [O que é coletado para clusters](performance-history-for-clusters.md)                 |

Muitas séries são agregadas em objetos pares ao seu pai: por exemplo, `NetAdapter.Bandwidth.Inbound` são coletadas para cada adaptador de rede separadamente e agregadas para o servidor geral; da mesma forma, `ClusterNode.Cpu.Usage` é agregada ao cluster geral e assim por diante.

## <a name="timeframes"></a>Períodos

O histórico de desempenho é armazenado por até um ano, com menor granularidade. Para a hora mais recente, as medições estão disponíveis a cada dez segundos. Depois disso, eles são mesclados de forma inteligente (por média ou soma, conforme apropriado) em séries menos granulares que abrangem mais tempo. Para o dia mais recente, as medições estão disponíveis a cada cinco minutos; para a semana mais recente, a cada quinze minutos; e assim por diante.

No centro de administração do Windows, você pode selecionar o período de tempo no canto superior direito acima do gráfico.

![Períodos de tempo no centro de administração do Windows](media/performance-history/timeframes-in-honolulu.png)

No PowerShell, use o `-TimeFrame` parâmetro.

Estes são os períodos de tempo disponíveis:

| Período de tempo   | Frequência de medição | Retido para |
|-------------|-----------------------|--------------|
| `LastHour`  | A cada 10 segundos         | 1 hora       |
| `LastDay`   | A cada 5 minutos       | 25 horas     |
| `LastWeek`  | A cada 15 minutos      | 8 dias       |
| `LastMonth` | A cada 1 hora          | 35 dias      |
| `LastYear`  | A cada 1 dia           | 400 dias     |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o `Get-ClusterPerformanceHistory` cmdlet para consultar e processar o histórico de desempenho no PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Use o alias **Get-ClusterPerf** para salvar alguns pressionamentos de tecla.

### <a name="example"></a>Exemplo

Obtenha o uso da CPU da máquina virtual *MyVM* na última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obter exemplos mais avançados, consulte os [scripts de exemplo](performance-history-scripting.md) publicados que fornecem o código inicial para localizar valores de pico, calcular médias, plotar linhas de tendência, executar detecção de exceção e muito mais.

### <a name="specify-the-object"></a>Especifique o objeto

Você pode especificar o objeto desejado pelo pipeline. Isso funciona com sete tipos de objetos:

| Objeto do pipeline | Exemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Se você não especificar, o histórico de desempenho para o cluster geral será retornado.

### <a name="specify-the-series"></a>Especificar a série

Você pode especificar a série que deseja com estes parâmetros:


| Parâmetro                 | Exemplo                       | Lista                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [O que é coletado para unidades](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [O que é coletado para adaptadores de rede](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [O que é coletado para servidores](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [O que é coletado para discos rígidos virtuais](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [O que é coletado para máquinas virtuais](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [O que é coletado para volumes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [O que é coletado para clusters](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Use o preenchimento com Tab para descobrir a série disponível.

Se você não especificar, todas as séries disponíveis para o objeto especificado serão retornadas.

### <a name="specify-the-timeframe"></a>Especificar o período de tempo

Você pode especificar o período de tempo do histórico desejado com o `-TimeFrame` parâmetro.

   > [!TIP]
   > Use o preenchimento com Tab para descobrir os períodos de tempo disponíveis.

Se você não especificar, a `MostRecent` medida será retornada.

## <a name="how-it-works"></a>Como ele funciona

### <a name="performance-history-storage"></a>Armazenamento de histórico de desempenho

Logo após o Espaços de Armazenamento Diretos ser habilitado, um volume de aproximadamente 10 GB chamado `ClusterPerformanceHistory` é criado e uma instância do mecanismo de armazenamento extensível (também conhecido como Microsoft Jet) é provisionada lá. Esse banco de dados leve armazena o histórico de desempenho sem qualquer envolvimento ou gerenciamento de administrador.

![Volume para armazenamento de histórico de desempenho](media/performance-history/perf-history-volume.png)

O volume é apoiado por espaços de armazenamento e usa um espelho simples, bidirecional ou resiliência de espelho triplo, dependendo do número de nós no cluster. Ele é reparado após falhas de unidade ou servidor, assim como qualquer outro volume em Espaços de Armazenamento Diretos.

O volume usa ReFS, mas não é Volume Compartilhado Clusterizado (CSV), portanto, ele só aparece no nó proprietário do grupo de clusters. Além de ser criado automaticamente, não há nada de especial sobre este volume: você pode vê-lo, procurá-lo, redimensioná-lo ou excluí-lo (não recomendado). Se algo der errado, consulte [solução de problemas](#troubleshooting).

### <a name="object-discovery-and-data-collection"></a>Descoberta de objeto e coleta de dados

O histórico de desempenho descobre automaticamente os objetos relevantes, como máquinas virtuais, em qualquer lugar no cluster e começa a transmitir seus contadores de desempenho. Os contadores são agregados, sincronizados e inseridos no banco de dados. O streaming é executado continuamente e é otimizado para um mínimo impacto no sistema.

A coleção é manipulada pelo Serviço de Integridade, que é altamente disponível: se o nó em que está sendo executado ficar inativo, ele será retomado em instantes mais tarde em outro nó no cluster. O histórico de desempenho pode se sobrepor rapidamente, mas será retomado automaticamente. Você pode ver o Serviço de Integridade e seu nó de proprietário executando `Get-ClusterResource Health` no PowerShell.

### <a name="handling-measurement-gaps"></a>Lidando com lacunas de medida

Quando as medidas são mescladas em séries menos granulares que abrangem mais tempo, conforme [descrito em períodos de período,](#timeframes)os pontos de dados ausentes são excluídos. Por exemplo, se o servidor esteve inoperante por 30 minutos e, em seguida, em execução às 50% da CPU nos próximos 30 minutos, a `ClusterNode.Cpu.Usage` média da hora será registrada corretamente como 50% (não 25%).

### <a name="extensibility-and-customization"></a>Extensibilidade e personalização

O histórico de desempenho é amigável para scripts. Use o PowerShell para efetuar pull de qualquer histórico disponível diretamente do banco de dados para criar relatórios automatizados ou alertas, exportar o histórico para fins de proteção, distribuir suas próprias visualizações, etc. Consulte os [scripts de exemplo](performance-history-scripting.md) publicados para obter um código de início útil.

Não é possível coletar histórico para objetos adicionais, períodos de tempo ou séries.

A frequência de medição e o período de retenção não são configuráveis no momento.

## <a name="start-or-stop-performance-history"></a>Iniciar ou parar o histórico de desempenho

### <a name="how-do-i-enable-this-feature"></a>Como fazer habilitar esse recurso?

A menos que você `Stop-ClusterPerformanceHistory` , o histórico de desempenho é habilitado por padrão.

Para reabilitá-lo, execute este cmdlet do PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Como fazer desabilitar esse recurso?

Para parar de coletar o histórico de desempenho, execute este cmdlet do PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para excluir as medições existentes, use o `-DeleteHistory` sinalizador:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante a implantação inicial, você pode impedir que o histórico de desempenho seja iniciado definindo o `-CollectPerformanceHistory` parâmetro de `Enable-ClusterStorageSpacesDirect` para `$False` .

## <a name="troubleshooting"></a>Solução de problemas

### <a name="the-cmdlet-doesnt-work"></a>O cmdlet não funciona

Uma mensagem de erro como "*o termo ' Get-ClusterPerf ' não é reconhecida como o nome de um cmdlet*" significa que o recurso não está disponível ou instalado. Verifique se você tem o Windows Server Insider Preview versão 17692 ou posterior, se você instalou o clustering de failover e se está executando o Espaços de Armazenamento Diretos.

   > [!NOTE]
   > Este recurso não está disponível no Windows Server 2016 ou anterior.

### <a name="no-data-available"></a>Não há dados disponíveis

Se um gráfico Mostrar "*nenhum dado disponível*", conforme a imagem, veja como solucionar problemas:

![Não há dados disponíveis](media/performance-history/no-data-available.png)

1. Se o objeto tiver sido adicionado ou criado recentemente, aguarde até que ele seja descoberto (até 15 minutos).

2. Atualize a página ou aguarde a próxima atualização em segundo plano (até 30 segundos).

3. Determinados objetos especiais são excluídos do histórico de desempenho – por exemplo, máquinas virtuais que não são clusterizadas e volumes que não usam o sistema de arquivos de Volume Compartilhado Clusterizado (CSV). Verifique o subtópico para o tipo de objeto, como o [histórico de desempenho dos volumes](performance-history-for-volumes.md), para impressão refinada.

4. Se o problema persistir, abra o PowerShell como administrador e execute o `Get-ClusterPerf` cmdlet. O cmdlet inclui a lógica de solução de problemas para identificar problemas comuns, como se o volume ClusterPerformanceHistory estiver ausente e fornecer instruções de correção.

5. Se o comando na etapa anterior não retornar nada, você poderá tentar reiniciar o Serviço de Integridade (que coleta o histórico de desempenho) executando `Stop-ClusterResource Health ; Start-ClusterResource Health` no PowerShell.

## <a name="additional-references"></a>Referências adicionais

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
