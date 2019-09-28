---
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: Executar a Eliminação de Duplicação de Dados
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 86aff55b4c548ccf4fcbb04cc477dd63a889bebd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403204"
---
# <a name="running-data-deduplication"></a>Executar a Eliminação de Duplicação de Dados

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

## <a id="running-dedup-jobs-manually"></a>Executando trabalhos de eliminação de duplicação de dados manualmente

Você pode executar todo trabalho de Eliminação de Duplicação de Dados agendado manualmente usando os seguintes cmdlets do PowerShell:
* [`Start-DedupJob`](https://technet.microsoft.com/library/hh848442.aspx): Inicia um novo trabalho de eliminação de duplicação de dados
* [`Stop-DedupJob`](https://technet.microsoft.com/library/hh848439.aspx): Para um trabalho de eliminação de duplicação de dados já em andamento (ou remove-o da fila)
* [`Get-DedupJob`](https://technet.microsoft.com/library/hh848452.aspx): Mostra todos os trabalhos de eliminação de duplicação de dados ativos e em fila

Todas as [configurações disponíveis ao agendar um trabalho de Eliminação de Duplicação de Dados](advanced-settings.md#modifying-job-schedules-available-settings) também estão disponíveis quando você inicia um trabalho manualmente, exceto para as configurações específicas do agendamento. Por exemplo, para iniciar um trabalho de [Otimização](understand.md#job-info-optimization) manualmente com prioridade alta e uso máximo de CPU e de memória, execute o seguinte comando do PowerShell com privilégio de administrador:

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a id="monitoring-dedup"></a>Monitoramento de eliminação de duplicação de dados

### <a id="monitoring-dedup-job-successes"></a>Sucessos do trabalho

Como a Eliminação de Duplicação de Dados usa um modelo de pós-processamento, é importante que os [trabalhos de Eliminação de Duplicação de Dados](understand.md#job-info) sejam bem-sucedidos. Uma maneira fácil de verificar o status do trabalho mais recente é usar o cmdlet do PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx). Verifique periodicamente os seguintes campos:

* Para o [Trabalho de otimização](understand.md#job-info-optimization), examine `LastOptimizationResult` (0 = Êxito), `LastOptimizationResultMessage` e `LastOptimizationTime` (deve ser recente).
* Para o [trabalho de Coleta de Lixo](understand.md#job-info-gc), examine `LastGarbageCollectionResult` (0 = Êxito), `LastGarbageCollectionResultMessage` e `LastGarbageCollectionTime` (deve ser recente).
* Para o [trabalho de Depuração de Integridade](understand.md#job-info-scrubbing), examine `LastScrubbingResult` (0 = Êxito), `LastScrubbingResultMessage` e `LastScrubbingTime` (deve ser recente).

> [!Note]  
> Mais detalhes sobre falhas e êxitos de trabalhos podem ser encontrados no Visualizador de Eventos do Windows em `\Applications and Services Logs\Windows\Deduplication\Operational`.

### <a id="monitoring-dedup-optimization-rates"></a>Taxas de otimização

Um indicador de falha do [Trabalho de otimização](understand.md#job-info-optimization) é uma taxa de otimização com tendência para baixo, que pode indicar que os Trabalhos de otimização não estão acompanhando a taxa de alterações ou variação. Você pode verificar a taxa de otimização usando o cmdlet do PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx).

> [!Important]
> `Get-DedupStatus` tem dois campos que são relevantes para a taxa de otimização: `OptimizedFilesSavingsRate` e `SavingsRate`. Esses são dois valores importantes para rastrear, mas cada um tem um significado exclusivo.
> - `OptimizedFilesSavingsRate` aplica-se somente aos arquivos que são "in-Policy" para otimização (`space used by optimized files after optimization / logical size of optimized files`).
> - `SavingsRate` aplica-se a todo o volume (`space used by optimized files after optimization / total logical size of the optimization`).

## <a id="disabling-dedup"></a>Desabilitando a eliminação de duplicação de dados
Para desligar a Eliminação de Duplicação de Dados, execute o [Trabalho de cancelamento da otimização](understand.md#job-info-unoptimization). Para desfazer a otimização de volume, execute o seguinte comando:

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]  
> O trabalho de Cancelamento da otimização falhará se o volume não tiver espaço suficiente para manter os dados não otimizados.

## <a id="faq"></a>Perguntas frequentes
**Há um pacote de gerenciamento System Center Operations Manager disponível para monitorar a eliminação de duplicação de dados?**  
Sim. A Eliminação de Duplicação de Dados pode ser monitorada por meio do Pacote de gerenciamento do System Center para o Servidor de Arquivos. Para obter mais informações, consulte o documento [Guide for System Center Management Pack for File Server 2012 R2](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc) (Guia de Pacote de Gerenciamento do System Center para Servidor de Arquivos 2012 R2).
