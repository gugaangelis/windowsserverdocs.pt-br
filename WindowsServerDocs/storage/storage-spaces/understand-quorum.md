---
title: Noções básicas de quorum de cluster e de pool
description: Compreender o quorum do cluster e do pool, com exemplos específicos para percorrer as complicações.
ms.prod: windows-server
ms.author: adagashe
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: f13affc3ef15c3a39f4fd3839506897f7807d93a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820989"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Noções básicas de quorum de cluster e de pool

>Aplica-se a: Windows Server 2019, Windows Server 2016

O [Windows Server failover clustering](../../failover-clustering/failover-clustering-overview.md) fornece alta disponibilidade para cargas de trabalho. Esses recursos serão considerados altamente disponíveis se os nós que hospedam recursos estiverem ativos; no entanto, o cluster geralmente requer mais da metade dos nós em execução, o que é conhecido como tendo *Quorum*.

O quorum foi projetado para evitar cenários de *divisão-Brain* que podem ocorrer quando há uma partição na rede e subconjuntos de nós não podem se comunicar entre si. Isso pode fazer com que subconjuntos de nós tentem possuir a carga de trabalho e gravar no mesmo disco, o que pode levar a vários problemas. No entanto, isso é evitado com o conceito de quorum do clustering de failover que força apenas um desses grupos de nós a continuar em execução, de modo que apenas um desses grupos permanecerá online.

O quorum determina o número de falhas que o cluster pode sustentar enquanto ainda permanece online. O quorum foi projetado para lidar com o cenário quando há um problema com a comunicação entre subconjuntos de nós de cluster, para que vários servidores não tentem hospedar um grupo de recursos simultaneamente e gravem no mesmo disco ao mesmo tempo. Ao ter esse conceito de quorum, o cluster forçará o serviço de cluster a parar em um dos subconjuntos de nós para garantir que haja apenas um verdadeiro proprietário de um determinado grupo de recursos. Depois que os nós que foram interrompidos puderem se comunicar novamente com o grupo principal de nós, eles reingressarão automaticamente no cluster e iniciarão o serviço de cluster.

No Windows Server 2019 e no Windows Server 2016, há dois componentes do sistema que têm seus próprios mecanismos de quorum:

- **Quorum do cluster**: isso opera no nível do cluster (ou seja, você pode perder os nós e fazer com que o cluster fique ativo)
- **Quorum do pool**: isso opera no nível do pool quando espaços de armazenamento diretos está habilitado (ou seja, você pode perder nós e unidades e fazer com que o pool fique ativo). Os pools de armazenamento foram projetados para serem usados em cenários clusterizados e não clusterizados, motivo pelo qual eles têm um mecanismo de quorum diferente.

## <a name="cluster-quorum-overview"></a>Visão geral do quorum do cluster

A tabela a seguir fornece uma visão geral dos resultados de quorum do cluster por cenário:

| Nós de servidor | Pode sobreviver a uma falha de nó de servidor | Pode sobreviver a uma falha de nó de servidor e, em seguida, a outra | Pode sobreviver a duas falhas de nó de servidor simultâneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | 50/50                                             | Não                                                 |
| 3 + testemunha  | Sim                                 | Sim                                               | Não                                                 |
| 4            | Sim                                 | Sim                                               | 50/50                                              |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e acima  | Sim                                 | Sim                                               | Sim                                                |

### <a name="cluster-quorum-recommendations"></a>Recomendações de quorum de cluster

- Se você tiver dois nós, uma testemunha será **necessária**.
- Se você tiver três ou quatro nós, a testemunha é **altamente recomendável**.
- Se você tiver acesso à Internet, use uma  **[testemunha de nuvem](../../failover-clustering/deploy-cloud-witness.md)**
- Se você estiver em um ambiente de ti com outros computadores e compartilhamentos de arquivos, use uma testemunha de compartilhamento de arquivos

## <a name="how-cluster-quorum-works"></a>Como o quorum de cluster funciona

Quando os nós falham ou quando algum subconjunto de nós perde o contato com outro subconjunto, os nós restantes precisam verificar se constituem a *maioria* do cluster para permanecer online. Se eles não conseguirem verificar, eles ficarão offline.

Mas o conceito de *maioria* só funciona corretamente quando o número total de nós no cluster é estranho (por exemplo, três nós em um cluster de cinco nós). Então, e quanto aos clusters com um número par de nós (digamos, um cluster de quatro nós)?

