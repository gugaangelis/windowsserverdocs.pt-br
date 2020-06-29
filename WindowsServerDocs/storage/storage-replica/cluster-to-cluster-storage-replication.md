---
title: Replicação de armazenamento de cluster para cluster
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
ms.assetid: 834e8542-a67a-4ba0-9841-8a57727ef876
author: nedpyle
ms.date: 04/26/2019
description: Como usar a réplica de armazenamento para replicar volumes em um cluster para outro cluster que executa o Windows Server.
ms.openlocfilehash: 21e054d42d0264bb22fbd0e02382ee429958a597
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475663"
---
# <a name="cluster-to-cluster-storage-replication"></a>Replicação de armazenamento de cluster para cluster

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

A réplica de armazenamento pode replicar volumes entre clusters, incluindo a replicação de clusters usando Espaços de Armazenamento Diretos. O gerenciamento e a configuração são semelhantes aos da replicação de servidor para servidor.

Você configurará esses computadores e o armazenamento em uma configuração de cluster para cluster, em que um cluster replica seu próprio conjunto de armazenamento com outro cluster e seu conjunto de armazenamento. Esses nós e seu armazenamento devem estar localizados em locais físicos separados, embora isso não seja obrigatório.

> [!IMPORTANT]
> No teste, os quatro servidores são um exemplo. Você pode usar qualquer número de servidores com suporte da Microsoft em cada cluster, que atualmente é 8 para um cluster Espaços de Armazenamento Diretos e 64 para um cluster de armazenamento compartilhado.
>
> Este guia não abrange a configuração de Espaços de Armazenamento Diretos. Para obter informações sobre como configurar Espaços de Armazenamento Diretos, consulte [espaços de armazenamento diretos visão geral](../storage-spaces/storage-spaces-direct-overview.md).

Este passo a passo usa o seguinte ambiente como exemplo:

-   Dois servidores membros, denominados **SR-SRV01** e **SR-SRV02** que posteriormente são transformados em um cluster denominado **SR-SRVCLUSA**.

-   Dois servidores membros, denominados **SR-SRV03** e **SR-SRV04** que posteriormente são transformados em um cluster denominado **SR-SRVCLUSB**.

-   Um par de "locais" lógicos que representam dois data centers diferentes, com um chamado **Redmond** e um chamado **Bellevue**.

![Diagrama que mostra um ambiente de exemplo com um cluster no site de Redmond replicando com um cluster no site Bellevue](./media/Cluster-to-Cluster-Storage-Replication/SR_ClustertoCluster.png)

**FIGURA 1: replicação de cluster para cluster**

## <a name="prerequisites"></a>Pré-requisitos

* Floresta dos Active Directory Domain Services (não precisa ser executada no Windows Server 2016).
* 4-128 servidores (dois clusters de servidores 2-64) que executam o Windows Server 2019 ou o Windows Server 2016, Datacenter Edition. Se estiver executando o Windows Server 2019, você poderá usar a Standard Edition se estiver OK replicando apenas um único volume de até 2 TB de tamanho.
* Dois conjuntos de armazenamento, usando JBODs SAS, SAN fibre channel, Shared VHDX, Storage Spaces Direct ou iSCSI target. O armazenamento deve conter uma mistura de mídias HDD e SSD. Você disponibilizará cada conjunto de armazenamento apenas para cada um dos clusters, sem acesso compartilhado entre clusters.
* Cada conjunto de armazenamento deve permitir a criação de pelo menos dois discos virtuais, um para dados replicados e outro para logs. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de dados. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de logs.
* Pelo menos uma conexão de Ethernet/TCP em cada servidor para replicação síncrona, mas, preferencialmente, RDMA.
* Regras de firewall e roteador apropriadas para permitir ICMP, SMB (porta 445, além de 5445 para SMB Direct) e tráfego bidirecional de WS-MAN (porta 5985) entre todos os nós.
* Uma rede entre servidores com largura de banda suficiente para conter a carga de trabalho de gravação de E/S e uma média de =5 ms de latência de viagem de ida e volta, para replicação síncrona. A replicação assíncrona não tem uma recomendação de latência.
* O armazenamento replicado não pode ser localizado na unidade que contém a pasta do sistema operacional Windows.
* Há considerações importantes & limitações para a replicação de Espaços de Armazenamento Diretos-consulte as informações detalhadas abaixo.

