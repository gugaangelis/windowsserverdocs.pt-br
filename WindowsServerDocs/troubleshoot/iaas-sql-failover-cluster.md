---
title: Ajustar o limite de rede de linha de base de failover
description: Este artigo apresenta soluções para ajustar o limite de redes de cluster de failover.
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 9f28aa9c10fe64e0b86a405c1feb480396bcb76b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965803"
---
# <a name="iaas-with-sql-alwayson---tuning-failover-cluster-network-thresholds"></a>IaaS com SQL AlwaysOn - Ajustando Limites de Rede de Cluster de Failover

Este artigo apresenta soluções para ajustar o limite de redes de cluster de failover.

## <a name="symptom"></a>Sintoma

Ao executar os nós de cluster de failover do Windows no IaaS com SQL Server AlwaysOn, é recomendável alterar a configuração de cluster para um estado de monitoramento mais relaxado. As configurações de cluster prontas são restritivas e podem causar interrupções desnecessárias. As configurações padrão são projetadas para redes locais altamente ajustadas e não levam em conta a possibilidade de latência induzida causada por um ambiente multilocatário, como o Windows Azure (IaaS).

O clustering de failover do Windows Server está monitorando constantemente as conexões de rede e a integridade dos nós em um cluster do Windows.  Se um nó não puder ser acessado pela rede, uma ação de recuperação será executada para recuperar e colocar os aplicativos e os serviços online em outro nó no cluster. A latência na comunicação entre os nós de cluster pode levar ao seguinte erro:

> Erro 1135 (log de eventos do sistema)

O nó de cluster **Node1** foi removido da Associação de cluster de failover ativa. O Serviço de cluster neste nó pode ter sido interrompido. Isso também pode ser devido ao nó ter perdido a comunicação com outros nós ativos no cluster de failover. Execute o assistente para validar uma configuração para verificar sua configuração de rede. Se a condição persistir, verifique se há erros de hardware ou software relacionados aos adaptadores de rede neste nó. Verifique também se há falhas em quaisquer outros componentes de rede aos quais o nó está conectado, como hubs, comutadores ou pontes.

Exemplo de cluster. log:

```console
0000ab34.00004e64::2014/06/10-07:54:34.099 DBG   [NETFTAPI] Signaled NetftRemoteUnreachable event, local address 10.xx.x.xxx:3343 remote address 10.x.xx.xx:3343
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] got event: Remote endpoint 10.xx.xx.xxx:~3343~ unreachable from 10.xx.x.xx:~3343~
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Marking Route from 10.xxx.xxx.xxxx:~3343~ to 10.xxx.xx.xxxx:~3343~ as down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] Checking to see if all routes for route (virtual) local fexx::xxx:5dxx:xxxx:3xxx:~0~ to remote xxx::cxxx:xxxd:xxx:dxxx:~0~ are down
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [NDP] All routes for route (virtual) local fxxx::xxxx:5xxx:xxxx:3xxx:~0~ to remote fexx::xxxx:xxxx:xxxx:xxxx:~0~ are down
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [CORE] Node 8: executing node 12 failed handlers on a dedicated thread
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: Cleaning up connections for n12.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [Nodename] Clearing 0 unsent and 15 unacknowledged messages.
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: n12 node object is closing its connections
0000ab34.00008b68::2014/06/10-07:54:34.099 INFO  [DCM] HandleNetftRemoteRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 1: Old: 05.936, Message: Response, Route sequence: 150415, Received sequence: 150415, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:28.000, Ticks since last sending: 4
0000ab34.00007328::2014/06/10-07:54:34.099 INFO  [NODE] Node 8: closing n12 node object channels
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 2: Old: 06.434, Message: Request, Route sequence: 150414, Received sequence: 150402, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.665, Ticks since last sending: 36
0000ab34.0000a8ac::2014/06/10-07:54:34.099 INFO  [DCM] HandleRequest: dcm/netftRouteChange
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 3: Old: 06.934, Message: Response, Route sequence: 150414, Received sequence: 150414, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:27.165, Ticks since last sending: 4
0000ab34.00004b38::2014/06/10-07:54:34.099 INFO  [IM] Route history 4: Old: 07.434, Message: Request, Route sequence: 150413, Received sequence: 150401, Heartbeats counter/threshold: 5/5, Error: Success, NtStatus: 0 Timestamp: 2014/06/10-07:54:26.664, Ticks since last sending: 36
```

```console
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realLocal>10.xxx.xx.xxx:~3343~</realLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <realRemote>10.xxx.xx.xxx:~3343~</realRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualLocal>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualLocal>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <virtualRemote>fexx::xxxx:xxxx:xxxx:xxxx:~0~</virtualRemote>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Delay>1000</Delay>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Threshold>5</Threshold>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Priority>140481</Priority>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO    <Attributes>2147483649</Attributes>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO  </struct mscs::FaultTolerantRoute>
0000ab34.00007328::2014/06/10-07:54:34.100 INFO   removed
```

