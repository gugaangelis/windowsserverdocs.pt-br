---
title: Noções básicas sobre cluster e pool de quorum
description: Noções básicas sobre Cluster e Pool de Quorum, com exemplos específicos para falar sobre as complicações.
keywords: Espaços de armazenamento diretos, Quorum, a testemunha, S2D, Cluster de Quorum, o Quorum do Pool, Pool de Cluster
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879967"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Noções básicas sobre cluster e pool de quorum

>Aplica-se a: Windows Server 2019, Windows Server 2016

[Windows Server Failover Clustering](../../failover-clustering/failover-clustering-overview.md) fornece alta disponibilidade para cargas de trabalho. Esses recursos são considerados altamente disponíveis se os nós que hospedam recursos estejam funcionando; No entanto, o cluster geralmente requer mais de metade de nós em execução, que é conhecido como tendo *quorum*.

Quorum é projetado para evitar *separação* cenários que podem acontecer quando há uma partição na rede e subconjuntos de nós não podem se comunicar entre si. Isso pode causar os dois subconjuntos de nós para tentar possui a carga de trabalho e gravar no mesmo disco que pode levar a diversos problemas. No entanto, isso não é possível com conceito do cluster de Failover de quorum que força a apenas um desses grupos de nós para continuar em execução, portanto, apenas um desses grupos permanecerá online.

Quorum determina o número de falhas que o cluster pode sustentar enquanto ainda permanecem online. Quorum é projetado para lidar com o cenário quando há um problema com a comunicação entre os subconjuntos de nós de cluster, para que vários servidores não tentam simultaneamente um grupo de recursos de host e gravar no mesmo disco ao mesmo tempo. Fazendo com que esse conceito de quorum, o cluster será forçar o serviço de cluster para parar em um dos subconjuntos de nós para garantir que haja apenas um verdadeiro proprietário de um determinado grupo de recursos. Depois que nós que tenham sido interrompidos mais uma vez podem se comunicar com o grupo principal de nós, eles serão automaticamente reingresso no cluster e iniciar seu serviço de cluster.

No Windows Server 2016, há dois componentes do sistema que têm seus próprios mecanismos de quorum:

- <strong>Quorum de cluster</strong>: Isso funciona no nível do cluster (ou seja, você pode perder a nós e fazer com que o cluster permaneça backup)
- <strong>Pool de Quorum</strong>: Isso funciona no nível do pool quando espaços de armazenamento diretos está habilitado (ou seja, você pode perder a nós e unidades e fazer com que o pool de manter). Pools de armazenamento foram projetados para ser usado em cenários clusterizados e não clusterizados, que é o motivo pelo qual eles têm um mecanismo de quorum diferentes.

## <a name="cluster-quorum-overview"></a>Visão geral sobre o quorum de cluster

A tabela a seguir fornece uma visão geral dos resultados de Quorum do Cluster por cenário:

| Nós de servidor | Pode sobreviver a uma falha de nó de servidor | Pode sobreviver a falha de nó de um servidor, em seguida, outro | Pode sobreviver a duas falhas de nó simultâneas do servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | 50/50                                             | Não                                                 |
| 3 + testemunha  | Sim                                 | Sim                                               | Não                                                 |
| 4            | Sim                                 | Sim                                               | 50/50                                              |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e superior  | Sim                                 | Sim                                               | Sim                                                |

### <a name="cluster-quorum-recommendations"></a>Recomendações de quorum do cluster