Muitos desses requisitos podem ser determinados usando o cmdlet `Test-SRTopology`. Você obtém acesso a essa ferramenta se instalar os recursos Ferramentas de Gerenciamento de Réplica de Armazenamento ou Réplica de Armazenamento em pelo menos um servidor. Não é necessário configurar a Réplica de Armazenamento para usar essa ferramenta, apenas para instalar o cmdlet. Mais informações estão incluídas nas etapas abaixo.

## <a name="step-1-provision-operating-system-features-roles-storage-and-network"></a>Etapa 1: Provisionar o sistema operacional, os recursos, as funções, o armazenamento e a rede

1.  Instale o Windows Server em todos os quatro nós de servidor com um tipo de instalação do Windows Server **(experiência desktop)**.

2.  Adicione as informações de rede e vincule-as ao domínio, depois reinicie-as.

    > [!IMPORTANT]
    > Desse ponto em diante, sempre faça logon como um usuário de domínio que é membro do grupo de administradores internos em todos os servidores. Lembre-se sempre de elevar seus prompts de CMD e do Windows PowerShell ao executar em uma instalação de servidor gráfico ou em um computador com Windows 10.

3.  Conecte o primeiro conjunto de compartimento de armazenamento JBOD, destino iSCSI, SAN FC ou armazenamento em disco fixo local (DAS) ao servidor no local **Redmond**.

4.  Conecte o segundo conjunto de armazenamento ao servidor no local **Bellevue**.

5.  Conforme for apropriado, instale o firmware e os drivers mais recentes de fornecedor de armazenamento e compartimento, os drivers mais recentes de fornecedor de HBA, o firmware mais recente do fornecedor de UEFI/BIOS, os drivers mais recentes de fornecedor de rede e os drivers mais recentes de chipsets da placa-mãe em todos os quatro nós. Reinicie os nós conforme necessário.

    > [!NOTE]
    > confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.

6.  Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Verifique se o gerenciamento de energia no Windows Server está definido como alto desempenho. Reinicie conforme necessário.

