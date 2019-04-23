---
title: Histórico de desempenho para espaços de armazenamento diretos
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870857"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Histórico de desempenho para espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019

Histórico de desempenho é um novo recurso que oferece [espaços de armazenamento diretos](storage-spaces-direct-overview.md) administradores acessem facilmente medidas históricas de computação, memória, rede e armazenamento em servidores de host, unidades, volumes, máquinas virtuais e muito mais. Histórico de desempenho é coletado automaticamente e armazenado no cluster de um ano.

   > [!IMPORTANT]
   > Esse recurso é novo no Windows Server 2019. Ele não está disponível no Windows Server 2016.

## <a name="get-started"></a>Introdução

Histórico de desempenho é coletado por padrão com espaços de armazenamento diretos no Windows Server 2019. Você não precisará instalar, configurar ou iniciar qualquer coisa. Uma conexão de Internet não é necessária, o System Center não é necessário e um banco de dados externo não é necessário.

Para ver o histórico de desempenho do seu cluster graficamente, use [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Histórico de desempenho no Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Para consultar e processá-lo de forma programática, use o novo `Get-ClusterPerf` cmdlet. Ver [uso do PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>O que é coletado

Histórico de desempenho é coletado para 7 tipos de objetos:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tiver várias séries: por exemplo, `ClusterNode.Cpu.Usage` são coletados para cada servidor.

Para obter detalhes sobre o que é coletado para cada tipo de objeto e como interpretá-los, consulte esses subtópicos:

| Object             | série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [O que é coletado para unidades](performance-history-for-drives.md)                     |
| Adaptadores de Rede   | [O que é coletado para adaptadores de rede](performance-history-for-network-adapters.md) |
| Servidores            | [O que é coletado para servidores](performance-history-for-servers.md)                   |
| Discos rígidos virtuais | [O que é coletado para discos rígidos virtuais](performance-history-for-vhds.md)           |
| Máquinas Virtuais   | [O que é coletado para máquinas virtuais](performance-history-for-vms.md)              |
| Volumes            | [O que é coletado para volumes](performance-history-for-volumes.md)                   |
| Clusters           | [O que é coletado para clusters](performance-history-for-clusters.md)                 |

Várias séries são agregados entre objetos de ponto a ponto para seu pai: por exemplo, `NetAdapter.Bandwidth.Inbound` são coletados para cada adaptador de rede separadamente e agregados para o servidor geral; da mesma forma `ClusterNode.Cpu.Usage` são agregadas para o cluster geral; e assim por diante.

## <a name="timeframes"></a>Períodos de tempo

Histórico de desempenho é armazenado por até um ano, com granularidade reduzida. Para a hora mais recente, as medidas estão disponíveis a cada dez segundos. Depois disso, eles são mesclados com inteligência (por média ou soma, conforme apropriado) em série menos granular que se estendem por mais tempo. Para o dia mais recente, as medidas estão disponíveis a cada cinco minutos; para a semana mais recente, a cada quinze minutos; e assim por diante.

No do Windows Admin Center, você pode selecionar o período de tempo no canto superior direito acima do gráfico.

![Períodos de tempo no Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

No PowerShell, use o `-TimeFrame` parâmetro.

Aqui estão os períodos de tempo disponíveis:

| Prazo   | Frequência de medição | Retido para |
|-------------|-----------------------|--------------|
| `LastHour`  | Cada 10 segundos         | 1 hora       |
| `LastDay`   | Cada 5 minutos       | 25 horas     |
| `LastWeek`  | Cada 15 minutos      | 8 dias       |
| `LastMonth` | Cada 1 hora          | 35 dias      |
| `LastYear`  | Cada 1 dia           | 400 dias     |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o `Get-ClusterPerformanceHistory` cmdlet ao histórico de desempenho de consulta e o processo no PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Use o **Get-ClusterPerf** alias ao salvar alguns pressionamentos de teclas.

### <a name="example"></a>Exemplo

Obter o uso da CPU da máquina virtual *MyVM* de última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obter exemplos mais avançados, consulte o publicado [scripts de exemplo](performance-history-scripting.md) que fornecem código inicial para localizar valores de pico, calcular médias, criar gráficos de linhas de tendência, executam exceções detecção e muito mais.

### <a name="specify-the-object"></a>Especifique o objeto

Você pode especificar o objeto desejado pelo pipeline. Isso funciona com 7 tipos de objetos:

| Objeto de pipeline | Exemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Se você não especificar, histórico de desempenho para o cluster geral será retornado.

### <a name="specify-the-series"></a>Especificar a série

Você pode especificar a série desejada com estes parâmetros:


| Parâmetro                 | Exemplo                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [O que é coletado para unidades](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [O que é coletado para adaptadores de rede](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [O que é coletado para servidores](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [O que é coletado para discos rígidos virtuais](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [O que é coletado para máquinas virtuais](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [O que é coletado para volumes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [O que é coletado para clusters](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Use o preenchimento com tab para descobrir a série disponível.

Se você não especificar, cada série estão disponíveis para o objeto especificado é retornado.

### <a name="specify-the-timeframe"></a>Especifique o período de tempo

Você pode especificar o período de tempo de histórico que desejar com o `-TimeFrame` parâmetro.

   > [!TIP]
   > Use o preenchimento com tab para descobrir os períodos de tempo disponíveis.

Se não for especificado, o `MostRecent` medida será retornada.

## <a name="how-it-works"></a>Como funciona

### <a name="performance-history-storage"></a>Armazenamento de histórico de desempenho

Logo após os espaços de armazenamento diretos está habilitado, um volume GB cerca de 10 chamado `ClusterPerformanceHistory` é criado e uma instância do mecanismo de armazenamento extensível (também conhecido como Microsoft JET) é provisionada lá. Este banco de dados leve armazena o histórico de desempenho sem qualquer envolvimento do administrador ou gerenciamento.

![Volume de armazenamento do histórico de desempenho](media/performance-history/perf-history-volume.png)

O volume é apoiado por espaços de armazenamento e usa o espelho bidirecional de simple ou resiliência de espelho triplo, dependendo do número de nós no cluster. Ele é reparado após falhas de unidade ou servidor assim como qualquer outro volume em espaços de armazenamento diretos.

O volume usa ReFS, mas não é Cluster CSV (Volume compartilhado), portanto, ela só será exibida no nó do proprietário do grupo de clusters. Além do que está sendo criado automaticamente, não há nada especial sobre este volume: você pode vê-lo, navegar por ele, redimensioná-la ou excluí-lo (não recomendado). Se algo der errado, consulte [solução de problemas](#troubleshooting). 

### <a name="object-discovery-and-data-collection"></a>Coleta de dados e descoberta de objeto

Histórico de desempenho automaticamente descobre os objetos relevantes, como máquinas virtuais, em qualquer lugar no cluster e começa a transmitir seus contadores de desempenho. Os contadores são agregados, sincronizados e inseridos no banco de dados. Streaming é executado continuamente e é otimizado para impacto mínimos do sistema.

Coleção é manipulada pelo serviço de integridade, que é altamente disponível: se o nó onde ele está sendo executado ficar inativo, ele continuará minutos mais tarde em outro nó no cluster. Histórico de desempenho pode expirem rapidamente, mas ela será retomada automaticamente. Você pode ver o serviço de integridade e seu nó do proprietário executando `Get-ClusterResource Health` no PowerShell.

### <a name="handling-measurement-gaps"></a>Tratando intervalos de medição

Quando as medidas são mescladas em série menos granular que se estendem por mais tempo, conforme descrito em [períodos de tempo](#Timeframes), períodos de dados ausentes são excluídos. Por exemplo, se o servidor ficou inativo por 30 minutos, em seguida, executando a 50% da CPU por 30 minutos, o `ClusterNode.Cpu.Usage` média para a hora será registrada corretamente como 50% (não em 25%).

### <a name="extensibility-and-customization"></a>Personalização e extensibilidade

Histórico de desempenho é compatível com scripts. Use o PowerShell para extrair qualquer histórico disponível diretamente do banco de dados para criar relatórios automatizados ou alertas, exportar histórico por questões de segurança, troque suas próprias visualizações, etc. Consulte o publicado [scripts de exemplo](performance-history-scripting.md) para código inicial úteis.

Não é possível coletar o histórico de série, períodos de tempo ou objetos adicionais.

A frequência de medição e o período de retenção não são configuráveis no momento.

## <a name="start-or-stop-performance-history"></a>Iniciar ou parar o histórico de desempenho

### <a name="how-do-i-enable-this-feature"></a>Como para habilitar esse recurso?

A menos que você `Stop-ClusterPerformanceHistory`, histórico de desempenho é habilitado por padrão.

Para habilitá-la novamente, execute este cmdlet do PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Como posso desabilitar esse recurso?

Para interromper a coleta de histórico de desempenho, execute este cmdlet do PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para excluir medidas existentes, use o `-DeleteHistory` sinalizador:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante a implantação inicial, você pode impedir que o histórico de desempenho definindo a `-CollectPerformanceHistory` parâmetro de `Enable-ClusterStorageSpacesDirect` para `$False`.

## <a name="troubleshooting"></a>Solução de problemas

### <a name="the-cmdlet-doesnt-work"></a>O cmdlet não funciona

Uma mensagem de erro como "*o termo 'Get-ClusterPerf' não é reconhecido como o nome de um cmdlet*" significa que o recurso não está disponível ou está instalado. Verifique se você tem a compilação do Windows Server Insider Preview 17692 ou posterior, se você instalou o Clustering de Failover e que esteja executando espaços de armazenamento diretos.

   > [!NOTE]
   > Esse recurso não está disponível no Windows Server 2016 ou anterior.

### <a name="no-data-available"></a>Não há dados disponíveis 

Se um gráfico mostra "*não há dados disponíveis*" como mostra a figura, aqui está como solucionar problemas:

![Não há dados disponíveis](media/performance-history/no-data-available.png)

1. Se o objeto foi recentemente adicionado ou criado, aguarde para que ele seja descoberto (até 15 minutos).

2. Atualize a página ou aguardar a próxima atualização do plano de fundo (até 30 segundos).

3. Determinados objetos especiais são excluídos do histórico de desempenho – por exemplo, máquinas virtuais que não estão em cluster e volumes que não usam o sistema de arquivos do Cluster CSV (Volume compartilhado). Verifique o subtópico para o tipo de objeto, como [histórico de desempenho para volumes](performance-history-for-volumes.md), para as particularidades.

4. Se o problema persistir, abra o PowerShell como administrador e execute o `Get-ClusterPerf` cmdlet. O cmdlet inclui a solução de problemas de lógica para identificar os problemas mais comuns, por exemplo, se o volume de ClusterPerformanceHistory está ausente e fornece instruções de correção.

5. Se o comando na etapa anterior não retorne nada, você pode tentar reiniciar o serviço de integridade (que coleta o histórico de desempenho), executando `Stop-ClusterResource Health ; Start-ClusterResource Health` no PowerShell.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