- Se você tiver dois nós, uma testemunha estiver <strong>necessária</strong>.
- Se você tiver três ou quatro nós, a testemunha está <strong>altamente recomendável</strong>.
- Se você tiver acesso à Internet, use uma  <strong>[testemunha em nuvem](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Se você estiver em um ambiente de TI com outras máquinas e compartilhamentos de arquivos, use uma testemunha de compartilhamento de arquivo

## <a name="how-cluster-quorum-works"></a>Como funciona o quorum do cluster

Quando nós falharem, ou quando algum subconjunto de nós perde o contato com outro subconjunto, nós sobreviventes precisam verificar que eles constituem a *maioria* do cluster permaneçam online. Se eles não é possível verificar que, eles irão offline.

Mas o conceito de *maioria* funciona apenas corretamente quando o número total de nós no cluster é ímpar (por exemplo, três nós em um cluster de cinco nós). Dessa forma, e clusters com um número par de nós (digamos, um cluster de quatro nós)?

Há duas maneiras do cluster pode tornar o *o número total de votos* ímpar:

1. Primeiro, ele pode passar *para cima* um, adicione uma *testemunha* com um voto extra. Isso exige a configuração do usuário.
2.  Ou, ele pode passar *para baixo* um zerando voto do nó sorte (ocorre automaticamente, conforme necessário).

Verificar sempre que nós sobreviventes com êxito são as *maioria*, a definição da *maioria* é atualizada para estar entre apenas os sobreviventes. Isso permite que o cluster perder um nó, em seguida, outro e outro e assim por diante. Esse conceito do *o número total de votos* adaptando após falhas sucessivas é conhecido como  <strong>*dinâmico de quorum*</strong>.  

### <a name="dynamic-witness"></a>Testemunha dinâmica

Testemunha dinâmica alterna o voto de testemunha para certificar-se de que o *o número total de votos* é ímpar. Se houver um número ímpar de votos, a testemunha não tem um voto. Se houver um número par de votos, a testemunha tem um voto. Testemunha dinâmica reduz significativamente o risco de que o cluster ficará inativo devido a falha de testemunha. O cluster decide se deseja usar o voto de testemunha com base no número de nós de votação que estão disponíveis no cluster.

Quorum dinâmico funciona com testemunha dinâmica no modo descrito abaixo.

### <a name="dynamic-quorum-behavior"></a>Comportamento de quorum dinâmico

- Se você tiver um <strong>ainda</strong> número de nós e nenhuma testemunha *um nó obtém seu voto zerado*. Por exemplo, apenas três dos quatro nós obtém votos, portanto, o *o número total de votos* é três e dois sobreviventes com votos são considerados uma maioria.
- Se você tiver um <strong>ímpar</strong> número de nós e nenhuma testemunha *recebem votos*.
- Se você tiver um <strong>ainda</strong> número de nós e a testemunha, *votos a testemunha*, portanto, o total é ímpar.
- Se você tiver um <strong>ímpar</strong> número de nós e a testemunha, *a testemunha não votar*.

Quorum dinâmico permite que a capacidade de atribuir um voto para um nó dinamicamente para evitar perder a maioria dos votos e para permitir que o cluster seja executado com um nó (conhecido como permanente de última-man). Vamos tomar um cluster de quatro nós como um exemplo. Suponha que o quorum requer 3 votos. 

Nesse caso, o cluster teria ido para baixo se você perdeu os dois nós.

![Diagrama mostrando quatro nós de cluster, cada um dos quais obtém um voto](media/understand-quorum/dynamic-quorum-base.png)

No entanto, o quorum dinâmico impede que isso ocorra. O *o número total de votos* necessária para quorum agora é determinado com base no número de nós disponíveis. Assim, com o quorum dinâmico, o cluster permanecerá ativo mesmo se você perder a três nós.

![Diagrama mostrando quatro nós de cluster conosco falhar um de cada vez e o número de votos necessários ajustando após cada falha.](media/understand-quorum/dynamic-quorum-step-through.png)

O cenário acima aplica-se a um cluster geral que não tem espaços de armazenamento diretos habilitados. No entanto, quando espaços de armazenamento diretos está habilitado, o cluster pode suportar apenas duas falhas de nó. Isso será explicado mais a [seção de quorum do pool](#poolQuorum).

### <a name="examples"></a>Exemplos

#### <a name="two-nodes-without-a-witness"></a>Dois nós sem uma testemunha. 
Voto do nó de um é zerado, portanto, o *maioria* voto é determinado de um total de <strong>1 voto</strong>. Se o nó não votação falhar inesperadamente, o sobrevivente tem 1/1 e o cluster sobrevive. Se o nó de votação falhar inesperadamente, o sobrevivente tem 0/1 e o cluster ficar inativo. Se o nó de votação é desligado normalmente, o voto é transferido para outro nó e sobrevive a cluster. *<strong>É por isso é essencial para configurar uma testemunha.</strong>*

![Explicado o caso com dois nós sem uma testemunha de quorum](media/understand-quorum/2-node-no-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Cinquenta por cento de chance</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Não</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Não</strong>. 

#### <a name="two-nodes-with-a-witness"></a>Dois nós com uma testemunha. 
Ambos os nós de votação, além de votos a testemunha, portanto, o *maioria* é determinado de um total de <strong>3 votos</strong>. Se qualquer nó falhar, o sobrevivente tem 2/3 e o cluster sobrevive.

![Explicado o caso com dois nós com uma testemunha de quorum](media/understand-quorum/2-node-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Não</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Não</strong>. 

#### <a name="three-nodes-without-a-witness"></a>Três nós sem uma testemunha.
Votam em todos os nós, portanto, o *maioria* é determinado de um total de <strong>3 votos</strong>. Se qualquer nó falhar, os sobreviventes são 2/3 e o cluster sobrevive. O cluster de dois nós sem uma testemunha se tornar – nesse ponto, você está no cenário 1.

![Explicado o caso com três nós sem uma testemunha de quorum](media/understand-quorum/3-node-no-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Cinquenta por cento de chance</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Não</strong>. 

#### <a name="three-nodes-with-a-witness"></a>Três nós com uma testemunha.
Todos os nós votam, portanto, a testemunha não votar inicialmente. O *maioria* é determinado de um total de <strong>3 votos</strong>. Depois de uma falha, o cluster tem dois nós com uma testemunha – o que está de volta para o cenário 2. Portanto, agora os dois nós e a testemunha votam.

![Explicado o caso com três nós com uma testemunha de quorum](media/understand-quorum/3-node-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Não</strong>. 

#### <a name="four-nodes-without-a-witness"></a>Quatro nós sem uma testemunha
Voto do nó de um é zerado, portanto, o *maioria* é determinado de um total de <strong>3 votos</strong>. Depois de uma falha, o cluster se tornar a três nós e você está no cenário 3.

![Explicado o caso com quatro nós sem uma testemunha de quorum](media/understand-quorum/4-node-no-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Cinquenta por cento de chance</strong>. 

#### <a name="four-nodes-with-a-witness"></a>Quatro nós com uma testemunha.
Todos os votos de nós e os votos de testemunha, portanto, o *maioria* é determinado de um total de <strong>5 votos</strong>. Depois de uma falha, você está no cenário 4. Após duas falhas simultâneas, pule para baixo para o cenário 2.

![Explicado o caso com quatro nós com uma testemunha de quorum](media/understand-quorum/4-node-witness.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Sim</strong>. 

#### <a name="five-nodes-and-beyond"></a>Cinco nós e muito mais.
Todos os nós de votação ou apenas um voto, seja qual for cria total ímpar. Espaços de armazenamento diretos não pode lidar com mais de dois nós para baixo de qualquer forma, portanto, neste ponto, nenhuma testemunha é necessária ou útil.

![Quorum explicado em casos com cinco nós e além](media/understand-quorum/5-nodes.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Sim</strong>. 

Agora que compreendemos como o quorum funciona, vamos examinar os tipos de testemunhas de quorum.

### <a name="quorum-witness-types"></a>Tipos de testemunha de quorum

Clustering de failover oferece suporte a três tipos de testemunhas de Quorum:

- <strong>[Testemunha de nuvem](../../failover-clustering\deploy-cloud-witness.md)</strong>  -armazenamento de BLOBs no Azure acessível por todos os nós do cluster. Ele mantém informações de clusters em um arquivo WITNESS log, mas não armazena uma cópia do banco de dados do cluster.
- <strong>Testemunha de compartilhamento de arquivos</strong> – compartilhamento de arquivos SMB de um configurado em um servidor de arquivos executando o Windows Server. Ele mantém informações de clusters em um arquivo WITNESS log, mas não armazena uma cópia do banco de dados do cluster.
- <strong>Testemunha de disco</strong> -um disco de cluster pequeno que está no grupo de armazenamento de Cluster disponível. Esse disco está altamente disponível e poderá realizar failover entre nós. Ele contém uma cópia do banco de dados do cluster.  <strong>*Uma testemunha de disco não é suportada com espaços de armazenamento diretos*</strong>.

## <a id="poolQuorum"></a>Visão geral de quorum do pool

Acabamos de falar sobre o Quorum do Cluster, que opera no nível do cluster. Agora, vamos nos aprofundar no Pool de Quorum, que opera em nível do pool (ou seja, você pode perder a nós e unidades e têm o pool de manter). Pools de armazenamento foram projetados para ser usado em cenários clusterizados e não clusterizados, que é o motivo pelo qual eles têm um mecanismo de quorum diferentes.

A tabela a seguir fornece uma visão geral dos resultados de Quorum do Pool por cenário:

| Nós de servidor | Pode sobreviver a uma falha de nó de servidor | Pode sobreviver a falha de nó de um servidor, em seguida, outro | Pode sobreviver a duas falhas de nó simultâneas do servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Não                                  | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | Não                                                | Não                                                 |
| 3 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 4            | Sim                                 | Não                                                | Não                                                 |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e superior  | Sim                                 | Sim                                               | Sim                                                |

## <a name="how-pool-quorum-works"></a>Como funciona o quorum do pool

Quando os drives falham ou quando algum subconjunto das unidades perde o contato com outro subconjunto, unidades sobreviventes precisam verificar que eles constituem a *maioria* do pool permaneçam online. Se eles não é possível verificar que, eles irão offline. O pool é a entidade que fica offline ou permanece online com base em se ele tem discos suficientes para o quorum (50% + 1). O proprietário do recurso de pool (nó de cluster ativo) pode ser o + 1.

Mas o quorum do pool funciona de maneira diferente de quorum do cluster, das seguintes maneiras:

- o pool usa um nó no cluster como uma testemunha como um separador de ligações para resistir a metade das unidades passadas (esse nó é o proprietário do recurso de pool)
- o pool não tem quorum dinâmico
- o pool não implementa sua própria versão da remoção de um voto

### <a name="examples"></a>Exemplos

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quatro nós com um layout simétrico. 
Cada uma das 16 unidades tem um voto e nó dois também tem um voto (já que é o proprietário do recurso de pool). O *maioria* é determinado de um total de <strong>16 votos</strong>. Se nós três e quatro ficam inativos, o subconjunto sobrevivente tem 8 unidades e o proprietário do recurso de pool, que é 9/16 votos. Portanto, o pool sobrevive.

![Quorum pool 1](media/understand-quorum/pool-1.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Sim</strong>. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quatro nós com uma falha de layout e a unidade simétrico. 
Cada uma das 16 unidades tem um voto e nó 2 também tem um voto (já que é o proprietário do recurso de pool). O *maioria* é determinado de um total de <strong>16 votos</strong>. Em primeiro lugar, a unidade 7 fica inoperante. Se nós três e quatro ficam inativos, o subconjunto sobrevivente tem unidades de 7 e o proprietário do recurso de pool, que é 8/16 votos. Assim, o pool não tem a maioria e fica inativo.

![Quorum do pool 2](media/understand-quorum/pool-2.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Não</strong>.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Não</strong>. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quatro nós com um layout não simétricos. 
Cada uma das 24 unidades tem um voto e nó dois também tem um voto (já que é o proprietário do recurso de pool). O *maioria* é determinado de um total de <strong>24 votos</strong>. Se nós três e quatro ficam inativos, o subconjunto sobrevivente tem 8 unidades e o proprietário do recurso de pool, que é votos 9/24. Assim, o pool não tem a maioria e fica inativo.

![3 de Quorum do pool](media/understand-quorum/pool-3.png)

- Pode sobreviver a uma falha do servidor: <strong>Sim</strong>.
- Pode sobreviver a falha de um servidor, em seguida, outro: <strong>Depende</strong> (não pode sobreviver se ambos os nós, três e quatro ficam inativos, mas podem sobreviver a todos os outros cenários.
- Ao mesmo tempo, você pode sobreviver a duas falhas de servidor: <strong>Depende</strong> (não pode sobreviver se ambos os nós, três e quatro ficam inativos, mas podem sobreviver a todos os outros cenários.

### <a name="pool-quorum-recommendations"></a>Recomendações de quorum do pool

- Certifique-se de que cada nó no cluster é simétrico (cada nó tem o mesmo número de unidades)
- Habilite espelho triplo ou paridade dupla para que você possa tolerar falhas de um nó e manter os discos virtuais on-line. Consulte nosso [página de orientação de volume](plan-volumes.md) para obter mais detalhes.
- Se mais de dois nós estão inativos ou dois nós e um disco em outro nó estiverem inativo, volumes podem não ter acesso a todas as três cópias de seus dados e, portanto, ser colocados offline e indisponível. É recomendável para trazer de volta os servidores ou substituir os discos rapidamente para garantir a resiliência a maioria para todos os dados no volume.

## <a name="more-information"></a>Mais informações

- [Configurar e gerenciar o quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem](../../failover-clustering/deploy-cloud-witness.md)
