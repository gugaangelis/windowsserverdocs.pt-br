---
title: Quorum de cluster e pool de compreensão
description: Noções básicas sobre Cluster e Pool de Quorum, com exemplos específicos para passar pela complexidades.
keywords: Espaços de armazenamento diretos, Quorum, testemunha, S2D, Quorum, Pool de Quorum, Cluster, Pool de Cluster
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024034"
---
# Quorum de cluster e pool de compreensão

>Aplica-se a: Windows Server 2019, Windows Server 2016

[Clustering de Failover do Windows Server](../../failover-clustering/failover-clustering-overview.md) fornece alta disponibilidade para cargas de trabalho. Esses recursos são considerados altamente disponíveis se os nós que hospedam os recursos são No entanto, o cluster geralmente requer mais da metade de nós para estar em execução, que é conhecido como tendo *quorum*.

Quorum foi projetado para evitar *a* cenários que podem ocorrer quando há uma partição na rede e subconjuntos de nós não podem se comunicar uns com os outros. Isso pode causar ambos os subconjuntos de nós para tentar possui a carga de trabalho e gravar no mesmo disco que pode resultar em vários problemas. No entanto, isso é impedido com conceito do cluster de Failover de quorum que força apenas um desses grupos de nós para continuar em execução, portanto, apenas um desses grupos permanecerão online.

Quorum determina o número de falhas que um cluster pode suportar enquanto ainda permanece online. Quorum foi projetado para tratar o cenário quando há um problema com a comunicação entre subconjuntos de nós de cluster, para que vários servidores não tentam simultaneamente hospedar um grupo de recursos e gravar no mesmo disco ao mesmo tempo. Tendo esse conceito de quorum, o cluster forçará o serviço de cluster para parar em um dos subconjuntos de nós para garantir que haja apenas um verdadeiro proprietário de um grupo de recurso específico. Depois que nós que foram interrompidas mais uma vez podem se comunicar com o grupo principal de nós, eles serão automaticamente reingresso no cluster e iniciar o serviço de cluster.

No Windows Server 2016, existem dois componentes do sistema que têm seus próprios mecanismos de quorum:

- <strong>Quórum do cluster</strong>: isso funciona no nível do cluster (ou seja, você pode perder nós e ter o cluster mantenha-se para cima)
- <strong>Pool de Quorum</strong>: isso opera em nível de pool quando espaços de armazenamento diretos está habilitado (ou seja, você pode perder nós e unidades e ter o pool mantenha-se para cima). Pools de armazenamento foram projetados para ser usado em cenários de clusters e sem cluster, por isso, eles têm um mecanismo de quorum diferentes.

## Visão geral de quorum de cluster

A tabela a seguir fornece uma visão geral dos resultados de Quorum de Cluster por cenário:

| Nós de servidor | Sobrevivem a uma falha de nó de servidor | Podem sobreviver falhas do nó de um servidor, em seguida, outro | Podem sobreviver duas falhas do nó de servidor simultâneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | 50/50                                             | Não                                                 |
| 3 + testemunha  | Sim                                 | Sim                                               | Não                                                 |
| 4            | Sim                                 | Sim                                               | 50/50                                              |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e acima  | Sim                                 | Sim                                               | Sim                                                |

### Recomendações de quorum de cluster

