---
title: Histórico de desempenho de espaços de armazenamento diretos
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239253"
---
# Histórico de desempenho de espaços de armazenamento diretos

> Aplicável a: Windows Server 2019

Histórico de desempenho é um novo recurso que oferece [Espaços de armazenamento diretos](storage-spaces-direct-overview.md) administradores acesso fácil a computação histórica, memória, rede e armazenamento medidas em todos os servidores do host, unidades, volumes, as máquinas virtuais e muito mais. Histórico de desempenho é coletado automaticamente e armazenado no cluster por até um ano.

   > [!IMPORTANT]
   > Esse recurso é novo no Windows Server 2019. Não está disponível no Windows Server 2016.

## Introdução

Histórico de desempenho é coletado por padrão com espaços de armazenamento diretos no Windows Server 2019. Você não precisa instalar, configurar ou começar a nada. Não é necessária uma conexão de Internet, System Center não é necessário e um banco de dados externo não é necessário.

Para ver o desempenho do cluster histórico graficamente, use [O Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Histórico de desempenho no Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Para consultar e processá-los programaticamente, use a nova `Get-ClusterPerf` cmdlet. Consulte [uso no PowerShell](#usage-in-powershell).

## O que é coletado

Histórico de desempenho é coletado para 7 tipos de objetos:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tem muitos série: por exemplo, `ClusterNode.Cpu.Usage` são coletadas para cada servidor.

Para obter detalhes de como interpretá-los e o que é coletado para cada tipo de objeto, consulte estes tópicos subpropriedade:

| Objeto             | Série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [O que é coletado para unidades](performance-history-for-drives.md)                     |
| Adaptadores de Rede   | [O que é coletado para adaptadores de rede](performance-history-for-network-adapters.md) |
| Servidor            | [O que é coletado para servidores](performance-history-for-servers.md)                   |
| Discos rígidos virtuais | [O que é coletado para discos rígidos virtuais](performance-history-for-vhds.md)           |
| Máquinas virtuais   | [O que é coletado para máquinas virtuais](performance-history-for-vms.md)              |
| Volumes            | [O que é coletado para volumes](performance-history-for-volumes.md)                   |
| Clusters           | [O que é coletado para clusters](performance-history-for-clusters.md)                 |

Muitos série é agregada em objetos de par para seu pai: por exemplo, `NetAdapter.Bandwidth.Inbound` é coletado separadamente para cada adaptador de rede e agregados para o servidor geral; da mesma forma `ClusterNode.Cpu.Usage` são agregados ao cluster geral; e assim por diante.

## Períodos

Histórico de desempenho é armazenado por até um ano, com granularidade de tempo. Para a hora mais recente, medições estão disponíveis cada 10 segundos. Depois disso, eles são mesclados inteligente (média ou soma, conforme apropriado) em série menos granular que abrangem mais tempo. Para o dia mais recente, medições estão disponíveis a cada cinco minutos; para a semana mais recente, a cada 15 minutos; e assim por diante.

No Windows Admin Center, você pode selecionar o período de tempo no canto superior direito acima do gráfico.

![Períodos no Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

No PowerShell, use o `-TimeFrame` parâmetro.

Aqui estão os prazos disponíveis:

| Período de tempo   | Frequência de medição | Mantidos por |
|-------------|-----------------------|--------------|
| `LastHour`  | Cada 10 segundos         | 1 hora       |
| `LastDay`   | Cada 5 minutos       | 25 horas     |
| `LastWeek`  | Cada 15 minutos      | 8 dias       |
| `LastMonth` | Cada 1 hora          | 35 dias      |
| `LastYear`  | Cada 1 dia           | 400 dias     |

## Uso do PowerShell

Use o `Get-ClusterPerformanceHistory` cmdlet ao histórico de desempenho de consulta e o processo no PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Use o alias de **Get-ClusterPerf** para salvar alguns pressionamentos de teclas.

### Exemplo

Obter o uso da CPU de máquina virtual *MyVM* de última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obter exemplos mais avançados, consulte os [scripts de exemplo](performance-history-scripting.md) publicado que fornecem código inicial para encontrar os valores de pico, calcular médias, plotar linhas de tendência, executar excepcionais detecção e muito mais.

### Especifique o objeto

Você pode especificar o objeto desejado pelo pipeline. Isso funciona com 7 tipos de objetos:

| Objeto do pipeline | Exemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Se você não especificar, histórico de desempenho para o cluster geral será retornado.

### Especificar a série

Você pode especificar a série de que ser com esses parâmetros:


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
   > Use a conclusão de guia para descobrir série disponível.

Se você não especificar, cada série disponível para o objeto especificado será retornado.

### Especifique o período de tempo

Você pode especificar o período de tempo de histórico que deseja com o `-TimeFrame` parâmetro.

   > [!TIP]
   > Use a conclusão de guia para descobrir períodos disponíveis.

Se você não especificar, o `MostRecent` medição é retornada.

## Como funciona

### Armazenamento de histórico de desempenho

Logo após espaços de armazenamento diretos está habilitado, um volume GB aproximadamente 10 chamado `ClusterPerformanceHistory` é criado e uma instância do mecanismo de armazenamento extensível (também conhecido como Microsoft JET) é provisionada lá. Esse banco de dados leve armazena o histórico de desempenho sem nenhum envolvimento do administrador ou gerenciamento.

![Volume de armazenamento de histórico de desempenho](media/performance-history/perf-history-volume.png)

O volume é salvo por espaços de armazenamento e usa espelhamento bidirecional, simple ou a resiliência por espelhamento de três vias, dependendo do número de nós do cluster. Ele é reparado após falhas de unidade ou servidor assim como qualquer outro volume em espaços de armazenamento diretos.

O volume usa ReFS, mas não é Volume compartilhado clusterizado (CSV), então ele aparece apenas no nó proprietário do grupo de Cluster. Além do que está sendo criado automaticamente, há nada de especial sobre esse volume: você pode vê-lo, procurá-lo, redimensioná-lo ou excluí-lo (não recomendado). Se algo der errado, consulte [Solucionando problemas](#troubleshooting). 

### Objeto descoberta e coleta de dados

Histórico de desempenho automaticamente detecta objetos relevantes, como máquinas virtuais, em qualquer lugar no cluster e começa a transmitir seus contadores de desempenho. Os contadores são agregados, sincronizados e inseridos no banco de dados. Streaming é executado continuamente e é otimizado para o impacto de sistema mínimo.

Coleção é manipulada pelo serviço de integridade, que é altamente disponível: se o nó onde ele está em execução falhar, ele será retomado momentos posteriormente em outro nó no cluster. Histórico de desempenho pode lapse rapidamente, mas ele continuará automaticamente. Você pode ver o serviço de integridade e seu nó proprietário executando `Get-ClusterResource Health` no PowerShell.

### Lacunas de medição de manipulação

Quando as medições são mescladas em série menos granular que abrangem mais tempo, conforme descrito em [intervalos de tempo](#Timeframes), períodos de dados ausentes são excluídos. Por exemplo, se o servidor estava inativo por 30 minutos, em seguida, em execução na CPU de 50% por 30 minutos, o `ClusterNode.Cpu.Usage` médio para a hora será gravada corretamente como 50% (não 25%).

### Personalização e extensibilidade

Histórico de desempenho é amigável para scripts. Use o PowerShell para efetuar pull de qualquer histórico disponível diretamente do banco de dados para criar relatórios automatizados ou alerta, exportar o histórico para fins de proteção, agrupar seus próprios visualizações, etc. Consulte os publicado [scripts de exemplo](performance-history-scripting.md) de código inicial útil.

Não é possível coletar histórico para objetos adicionais, períodos ou série.

A frequência de medição e o período de retenção não são configuráveis no momento.

## Iniciar ou parar o histórico de desempenho

### Como habilitar esse recurso?

A menos que você `Stop-ClusterPerformanceHistory`, histórico de desempenho é habilitado por padrão.

Para reativá-la, execute este cmdlet do PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### Como desativar esse recurso?

Para interromper a coleta de histórico de desempenho, execute este cmdlet do PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para excluir medições existentes, use o `-DeleteHistory` sinalizador:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante a implantação inicial, você pode impedir que o histórico de desempenho definindo o `-CollectPerformanceHistory` parâmetro de `Enable-ClusterStorageSpacesDirect` para `$False`.

## Solução de problemas

### O cmdlet não funciona

Uma mensagem de erro como "*o termo ' Get-ClusterPerf' não é reconhecido como o nome de um cmdlet*" significa que o recurso não está disponível ou instalado. Verifique se que você tenha a compilação do Windows Server Insider Preview 17692 ou posterior, que você instalou o Clustering de Failover, e que você está executando espaços de armazenamento diretos.

   > [!NOTE]
   > Esse recurso não está disponível no Windows Server 2016 ou anterior.

### Não há dados disponíveis 

Se um gráfico mostra "*nenhum dado disponível*", como mostrado, aqui está como solucionar:

![Não há dados disponíveis](media/performance-history/no-data-available.png)

1. Se o objeto foi adicionado recentemente ou criado, aguarde-o para ser descoberto (até 15 minutos).

2. Atualizar a página, ou aguardar a próxima atualização em segundo plano (até 30 segundos).

3. Determinados objetos especiais são excluídos do histórico de desempenho – por exemplo, as máquinas virtuais que não estão em cluster e volumes que não usam o sistema de arquivos do Volume compartilhado clusterizado (CSV). Consulte o tópico sub para o tipo de objeto, como o [histórico de desempenho para volumes](performance-history-for-volumes.md), para a impressão de belas.

4. Se o problema persistir, abra o PowerShell como administrador e execute o `Get-ClusterPerf` cmdlet. O cmdlet inclui solução lógica para identificar problemas comuns, por exemplo, se o volume ClusterPerformanceHistory está ausente e fornece instruções de correção.

5. Se o comando na etapa anterior retorna nada, tente reiniciar o serviço de integridade (que coleta histórico de desempenho) executando `Stop-ClusterResource Health ; Start-ClusterResource Health` no PowerShell.

## Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
