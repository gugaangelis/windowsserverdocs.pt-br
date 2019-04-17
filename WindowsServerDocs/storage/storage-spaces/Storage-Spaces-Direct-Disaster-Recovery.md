---
title: Cenários de recuperação de desastres de infraestrutura Hiperconvergente
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Este artigo descreve os cenários disponíveis atualmente para recuperação de desastres do Microsoft HCI (espaços de armazenamento diretos)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678605"
---
# Recuperação de desastres com espaços de armazenamento diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece cenários em como infraestrutura hiperconvergente (HCI) podem ser configurados para recuperação de desastres.

Várias empresas estão executando soluções com hiperconvergência e planejando um desastre oferece a capacidade de permanecer em ou voltar à produção rapidamente se ocorrer um desastre. Há várias maneiras de configurar HCI para recuperação de desastres e este documento explica as opções que estão atualmente disponíveis para você.

Quando discussões de restauração disponibilidade se ocorrer desastre giram em torno de que é conhecido como objetivo de tempo de recuperação (RTO). Isso é a duração de tempo direcionada onde os serviços devem ser restaurados para evitar consequências inaceitáveis para os negócios. Em alguns casos, esse processo pode ocorrer automaticamente com a produção restaurada quase imediatamente. Em outros casos, intervenção manual administrador deve ocorrer para restaurar os serviços.

As opções de recuperação de desastres com um hiperconvergente hoje são:

1. Vários clusters utilizando a réplica de armazenamento
2. Réplica do Hyper-V entre clusters
3. Backup e Restauração

## Vários clusters utilizando a réplica de armazenamento

[A réplica de armazenamento](../storage-replica/storage-replica-overview.md) permite a replicação de volumes e oferece suporte à replicação síncrona e assíncrona. Ao escolher entre usar replicação síncrona ou assíncrona, você deve considerar seu objetivo de ponto de recuperação (RPO). Objetivo de ponto de recuperação é a quantidade de possível perda de dados que está disposto a incorre antes que ele seja considerado perda principais. Se você optar por replicação síncrona, ele sequencialmente gravará em ambas as extremidades ao mesmo tempo. Se você optar por assíncrona, gravações replicarão muito rápido, mas ainda poderão ser perdidas. Você deve considerar o uso de arquivos ou aplicativos para ver o que funciona melhor para você.

A réplica de armazenamento é um mecanismo de cópia de nível de bloco em comparação com nível de arquivo; ou seja, não importa quais tipos de dados replicados. Isso torna uma opção popular para infraestrutura hiperconvergente. A réplica de armazenamento também pode utilizar diferentes tipos de unidades entre os parceiros de replicação, portanto, ter todo o armazenamento de um tipo em um HCI e outro armazenamento de tipo em outro é perfeito. 

Um recurso importante de réplica de armazenamento é que ele pode ser executado no Azure, bem como no local. Você pode configurar local para locais, do Azure para Azure, ou até mesmo local ao Azure (ou vice-versa).