```console
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: Lost quorum (3 4 5 6 7 8)
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   [QUORUM] Node 8: goingAway: 0, core.IsServiceShutdown: 0
0000ab34.0000a7c0::2014/06/10-07:54:38.433 ERR   lost quorum (status = 5925)
```

## <a name="cause"></a>Causa

Há duas configurações que são usadas para configurar a integridade da conectividade do cluster.

**Atraso** – define a frequência com que as pulsações de cluster são enviadas entre nós.  O atraso é o número de segundos antes que a próxima pulsação seja enviada.  No mesmo cluster, pode haver atrasos diferentes entre os nós na mesma sub-rede e entre os nós, que estão em sub-redes diferentes.

**Limite** – define o número de pulsações que são perdidas antes que o cluster aceite a ação de recuperação.  O limite é um número de pulsações.  No mesmo cluster, pode haver limites diferentes entre os nós na mesma sub-rede e entre os nós que estão em sub-redes diferentes.

Por padrão, o Windows Server 2016 define o **SameSubnetThreshold** como 10 e **SameSubnetDelay** como 1000 MS. Por exemplo, se o monitoramento de conectividade falhar por 10 segundos, o limite de failover será atingido, resultando no inalcançável que o nó está sendo removido da associação do cluster. Isso faz com que os recursos sejam movidos para outro nó disponível no cluster. Erros de cluster serão relatados, incluindo o erro de cluster 1135 (acima) é relatado.

## <a name="resolution"></a>Resolução

Em um ambiente de IaaS, relaxe as definições de configuração de rede de cluster.

### <a name="steps-to-verify-current-configuration"></a>Etapas para verificar a configuração atual

Verifique os parâmetros de configuração de rede de cluster atuais use o comando Get-cluster:

```powershell
C:\Windows\system32> get-cluster | fl *subnet*
```

Valores padrão, mínimo, máximo e recomendado para cada sistema operacional de suporte

| Descrição | Sistema operacional | Mín | Max | Padrão | Recomendado |
|--|--|--|--|--|--|
| CrossSubnetThreshold | 2008 R2 | 3 | 20 | 5 | 20 |
| Limite de CrossSubnet | 2012 | 3 | 120 | 5 | 20 |
| Limite de CrossSubnet | 2012 R2 | 3 | 120 | 5 | 20 |
| Limite de CrossSubnet | 2016 | 3 | 120 | 20 | 20 |
| Limite de SameSubnet | 2008 R2 | 3 | 10 | 5 | 10 |
| Limite de SameSubnet | 2012 | 3 | 120 | 5 | 10 |
| Limite de SameSubnet | 2012 R2 | 3 | 120 | 5 | 10 |
| SameSubnetThreshold | 2016 | 3 | 120 | 10 | 10 |

Os valores para limite refletem as recomendações atuais referentes ao escopo da implantação, conforme descrito no seguinte artigo:

[Ajuste fino dos limites de rede de cluster de failover no Windows Server 2012 R2](https://support.microsoft.com/en-us/help/3153887/fine-tuning-failover-cluster-network-thresholds-in-windows-server-2012)

O **limite** define o número de pulsações que são perdidas antes que o cluster receba a ação de recuperação.  O limite é um número de pulsações.  No mesmo cluster, pode haver limites diferentes entre os nós na mesma sub-rede e entre os nós, que estão em sub-redes diferentes.

## <a name="recommendations-for-changing-to-more-relaxed-settings-for-multi-tenant-environments-like-azure-iaas"></a>Recomendações para alterar para configurações mais reduzidas para ambientes multilocatário como Azure (IaaS)

> [!NOTE]
> Aumentar a resiliência do ambiente de cluster ajustando as definições de configuração de rede de cluster pode resultar em maior tempo de inatividade. Para obter mais informações, consulte [ajustando limites de rede de cluster de failover](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

1. Modificar para configurações mais relaxadas:

    > [!NOTE]
    > A alteração do limite do cluster entrará em vigor imediatamente, você não precisará reiniciar o cluster ou todos os recursos.

    As configurações a seguir são recomendadas para a mesma sub-rede e implantações entre regiões de grupos de disponibilidade AlwaysOn.

    ```powershell
    C:\Windows\system32> (get-cluster).SameSubnetThreshold = 20
    ```

    ```powershell
    C:\Windows\system32> (get-cluster).CrossSubnetThreshold = 20
    ```

2. Verifique as alterações:

    ```powershell
    C:\Windows\system32> get-cluster | fl *subnet*
    ```

    :::image type="content" source="media/iaas-sql-failover-cluster/cmd.png" alt-text="cmd" border="false":::

## <a name="references"></a>Referências

Para obter mais informações sobre como ajustar as definições de configuração de rede do cluster do Windows, consulte [ajustando limites de rede de cluster de failover](https://techcommunity.microsoft.com/t5/failover-clustering/tuning-failover-cluster-network-thresholds/ba-p/371834).

Para obter informações sobre como usar cluster.exe para ajustar as definições de configuração de rede do cluster do Windows, consulte [como configurar redes de cluster para um cluster de failover](/previous-versions/office/exchange-server-2007/bb690953(v=exchg.80)?redirectedfrom=MSDN).
