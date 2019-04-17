---
title: "Replicação de armazenamento de servidor para servidor"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/11/2016
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: 9dd2820f641e23dceb5c2ac7110efda0d3345dcf
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="server-to-server-storage-replication"></a>Replicação de armazenamento de servidor para servidor

> Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar a Réplica de Armazenamento para configurar dois servidores para sincronizar dados de forma que cada um tenha uma cópia idêntica do mesmo volume. Este tópico fornece informações sobre essa configuração de replicação de servidor para servidor e sobre a configuração e o gerenciamento do ambiente.

Para gerenciar a Réplica de Armazenamento, use o PowerShell ou as [Ferramentas de gerenciamento do Azure Server](https://blogs.technet.microsoft.com/servermanagement/).

> [!IMPORTANT]
>  Neste cenário, cada servidor deve estar em um local físico ou lógico diferente. Cada servidor deve ser capaz de se comunicar com o outro através de uma rede.  

## <a name="terms"></a>Termos  
Este passo a passo usa o seguinte ambiente como exemplo:  

-   Dois servidores, chamados **SR-SRV05** e **SR SRV06**.  

-   Um par de "locais" lógicos que representam dois data centers diferentes, com um chamado **Redmond** e um chamado **Bellevue**.  

![Diagrama que mostra um servidor no Prédio 5 replicando com um servidor no Prédio 9](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**Figura 1: Replicação de servidor para servidor**  

## <a name="prerequisites"></a>Pré-requisitos  

* Floresta dos Active Directory Domain Services (não precisa ser executada no Windows Server 2016).  
* Dois servidores com Windows Server 2016 Datacenter Edition instalado.  
* Dois conjuntos de armazenamento, usando JBODs SAS, SAN fibre channel, Destino iSCSI ou armazenamento SCSI/SATA local. O armazenamento deve conter uma mistura de mídias HDD e SSD. Você disponibilizará cada conjunto de armazenamento apenas para cada um dos servidores, sem acesso compartilhado.  
* Cada conjunto de armazenamento deve permitir a criação de pelo menos dois discos virtuais, um para dados replicados e outro para logs. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de dados. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de logs.  
* Pelo menos uma conexão de Ethernet/TCP em cada servidor para replicação síncrona, mas, preferencialmente, RDMA.   
* Regras de firewall e roteador apropriadas para permitir ICMP, SMB (porta 445, além de 5445 para SMB Direct) e tráfego bidirecional de WS-MAN (porta 5985) entre todos os nós.  
* Uma rede entre servidores com largura de banda suficiente para conter a carga de trabalho de gravação de E/S e uma média de =5 ms de latência de viagem de ida e volta, para replicação síncrona. A replicação assíncrona não tem uma recomendação de latência.  
* O armazenamento replicado não pode ser localizado na unidade que contém a pasta do sistema operacional Windows.

Muitos desses requisitos podem ser determinados usando `Test-SRTopology cmdlet`. Você obtém acesso a essa ferramenta se instalar os recursos Ferramentas de Gerenciamento de Réplica de Armazenamento ou Réplica de Armazenamento em pelo menos um servidor. Não é necessário configurar a Réplica de Armazenamento para usar essa ferramenta, apenas para instalar o cmdlet. Mais informações estão incluídas nas etapas abaixo.  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Provisionar o sistema operacional, os recursos, as funções, o armazenamento e a rede  
1.  Instale o Windows Server 2016 em ambos os nós de servidor com um tipo de instalação do Windows Server 2016 Datacenter **(Desktop Experience)**. Não escolha Standard Edition se estiver disponível, pois ela não contém a Réplica de Armazenamento.  

2.  Adicione as informações de rede e vincule-as ao domínio, depois reinicie-as.  

    > [!NOTE]
    > Desse ponto em diante, sempre faça logon como um usuário de domínio que é membro do grupo de administradores internos em todos os servidores. Lembre-se sempre de elevar seus prompts de CMD e do PowerShell ao executar em uma instalação de servidor gráfico ou em um computador com Windows 10.  

3.  Conecte o primeiro conjunto de compartimento de armazenamento JBOD, destino iSCSI, SAN FC ou armazenamento em disco fixo local (DAS) ao servidor no local **Redmond**.  

4.  Conecte o segundo conjunto de armazenamento ao servidor no local **Bellevue**.  

5.  Conforme apropriado, instale o firmware e os drivers de fornecedor mais recentes para o armazenamento e o compartimento, os drivers de fornecedor mais recentes para o HBA, o firmware de fornecedor mais recente para UEFI/BIOS, os drivers de fornecedor mais recentes para rede e os drivers de chipsets da placa-mãe mais recentes em ambos os nós. Reinicie os nós conforme necessário.  

    > [!NOTE]
    > confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.  

6.  Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Verifique se o gerenciamento de energia no Windows Server está definido como alto desempenho. Reinicie conforme necessário.  

7.  Configure as funções da seguinte maneira:  

    -   **Método gráfico**  

        1.  Execute **ServerManager.exe** e crie um Grupo de Servidores, adicionando todos os nós de servidor.  

        2.  Instale as funções e os recursos de **Servidor de Arquivos** e **Réplica de Armazenamento** em cada um dos nós e reinicie-os.  

    -   **Método do Windows PowerShell**  

        No SR-SRV06 ou em um computador de gerenciamento remoto, execute o seguinte comando em um console do Windows PowerShell para instalar os recursos e funções necessários e reinicie-os:  

        ```  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Para saber mais sobre essas etapas, confira [Instalar ou desinstalar funções, serviços de função ou recursos](http://technet.microsoft.co/library/hh831809.aspx)  

8.  Configure o armazenamento da seguinte maneira:  

    > [!IMPORTANT]  
    > -   Você deve criar dois volumes em cada compartimento: um para dados e outro para logs.  
    > -   Os discos de dados e log devem ser inicializados como GPT, não MBR.  
    > -   Os dois volumes de dados devem ser de tamanho idêntico.  
    > -   Os dois volumes de log devem ser de tamanho idêntico.  
    > -   Todos os discos de dados replicados devem ter os mesmos tamanhos de setor.  
    > -   Todos os discos de log devem ter os mesmos tamanhos de setor.  
    > -   Os volumes de log devem usar armazenamento baseado em flash, como SSD. A Microsoft recomenda que o armazenamento de log seja mais rápido do que o armazenamento de dados. Volumes de log nunca devem ser usados para outras cargas de trabalho.
    > -   Os discos de dados podem usar HDD, SSD ou uma combinação em camadas e podem usar os espaços espelhados ou de paridade, ou RAID 1 ou 10, RAID 5 ou RAID 50.  
    > -   O volume do log deve ter pelo menos 9 GB e talvez precise ser maior dependendo dos requisitos de log.  
    > -   A função de Servidor de Arquivo só é necessária para Test-SRTopology funcionar, pois abre as portas de firewall necessárias para teste.
    
    - **Para compartimentos JBOD:**  

        1.  Verifique se cada servidor pode ver apenas os compartimentos de armazenamento do local e se as conexões SAS estão configuradas corretamente.  

        2.  Provisione o armazenamento usando Espaços de Armazenamento seguindo as **etapas 1 a 3** fornecidas em [Implantar espaços de armazenamento em um servidor autônomo](http://technet.microsoft.com/library/jj822938.aspx) usando o Windows PowerShell ou Gerenciador do Servidor.  

    - **Para o armazenamento de iSCSI:**  

        1.  Verifique se cada cluster pode ver apenas compartimentos de armazenamento do site. Você deverá usar mais de um único adaptador de rede se usar iSCSI.    

        2.  Provisione o armazenamento usando a documentação do fornecedor. Se estiver usando o destino iSCSI baseado em Windows, confira [Armazenamento em bloco de destino iSCSI, Como](http://technet.microsoft.com/library/hh848268.aspx).  

    - **Para o armazenamento SAN FC:**  

        1.  Verifique se cada cluster pode ver apenas os compartimentos de armazenamento desse local e se você definiu corretamente as zonas dos hosts.   

        2.  Provisione o armazenamento usando a documentação do fornecedor.  

    - **Para o armazenamento de disco fixo local (DAS):**  

        -   Verifique se o armazenamento não contém um volume do sistema, arquivo de paginação ou arquivos de despejo.  

        -   Provisione o armazenamento usando a documentação do fornecedor.  

9. Inicie o Windows PowerShell e use o cmdlet **Test-SRTopology** para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, assim como um modo de avaliação de desempenho de execução longa.  

    Por exemplo, para validar os nós propostos em que cada um tem um volume **F:** e **G:** e executar o teste por 30 minutos:  

        MD c:\temp  

        Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  

    > [!IMPORTANT]
      > Ao usar um servidor de teste com nenhuma carga de gravação de E/S no volume de origem especificado durante o período de avaliação, adicione uma carga de trabalho ou o servidor não gerará um relatório útil. Você deve testar com cargas de trabalho de produção para ver os números reais e os tamanhos de log recomendados. Como alternativa, basta copiar alguns arquivos no volume de origem durante o teste ou baixar e executar [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) para gerar a E/S de gravação. Por exemplo, uma amostra com uma carga de trabalho de E/S de gravação baixa por dez minutos para o volume D:  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 d:\test` 

10. Examine o relatório **TestSrTopologyReport.html** para garantir que você atende aos requisitos da Réplica de Armazenamento.  

    ![Tela que mostra o relatório de topologia](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)  

## <a name="configure-server-to-server-replication-using-windows-powershell"></a>Configurar a replicação de servidor a servidor usando o Windows PowerShell  
Agora você configurará a replicação de servidor para servidor usando o Windows PowerShell. Você deve executar todas as etapas abaixo nos nós diretamente ou de um computador de gerenciamento remoto que contém as ferramentas de gerenciamento RSAT do Windows Server 2016.  

O Gerenciamento Gráfico da Réplica de Armazenamento é suportado ao usar a Ferramenta Server Manager (SMT). Examine a documentação do Azure para saber mais sobre a disponibilidade da versão de Visualização desse conjunto de ferramentas de gerenciamento. Este documento será atualizado depois que a SMT atingir disponibilidade geral.

1. Verifique se você está usando um console do Powershell com privilégios elevados como um administrador.  
2. Configure a replicação de servidor para servidor, especificando os discos de origem e destino, os logs de origem e destino, os nós de origem e destino e o tamanho do log.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   Output:
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > O tamanho do log padrão é 8 GB. Dependendo dos resultados do cmdlet `Test-SRTopology`, você pode optar por usar -LogSizeInBytes com um valor maior ou menor.  

2.  Para obter o estado da origem e do destino de replicação, use `Get-SRGroup` e `Get-SRPartnership` da seguinte maneira:  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    Output:

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  Determine o andamento da replicação da seguinte maneira:  

    1.  No servidor de origem, execute o comando a seguir e examine os eventos 5015, 5002, 5004, 1237, 5001 e 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  No servidor de destino, execute o seguinte comando para ver os eventos de Réplica de Armazenamento que mostram a criação da parceria. Esse evento indica o número de bytes copiados e o tempo gasto. Exemplo:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

                ReplicationGroupName: rg02  
                ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
                ReplicaName: f:\  
                ReplicaId: {00000000-0000-0000-0000-000000000000}  
                End LSN in bitmap:   
                LogGeneration: {00000000-0000-0000-0000-000000000000}  
                LogFileId: 0  
                CLSFLsn: 0xFFFFFFFF  
                Number of Bytes Recovered: 68583161856  
                Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > A Réplica de Armazenamento desmonta os volumes de destino e suas letras de unidade ou pontos de montagem. Isso ocorre por design.  

    3.  Como alternativa, o grupo de servidores de destino para a réplica informa o número de bytes restantes para copiar todo o tempo e pode ser consultado por meio do PowerShell. Por exemplo:  

        ```  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Como um exemplo de andamento (que não será encerrado):  

        ```  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  No servidor de destino, execute o seguinte comando e examine os eventos 5009, 1237, 5001, 5015, 5005 e 2200 para compreender o andamento do processamento. Não deve haver nenhum aviso de erro nessa sequência. Haverá muitos eventos 1237; eles indicam o progresso.  

        ```  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="manage-replication"></a>Gerenciar a replicação  
Agora você gerenciará e operará sua infra-estrutura replicada de servidor para servidor. Você pode executar todas as etapas abaixo diretamente nos nós ou de um computador de gerenciamento remoto que contém as ferramentas de gerenciamento RSAT do Windows Server 2016.  

1.  Use `Get-SRPartnership` e `Get-SRGroup` para determinar a origem e o destino atuais da replicação e seus status.  

2.  Para medir o desempenho da replicação, use o cmdlet `Get-Counter` nos nós de origem e destino. Os nomes de contador são:  

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

    Para saber mais sobre contadores de desempenho no Windows PowerShell, confira [Get-Counter](http://technet.microsoft.com/library/hh849685.aspx).  

3.  Para mover a direção da replicação de um site, use o cmdlet `Set-SRPartnership`.  

    ```  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > O Windows Server 2016 impede a troca de função quando a sincronização inicial está em andamento, o que pode levar à perda de dados se você tentar alternar antes de permitir que a replicação inicial seja concluída. Não force a alternação de direções até que a sincronização inicial seja concluída.  

    Verifique os logs de evento para ver a mudança da direção de replicação e a ocorrência do modo de recuperação e reconcilie. A gravação de E/Ss pode gravar no armazenamento pertencente ao novo servidor de origem. Alterar a direção da replicação bloqueará a gravação de E/Ss no computador de origem anterior.  

4.  Para remover a replicação, use `Get-SRGroup`, `Get-SRPartnership`, `Remove-SRGroup` e `Remove-SRPartnership` em cada nó. Execute o cmdlet `Remove-SRPartnership` na origem atual apenas na origem atual da replicação, não no servidor de destino. Execute o `Remove-Group` nos dois servidores. Por exemplo, para remover toda a replicação de dois servidores:  

    ```  
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>Substituir a replicação DFS pela Réplica de Armazenamento  
Muitos clientes da Microsoft implantam a replicação DFS como uma solução de recuperação de desastres para dados de usuário não estruturados como pastas base e compartilhamentos de departamento. A Replicação DFS acompanha o Windows Server 2003 R2 e todos os sistemas operacionais posteriores e opera em redes de baixa largura de banda, tornando-o atraente para ambientes de alta latência e baixa alteração com vários nós. No entanto, a Replicação DFS tem limitações importantes como solução de replicação de dados:  
* Ele não replica arquivos abertos ou em uso.  
* Ele não replica de modo síncrono.  
* Sua latência de replicação assíncrona pode levar muitos minutos, horas ou até mesmo dias.  
* Ele se baseia em um banco de dados que pode exigir verificações de consistência demoradas após uma queda de energia.  
* Geralmente, ele é configurado como vários mestres, o que permite que alterações fluam em ambas as direções, possivelmente substituindo dados mais recentes.  

A Réplica de Armazenamento não tem nenhuma dessas limitações. No entanto, ela tem várias que podem torná-la menos interessante em alguns ambientes:  

* Só permite replicação individual entre volumes. É possível replicar volumes diferentes entre vários servidores.  
* Embora ela ofereça suporte à replicação assíncrona, não foi projetada para redes de baixa largura de banda e alta latência.  
* Ela não permite acesso de usuário aos dados protegidos no destino enquanto a replicação está em andamento  

Se esses não forem fatores de bloqueio, a Réplica de Armazenamento permite substituir servidores de Replicação DFS por essa tecnologia mais recente.   
O processo é, em um alto nível:  

1.  Instalar o Windows Server 2016 em dois servidores e configurar o armazenamento. Isso pode significar atualizar um conjunto existente de servidores ou uma nova instalação.  
2.  Verifique se os dados que você deseja replicar existem em um ou mais volumes de dados e não na unidade C:.   
a.  Você também pode propagar os dados em outro servidor para economizar tempo, usando um backup ou cópias de arquivos, além de usar o armazenamento de provisionamento dinâmico. Não é necessário corresponder perfeitamente a segurança dos metadados, ao contrário da Replicação DFS.  
3.  Compartilhe os dados no servidor de origem e disponibilize-o por meio de um Namespace do DFS. Isso é importante para garantir que os usuários possam ainda acessá-lo se o nome do servidor for alterado em um local de desastre.  
a.  Você pode criar compartilhamentos correspondentes no servidor de destino, que estará indisponível durante as operações normais,   
b.  Não adicione o servidor de destino ao namespace DFS Namespaces ou, se fizer isso, verifique se todos os seus destinos de pasta estão desabilitados.  
4.  Habilite a replicação de Réplica de armazenamento e sincronização inicial completa. A replicação pode ser síncrona ou assíncrona.   
a.  No entanto, síncrona é recomendada para garantir a consistência de dados de E/S no servidor de destino.   
b.  É altamente recomendável habilitar Cópias de Sombra de Volume e periodicamente fazer instantâneos com VSSADMIN ou outras ferramentas de escolha. Isso garantirá que os aplicativos liberem os seus arquivos de dados em disco de forma consistente. Em caso de desastre, você poderá recuperar arquivos de instantâneos no servidor de destino que pode ter sido parcialmente replicado de forma assíncrona. Os instantâneos são replicados junto com os arquivos.  
5.  Opere normalmente até que haja um desastre.  
6.  Alterne o servidor de destino para ser a nova origem, que expõe seus volumes replicados para os usuários.  
7.  Se usar a replicação síncrona, nenhuma restauração de dados será necessária a menos que o usuário esteja usando um aplicativo que estava gravando dados sem proteção de transações (isso independe da replicação) durante a perda do servidor de origem. Se usar a replicação assíncrona, a necessidade de uma montagem de instantâneo de VSS será maior, mas use o VSS em todas as circunstâncias para instantâneos de aplicativo consistentes.  
8.  Adicione o servidor e seus compartilhamentos como um destino de pasta do Namespace do DFS.   
9.  Os usuários podem acessar seus dados.  

 > [!NOTE]
 > O planejamento de Recuperação de Desastres é um assunto complexo e requer grande atenção aos detalhes. A criação de runbooks e o desempenho de rotinas anuais de failover dinâmico é altamente recomendável. Quando ocorre um desastre real, o caos reina e os funcionários experientes podem ficar permanentemente indisponíveis.  

## <a name="related-topics"></a>Tópicos relacionados  
- [Visão geral da Réplica de Armazenamento](storage-replica-overview.md)  
- [Replicação de cluster estendido usando Armazenamento Compartilhado](stretch-cluster-replication-using-shared-storage.md)  
- [Replicação de armazenamento de cluster para cluster](cluster-to-cluster-storage-replication.md)
- [Réplica de Armazenamento: problemas conhecidos](storage-replica-known-issues.md)  
- [Réplica de Armazenamento: perguntas frequentes](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>Consulte Também  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
