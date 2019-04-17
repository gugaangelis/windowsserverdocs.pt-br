---
title: Entender e ver ressincronização de armazenamento
description: Informações detalhadas sobre quando ocorre a ressincronização de armazenamento e como vê-lo no Windows Server 2019.
keywords: Espaços de armazenamento diretos, ressincronização de armazenamento, ressincronizar, armazenamento, S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009578"
---
# Entender e monitorar ressincronização de armazenamento

>Aplica-se a: Windows Server 2019

Alertas de ressincronização de armazenamento são um novo recurso de [Espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server 2019 que permite que o serviço de integridade lançar uma falha quando o armazenamento é a ressincronização. O alerta é útil para notificar você quando ressincronização está acontecendo, para que você não acidentalmente tirar mais servidores para baixo (o que poderia causar vários domínios de falha de serem afetados, resultando em seu cluster indo para baixo). 

Este tópico fornece o plano de fundo e as etapas para entender e ver ressincronização de armazenamento em um cluster de failover do Windows Server com espaços de armazenamento diretos.

## Ressincronização compreensão

Vamos começar com um exemplo simples para entender como armazenamento obtém fora de sincronia. Lembre-se de que qualquer nada compartilhado (unidades locais somente) solução de armazenamento distribuído exibe esse comportamento. Como você verá abaixo, se um nó de servidor falhar, em seguida, suas unidades não serão atualizadas até que ele fique online novamente - isso é verdadeiro para qualquer arquitetura hiperconvergente. 

Suponha que queremos armazenar a cadeia de caracteres "Olá". 

![ASCII de cadeia de caracteres "Olá"](media/understand-storage-resync/hello.png)

Asssuming que temos a resiliência por espelhamento de três vias, temos três cópias dessa cadeia de caracteres. Agora, se nós derrubará o servidor 1 # temporariamente (para maintanence), nós não pode acessar cópia #1.

![Não é possível acessar a cópia #1](media/understand-storage-resync/copy1.png)

Suponha que atualizamos nossa cadeia de caracteres da "Olá" para "Ajuda!" neste momento.

![ASCII de cadeia de caracteres "Ajuda!"](media/understand-storage-resync/help.png)

Depois que atualizamos a cadeia de caracteres, cópia #2 e 3 # será atualizado com êxito. No entanto, cópia #1 ainda não pode ser acessada como servidor #1 é pressionada temporariamente (para maintanence). 

![GIF da escrita copiar #2 e #2 "](media/understand-storage-resync/write.gif)

Agora, temos cópia #1 que tem dados que está fora de sincronia. O sistema operacional usa região sujo granular de rastreamento para controlar os bits que estão fora de sincronia. Dessa forma, quando o servidor 1 # fique online novamente, podemos podem sincronizar as alterações lendo os dados de cópia #2 ou 3 # e substituir os dados na cópia #1. As vantagens dessa abordagem são que precisamos apenas copiar os dados que estão desatualizados, em vez de a ressincronização todos os dados do servidor #2 ou 3 #.

![GIF de substituição para copiar #1 "](media/understand-storage-resync/overwrite.gif)

Portanto, isso explica como dados obtém fora de sincronia. Mas o que ela aparece em um nível alto? Suponha que para este exemplo que temos um cluster de hiperconvergência de três servidores. Quando o servidor 1 # está em manutenção, você verá-lo como sendo pressionada. Quando você colocar o servidor 1 # fazer backup, ele começará a ressincronização todo seu armazenamento usando o rastreamento de região sujos granular (explicado acima). Depois que os dados são todos novamente em sincronia, todos os servidores serão mostrados como ativo.

![GIF do modo de exibição de administração de ressincronização"](media/understand-storage-resync/admin.gif)

## Como monitorar ressincronização de armazenamento no Windows Server 2019

Agora que você entende como funciona a ressincronização de armazenamento, vejamos como isso é mostrado no Windows Server 2019. Adicionamos uma novo Falha ao [Serviço de integridade](../../failover-clustering/health-service-overview.md) que serão mostradas quando o armazenamento é a ressincronização.

Para ver essa falha no PowerShell, execute:

``` PowerShell
Get-HealthFault
```

Isso é uma falha de novo no Windows Server 2019 e aparecerá no PowerShell, o relatório de validação de cluster e qualquer outro lugar que se baseia nas falhas de integridade. 

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

Em especial, o Windows Admin Center usa falhas de integridade para definir o status e a cor de nós de cluster. Assim, essa nova falha fará com que nós de cluster para fazer a transição de vermelho (para baixo) para amarelo (a ressincronização) para verde (para cima), em vez de passar diretamente de vermelho para verde, no painel de HCI.

![Imagem do modo de exibição de 2016 vs 2019 de ressincronização"](media/understand-storage-resync/compare.png)

Mostrando o progresso geral de ressincronização de armazenamento, você pode saber com precisão a quantidade de dados está fora de sincronia e se o sistema está fazendo progresso. Quando você abre o Windows Admin Center e vá para o *painel*, você verá os novos alertas da seguinte maneira:

![Imagem de alerta no Windows Admin Center"](media/understand-storage-resync/alert.png)

O alerta é útil para notificar você quando ressincronização está acontecendo, para que você não acidentalmente tirar mais servidores para baixo (o que poderia causar vários domínios de falha de serem afetados, resultando em seu cluster indo para baixo). 

Se você navegar para a página de *servidores* no Centro de administração do Windows, clique no *inventário*e escolha um servidor específico, você pode obter uma visão mais detalhada da aparência de ressincronização esse armazenamento em uma base por servidor. Se você navegue até seu servidor e examine o gráfico de *armazenamento* , você verá a quantidade de dados que precisam ser reparado em uma linha com o número exato de *roxo* certa acima. Esse valor será aumentar quando o servidor está desativado (necessidades mais dados tenha de ser ressincronizado) e diminuir gradualmente quando o servidor fique online novamente (dados estão sendo sincronizados). Quando a quantidade de dados que precisa ser reparo é 0, o armazenamento é feita a ressincronização - agora você está livre para colocar um servidor para baixo se você precisar. Uma captura de tela dessa experiência no Windows Admin Center é mostrada abaixo:

![Imagem do modo de exibição do servidor no Windows Admin Center"](media/understand-storage-resync/server.png)

## Como ver ressincronização de armazenamento no Windows Server 2016

Como você pode ver, esse alerta é particularmente útil para obter uma visão abrangente do que está acontecendo na camada de armazenamento. Ele efetivamente resume as informações que você pode obter do cmdlet Get-StorageJob, que retorna informações sobre trabalhos de módulo de armazenamento de longa, como uma operação de reparo em um espaço de armazenamento. Um exemplo é mostrado abaixo:

```PowerShell
Get-StorageJob
```

Veja um exemplo de saída:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Esse modo de exibição é muito mais granular, já que os trabalhos de armazenamento listados são por volume, você pode ver a lista de trabalhos que estão em execução e você pode controlar o andamento individual. Esse cmdlet funciona no Windows Server 2016 e 2019.

## Consulte também

- [Colocar um servidor offline para manutenção](maintain-servers.md)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)