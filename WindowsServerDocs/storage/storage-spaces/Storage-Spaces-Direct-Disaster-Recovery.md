---
title: Cenários de recuperação de desastre para infraestrutura Hiperconvergente
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Este artigo descreve os cenários disponíveis para recuperação de desastres do Microsoft HCI (espaços de armazenamento diretos)
ms.localizationpriority: medium
ms.openlocfilehash: c844c56c3a1717658bcdb970e78d45b5cdda861c
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453125"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Recuperação de desastres com espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece cenários na infraestrutura como hiperconvergente (HCI) podem ser configurados para recuperação de desastres.

Várias empresas estão executando soluções hiperconvergente e planejamento de um desastre oferece a capacidade de permanecer em ou voltar para a produção rapidamente se um desastre ocorrer. Há várias maneiras de configurar HCI para recuperação de desastres e este documento explica as opções que estão atualmente disponíveis para você.

Quando as discussões de restauração da disponibilidade se ocorrer um desastre giram em torno do que é conhecido como o objetivo de tempo de recuperação (RTO). Esta é a duração de tempo de destino onde os serviços devem ser restaurados para evitar consequências inaceitáveis para os negócios. Em alguns casos, esse processo pode ocorrer automaticamente com a produção restaurada quase imediatamente. Em outros casos, a intervenção manual do administrador deve ocorrer para restaurar os serviços.

As opções de recuperação de desastres com um hiperconvergente hoje são:

1. Vários clusters utilizando a réplica de armazenamento
2. Réplica do Hyper-V entre clusters
3. Backup e restauração

## <a name="multiple-clusters-utilizing-storage-replica"></a>Vários clusters utilizando a réplica de armazenamento

[A réplica de armazenamento](../storage-replica/storage-replica-overview.md) habilita a replicação de volumes e dá suporte à replicação síncrona e assíncrona. Ao escolher entre usar a replicação síncrona ou assíncrona, você deve considerar seu objetivo de ponto de recuperação (RPO). O objetivo de ponto de recuperação é a quantidade de perda de dados possíveis que você está disposto a incorrer em antes de ser considerado grande perda. Se você entrar com a replicação síncrona, ele sequencialmente gravará em ambas as extremidades ao mesmo tempo. Se você entrar com assíncrona, gravações serão replicadas de forma muito rápida, mas ainda poderão ser perdidas. Você deve considerar o uso do aplicativo ou arquivo para ver o que funciona melhor para você.

A réplica de armazenamento é um mecanismo de cópia de nível de bloco versus nível de arquivo; ou seja, não importa quais tipos de dados que está sendo replicados. Isso torna uma opção popular para infraestrutura hiperconvergida. A réplica de armazenamento também pode utilizar diferentes tipos de unidades entre os parceiros de replicação, então, ter todo o armazenamento de um tipo em um HCI e outro armazenamento de tipo na outra é perfeitamente normal. 

Um recurso importante de réplica de armazenamento é que ele possa ser executado no Azure, bem como no local. Você pode definir o local para local, Azure para Azure, ou até mesmo localmente para o Azure (ou vice-versa).

