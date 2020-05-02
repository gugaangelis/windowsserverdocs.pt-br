---
title: Afinidade de cluster
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/07/2019
description: Este artigo descreve os níveis de afinidade e antiafinidade de cluster de failover
ms.openlocfilehash: b0c2209680f3c34ac8376d5662620595aff92c0b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720610"
---
# <a name="cluster-affinity"></a>Afinidade de cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover pode conter várias funções que podem se mover entre nós e executar. Há ocasiões em que determinadas funções (ou seja, máquinas virtuais, grupos de recursos, etc.) não devem ser executadas no mesmo nó.  Isso pode ser devido ao consumo de recursos, uso de memória, etc.  Por exemplo, há duas máquinas virtuais com uso intensivo de memória e CPU e, se as duas máquinas virtuais estiverem em execução no mesmo nó, uma ou ambas as máquinas virtuais poderão ter problemas de impacto no desempenho.  Este artigo explicará os níveis de antiafinidade de cluster e como você pode usá-los.

## <a name="what-is-affinity-and-antiaffinity"></a>O que é afinidade e antiafinidade?

Affinity é uma regra que você configuraria que estabelece uma relação entre duas ou mais funções (i, e, máquinas virtuais, grupos de recursos, etc) para mantê-las juntas.  A antiafinidade é a mesma, mas é usada para tentar manter as funções especificadas separadas umas das outras. Os clusters de failover usam a antiafinidade para suas funções.  Mais especificamente, o parâmetro [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) definido nas funções para que eles não sejam executados no mesmo nó.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Ao examinar as propriedades de um grupo, há o parâmetro AntiAffinityClassNames e ele fica em branco como padrão.  Nos exemplos abaixo, GRUPO1 e group2 devem ser separados da execução no mesmo nó.  Para exibir a propriedade, o comando e o resultado do PowerShell seriam:

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Como AntiAffinityClassNames não são definidos como padrão, essas funções podem ser executadas juntas ou separadas.  O objetivo é mantê-los separados.  O valor de AntiAffinityClassNames pode ser o que você deseja que sejam, eles só precisam ser iguais.  Digamos que GRUPO1 e group2 sejam controladores de domínio em execução em máquinas virtuais e eles seriam mais bem atendidos em execução em nós diferentes.  Como esses são controladores de domínio, usarei o DC para o nome da classe.  Para definir o valor, o comando do PowerShell e os resultados seriam:

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Agora que elas estão definidas, o clustering de failover tentará mantê-las separadas.  

O parâmetro definir antiaffinityclassname é um bloco "soft".  Ou seja, ele tentará mantê-los separados, mas se não puder, ele ainda permitirá que eles sejam executados no mesmo nó.  Por exemplo, os grupos estão sendo executados em um cluster de failover de dois nós.  Se um nó precisar ficar inativo para manutenção, significa que ambos os grupos seriam ativos e em execução no mesmo nó.  Nesse caso, seria bom ter isso.  Talvez não seja o mais ideal, mas as duas máquinas virtial ainda serão executadas dentro de intervalos de desempenho aceitáveis.

## <a name="i-need-more"></a>Preciso de mais

Como mencionado, AntiAffinityClassNames é um bloco suave.  Mas e se um bloco de hardware for necessário?  As máquinas virtuais não podem ser executadas no mesmo nó; caso contrário, o impacto no desempenho ocorrerá e fará com que alguns serviços fiquem inativos.

Para esses casos, há uma propriedade de cluster adicional de ClusterEnforcedAntiAffinity.  Esse nível de antiafinidade impedirá que qualquer um dos mesmos valores de AntiAffinityClassNames seja executado no mesmo nó.

Para exibir a propriedade e o valor, o comando do PowerShell (e o resultado) seria:

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

O valor de "0" significa que ele está desabilitado e não deve ser imposto.  O valor de "1" permite e é o bloco físico.  Para habilitar esse bloco físico, o comando (e o resultado) é:

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Quando ambos estiverem definidos, o grupo será impedido de ficar online juntos.  Se eles estiverem no mesmo nó, isso será o que você veria em Gerenciador de Cluster de Failover.

![Afinidade de cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

Em uma listagem do PowerShell dos grupos, você verá isto:

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Comentários Adicionais

- Verifique se você está usando a configuração de antiafinidade apropriada dependendo das necessidades.
- Tenha em mente que, em um cenário de dois nós e ClusterEnforcedAntiAffinity, se um nó estiver inativo, os dois grupos não serão executados.  

- O uso de proprietários preferenciais em grupos pode ser combinado com antiafinidade em um cluster de três ou mais nós.
- As configurações de AntiAffinityClassNames e ClusterEnforcedAntiAffinity só ocorrerão após uma reciclagem dos recursos. ,. Você pode defini-los, mas se ambos os grupos estiverem online no mesmo nó quando definidos, ambos continuarão a permanecer online.