Nesse cenário, há dois clusters separados de independentes. Para configurar a réplica de armazenamento entre HCI, você pode seguir as etapas na [replicação de armazenamento de cluster para Cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de replicação de armazenamento](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

As considerações a seguir se aplicam ao implantar a réplica de armazenamento. 

1.  Configurar a replicação é feita fora do cluster de Failover. 
2.  Escolher o método de replicação será dependente de seus requisitos de RPO e latência da rede. Síncrono replica os dados em redes de baixa latência com consistência de falhas para garantir que nenhuma perda de dados em vez de falha. Assíncrona replica os dados em redes com latências maiores, mas cada site podem não ter cópias idênticas em vez de falha. 
3.  No caso de desastre, failovers entre os clusters não são automáticas e precisará ser coordenados manualmente por meio de cmdlets do PowerShell de réplica de armazenamento. No diagrama acima, ClusterA é o principal e ClusterB é o secundário. Se ClusterA falhar, você precisará definir manualmente ClusterB como primário antes que você pode exibir os recursos. Uma vez ClusterA fazer backup, você precisa para torná-lo secundário. Depois que todos os dados tem sido sincronizados para cima, fazer a alteração e troque as funções de volta à maneira como eles foram originalmente definidos.
4.  Desde que a réplica de armazenamento está replicando apenas os dados, uma nova máquina virtual ou escala Out arquivo SOFS (servidor) que utilizam esses dados precisaria ser criado dentro do Gerenciador de Cluster de Failover no parceiro de réplica.

A réplica de armazenamento pode ser usada se você tiver máquinas virtuais ou um SOFS em execução no cluster. Trazer recursos online na réplica HCI pode ser manual ou automática com o uso de scripts do PowerShell.

## Réplica do Hyper-V

[Réplica do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fornece replicação em nível de máquina virtual para recuperação de desastres sobre infraestruturas de hiperconvergência. Réplica do Hyper-V que pode fazer é criar uma máquina virtual e replicá-lo para um local secundário ou Azure (réplica). Em seguida, do local secundário, réplica do Hyper-V podem replicar a máquina virtual em um terceiro (réplica estendida).

![Diagrama de replicação do Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Com a réplica do Hyper-V, a replicação é administrada pelo Hyper-V. Quando você habilita primeiro uma máquina virtual para replicação, há três opções de como você desejar a cópia inicial a serem enviadas para os clusters de réplica correspondente.

1.  Enviar a cópia inicial pela rede
2.  Enviar a cópia inicial para mídia externa para que ele pode ser copiado para o servidor manualmente
3.  Use uma máquina virtual existente já criada nos hosts de réplica

A outra opção é para quando você quiser que essa replicação inicial deve ocorrer.

1.  Iniciar a replicação imediatamente
2.  Agende um horário para quando ocorre a replicação inicial. 

Outras considerações que será necessário são:

- Do qual VHD/VHDX você deseja replicar. Você pode optar por replicar todos eles ou apenas um deles.
- Número de pontos de recuperação que você deseja salvar. Se você quiser ter várias opções sobre que ponto no tempo que você deseja restaurar, você desejaria especificar quantas que você deseja. Se você quiser apenas um ponto de restauração, você pode escolher que também.
- Como muitas vezes desejar que o serviço de cópia de sombra de Volume (VSS) replicar uma cópia de sombra incremental.
- Frequência as alterações são replicadas (30 segundos, 5 minutos, 15 minutos).

Quando HCI participar da réplica do Hyper-V, você deve ter o recurso de [Agente de réplica do Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) criado em cada cluster. Esse recurso faz várias coisas:

1.  Oferece um único namespace para cada cluster para réplica do Hyper-V para se conectar a.
2.  Determina qual nó dentro do próprio cluster a réplica (ou réplica estendida) residirá quando ele recebe primeiro a cópia.
3.  Mantém o controle do nó que possui a réplica (ou réplica estendida) caso a máquina virtual é movido para outro nó. Ele precisa controlar isso para que quando a replicação ocorre, ele poderá enviar as informações para o nó adequado.

## Backup e restauração

Uma opção de recuperação de desastres tradicionais que não falamos sobre muito, mas é tão importante é a falha de todo o cluster ou um nó no cluster. Qualquer uma das opções com esse cenário faz uso do Backup do Windows NT. 

É sempre uma recomendação ter backups periódicos da infraestrutura hiperconvergente. Enquanto o serviço de Cluster é executado, se você executar um Backup de estado do sistema, o banco de dados de registro de cluster seria uma parte de backup. Restaurar o cluster ou o banco de dados tem dois métodos diferentes (Non-autoritativos e autoritativos).

### Não é de autorização

Uma restauração não autorizada pode ser feita usando o Backup do Windows NT e equivale a uma restauração completa dos apenas o nó de cluster em si. Se você precisar restaurar um nó de cluster (e o banco de dados de registro de cluster) e todos os cluster informações atuais BOM, você faria restaurar usando não autorizada. Restaurações não autoritativo podem ser feitas por meio da interface do Windows NT Backup ou a linha de comando WBADMIN. EXE.

Depois de restaurar o nó, permitir que ele ingressar no cluster. O que acontecerá é que ele será sair ao cluster em execução existente e atualizar todas as suas informações com o que há no momento.

### Autoritativo

Uma restauração autoritativa da configuração do cluster, por outro lado, leva a configuração do cluster no momento. Esse tipo de restauração deve ser feito somente quando as informações de cluster em si foi perdidas e precisa restauradas. Por exemplo, alguém acidentalmente excluído um servidor de arquivos que continha mais de 1.000 compartilhamentos e você precisa deles novamente. Concluir uma restauração autoritativa do cluster requer que o Backup ser executado da linha de comando.

Quando uma restauração de autorização é iniciada em um nó de cluster, o serviço de cluster é interrompido em todos os outros nós no modo de exibição de cluster e a configuração do cluster é congelada. É por isso é fundamental que o serviço de cluster no nó no qual a restauração foi executada ser iniciado pela primeira vez para que o cluster é formado usando a nova cópia da configuração do cluster.

Para executar uma restauração autoritativa, as etapas a seguir podem ser feitas.

1.  Execute WBADMIN. EXE em um prompt de comando administrativo para obter a versão mais recente de backups que você deseja instalar e certifique-se de que o estado do sistema é um dos componentes que você pode restaurar.

    ```powershell
    Wbadmin get versions
    ```

2.  Determine se o backup de versão que você tiver tem as informações de registro de cluster nele como um componente. Há alguns itens que você precisará desse comando, a versão e o aplicativo/componente para uso na etapa 3. Para obter a versão, por exemplo, digamos que o backup foi feito 3 de janeiro de 2018 2h04 e isso é o que você precisa restaurada.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Inicie a restauração autoritativa para recuperar somente a versão de registro do cluster que é necessário. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Depois que a restauração tiver sido feita, esse nó deve ser a pessoa para iniciar o serviço de Cluster primeiro e formar o cluster. Todos os outros nós, em seguida, precisaria ser iniciado e ingressar no cluster.

## Resumo 

Para Some tudo isso, a recuperação de desastres hiperconvergente é algo que deve ser planejada out com cuidado. Há vários cenários que podem mais adequado às suas necessidades e devem ser totalmente testados. Um item a observar é que, se você estiver familiarizado com os Clusters de Failover no passado, clusters estendidos foram uma opção muito popular ao longo dos anos. Houve um pouco de uma alteração de design com a solução hiperconvergente e é baseado em resiliência. Se você perder dois nós em um cluster hiperconvergente, todo o cluster passará para baixo. Com esse o caso, em um ambiente hiperconvergente, sendo o cenário estendido não é compatível.