Há duas maneiras pelas quais o cluster pode tornar o *número total de votos* ímpares:

1. Primeiro, ele *pode subir um* adicionando uma *testemunha* com um voto extra. Isso requer a configuração do usuário.
2.    Ou então, ele pode ficar *abaixo* de um voto de um nó diferente de sorte (ocorre automaticamente conforme necessário).

Sempre que os nós restantes verificarem com êxito se forem a *maioria*, a definição da *maioria* será atualizada para estar entre apenas o os sobreviventes. Isso permite que o cluster perca um nó, então outro, então outro, e assim por diante. Esse conceito do *número total de votos* se adaptando após falhas sucessivas é conhecido como ***Quorum dinâmico***.  

### <a name="dynamic-witness"></a>Testemunha dinâmica

A testemunha dinâmica alterna o voto da testemunha para garantir que o *número total de votos* seja ímpar. Se houver um número ímpar de votos, a testemunha não terá um voto. Se houver um número par de votos, a testemunha terá um voto. A testemunha dinâmica reduz significativamente o risco de o cluster ficar inativo devido a uma falha de testemunha. O cluster decide se o voto de testemunha deve ser usado com base no número de nós de votação disponíveis no cluster.

O quorum dinâmico funciona com a testemunha dinâmica da maneira descrita abaixo.

### <a name="dynamic-quorum-behavior"></a>Comportamento de quorum dinâmico

- Se você tiver um número **par** de nós e nenhuma testemunha, *um nó obterá seu voto zerado*. Por exemplo, apenas três dos quatro nós recebem votos, portanto, o *número total de votos* é três e dois os sobreviventes com votos são considerados uma maioria.
- Se você tiver um número **ímpar** de nós e nenhuma testemunha, *todos eles receberão votos*.
- Se você tiver um número **par** de nós, mais testemunha, *os votos de testemunha*, o total será ímpar.
- Se você tiver um número **ímpar** de nós mais testemunha, *a testemunha não votará*.

O quorum dinâmico habilita a capacidade de atribuir um voto a um nó dinamicamente para evitar perder a maioria dos votos e permitir que o cluster seja executado com um nó (conhecido como última posição no último homem). Vamos pegar um cluster de quatro nós como exemplo. Suponha que o quorum exija 3 votos. 

Nesse caso, o cluster terá ficado inativo se você tiver perdido dois nós.

![Diagrama mostrando quatro nós de cluster, cada um deles recebe um voto](media/understand-quorum/dynamic-quorum-base.png)

No entanto, o quorum dinâmico impede que isso aconteça. O *número total de votos* necessários para o quorum agora é determinado com base no número de nós disponíveis. Portanto, com o quorum dinâmico, o cluster ficará ativo mesmo se você perder três nós.

![Diagrama mostrando quatro nós de cluster, com nós falhando um de cada vez e o número de votos necessários que se ajustam após cada falha.](media/understand-quorum/dynamic-quorum-step-through.png)