Nesse cenário, há dois clusters independentes separados. Para configurar a réplica de armazenamento entre HCI, você pode seguir as etapas em [replicação de armazenamento de cluster para Cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de replicação de armazenamento](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

As seguintes considerações se aplicam ao implantar a réplica de armazenamento. 

1.  Configurando a replicação é feita fora do cluster de Failover. 
2.  Escolhendo o método de replicação será dependente de seus requisitos de RPO e latência de rede. Synchronous replica os dados em redes de baixa latência com consistência de falhas para garantir que nenhuma perda de dados em um tempo de falha. Assíncrona replica os dados em redes com latências maiores, mas cada site podem não ter cópias idênticas em um tempo de falha. 
3.  Em caso de desastre, failovers entre os clusters não são automáticas e precisa ser orquestrada manualmente por meio de cmdlets do PowerShell de réplica de armazenamento. No diagrama acima, ClusterA é o primário e ClusterB é o secundário. Se ClusterA ficar inativo, você precisa definir manualmente ClusterB como primário antes de você colocar os recursos de. Uma vez ClusterA fazer backup, você precisa torná-lo secundário. Depois que todos os dados foram sincronizadas, faça a alteração e trocar as funções de volta à forma como eles foram originalmente definidos.
4.  Uma vez que a réplica de armazenamento só é replicar os dados, uma nova máquina virtual ou a Scale-Out arquivo SOFS (servidor) utilizando esses dados precisaria ser criado dentro do Gerenciador de Cluster de Failover no parceiro de réplica.

A réplica de armazenamento pode ser usada se você tiver máquinas virtuais ou um SOFS em execução no seu cluster. Trazer recursos online na réplica HCI pode ser manual ou automatizada com o uso de scripts do PowerShell.

## <a name="hyper-v-replica"></a>Réplica do Hyper-V

[Réplica do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fornece replicação em nível de máquina virtual para recuperação de desastres em infra-estruturas hiperconvergente. O que a réplica do Hyper-V pode fazer é pegar uma máquina virtual e replicá-la para um site secundário ou do Azure (réplica). Em seguida, do site secundário, réplica do Hyper-V pode replicar a máquina virtual para um terceiro (réplica estendida).

![Diagrama de replicação do Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Com a réplica do Hyper-V, a replicação é tratada pelo Hyper-V. Quando você habilita uma máquina virtual para replicação, há três opções para como você deseja que a cópia inicial a ser enviado para o cluster de réplica correspondente (s).

1.  Enviar a cópia inicial pela rede
2.  Enviar a cópia inicial para mídia externa, de forma que ele pode ser copiado para seu servidor manualmente
3.  Usar uma máquina virtual existente já criada nos hosts de réplica

A outra opção é para quando você deseja que a replicação inicial deve ocorrer.

1.  Iniciar a replicação imediatamente
2.  Agende um horário para quando ocorre a replicação inicial. 

Outras considerações, que será necessário são:

- Do VHD/VHDX qual você deseja replicar. Você pode optar por replicar todas elas ou apenas um deles.
- Número de pontos de recuperação que você deseja salvar. Se você quiser ter várias opções sobre qual ponto no tempo que você deseja restaurar, você desejaria especificar quantos você deseja. Se você quiser apenas um ponto de restauração, você pode escolher que também.
- A frequência com que você deseja ter a Volume Shadow Copy Service (VSS) replicar uma cópia de sombra incremental.
- A frequência com que as alterações são replicadas (30 segundos, 5 minutos, 15 minutos).

Quando HCI participa da réplica do Hyper-V, você deve ter o [agente de réplica do Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) recursos criados em cada cluster. Este recurso faz várias coisas:

1.  Fornece um único namespace para cada cluster para a réplica do Hyper-V para se conectar ao.
2.  Determina em qual nó dentro do próprio cluster de réplica (ou réplica estendida) residirá no quando ele recebe primeiro a cópia.
3.  Controla de qual nó possui a réplica (ou réplica estendida) caso a máquina virtual é movida para outro nó. Ele precisa controlar isso de forma que quando ocorre a replicação, ele pode enviar as informações para o nó adequado.

## <a name="backup-and-restore"></a>Backup e restauração

Uma opção de recuperação de desastres tradicionais que não falamos sobre muito, mas é tão importante é a falha de todo o cluster ou um nó no cluster. Uma das opções com esse cenário faz uso do Backup do Windows NT. 

É sempre uma recomendação para ter backups periódicos da infraestrutura hiperconvergente. Enquanto o serviço de Cluster está em execução, se você executar um Backup de estado do sistema, o banco de dados de registro de cluster seria uma parte desse backup. Restaurar o banco de dados ou o cluster tem dois métodos diferentes (não-autoritativo no domínio e autoritativo no domínio).

### <a name="non-authoritative"></a>Não autoritativa

Uma restauração não autoritativa pode ser feita usando o Backup do Windows NT e é igual a uma restauração completa do que apenas o próprio nó de cluster. Se você precisar restaurar um nó de cluster (e o banco de dados de registro de cluster) e todos os cluster informações atuais boa, você deve restaurar usando o não autoritativa. Restaurações autoritativas de não podem ser feitas por meio da interface do Windows NT Backup ou a linha de comando WBADMIN. EXE.

Depois de restaurar o nó, deixe-o ingressar no cluster. O que acontece é que ele sair para o cluster em execução existente e atualizar todas as suas informações com o que há no momento.

### <a name="authoritative"></a>Autoritativo

Uma restauração autoritativa da configuração do cluster, por outro lado, usa a configuração de cluster de volta no tempo. Esse tipo de restauração só deve ser realizado quando as informações de cluster foi perdidas e necessidades restauradas. Por exemplo, alguém acidentalmente excluído um servidor de arquivos que continha mais de 1000 compartilhamentos e você precisar deles novamente. A conclusão de uma restauração autoritativa do cluster requer que o Backup ser executado da linha de comando.

Quando uma restauração autoritativa é iniciada em um nó de cluster, o serviço de cluster é interrompido em todos os outros nós na exibição do cluster e a configuração do cluster está congelada. É por isso é essencial que o serviço de cluster no nó em que a restauração foi executada ser iniciado pela primeira vez para que o cluster é formado usando a nova cópia da configuração do cluster.

Para executar por meio de uma restauração autoritativa, as etapas a seguir podem ser feitas.

1.  Execute o WBADMIN. EXE em um prompt de comando administrativo para obter a versão mais recente de backups que você deseja instalar e certifique-se de que o estado do sistema é um dos componentes que você pode restaurar.

    ```powershell
    Wbadmin get versions
    ```

2.  Determine se o backup da versão que tem as informações de registro de cluster nele como um componente. Há alguns itens que você precisará desse comando, a versão e o aplicativo/componente para uso na etapa 3. Para obter a versão, por exemplo, digamos que o backup foi feito em 3 de janeiro de 2018 2h04 e isso é aquele que você precisa restaurado.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Inicie a restauração autoritativa para recuperar apenas a versão de registro de cluster que é necessário. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Depois que a restauração tiver sido feita, esse nó deve ser o para iniciar o serviço de Cluster pela primeira vez e formar o cluster. Todos os outros nós, em seguida, precisa ser iniciado e ingressar no cluster.

## <a name="summary"></a>Resumo 

Para somar tudo isso, a recuperação de desastres hiperconvergente é algo que deve ser out cuidadosamente planejada. Há vários cenários que podem mais adequado às suas necessidades e devem ser totalmente testados. Um item a observar é que, se você estiver familiarizado com Clusters de Failover no passado, clusters estendidos foram uma opção popular muito ao longo dos anos. Houve um pouco de uma alteração de design com a solução hiperconvergente e ele se baseia em resiliência. Se você perder os dois nós em um cluster hiperconvergente, todo o cluster ficará inativo. Com assim sendo, em um ambiente hiperconvergente, não há suporte para o cenário de ampliação.


