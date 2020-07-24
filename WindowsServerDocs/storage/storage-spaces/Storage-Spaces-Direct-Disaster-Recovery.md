---
title: Cenários de recuperação de desastre para a infraestrutura hiperconvergente
ms.prod: windows-server
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/29/2018
description: Este artigo descreve os cenários disponíveis hoje para recuperação de desastre do Microsoft HCI (Espaços de Armazenamento Diretos)
ms.localizationpriority: medium
ms.openlocfilehash: 5c9c36e90f9bfae053197b6a36201748cb7e88d7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966448"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Recuperação de desastre com Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece cenários sobre como a HCI (infraestrutura hiperconvergente) pode ser configurada para recuperação de desastres.

Várias empresas que estão executando soluções hiperconvergentes e planejando um desastre oferecem a capacidade de permanecer em ou voltar à produção rapidamente, caso ocorra um desastre. Há várias maneiras de configurar o HCI para recuperação de desastres e este documento explica as opções disponíveis hoje.

Quando ocorrerem discussões para restaurar a disponibilidade, se ocorrer um desastre, o que é conhecido como RTO (objetivo de tempo de recuperação). Essa é a duração de tempo direcionada para onde os serviços devem ser restaurados para evitar consequências inaceitáveis na empresa. Em alguns casos, esse processo pode ocorrer automaticamente com a produção restaurada quase imediatamente. Em outros casos, a intervenção manual do administrador deve ocorrer para restaurar os serviços.

As opções de recuperação de desastre com um hiperconvergente hoje são:

1. Vários clusters utilizando a réplica de armazenamento
2. Réplica do Hyper-V entre clusters
3. Backup e restauração

## <a name="multiple-clusters-utilizing-storage-replica"></a>Vários clusters utilizando a réplica de armazenamento

A [réplica de armazenamento](../storage-replica/storage-replica-overview.md) permite a replicação de volumes e oferece suporte à replicação síncrona e assíncrona. Ao escolher entre usar a replicação síncrona ou assíncrona, você deve considerar seu RPO (objetivo de ponto de recuperação). Objetivo de ponto de recuperação é a quantidade de possíveis perdas de dados que você está disposto a incorrer antes de ser considerada uma grande perda. Se você for com replicação síncrona, ela será gravada em sequência em ambas as extremidades ao mesmo tempo. Se você for assíncrona, as gravações serão replicadas muito rapidamente, mas ainda poderão ser perdidas. Você deve considerar o uso do aplicativo ou do arquivo para ver o que melhor funciona para você.

A réplica de armazenamento é um mecanismo de cópia de nível de bloco versus nível de arquivo; ou seja, não importa quais tipos de dados estão sendo replicados. Isso o torna uma opção popular para a infraestrutura hiperconvergente. A réplica de armazenamento também pode utilizar diferentes tipos de unidades entre os parceiros de replicação, portanto, ter todo o armazenamento de um tipo em um HCI e outro armazenamento de tipo no outro é perfeitamente bom. 

Um recurso importante da réplica de armazenamento é que ela pode ser executada no Azure, bem como no local. Você pode configurar de forma local para local, Azure para Azure ou até mesmo localmente para o Azure (ou vice-versa).

