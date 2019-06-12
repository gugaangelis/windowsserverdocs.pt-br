---
title: Afinidade de cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: Este artigo descreve os níveis de afinidade e antiAffinity de cluster de failover
ms.openlocfilehash: 67929e6d3399633ebfec0b908463131973aecaf7
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453033"
---
# <a name="cluster-affinity"></a>Afinidade de cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover pode conter várias funções que podem mover entre nós e executar.  Há vezes quando determinadas funções (ou seja, máquinas virtuais, grupos de recursos, etc.) não devem executar no mesmo nó.  Isso pode ser devido ao consumo de recursos, o uso de memória, etc.  Por exemplo, há duas máquinas virtuais que estão com uso intensivo de CPU e de memória e se as duas máquinas virtuais estão em execução no mesmo nó, uma ou ambas as máquinas virtuais podem apresentar problemas de impacto de desempenho.  Este artigo explicará cluster antiaffinity níveis e como você pode usá-los.

## <a name="what-is-affinity-and-antiaffinity"></a>O que é a afinidade de e AntiAffinity?

A afinidade é uma regra que você configuraria que estabelece uma relação entre duas ou mais funções (i, e, as máquinas virtuais, grupos de recursos, etc) para mantê-los juntos.  AntiAffinity é o mesmo, mas é usado para tentar e manter as funções especificadas distantes um do outro.  Clusters de failover usam AntiAffinity para suas funções.  Mais especificamente, o [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) parâmetro definido em funções para que eles não são executados no mesmo nó.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Ao examinar as propriedades de um grupo, há o parâmetro AntiAffinityClassNames e ele está em branco por padrão.  Nos exemplos a seguir, Grupo1 e grupo2 devem ser separados sejam executados no mesmo nó.  Para exibir a propriedade, o comando do PowerShell e o resultado seria:

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Como AntiAffinityClassNames não são definidos por padrão, esses podem funções execução juntos ou separados.  A meta é mantê-los para ser separados.  O valor de AntiAffinityClassNames pode ser que você quiser que eles sejam, apenas precisam ser iguais.  Digamos que Group1 e grupo2 são controladores de domínio executados em máquinas virtuais e eles poderiam ser mais bem atendidos em execução em nós diferentes.  Como esses são controladores de domínio, eu usarei o controlador de domínio para o nome da classe.  Para definir o valor, o comando do PowerShell e os resultados seria:

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Agora que eles são definidos, o clustering de failover tentará mantê-los separados.  

O parâmetro de AntiAffinityClassName é um bloco "soft".  Ou seja, ele tentará para mantê-los separados, mas se não for possível, ele ainda permitirá que sejam executados no mesmo nó.  Por exemplo, os grupos estão em execução em um cluster de failover com dois nós.  Se precisar de um nó descer para manutenção, ele significa que ambos os grupos devem estar em execução no mesmo nó.  Nesse caso, seria okey ter isso.  Talvez não seja mais ideal, mas as duas máquinas virtial serão ainda em execução dentro de intervalos de um desempenho aceitável.

## <a name="i-need-more"></a>Preciso de mais

Conforme mencionado, AntiAffinityClassNames é um bloqueio suave.  Mas e se um bloco de disco rígido é necessária?  As máquinas virtuais não podem ser executadas nele mesmo nó; Caso contrário, o impacto no desempenho ocorrerá e fazer com que alguns serviços, possivelmente, vá para baixo.

Para esses casos, há uma propriedade de cluster adicional de ClusterEnforcedAntiAffinity.  Esse nível antiaffinity impedirá a todos os custos qualquer um dos valores AntiAffinityClassNames mesmos em execução no mesmo nó.

Para exibir a propriedade e o valor, o comando do PowerShell (e o resultado) seria:

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

O valor de "0" significa que ele é desabilitado e não devem ser aplicadas.  O valor de "1" permite que ele e é o bloco de disco rígido.  Para habilitar este bloco de disco rígido, o comando (e o resultado) são:

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Quando as duas opções forem definidas, o grupo será impedido de entrar online em conjunto.  Se eles estiverem no mesmo nó, isso é o que você veria no Gerenciador de Cluster de Failover.

![Afinidade de cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

Uma lista os grupos do PowerShell, você veria isso:

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Comentários adicionais

- Verifique se que você estiver usando a configuração apropriada AntiAffinity dependendo das necessidades.
- Tenha em mente que em um cenário de dois nós e ClusterEnforcedAntiAffinity, se um nó estiver inativo, ambos os grupos não será executado.  

- O uso de proprietários preferenciais em grupos pode ser combinado com AntiAffinity em um cluster de três ou mais nós.
- As configurações de AntiAffinityClassNames e ClusterEnforcedAntiAffinity ocorrerá somente após uma reciclagem dos recursos. I.E. Você pode defini-las, mas se ambos os grupos estiverem online no mesmo nó quando definido, eles continuarão permanecer online.