- Se você tiver dois nós, uma testemunha é <strong>necessário</strong>.
- Se você tiver três ou quatro nós, testemunha é <strong>altamente recomendável</strong>.
- Se você tiver acesso à Internet, use um <strong> [testemunha de nuvem](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Se você estiver em um ambiente de TI com outros computadores e compartilhamentos de arquivos, use uma testemunha de compartilhamento de arquivo

## Como funciona o quórum do cluster

Quando ocorre falha de nós, ou quando algum subconjunto de nós perde contato com outro subconjunto, nós sobreviventes precisam verificar que eles constituem a *maioria* do cluster permaneçam on-line. Se eles não é possível verificar que, ele vai ficar offline.

Mas o conceito de *maioria* só funciona corretamente quando o número total de nós do cluster é ímpar (por exemplo, três nós em um cluster de cinco nó). Portanto, e clusters com um número par de nós (digamos, um cluster de quatro nós)?

Há duas maneiras de que cluster pode tornar o *número total de votos* ímpar:

1. Primeiro, ele pode ir *inscrever* um adicionando uma *testemunha* com um voto extra. Isso requer a configuração do usuário.
2.  Ou então, ele pode ir *para baixo* uma, concentrando-se voto do nó sorte (ocorre automaticamente, conforme necessário).

Sempre que nós sobreviventes verificar com êxito são a *maioria*, a definição da *maioria* é atualizada para estar entre apenas sobreviventes. Isso permite que o cluster perder um nó, em seguida, outro e outro e assim por diante. Esse conceito de adaptar o *número total de votos* após falhas sucessivas é conhecido como <strong> *quorum dinâmico*</strong>.  

### Testemunha dinâmica

Testemunha dinâmica alterna o voto de testemunha para certificar-se de que o *número total de votos* é indefinido. Se houver um número ímpar de votos, a testemunha não tem um voto. Se houver um número par de votos, a testemunha tem um voto. Testemunha dinâmica reduz significativamente o risco de que o cluster passará para baixo devido a falha de testemunha. Cluster decide se deseja usar o voto testemunha com base no número de nós voto que estão disponíveis no cluster.

Quorum dinâmico funciona com testemunha dinâmica na maneira como descrito abaixo.

### Comportamento de quorum dinâmico

- Se você tiver um <strong>mesmo</strong> número de nós e nenhuma testemunha, *um nó obtém seu voto zerado*. Por exemplo, somente três desses quatro nós obtém votos, portanto, o *número total de votos* é três e dois sobreviventes com votos são considerados a maioria.
- Se você tiver um <strong>ímpar</strong> número de nós e nenhuma testemunha, *todos eles obtém votos*.
- Se você tiver um <strong>mesmo</strong> número de nós mais testemunha, *votos a testemunha*, portanto, o total é indefinido.
- Se você tiver um <strong>ímpar</strong> número de nós mais testemunha, *não votar a testemunha*.

Quorum dinâmico habilita a capacidade de atribuir um voto para um nó dinamicamente para evitar a perda da maioria dos votos e permitir que o cluster seja executado com um nó (conhecido como uma última-man reputação). Vamos dar um cluster de quatro nós como exemplo. Suponha que quorum requer 3 votos. 

Nesse caso, o cluster teria sido se você perdeu dois nós.

![Diagrama mostrando quatro nós de cluster, cada um deles obtém um voto](media/understand-quorum/dynamic-quorum-base.png)

No entanto, quorum dinâmico impede que isso aconteça. O *número total de votos* necessário para quorum agora é determinado com base no número de nós disponíveis. Assim, com quorum dinâmico, o cluster permanecerá o mesmo se você perder três nós.

![Diagrama mostrando os quatro nós de cluster conosco falha um de cada vez e o número de votos necessários ajustar após cada falha.](media/understand-quorum/dynamic-quorum-step-through.png)

O cenário acima se aplica a um cluster geral que não têm espaços de armazenamento diretos habilitado. No entanto, quando espaços de armazenamento diretos está habilitado, o cluster só pode suportar duas falhas do nó. Isso é explicado mais na [seção de quorum pool](#poolQuorum).

### Exemplos

#### Dois nós sem uma testemunha. 
Vote de um nó é zerado, portanto, o *maioria* voto é determinado fora de um total de <strong>1 voto</strong>. Se o nó não voto falhar inesperadamente, a sobrevivência tem 1/1 e o cluster prevalece. Se o nó voto falhar inesperadamente, a sobrevivência tem 0/1 e o cluster cair. Se o nó voto normalmente é desligado, o voto é transferido para outro nó e o cluster sobrevive à. *<strong>É por isso é fundamental para configurar uma testemunha.</strong>*

![Explicado no caso com dois nós sem uma testemunha de quorum](media/understand-quorum/2-node-no-witness.png)

- Sobrevivem a uma falha de servidor: <strong>chance de 50%</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>não</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>não</strong>. 

#### Dois nós com uma testemunha. 
Ambos os nós votam, além de votos testemunha, portanto, a *maioria* é determinado fora de um total de <strong>3 votos</strong>. Se qualquer nó falhar, a sobrevivência tem 2/3 e o cluster prevalece.

![Explicada no caso com dois nós com uma testemunha de quorum](media/understand-quorum/2-node-witness.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>não</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>não</strong>. 

#### Três nós sem uma testemunha.
Todos os nós votam, portanto, a *maioria* é determinado fora de um total de <strong>3 votos</strong>. Se qualquer nó falhar, os sobreviventes são 2/3 e o cluster prevalece. Dois nós sem uma testemunha se torna o cluster – nesse ponto, você está no cenário 1.

![Explicado no caso com três nós sem uma testemunha de quorum](media/understand-quorum/3-node-no-witness.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>chance de 50%</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>não</strong>. 

#### Três nós com uma testemunha.
Todos os nós votam, portanto, a testemunha não votar inicialmente. A *maioria* é determinado fora de um total de <strong>3 votos</strong>. Após uma falha, o cluster tem dois nós com uma testemunha – que é o cenário 2. Portanto, agora os dois nós e a testemunha votam.

![Explicada no caso com três nós com uma testemunha de quorum](media/understand-quorum/3-node-witness.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>não</strong>. 

#### Quatro nós sem uma testemunha
Vote de um nó é zerado, portanto, a *maioria* é determinado fora de um total de <strong>3 votos</strong>. Após uma falha, o cluster se torna três nós, e você está no cenário 3.

![Explicado no caso com quatro nós sem uma testemunha de quorum](media/understand-quorum/4-node-no-witness.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>chance de 50%</strong>. 

#### Quatro nós com uma testemunha.
Todos os votos de nós e votos a testemunha, portanto, a *maioria* é determinado fora de um total de <strong>5 votos</strong>. Após uma falha, você está no cenário 4. Depois de duas falhas simultâneas, você ignorar até 2 de cenário.

![Explicada no caso com quatro nós com uma testemunha de quorum](media/understand-quorum/4-node-witness.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>Sim</strong>. 

#### Cinco nós e superiores.
Todos os nós votem, ou apenas um voto, qualquer torna o total ímpar. Espaços de armazenamento diretos não pode manipular mais de dois nós para baixo mesmo assim, portanto, neste ponto, nenhuma testemunha é útil ou necessárias.

![Explicado no caso com cinco nós e além de quorum](media/understand-quorum/5-nodes.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>Sim</strong>. 

Agora que sabemos como funciona o quorum, vamos examinar os tipos de testemunhas de quorum.

### Tipos de testemunha de quorum

Clustering de failover dá suporte a três tipos de Quorum testemunhas:

- <strong>[Testemunha de nuvem](../../failover-clustering\deploy-cloud-witness.md) </strong> -Blob de armazenamento no Azure acessível por todos os nós do cluster. Ele mantém informações de clusters em um arquivo de witness.log, mas não armazena uma cópia do banco de dados do cluster.
- <strong>Testemunha de compartilhamento de arquivos</strong> – compartilhamento de arquivos SMB A que está configurado em um servidor de arquivos que executam o Windows Server. Ele mantém informações de clusters em um arquivo de witness.log, mas não armazena uma cópia do banco de dados do cluster.
- <strong>Testemunha de disco</strong> -um disco de cluster pequeno que esteja no grupo de armazenamento de Cluster disponível. O disco é altamente disponível e pode fazer o failover entre nós. Ele contém uma cópia do banco de dados do cluster.  <strong>*Uma testemunha de disco não é compatível com espaços de armazenamento diretos*</strong>.

## <a id="poolQuorum"></a>Visão geral de quorum de pool

Assim, falamos sobre quórum do Cluster, que opera no nível do cluster. Agora, vamos nos aprofundar no Pool de Quorum, que opera em nível de pool (ou seja, você pode perder nós e unidades e têm o pool mantenha-se para cima). Pools de armazenamento foram projetados para ser usado em cenários de clusters e sem cluster, por isso, eles têm um mecanismo de quorum diferentes.

A tabela a seguir apresenta uma visão geral dos resultados do Pool Quorum por cenário:

| Nós de servidor | Sobrevivem a uma falha de nó de servidor | Podem sobreviver falhas do nó de um servidor, em seguida, outro | Podem sobreviver duas falhas do nó de servidor simultâneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Não                                  | Não                                                | Não                                                 |
| 2 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 3            | Sim                                 | Não                                                | Não                                                 |
| 3 + testemunha  | Sim                                 | Não                                                | Não                                                 |
| 4            | Sim                                 | Não                                                | Não                                                 |
| 4 + testemunha  | Sim                                 | Sim                                               | Sim                                                |
| 5 e acima  | Sim                                 | Sim                                               | Sim                                                |

## Como funciona o quorum de pool

Quando os drives falham, ou quando algum subconjunto de unidades perde contato com outro subconjunto, unidades sobreviventes precisam verificar que eles constituem a *maioria* do pool permaneçam on-line. Se eles não é possível verificar que, ele vai ficar offline. O pool é a entidade que fica offline ou fica online com base em se ele tem discos suficientes para quorum (50% + 1). O proprietário do recurso de pool (nó de cluster ativo) pode ser a + 1.

Mas quorum pool funciona de maneira diferente de quorum do cluster, das seguintes maneiras:

- o pool usa um nó no cluster como uma testemunha como um separador de vínculo para sobreviver metade das unidades passadas (esse nó é o proprietário do recurso de pool)
- o pool não tem quorum dinâmico
- o pool não implementar sua própria versão de remover um voto

### Exemplos

#### Quatro nós com um layout simétrico. 
Cada um dos 16 drives tem um voto e nó dois também tem um voto (já que ele é o proprietário do recurso de pool). A *maioria* é determinado fora de um total de <strong>16 votos</strong>. Se nós três e quatro ir para baixo, o subconjunto sobrevivente tem 8 unidades e o proprietário do recurso pool, que é votos 9/16. Portanto, o pool sobrevive à.

![Pool Quorum 1](media/understand-quorum/pool-1.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Sim</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>Sim</strong>. 

#### Quatro nós com uma falha de layout e unidade simétrico. 
Cada um dos 16 drives tem um voto e nó 2 também tem um voto (já que ele é o proprietário do recurso de pool). A *maioria* é determinado fora de um total de <strong>16 votos</strong>. Primeiro, unidade 7 cair. Se nós três e quatro ir para baixo, o subconjunto sobrevivente tem 7 unidades e o proprietário do recurso pool, que é votos de 8/16. Portanto, o pool não tem maioria e cair.

![Pool Quorum 2](media/understand-quorum/pool-2.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>não</strong>.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>não</strong>. 

#### Quatro nós com um layout não simétricos. 
Cada um dos 24 drives tem um voto e nó dois também tem um voto (já que ele é o proprietário do recurso de pool). A *maioria* é determinado fora de um total de <strong>24 votos</strong>. Se nós três e quatro ir para baixo, o subconjunto sobrevivente tem 8 unidades e o proprietário do recurso pool, que é votos 9/24. Portanto, o pool não tem maioria e cair.

![Pool Quorum 3](media/understand-quorum/pool-3.png)

- Sobrevivem a uma falha de servidor: <strong>Sim</strong>.
- Sobreviver a falha de um servidor, em seguida, outro: <strong>Depends </strong>(não é possível sobrevivem se ambos os nós três e quatro ir para baixo, mas podem sobreviver todos os outros cenários.
- Pode sobreviver a duas falhas de servidor ao mesmo tempo: <strong>Depends </strong>(não é possível sobrevivem se ambos os nós três e quatro ir para baixo, mas podem sobreviver todos os outros cenários.

### Recomendações de quorum de pool

- Certifique-se de que cada nó no cluster é simétrico (cada nó tem o mesmo número de unidades)
- Habilite o espelhamento de três vias ou paridade dupla para que você pode tolerar um falhas do nó e manter os discos virtuais on-line. Consulte nossa [página de orientação de volume](plan-volumes.md) para obter mais detalhes.
- Se mais de dois nós são para baixo, ou dois nós e um disco em outro nó para baixo, volumes podem não ter acesso a todos os três cópias de seus dados e, portanto, ser colocados offline e não estar disponíveis. É recomendável para trazer os servidores de volta ou substituir os discos rapidamente para garantir que a maioria dos resiliência para todos os dados no volume.

## Mais informações

- [Configurar e gerenciar o quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Implantar uma testemunha de nuvem](../../failover-clustering/deploy-cloud-witness.md)