7.  Configure as funções da seguinte maneira:

    -   **Método gráfico**

        1.  Execute **ServerManager.exe** e crie um grupo de servidores, adicionando todos os nós de servidor.

        2.  Instale as funções e os recursos de **Servidor de Arquivos** e **Réplica de Armazenamento** em cada um dos nós e reinicie-os.

    -   **Método do Windows PowerShell**

        No SR-SRV04 ou em um computador de gerenciamento remoto, execute o seguinte comando em um console do Windows PowerShell para instalar os recursos e as funções necessárias para um cluster estendido nos quatro nós e reinicie-os:

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }
        ```

        Para obter mais informações sobre essas etapas, consulte [instalar ou desinstalar funções, serviços de função ou recursos](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

9. Configure o armazenamento da seguinte maneira:

    > [!IMPORTANT]
    > -   Você deve criar dois volumes em cada compartimento: um para dados e outro para logs.
    > -   Os discos de log e de dados devem ser inicializados como **GPT**, não **MBR**.
    > -   Os dois volumes de dados devem ser de tamanho idêntico.
    > -   Os dois volumes de log devem ser de tamanho idêntico.
    > -   Todos os discos de dados replicados devem ter os mesmos tamanhos de setor.
    > -   Todos os discos de log devem ter os mesmos tamanhos de setor.
    > -   Os volumes de log devem usar armazenamento baseado em flash, como SSD.  A Microsoft recomenda que o armazenamento de log seja mais rápido do que o armazenamento de dados. Volumes de log nunca devem ser usados para outras cargas de trabalho.
    > -   Os discos de dados podem usar HDD, SSD ou uma combinação em camadas e podem usar os espaços espelhados ou de paridade, ou RAID 1 ou 10, RAID 5 ou RAID 50.
    > -   O volume de log deve ter pelo menos 8 GB por padrão e pode ser maior ou menor com base nos requisitos de log.
    > -   Ao usar Espaços de Armazenamento Diretos (Espaços de Armazenamento Diretos) com um cache NVME ou SSD, você verá um aumento maior que o esperado em latência ao configurar a replicação de réplica de armazenamento entre clusters de Espaços de Armazenamento Diretos. A alteração na latência é proporcionalmente muito maior do que você vê ao usar o NVME e SSD em uma configuração de desempenho + capacidade e nenhuma camada de HDD nem camada de capacidade.

    Esse problema ocorre devido a limitações arquitetônicas no mecanismo de log do SR, combinadas com a latência extremamente baixa de NVME quando comparada à mídia mais lenta. Ao usar Espaços de Armazenamento Diretos cache Espaços de Armazenamento Diretos, todas as e/s de logs do SR, juntamente com todas as e/s de leitura/gravação recentes de aplicativos, ocorrerão no cache e nunca nas camadas de desempenho ou capacidade. Isso significa que todas as atividades do SR acontecem na mesma mídia de velocidade – não há suporte para essa configuração não é recomendável (consulte https://aka.ms/srfaq para obter recomendações de log).

    Ao usar Espaços de Armazenamento Diretos com HDDs, você não pode desabilitar ou evitar o cache. Como alternativa, se estiver usando apenas SSD e NVME, você pode configurar apenas as camadas de desempenho e capacidade. Se estiver usando essa configuração e posicionar os logs do SR no nível de desempenho somente com os volumes de dados em que eles estão sendo atendidos apenas na camada de capacidade, você evitará o problema de alta latência descrito acima. O mesmo pode ser feito com uma combinação de SSDs mais rápidos e lentos e sem NVME.

    Essa solução alternativa não é ideal e alguns clientes podem não conseguir usá-la. A equipe do SR está trabalhando em otimizações e no mecanismo de log atualizado para o futuro para reduzir esses afunilamentos artificiais que ocorrem. Não há nenhum ETA para isso, mas quando disponível para tocar os clientes para teste, essas perguntas frequentes serão atualizadas.

-   **Para compartimentos JBOD:**

1. Verifique se cada cluster pode ver apenas os compartimentos de armazenamento do local e se as conexões SAS estão configuradas corretamente.

2. Provisione o armazenamento usando Espaços de Armazenamento seguindo as **etapas 1 a 3** fornecidas em [Implantar espaços de armazenamento em um servidor autônomo](../storage-spaces/deploy-standalone-storage-spaces.md) usando o Windows PowerShell ou Gerenciador do Servidor.

-   **Para armazenamento de destino iSCSI:**

1. Verifique se cada cluster pode ver apenas compartimentos de armazenamento do site. Você deverá usar mais de um único adaptador de rede se usar iSCSI.

2. Provisione o armazenamento usando a documentação do fornecedor. Se estiver usando o destino iSCSI baseado em Windows, confira [Armazenamento em bloco de destino iSCSI, Como](../iscsi/iscsi-target-server.md).

-   **Para o armazenamento SAN FC:**

1. Verifique se cada cluster pode ver apenas os compartimentos de armazenamento desse local e se você definiu corretamente as zonas dos hosts.

2. Provisione o armazenamento usando a documentação do fornecedor.

-   **Para Espaços de Armazenamento Diretos:**

1. Verifique se cada cluster pode ver apenas os compartimentos de armazenamento do local implantando Espaços de Armazenamento Diretos. (https://docs.microsoft.com/windows-server/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)

2. Certifique-se de que os volumes de log SR sempre estará no armazenamento flash mais rápido e os volumes de dados no armazenamento de mais lenta alta capacidade.

3. Inicie o Windows PowerShell e use o cmdlet `Test-SRTopology` para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, assim como um modo de avaliação de desempenho de execução longa.
   Por exemplo,

   ```PowerShell
   MD c:\temp

   Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV03 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp
   ```

     > [!IMPORTANT]
     > Ao usar um servidor de teste com nenhuma carga de gravação de E/S no volume de origem especificado durante o período de avaliação, adicione uma carga de trabalho ou o servidor não gerará um relatório útil. Você deve testar com cargas de trabalho de produção para ver os números reais e os tamanhos de log recomendados. Como alternativa, basta copiar alguns arquivos para o volume de origem durante o teste ou baixar e executar [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) para gerar o Ios de gravação. Por exemplo, um exemplo com uma carga de trabalho de e/s de gravação baixa por cinco minutos para o volume D:`Diskspd.exe -c1g -d300 -W5 -C5 -b8k -t2 -o2 -r -w5 -h d:\test.dat`

4. Examine o relatório **TestSrTopologyReport.html** para garantir que você atende aos requisitos da Réplica de Armazenamento.

   ![Tela que mostra os resultados do relatórios de topologia de replicação](./media/Cluster-to-Cluster-Storage-Replication/SRTestSRTopologyReport.png)

## <a name="step-2-configure-two-scale-out-file-server-failover-clusters"></a>Etapa 2: Configurar dois clusters de failover do Servidor de Arquivos de Escalabilidade Horizontal
Agora você criará dois clusters de failover normais. Após a configuração, a validação e o teste, você os replicará usando a Réplica de Armazenamento. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.

### <a name="graphical-method"></a>Método gráfico

1.  Execute **cluadmin.msc** em relação a um nó em cada site.

2.  Valide o cluster proposto e analise os resultados para garantir que você possa continuar. O exemplo usado abaixo é **SR-SRVCLUSA** e **SR-SRVCLUSB**.

3.  Crie os dois clusters. Verifique se os nomes dos clusters têm no máximo 15 caracteres.

4.  Configure uma testemunha de compartilhamento de arquivo ou testemunha de nuvem.

    > [!NOTE]
    > O WIndows Server agora inclui uma opção de testemunha baseada em nuvem (Azure). Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.

    > [!WARNING]
    > Para obter mais informações sobre a configuração de quorum, consulte a seção **configuração de testemunha** em [configurar e gerenciar quorum](../../failover-clustering/manage-cluster-quorum.md). Para saber mais sobre o cmdlet `Set-ClusterQuorum`, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).

5.  Adicione um disco no site **Redmond** ao CSV do cluster. Para fazer isso, clique com botão direito em um disco de origem no nó **Discos** da seção **Armazenamento** e, em seguida, clique em **Adicionar aos Volumes Compartilhados Clusterizados**.

6.  Crie o Servidor de Arquivos de Escalabilidade Horizontal clusterizado nos dois clusters usando as instruções em [Configurar Servidor de Arquivos de Escalabilidade Horizontal](https://technet.microsoft.com/library/hh831718.aspx)

### <a name="windows-powershell-method"></a>Método do Windows PowerShell

1.  Teste o cluster proposto e analise os resultados para garantir que você possa continuar:

    ```PowerShell
    Test-Cluster SR-SRV01,SR-SRV02
    Test-Cluster SR-SRV03,SR-SRV04
    ```

2.  Crie os clusters (você deve especificar seus próprios endereços IP estáticos para os clusters). Verifique se cada nome do cluster tem no máximo 15 caracteres:

    ```PowerShell
    New-Cluster -Name SR-SRVCLUSA -Node SR-SRV01,SR-SRV02 -StaticAddress <your IP here>
    New-Cluster -Name SR-SRVCLUSB -Node SR-SRV03,SR-SRV04 -StaticAddress <your IP here>
    ```

3.  Configure uma Testemunha de compartilhamento de arquivos ou Testemunha de nuvem (Azure) em cada cluster que aponta para um compartilhamento hospedado no controlador de domínio ou em outro servidor independente. Por exemplo:

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    > [!NOTE]
    > O WIndows Server agora inclui uma opção de testemunha baseada em nuvem (Azure). Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.

    > [!WARNING]
    > Para obter mais informações sobre a configuração de quorum, consulte a seção **configuração de testemunha** em [configurar e gerenciar quorum](../../failover-clustering/manage-cluster-quorum.md). Para saber mais sobre o cmdlet `Set-ClusterQuorum`, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).

4.  Crie o Servidor de Arquivos de Escalabilidade Horizontal clusterizado nos dois clusters usando as instruções em [Configurar Servidor de Arquivos de Escalabilidade Horizontal](https://technet.microsoft.com/library/hh831718.aspx)

## <a name="step-3-set-up-cluster-to-cluster-replication-using-windows-powershell"></a>Etapa 3: Configurar a replicação de cluster para cluster usando o Windows PowerShell
Agora você configurará a replicação de cluster para cluster usando o Windows PowerShell. Você pode executar todas as etapas abaixo nos nós diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto

1. Conceda ao primeiro cluster acesso completo ao outro cluster executando o cmdlet **Grant-SRAccess** em qualquer nó no primeiro cluster ou remotamente.  Ferramentas de Administração de Servidor Remoto do Windows Server

   ```PowerShell
   Grant-SRAccess -ComputerName SR-SRV01 -Cluster SR-SRVCLUSB
   ```

2. Conceda ao segundo cluster acesso completo ao outro cluster executando o cmdlet **Grant-SRAccess** em qualquer nó no segundo cluster ou remotamente.

   ```PowerShell
   Grant-SRAccess -ComputerName SR-SRV03 -Cluster SR-SRVCLUSA
   ```

3. Configure a replicação de cluster para cluster, especificando os discos de origem e destino, os logs de origem e destino, os nomes de cluster de origem e destino e o tamanho do log. Você pode executar esse comando localmente no servidor ou usando um computador de gerenciamento remoto.

   ```powershell
   New-SRPartnership -SourceComputerName SR-SRVCLUSA -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\Volume2 -SourceLogVolumeName f: -DestinationComputerName SR-SRVCLUSB -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\Volume2 -DestinationLogVolumeName f:
   ```

   > [!WARNING]
   > O tamanho do log padrão é 8 GB. Dependendo dos resultados do cmdlet **Test-SRTopology** , você pode decidir usar **-LogSizeInBytes** com um valor maior ou menor.

4. Para obter o estado de origem e de destino de replicação, use **Get-SRGroup** e **Get-SRPartnership** da seguinte maneira:

   ```powershell
   Get-SRGroup
   Get-SRPartnership
   (Get-SRGroup).replicas
   ```

5. Determine o andamento da replicação da seguinte maneira:

   1.  No servidor de origem, execute o comando a seguir e examine os eventos 5015, 5002, 5004, 1237, 5001 e 2200:

       ```PowerShell
       Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
       ```
   2.  No servidor de destino, execute o seguinte comando para ver os eventos de Réplica de Armazenamento que mostram a criação da parceria. Esse evento indica o número de bytes copiados e o tempo gasto. Exemplo:

       ```powershell
       Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | Format-List
       ```
       Veja um exemplo da saída:

       ```
       TimeCreated  : 4/8/2016 4:12:37 PM
       ProviderName : Microsoft-Windows-StorageReplica
       Id           : 1215
       Message      : Block copy completed for replica.
           ReplicationGroupName: rg02
           ReplicationGroupId:
           {616F1E00-5A68-4447-830F-B0B0EFBD359C}
           ReplicaName: f:\
           ReplicaId: {00000000-0000-0000-0000-000000000000}
           End LSN in bitmap:
           LogGeneration: {00000000-0000-0000-0000-000000000000}
           LogFileId: 0
           CLSFLsn: 0xFFFFFFFF
           Number of Bytes Recovered: 68583161856
           Elapsed Time (seconds): 117
       ```
   3. Como alternativa, o grupo de servidores de destino para a réplica informa o número de bytes restantes para copiar todo o tempo e pode ser consultado por meio do PowerShell. Por exemplo:

      ```PowerShell
      (Get-SRGroup).Replicas | Select-Object numofbytesremaining
      ```

      Como um exemplo de andamento (que não será encerrado):

      ```PowerShell
        while($true) {
        $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining
        [System.Console]::Write("Number of bytes remaining: {0}`n", $v.numofbytesremaining)
        Start-Sleep -s 5
       }
       ```

6. No servidor de destino no cluster de destino, execute o comando a seguir e examine os eventos 5009, 1237, 5001, 5015, 5005 e 2200 para compreender o andamento do processamento. Não deve haver nenhum aviso de erro nessa sequência. Haverá muitos eventos 1237; eles indicam o progresso.

   ```PowerShell
   Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL
   ```
   > [!NOTE]
   > O disco de cluster de destino sempre aparece como **Online (Sem acesso)** quando replicado.

## <a name="step-4-manage-replication"></a>Etapa 4: Gerenciar a replicação

Agora você irá gerenciar e operar a replicação de cluster para cluster. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.

1.  Use **Get-ClusterGroup** ou **Gerenciador de Cluster de Failover** para determinar a origem e o destino atuais da replicação e seus status.  Ferramentas de Administração de Servidor Remoto do Windows Server

2.  Para medir o desempenho da replicação, use o cmdlet **Get-Counter** nos nós de origem e de destino. Os nomes de contador são:

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de vezes que a liberação pausou

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de E/S com liberação pendente

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de solicitações da última gravação de log

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho da Fila de Liberação Méd

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho da Fila de Liberação Atual

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de Solicitações de Gravação de Aplicativo

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de solicitações por gravação de log Méd

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Média de Gravação de Aplicativo

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Média de Leitura de Aplicativo

    -   \Estatísticas de Réplica de Armazenamento(*)\RPO de Destino

    -   \Estatísticas de Réplica de Armazenamento(*)\RPO Atual

    -   \Estatísticas de Réplica de Armazenamento(*)\Comprimento da Fila de Log Méd

    -   \Estatísticas de Réplica de Armazenamento(*)\Comprimento da Fila de Log Atual

    -   \Estatísticas de Réplica de Armazenamento(*)\Total de Bytes Recebidos

    -   \Estatísticas de Réplica de Armazenamento(*)\Total de Bytes Enviados

    -   \Estatísticas de réplica de armazenamento (*)\Latência enviada de rede méd

    -   \Estatísticas de Réplica de Armazenamento(*)\Estado da Replicação

    -   \Estatísticas de réplica de armazenamento(*)\Média de Viagem de Ida e Volta da Mensagem

    -   \Estatísticas de Réplica de Armazenamento(*)\Tempo Decorrido da Última Recuperação

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Recuperação Liberadas

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Recuperação

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Replicação Liberadas

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Replicação

    -   \Estatísticas de Réplica de Armazenamento(*)\Número Máximo de Sequência de Log

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Mensagens Recebidas

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Mensagens Enviadas

    Para saber mais sobre contadores de desempenho no Windows PowerShell, confira [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter).

3.  Para mover a direção da replicação de um site, use o cmdlet **Set-SRPartnership**.

    ```PowerShell
    Set-SRPartnership -NewSourceComputerName SR-SRVCLUSB -SourceRGName rg02 -DestinationComputerName SR-SRVCLUSA -DestinationRGName rg01
    ```

    > [!NOTE]
    > O Windows Server impede a troca de função quando a sincronização inicial está em andamento, pois pode levar à perda de dados se você tentar alternar antes de permitir que a replicação inicial seja concluída. Não force a alternação de direções até que a sincronização inicial seja concluída.

    Verifique os logs de evento para ver a mudança da direção de replicação e a ocorrência do modo de recuperação e reconcilie. A gravação de E/Ss pode gravar no armazenamento pertencente ao novo servidor de origem. Alterar a direção da replicação bloqueará a gravação de E/Ss no computador de origem anterior.

    > [!NOTE]
    > O disco de cluster de destino sempre aparece como **Online (Sem acesso)** quando replicado.

4.  Para alterar o tamanho do log do padrão de 8 GB, use **set-SRGroup** nos grupos de réplica de armazenamento de origem e de destino.

    > [!IMPORTANT]
    > O tamanho do log padrão é 8 GB. Dependendo dos resultados do cmdlet **Test-SRTopology**, você pode optar por usar -LogSizeInBytes com um valor maior ou menor.

5.  Para remover a replicação, use **Get-SRGroup**, **Get-SRPartnership**, **Remove-SRGroup** e **Remove-SRPartnership** em cada cluster.

    ```powershell
    Get-SRPartnership | Remove-SRPartnership
    Get-SRGroup | Remove-SRGroup
    ```

    > [!NOTE]
    > Armazenamento réplica desmonta os volumes de destino. Isso ocorre por design.

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral da Réplica de Armazenamento](storage-replica-overview.md)
-   [Estender a replicação do cluster usando o armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)
-   [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)
-   [Réplica de armazenamento: problemas conhecidos](storage-replica-known-issues.md)
-   [Réplica de armazenamento: perguntas frequentes](storage-replica-frequently-asked-questions.md)
-   [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