O cenário acima se aplica a um cluster geral que não tem Espaços de Armazenamento Diretos habilitado. No entanto, quando Espaços de Armazenamento Diretos está habilitado, o cluster só pode dar suporte a duas falhas de nó. Isso é explicado mais na [seção quorum do pool](#pool-quorum-overview).

### <a name="examples"></a>Exemplos

#### <a name="two-nodes-without-a-witness"></a>Dois nós sem uma testemunha. 
O voto de um nó é zerado, portanto, o voto da *maioria* é determinado por um total de **1 voto**. Se o nó não votante ficar inativo inesperadamente, o sobrevivente terá 1/1 e o cluster sobreviver. Se o nó de votação ficar inativo inesperadamente, o sobrevivente terá 0/1 e o cluster ficará inativo. Se o nó de votação estiver normalmente desligado, o voto será transferido para o outro nó e o cluster sobreviver. ***É por isso que é essencial configurar uma testemunha.***

![Quorum explicado no caso de dois nós sem uma testemunha](media/understand-quorum/2-node-no-witness.png)

- Pode sobreviver a uma falha do servidor: **50% de chance**.
- Pode sobreviver a uma falha de servidor e, em seguida, outra: **não**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **não**. 

#### <a name="two-nodes-with-a-witness"></a>Dois nós com uma testemunha. 
Ambos os nós votam, além dos votos de testemunha, portanto, a *maioria* é determinada em um total de **três votos**. Se um dos nós ficar inativo, o sobrevivente terá 2/3 e o cluster sobreviver.

![Quorum explicado no caso de dois nós com uma testemunha](media/understand-quorum/2-node-witness.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, outra: **não**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **não**. 

#### <a name="three-nodes-without-a-witness"></a>Três nós sem uma testemunha.
Todos os nós votam, portanto, a *maioria* é determinada em um total de **três votos**. Se algum nó falhar, o os sobreviventes será 2/3 e o cluster sobreviver. O cluster se torna dois nós sem uma testemunha – nesse ponto, você está no cenário 1.

![Quorum explicado no caso de três nós sem uma testemunha](media/understand-quorum/3-node-no-witness.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor, então outra: **50% de chance**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **não**. 

#### <a name="three-nodes-with-a-witness"></a>Três nós com uma testemunha.
Todos os nós votam, portanto, a testemunha não votará inicialmente. A *maioria* é determinada em um total de **três votos**. Após uma falha, o cluster tem dois nós com uma testemunha – que está de volta para o cenário 2. Então, agora os dois nós e a testemunha votam.

![Quorum explicado no caso com três nós com uma testemunha](media/understand-quorum/3-node-witness.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **não**. 

#### <a name="four-nodes-without-a-witness"></a>Quatro nós sem uma testemunha
O voto de um nó é zerado, portanto, a *maioria* é determinada em um total de **três votos**. Após uma falha, o cluster se torna três nós e você está no cenário 3.

![Quorum explicado no caso de quatro nós sem uma testemunha](media/understand-quorum/4-node-no-witness.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **50% de chance**. 

#### <a name="four-nodes-with-a-witness"></a>Quatro nós com uma testemunha.
Todos os nós e votos e os votos de testemunha, portanto, a *maioria* é determinada em um total de **5 votos**. Após uma falha, você estará no cenário 4. Após duas falhas simultâneas, você passa para o cenário 2.

![Quorum explicado no caso de quatro nós com uma testemunha](media/understand-quorum/4-node-witness.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **Sim**. 

#### <a name="five-nodes-and-beyond"></a>Cinco nós e além.
Todos os nós votam, ou todos, exceto um voto, o que torna o total estranho. Espaços de Armazenamento Diretos não pode manipular mais de dois nós de qualquer forma, portanto, neste ponto, nenhuma testemunha é necessária ou útil.

![O quorum explicou no caso com cinco nós e além](media/understand-quorum/5-nodes.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **Sim**. 

Agora que compreendemos como o quorum funciona, vamos examinar os tipos de testemunhas de quorum.

### <a name="quorum-witness-types"></a>Tipos de testemunha de quorum

O clustering de failover dá suporte a três tipos de testemunhas de quorum:

- **[Testemunha em nuvem](../../failover-clustering/deploy-cloud-witness.md)** -armazenamento de BLOBs no Azure acessível por todos os nós do cluster. Ele mantém informações de clustering em um arquivo testemunha. log, mas não armazena uma cópia do banco de dados do cluster.
- **Testemunha de compartilhamento de arquivos** – um compartilhamento de arquivos SMB configurado em um servidor de arquivos que executa o Windows Server. Ele mantém informações de clustering em um arquivo testemunha. log, mas não armazena uma cópia do banco de dados do cluster.
- **Testemunha de disco** -um pequeno disco clusterizado que está no grupo de armazenamento disponível do cluster. Esse disco é altamente disponível e pode fazer failover entre nós. Ele contém uma cópia do banco de dados do cluster.  ***Não há suporte para uma testemunha de disco com espaços de armazenamento diretos***.

## <a name="pool-quorum-overview"></a>Visão geral do quorum do pool

Acabamos de falar sobre quorum de cluster, que opera no nível do cluster. Agora, vamos nos aprofundar no quorum do pool, que opera no nível do pool (ou seja, você pode perder nós e unidades e fazer com que o pool fique ativo). Os pools de armazenamento foram projetados para serem usados em cenários clusterizados e não clusterizados, motivo pelo qual eles têm um mecanismo de quorum diferente.

A tabela a seguir fornece uma visão geral dos resultados de quorum do pool por cenário:

| Nós de servidor | Pode sobreviver a uma falha de nó de servidor | Pode sobreviver a uma falha de nó de servidor e, em seguida, a outra | Pode sobreviver a duas falhas de nó de servidor simultâneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Não                                  | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | Não                                                | Não                                                 |
| 3 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 4            | Sim                                 | Não                                                | Não                                                 |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e acima  | Sim                                 | Sim                                               | Sim                                                |

## <a name="how-pool-quorum-works"></a>Como funciona o quorum de pool

Quando as unidades falham ou quando algum subconjunto de unidades perde contato com outro subconjunto, as unidades restantes precisam verificar se constituem a *maioria* do pool para permanecer online. Se eles não conseguirem verificar, eles ficarão offline. O pool é a entidade que fica offline ou permanece online com base no fato de ter discos suficientes para quorum (50% + 1). O proprietário do recurso de pool (nó de cluster ativo) pode ser o + 1.

Mas o quorum do pool funciona de modo diferente do quorum do cluster das seguintes maneiras:

- o pool usa um nó no cluster como uma testemunha como um disjuntor para sobreviver à metade das unidades (esse nó que é o proprietário do recurso do pool)
- o pool não tem quorum dinâmico
- o pool não implementa sua própria versão de remover um voto

### <a name="examples"></a>Exemplos

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quatro nós com um layout simétrico. 
Cada uma das 16 unidades tem um voto e o nó dois também tem um voto (já que é o proprietário do recurso do pool). A *maioria* é determinada em um total de **16 votos**. Se os nós três e quatro ficarem inativos, o subconjunto sobrevivente terá 8 unidades e o proprietário do recurso do pool, que será de 9/16 votos. Portanto, o pool sobreviver.

![Quorum de pool 1](media/understand-quorum/pool-1.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha do servidor e, em seguida, a outra: **Sim**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **Sim**. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quatro nós com um layout simétrico e uma falha de unidade. 
Cada uma das 16 unidades tem um voto e o nó 2 também tem um voto (já que é o proprietário do recurso do pool). A *maioria* é determinada em um total de **16 votos**. Primeiro, a unidade 7 fica inativa. Se os nós três e quatro ficarem inativos, o subconjunto sobrevivente terá 7 unidades e o proprietário do recurso do pool, que será de 8/16 votos. Portanto, o pool não tem a maioria e fica inativo.

![Quorum de Pool 2](media/understand-quorum/pool-2.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, outra: **não**.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: **não**. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quatro nós com um layout não simétrico. 
Cada uma das 24 unidades tem um voto e o nó dois também tem um voto (já que é o proprietário do recurso do pool). A *maioria* é determinada em um total de **24 votos**. Se os nós três e quatro ficarem inativos, o subconjunto sobrevivente terá 8 unidades e o proprietário do recurso do pool, que será de 9/24 votos. Portanto, o pool não tem a maioria e fica inativo.

![Quorum de pool 3](media/understand-quorum/pool-3.png)

- Pode sobreviver a uma falha de servidor: **Sim**.
- Pode sobreviver a uma falha de servidor e, em seguida, outra: * * depende de * * (não é possível sobreviver se ambos os nós três e quatro ficarem inativos, mas puderem sobreviver a todos os outros cenários.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: * * depende de * * (não é possível sobreviver se os nós três e quatro ficarem inativos, mas puderem sobreviver a todos os outros cenários.

### <a name="pool-quorum-recommendations"></a>Recomendações de quorum de pool

- Garantir que cada nó em seu cluster seja simétrico (cada nó tem o mesmo número de unidades)
- Habilite o espelho de três vias ou a paridade dupla para que você possa tolerar falhas de nó e manter os discos virtuais online. Consulte nossa [página de diretrizes de volume](plan-volumes.md) para obter mais detalhes.
- Se mais de dois nós estiverem inativos ou se dois nós e um disco em outro nó estiverem inativos, os volumes poderão não ter acesso a todas as três cópias de seus dados e, portanto, ficar offline e ficar indisponíveis. É recomendável colocar os servidores novamente ou substituir os discos rapidamente para garantir a maior resiliência para todos os dados no volume.

## <a name="more-information"></a>Mais informações

- [Configurar e gerenciar o quórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem](../../failover-clustering/deploy-cloud-witness.md)
