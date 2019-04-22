---
title: Entender e ver a ressincronização de armazenamento
description: Informações detalhadas sobre quando ocorre a ressincronização de armazenamento e como vê-lo no Windows Server 2019.
keywords: Espaços de armazenamento diretos, de ressincronização de armazenamento de ressincronização, armazenamento, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813457"
---
# <a name="understand-and-monitor-storage-resync"></a>Entender e monitorar a ressincronização de armazenamento

>Aplica-se a: Windows Server 2019

Alertas de ressincronização de armazenamento são uma nova funcionalidade do [espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server 2019 que permite que o serviço de integridade gerar uma falha quando o armazenamento é a ressincronização. O alerta é útil para notificar você quando a ressincronização está ocorrendo, para que você não acidentalmente colocar mais servidores para baixo (o que pode fazer com que vários domínios de falha seja afetado, resultando em seu cluster ficar inativo). 

Este tópico fornece o fundamento e as etapas para compreender e ver a ressincronização de armazenamento em um cluster de failover do Windows Server com espaços de armazenamento diretos.

## <a name="understanding-resync"></a>Ressincronização Noções básicas sobre

Vamos começar com um exemplo simples para entender como o armazenamento fica fora de sincronizado. Tenha em mente que qualquer nada compartilhado (unidades locais somente) solução de armazenamento distribuído exibe esse comportamento. Como você verá abaixo, se um nó de servidor ficar inativo, em seguida, suas unidades não serão atualizadas até que ele ficar online novamente - isso é verdadeiro para qualquer arquitetura hiperconvergente. 

Suponha que desejamos armazenar a cadeia de caracteres "HELLO". 

![ASCII da cadeia de caracteres "hello"](media/understand-storage-resync/hello.png)

Asssuming que temos de resiliência de espelho de três vias, temos três cópias dessa cadeia de caracteres. Agora, se podemos desativar o servidor 1 # temporariamente (para manutenção), podemos não é possível acessar cópia #1.

![Não é possível acessar a cópia #1](media/understand-storage-resync/copy1.png)

Suponha que atualizamos nossa cadeia de caracteres de "Olá" para "HELP"! neste momento.

![ASCII da cadeia de caracteres "help"!](media/understand-storage-resync/help.png)

Depois que atualizarmos a cadeia de caracteres, a cópia #2 e 3 de # será atualizado com êxito. No entanto, a cópia #1 ainda não pode ser acessada porque o servidor #1 está desativado temporariamente (para manutenção). 

![GIF da gravação copiar 2 # e #2 "](media/understand-storage-resync/write.gif)

Agora, temos cópia #1, que tem dados que estão fora de sincronia. O sistema operacional usa de região suja granular de controle para controlar os bits que estão fora de sincronizado. Dessa forma, quando o servidor 1 # fica online novamente, seja possível sincronizar as alterações ao ler os dados de cópia 2 # ou #3 e substituindo os dados na cópia #1. As vantagens dessa abordagem são que precisamos apenas copiar os dados que estão obsoletos, em vez de a ressincronização todos os dados de servidor 2 # ou #3 do servidor.

![GIF de substituição para copiar #1 "](media/understand-storage-resync/overwrite.gif)

Portanto, isso explica como os dados fica fora de sincronizado. Mas qual a aparência de alto nível? Suponha para este exemplo que temos um cluster hiperconvergente de três servidores. Quando o servidor 1 # está em manutenção, você verá ele como desativado. Quando você colocar o servidor 1 # fazer backup, ele iniciará a ressincronização todo seu armazenamento usando o controle granular de região suja (explicado acima). Depois que os dados novamente, todos em sincronia, todos os servidores serão mostrados como em execução.

![GIF da exibição do administrador de ressincronização"](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Como monitorar a ressincronização de armazenamento no Windows Server 2019

Agora que você entende como funciona a ressincronização de armazenamento, vejamos como isso aparece no Windows Server 2019. Adicionamos uma nova falha para o [serviço de integridade](../../failover-clustering/health-service-overview.md) que será exibido quando o armazenamento é a ressincronização.

Para exibir essa falha no PowerShell, execute:

``` PowerShell
Get-HealthFault
```

Isso é uma falha de novo no Windows Server 2019 e aparecerão no PowerShell, no relatório de validação de cluster e qualquer outro lugar que se baseia nas falhas de integridade. 

Para obter uma visão mais profunda, você pode consultar o banco de dados de série de tempo no PowerShell da seguinte maneira:

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
Veja a seguir um exemplo de saída:

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

Em especial, o Windows Admin Center usa falhas de integridade para definir o status e a cor de nós de cluster. Assim, essa nova falha fará com que nós de cluster para fazer a transição de vermelho (para baixo) para amarelo (a ressincronização) como verde (para cima), em vez de passar direto de vermelho, verde, no painel HCI.

![Imagem de exibição de 2016 vs 2019 de ressincronização"](media/understand-storage-resync/compare.png)

Mostrando o progresso geral de ressincronização de armazenamento, você pode saber com precisão a quantidade de dados está fora de sincronização e se o seu sistema está fazendo progresso. Quando você abrir o Windows Admin Center e vá para o *Dashboard*, você verá o novo alerta da seguinte maneira:

![Imagem de alerta no Windows Admin Center "](media/understand-storage-resync/alert.png)

O alerta é útil para notificar você quando a ressincronização está ocorrendo, para que você não acidentalmente colocar mais servidores para baixo (o que pode fazer com que vários domínios de falha seja afetado, resultando em seu cluster ficar inativo). 

Se você navegar para o *servidores* página no Windows Admin Center, clique em *inventário*e, em seguida, escolha um servidor específico, você pode obter uma visão mais detalhada de como a ressincronização de armazenamento aparece em uma base por servidor. Se você navegar até seu servidor e examine os *armazenamento* gráfico, você verá a quantidade de dados que precisa ser reparado em um *roxa* linha com o número exato logo acima. Essa quantidade aumente quando o servidor estiver inativo (mais dados precisa ser resynced) e diminuir gradualmente quando o servidor fica online novamente (dados estiverem sendo sincronizados). Quando a quantidade de dados que precisa ser reparo é 0, o armazenamento é feita a ressincronização - agora você está livre para desativar um servidor se você precisar. Abaixo está uma captura de tela essa experiência em Windows Admin Center:

![Imagem de exibição do servidor no Windows Admin Center "](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Como ver a ressincronização de armazenamento no Windows Server 2016

Como você pode ver, esse alerta é particularmente útil para obter uma visão holística do que está acontecendo na camada de armazenamento. Ele efetivamente resume as informações que você pode obter do cmdlet Get-StorageJob, que retorna informações sobre trabalhos de módulo de armazenamento de longa execução, como uma operação de reparo em um espaço de armazenamento. Um exemplo é mostrado abaixo:

```PowerShell
Get-StorageJob
```

Aqui está a saída de exemplo:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Este modo de exibição é muito mais granular, pois os trabalhos de armazenamento listados são por volume, você pode ver a lista de trabalhos que estão em execução e você pode acompanhar seu progresso individual. Esse cmdlet funciona no Windows Server 2016 e de 2019.

## <a name="see-also"></a>Consulte também

- [Colocar um servidor offline para manutenção](maintain-servers.md)
- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)