Nesse cenário, há dois clusters independentes separados. Para configurar a réplica de armazenamento entre o HCI, você pode seguir as etapas em [replicação de armazenamento de cluster para cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagrama de replicação de armazenamento](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

As considerações a seguir se aplicam ao implantar a réplica de armazenamento. 

1.    A configuração da replicação é feita fora do clustering de failover. 
2.    Escolher o método de replicação dependerá da latência de rede e dos requisitos de RPO. O Synchronous replica os dados em redes de baixa latência com consistência de falhas para garantir que não haja perda de dados em um momento de falha. O assíncrona replica os dados em redes com latências mais altas, mas cada site pode não ter cópias idênticas em um momento de falha. 
3.    No caso de um desastre, os failovers entre os clusters não são automáticos e precisam ser orquestrados manualmente por meio dos cmdlets do PowerShell da réplica de armazenamento. No diagrama acima, ClusterA é o primário e o ClusterB é o secundário. Se o ClusterA ficar inativo, você precisará definir manualmente ClusterB como primário antes de poder colocar os recursos. Depois de fazer backup do ClusterA, você precisaria torná-lo secundário. Depois que todos os dados tiverem sido sincronizados, faça a alteração e troque as funções de volta à maneira como elas foram originalmente definidas.
4.    Como a réplica de armazenamento só está replicando os dados, uma nova máquina virtual ou um SOFS (servidor de arquivos de Scale Out) utilizando esses dados precisarão ser criados dentro do Gerenciador de Cluster de Failover no parceiro de réplica.

A réplica de armazenamento pode ser usada se você tiver máquinas virtuais ou um SOFS em execução no cluster. Colocar os recursos online no HCI da réplica pode ser manual ou automatizado por meio do uso de scripts do PowerShell.

## <a name="hyper-v-replica"></a>Réplica do Hyper-V

[A réplica do Hyper-V](../../virtualization/hyper-v/manage/set-up-hyper-v-replica.md) fornece replicação em nível de máquina virtual para recuperação de desastre em infraestruturas hiperconvergentes. O que a réplica do Hyper-V pode fazer é pegar uma máquina virtual e duplicá-la em um site secundário ou Azure (réplica). Em seguida, no site secundário, a réplica do Hyper-V pode replicar a máquina virtual para uma terceira (réplica estendida).

![Diagrama de replicação do Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Com a réplica do Hyper-V, a replicação é manipulada pelo Hyper-V. Quando você habilita pela primeira vez uma máquina virtual para replicação, há três opções de como você deseja que a cópia inicial seja enviada para os clusters de réplica correspondentes.

1.    Enviar a cópia inicial pela rede
2.    Enviar a cópia inicial para a mídia externa para que ela possa ser copiada para o servidor manualmente
3.    Usar uma máquina virtual existente já criada nos hosts de réplica

A outra opção é para quando você desejar que a replicação inicial ocorra.

1.    Iniciar a replicação imediatamente
2.    Agende um horário para quando a replicação inicial ocorrer. 

Outras considerações que serão necessárias são:

- O VHD/VHDX que você deseja replicar. Você pode optar por replicar todos eles ou apenas um deles.
- Número de pontos de recuperação que você deseja salvar. Se você quiser ter várias opções sobre o ponto no tempo que deseja restaurar, convém especificar o número desejado. Se você quiser apenas um ponto de restauração, também poderá escolher isso.
- Com que frequência você deseja que o Serviço de Cópias de Sombra de Volume (VSS) replique uma cópia de sombra incremental.
- Com que frequência as alterações são replicadas (30 segundos, 5 minutos, 15 minutos).

Quando o HCI participa da réplica do Hyper-V, você deve ter o recurso [agente de réplica do Hyper-v](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization) criado em cada cluster. Esse recurso faz várias coisas:

1.    Fornece um namespace único para cada cluster para que a réplica do Hyper-V se conecte.
2.    Determina em qual nó do cluster a réplica (ou réplica estendida) residirá quando receber a cópia pela primeira vez.
3.    Mantém o controle de qual nó possui a réplica (ou réplica estendida), caso a máquina virtual seja movida para outro nó. Ele precisa controlar isso para que, quando a replicação ocorrer, possa enviar as informações para o nó apropriado.

## <a name="backup-and-restore"></a>Backup e restauração

Uma opção de recuperação de desastres tradicional que não é comentada muito, mas é tão importante quanto a falha de todo o cluster ou de um nó no cluster. Qualquer opção com esse cenário usa o backup do Windows NT. 

É sempre uma recomendação ter backups periódicos da infraestrutura hiperconvergente. Enquanto o serviço de cluster estiver em execução, se você fizer um backup de estado do sistema, o banco de dados do registro de cluster será parte desse backup. A restauração do cluster ou do banco de dados tem dois métodos diferentes (não autoritativo e autoritativos).

### <a name="non-authoritative"></a>Não autoritativo

Uma restauração não autoritativa pode ser realizada usando o backup do Windows NT e equivale a uma restauração completa apenas do nó de cluster em si. Se você só precisa restaurar um nó de cluster (e o banco de dados do registro de cluster) e todas as informações de cluster atuais forem válidas, restaure usando não autoritativo. As restaurações não autorizadas podem ser feitas por meio da interface de backup do Windows NT ou da WBADMIN.EXE de linha de comando.

Depois de restaurar o nó, permita que ele ingresse no cluster. O que acontecerá é que ele irá para o cluster em execução existente e atualizará todas as suas informações com o que está atualmente lá.

### <a name="authoritative"></a>Autoriza

Por outro lado, uma restauração autoritativa da configuração de cluster leva a configuração de cluster de volta no tempo. Esse tipo de restauração deve ser realizado somente quando as informações do cluster em si forem perdidas e as necessidades forem restauradas. Por exemplo, alguém excluiu acidentalmente um servidor de arquivos que continha mais de 1000 compartilhamentos e você precisa deles de volta. A conclusão de uma restauração autoritativa do cluster requer que o backup seja executado na linha de comando.

Quando uma restauração autoritativa é iniciada em um nó de cluster, o serviço de cluster é interrompido em todos os outros nós na exibição do cluster e a configuração do cluster é congelada. É por isso que é importante que o serviço de cluster no nó no qual a restauração foi executada seja iniciado primeiro, para que o cluster seja formado usando a nova cópia da configuração do cluster.

Para executar uma restauração autoritativa, as etapas a seguir podem ser realizadas.

1.    Execute WBADMIN.EXE em um prompt de comando administrativo para obter a versão mais recente dos backups que você deseja instalar e garantir que o estado do sistema seja um dos componentes que você pode restaurar.

    ```powershell
    Wbadmin get versions
    ```

2.    Determine se o backup de versão tem as informações de registro de cluster nele como um componente. Há alguns itens que serão necessários nesse comando, a versão e o aplicativo/componente para uso na etapa 3. Para a versão, por exemplo, digamos que o backup foi feito em 3 de janeiro de 2018 em 2:04am e esse é o que você precisa restaurar.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Inicie a restauração autoritativa para recuperar apenas a versão de registro de cluster necessária. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Depois que a restauração for feita, esse nó deverá ser o primeiro a iniciar o serviço de cluster e formar o cluster. Todos os outros nós precisariam ser iniciados e ingressarem no cluster.

## <a name="summary"></a>Resumo 

Para somar tudo isso, a recuperação de desastre hiperconvergente é algo que deve ser planejado com cuidado. Há vários cenários que podem atender melhor às suas necessidades e devem ser totalmente testados. Um item a ser observado é que, se você estiver familiarizado com clusters de failover no passado, os clusters de ampliação têm sido uma opção muito popular ao longo dos anos. Houve um pouco de alteração de design com a solução hiperconvergente e ela se baseia na resiliência. Se você perder dois nós em um cluster hiperconvergente, todo o cluster ficará inativo. Com esse ser o caso, em um ambiente hiperconvergente, não há suporte para o cenário de ampliação.
