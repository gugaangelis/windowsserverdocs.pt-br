---
title: Entender e ver a ressincronização do armazenamento
description: Informações detalhadas sobre quando a ressincronização de armazenamento acontece e como vê-la no Windows Server 2019.
keywords: Espaços de Armazenamento Diretos, resincronização de armazenamento, ressincronização, armazenamento, S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 53f48421bddd416d24c5f46e53652cc89c10c785
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402844"
---
# <a name="understand-and-monitor-storage-resync"></a>Entender e monitorar a ressincronização de armazenamento

>Aplica-se a: Windows Server 2019

Os alertas de resincronização de armazenamento são um novo recurso de [espaços de armazenamento diretos](storage-spaces-direct-overview.md) no Windows Server 2019 que permite ao serviço de integridade lançar uma falha quando o armazenamento é ressincronizado. O alerta é útil para notificá-lo quando a ressincronização estiver acontecendo, para que você não demore acidentalmente mais servidores (o que pode causar a falha de vários domínios de falhas, resultando no desligamento do seu cluster). 

Este tópico fornece o plano de fundo e as etapas para entender e ver a resincronização de armazenamento em um cluster de failover do Windows Server com o Espaços de Armazenamento Diretos.

## <a name="understanding-resync"></a>Compreendendo a ressincronização

Vamos começar com um exemplo simples para entender como o armazenamento fica fora de sincronia. Tenha em mente que nenhuma solução de armazenamento distribuído de nada compartilhado (somente unidades locais) exibe esse comportamento. Como você verá abaixo, se um nó de servidor ficar inativo, suas unidades não serão atualizadas até que ele volte a ficar online-isso é verdade para qualquer arquitetura hiperconvergente. 

Suponha que desejamos armazenar a cadeia de caracteres "Olá". 

![ASCII da cadeia de caracteres "Olá"](media/understand-storage-resync/hello.png)

Asssuming que temos resiliência de espelho de três vias, temos três cópias dessa cadeia de caracteres. Agora, se levarmos o servidor #1 temporariamente (para manutenção), não será possível acessar a cópia #1.

![Não é possível acessar a cópia #1](media/understand-storage-resync/copy1.png)

Suponha que nós atualizamos nossa cadeia de caracteres de "Olá" para "ajuda!" neste momento.

![ASCII da cadeia de caracteres "ajuda!"](media/understand-storage-resync/help.png)

Depois de atualizarmos a cadeia de caracteres, copie #2 e #3 serão atualizados com êxito. No entanto, a cópia #1 ainda não pode ser acessada porque o servidor #1 está inoperante temporariamente (para manutenção). 

![Gif de gravação para copiar #2 e #2 "](media/understand-storage-resync/write.gif)

Agora, temos a cópia #1 que tem dados fora de sincronia. O sistema operacional usa o rastreamento de região suja granular para controlar os bits que estão fora de sincronia. Dessa forma, quando o servidor #1 volta a ficar online, podemos sincronizar as alterações lendo os dados da cópia #2 ou #3 e substituir os dados em #1 de cópia. As vantagens dessa abordagem são que precisamos apenas copiar os dados que estão obsoletos, em vez de sincronizar todos os dados do servidor #2 ou #3 do servidor.

![Gif de substituição para copiar #1 "](media/understand-storage-resync/overwrite.gif)

Portanto, isso explica como os dados ficam fora de sincronia. Mas o que isso parece mais em um alto nível? Suponha que, neste exemplo, tenhamos um cluster hiperconvergente de três servidores. Quando o servidor #1 estiver em manutenção, você o verá como estando inativo. Quando você coloca o servidor #1 backup, ele começa a ressincronizar todo o seu armazenamento usando o controle de região sujo granular (explicado acima). Depois que os dados estiverem todos de volta na sincronização, todos os servidores serão mostrados como ativos.

![Gif da exibição do administrador de ressincronização "](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>Como monitorar a resincronização de armazenamento no Windows Server 2019

Agora que você entende como o armazenamento de ressincronização funciona, vamos ver como isso é mostrado no Windows Server 2019. Adicionamos uma nova falha à [serviço de integridade](../../failover-clustering/health-service-overview.md) que será exibida quando o armazenamento estiver sendo ressincronizado.

Para exibir essa falha no PowerShell, execute:

``` PowerShell
Get-HealthFault
```

Essa é uma nova falha no Windows Server 2019 e aparecerá no PowerShell, no relatório de validação de cluster e em qualquer outro lugar que se baseie em falhas de integridade. 

Para obter uma exibição mais profunda, você pode consultar o banco de dados de série temporal no PowerShell da seguinte maneira:

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

Notavelmente, o centro de administração do Windows usa falhas de integridade para definir o status e a cor dos nós de cluster. Assim, essa nova falha fará com que os nós de cluster façam a transição de vermelho (para baixo) para amarelo (ressincronização) para verde (para cima), em vez de passar diretamente de vermelho para verde, no painel do HCI.

![Imagem da exibição de 2016 vs 2019 de ressincronização "](media/understand-storage-resync/compare.png)

Ao mostrar o progresso geral da ressincronização do armazenamento, você pode saber com precisão a quantidade de dados fora de sincronia e se o seu sistema está progredindo para o futuro. Ao abrir o centro de administração do Windows e ir para o *painel*, você verá o novo alerta da seguinte maneira:

![Imagem de alerta no centro de administração do Windows "](media/understand-storage-resync/alert.png)

O alerta é útil para notificá-lo quando a ressincronização estiver acontecendo, para que você não demore acidentalmente mais servidores (o que pode causar a falha de vários domínios de falhas, resultando no desligamento do seu cluster). 

Se você navegar até a página *servidores* no centro de administração do Windows, clicar em *inventário*e, em seguida, escolher um servidor específico, poderá obter uma visão mais detalhada de como essa ressincronização de armazenamento é procurada por servidor. Se você navegar até o servidor e examinar o gráfico de *armazenamento* , verá a quantidade de dados que precisa ser reparada em uma linha *roxa* com o número exato direito acima. Esse valor aumentará quando o servidor estiver inoperante (mais dados precisa ser ressincronizado) e diminuir gradualmente quando o servidor voltar a ficar online (os dados estão sendo sincronizados). Quando a quantidade de dados que precisa ser reparada for 0, seu armazenamento será feito com a ressincronização – agora você está livre para retirar um servidor se precisar. Uma captura de tela dessa experiência no centro de administração do Windows é mostrada abaixo:

![Imagem da exibição de servidor no centro de administração do Windows "](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>Como ver o armazenamento de ressincronização no Windows Server 2016

Como você pode ver, esse alerta é particularmente útil para obter uma visão holística do que está acontecendo na camada de armazenamento. Ele resume efetivamente as informações que você pode obter do cmdlet Get-StorageJob, que retorna informações sobre trabalhos de módulo de armazenamento de longa execução, como uma operação de reparo em um espaço de armazenamento. Um exemplo é mostrado abaixo:

```PowerShell
Get-StorageJob
```

Veja o exemplo de saída:

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

Essa exibição é muito mais granular, já que os trabalhos de armazenamento listados são por volume, você pode ver a lista de trabalhos em execução e pode acompanhar o progresso individual. Esse cmdlet funciona no Windows Server 2016 e 2019.

## <a name="see-also"></a>Consulte também

- [Colocar um servidor offline para manutenção](maintain-servers.md)
- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)