---
title: Replicação de armazenamento de servidor para servidor
description: Como configurar e usar a réplica de armazenamento para replicação de servidor para servidor no Windows Server, incluindo Windows Admin Center e o PowerShell.
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 06/04/2018
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: 620d339a505da77649d65537abc92f301760d40d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821287"
---
# <a name="server-to-server-storage-replication-with-storage-replica"></a>Replicação de armazenamento de servidor para servidor com a réplica de armazenamento

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar a Réplica de Armazenamento para configurar dois servidores para sincronizar dados de forma que cada um tenha uma cópia idêntica do mesmo volume. Este tópico fornece informações sobre essa configuração de replicação de servidor para servidor e sobre a configuração e o gerenciamento do ambiente.

Para gerenciar a réplica de armazenamento, você pode usar [Windows Admin Center](../../manage/windows-admin-center/overview.md) ou o PowerShell.

Aqui está um vídeo de visão geral de como usar a réplica de armazenamento no Windows Admin Center.
> [!video https://www.microsoft.com/videoplayer/embed/3aa09fd4-867b-45e9-953e-064008468c4b?autoplay=false]


## <a name="prerequisites"></a>Pré-requisitos  

* Floresta de serviços de domínio Active Directory (não precisa executar o Windows Server 2016).  
* Dois servidores com Windows Server 2016 Datacenter Edition instalado.  
* Dois conjuntos de armazenamento, usando JBODs SAS, SAN fibre channel, Destino iSCSI ou armazenamento SCSI/SATA local. O armazenamento deve conter uma mistura de mídias HDD e SSD. Você disponibilizará cada conjunto de armazenamento apenas para cada um dos servidores, sem acesso compartilhado.  
* Cada conjunto de armazenamento deve permitir a criação de pelo menos dois discos virtuais, um para dados replicados e outro para logs. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de dados. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de logs.  
* Pelo menos uma conexão de Ethernet/TCP em cada servidor para replicação síncrona, mas, preferencialmente, RDMA.   
* Regras de firewall e roteador apropriadas para permitir ICMP, SMB (porta 445, além de 5445 para SMB Direct) e tráfego bidirecional de WS-MAN (porta 5985) entre todos os nós.  
* Uma rede entre servidores com largura de banda suficiente para conter a carga de trabalho de gravação de E/S e uma média de =5 ms de latência de viagem de ida e volta, para replicação síncrona. A replicação assíncrona não tem uma recomendação de latência.<br>
Se você estiver replicando entre servidores locais e VMs do Azure, você deve criar um link de rede entre os servidores locais e as VMs do Azure. Para fazer isso, use [Express Route](#add-azure-vm-expressroute), um [conexão de gateway VPN Site a Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), ou instalar o software VPN em suas VMs do Azure para conectar-se com sua rede local.
* O armazenamento replicado não pode ser localizado na unidade que contém a pasta do sistema operacional Windows.

> [!IMPORTANT]
>  Neste cenário, cada servidor deve estar em um local físico ou lógico diferente. Cada servidor deve ser capaz de se comunicar com o outro através de uma rede.  


Muitos desses requisitos podem ser determinados usando `Test-SRTopology cmdlet`. Você obtém acesso a essa ferramenta se instalar os recursos Ferramentas de Gerenciamento de Réplica de Armazenamento ou Réplica de Armazenamento em pelo menos um servidor. Não é necessário configurar a Réplica de Armazenamento para usar essa ferramenta, apenas para instalar o cmdlet. Mais informações estão incluídas nas etapas abaixo.  

## <a name="windows-admin-center-requirements"></a>Requisitos do Windows Admin Center

Para usar a réplica de armazenamento e o Windows Admin Center juntos, você precisa do seguinte:

| Sistema                        | Sistema operacional                                            | Necessário para     |
|-------------------------------|-------------------------------------------------------------|------------------|
| Dois servidores <br>(qualquer combinação de hardware locais, VMs e VMs, incluindo as VMs do Azure na nuvem)| Datacenter edition do Windows Server (canal semestral) ou Windows Server 2016 | Réplica de Armazenamento  |
| Um único PC                     | Windows 10                                                  | Windows Admin Center |

> [!NOTE]
> Agora é possível usar o Windows Admin Center em um servidor para gerenciar a réplica de armazenamento.

## <a name="terms"></a>Termos  
Este passo a passo usa o seguinte ambiente como exemplo:  

-   Dois servidores, chamados **SR-SRV05** e **SR SRV06**.  

-   Um par de "locais" lógicos que representam dois data centers diferentes, com um chamado **Redmond** e um chamado **Bellevue**.  

![Diagrama que mostra um servidor no Prédio 5 replicando com um servidor no Prédio 9](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**Figura 1: Replicação de servidor para servidor**  

## <a name="step-1-install-and-configure-windows-admin-center-on-your-pc"></a>Etapa 1: Instalar e configurar o Windows Admin Center em seu PC

Se você estiver usando o Windows Admin Center para gerenciar a réplica de armazenamento, use as seguintes etapas para preparar seu computador para gerenciar a réplica de armazenamento.
1. Baixe e instale [Windows Admin Center](../../manage/windows-admin-center/overview.md).
2. Baixe e instale o [ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520).
    - Se você estiver usando o Windows 10, versão 1809 ou posterior, instale o "RSAT: Réplica módulo de armazenamento para o Windows PowerShell"de recursos sob demanda.
3. Abra uma sessão do PowerShell como administrador, selecionando o **inicie** botão, digitando **PowerShell**, com o botão direito **Windows PowerShell,** e, em seguida, selecionando  **Executar como administrador**.
4. Digite o seguinte comando para habilitar o protocolo WS-Management no computador local e definir a configuração padrão para o gerenciamento remoto no cliente.

    ```PowerShell
    winrm quickconfig
    ```

5. Tipo de **Y** para habilitar os serviços do WinRM e habilitar a exceção de Firewall do WinRM.

## <a name="provision-os"></a>Etapa 2: Provisionar o sistema operacional, os recursos, as funções, o armazenamento e a rede

1.  Instale o Windows Server 2016 em ambos os nós de servidor com um tipo de instalação do Windows Server 2016 Datacenter **(Desktop Experience)**. Não escolha Standard Edition se ele estiver disponível, pois ele não contém a réplica de armazenamento.
 
    Para usar uma VM do Azure conectada à sua rede por meio de um ExpressRoute, consulte [adicionar uma VM do Azure conectada à sua rede por meio do ExpressRoute](#add-azure-vm-expressroute).

3.  Adicionar informações de rede, ingressar nos servidores ao mesmo domínio do seu PC (se você estiver usando um) de gerenciamento do Windows 10 e, em seguida, reiniciar os servidores.  

    > [!NOTE]
    > Desse ponto em diante, sempre faça logon como um usuário de domínio que é membro do grupo de administradores internos em todos os servidores. Lembre-se sempre de elevar seus prompts de CMD e do PowerShell ao executar em uma instalação de servidor gráfico ou em um computador com Windows 10.  

3.  Conectar-se o primeiro conjunto de compartimento de armazenamento JBOD, destino iSCSI, SAN FC ou armazenamento em disco fixo local (DAS) para o servidor no site **Redmond**.  

4.  Conecte o segundo conjunto de armazenamento para o servidor no site **Bellevue**.  

5.  Conforme apropriado, instale o firmware e os drivers de fornecedor mais recentes para o armazenamento e o compartimento, os drivers de fornecedor mais recentes para o HBA, o firmware de fornecedor mais recente para UEFI/BIOS, os drivers de fornecedor mais recentes para rede e os drivers de chipsets da placa-mãe mais recentes em ambos os nós. Reinicie os nós conforme necessário.  

    > [!NOTE]
    > confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.  

6.  Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Certifique-se de que o gerenciamento de energia no Windows Server é definido como de alto desempenho. Reinicie conforme necessário.  

7.  Configure as funções da seguinte maneira:  

    -   **Método Windows Admin Center**
        1. No do Windows Admin Center, navegue até o Gerenciador do servidor e, em seguida, selecione um dos servidores.
        2. Navegue até **funções e recursos**.
        3. Selecione **recursos** > **réplica de armazenamento**e, em seguida, clique em **instalar**.
        4. Se repetem em outro servidor.
    -   **Método do Gerenciador do servidor**  

        1.  Execute **ServerManager.exe** e crie um Grupo de Servidores, adicionando todos os nós de servidor.  

        2.  Instale as funções e os recursos de **Servidor de Arquivos** e **Réplica de Armazenamento** em cada um dos nós e reinicie-os.  

    -   **Método do Windows PowerShell**  

        No SR-SRV06 ou em um computador de gerenciamento remoto, execute o seguinte comando em um console do Windows PowerShell para instalar os recursos e funções necessários e reinicie-os:  

        ```powershell  
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

        2.  Provisione o armazenamento usando Espaços de Armazenamento seguindo as **etapas 1 a 3** fornecidas em [Implantar espaços de armazenamento em um servidor autônomo](../storage-spaces/deploy-standalone-storage-spaces.md) usando o Windows PowerShell ou Gerenciador do Servidor.  

    - **Para o armazenamento iSCSI:**  

        1.  Verifique se cada cluster pode ver apenas compartimentos de armazenamento do site. Você deverá usar mais de um único adaptador de rede se usar iSCSI.    

        2.  Provisione o armazenamento usando a documentação do fornecedor. Se estiver usando o destino iSCSI baseado em Windows, confira [Armazenamento em bloco de destino iSCSI, Como](https://technet.microsoft.com/library/hh848268.aspx).  

    - **Para o armazenamento SAN FC:**  

        1.  Verifique se cada cluster pode ver apenas os compartimentos de armazenamento desse local e se você definiu corretamente as zonas dos hosts.   

        2.  Provisione o armazenamento usando a documentação do fornecedor.  

    - **Para o armazenamento de disco fixo local:**  

        -   Verifique se o armazenamento não contêm um volume do sistema, o arquivo de paginação ou arquivos de despejo.  

        -   Provisione o armazenamento usando a documentação do fornecedor.  

9. Inicie o Windows PowerShell e use o cmdlet **Test-SRTopology** para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, assim como um modo de avaliação de desempenho de execução longa.  

    Por exemplo, para validar os nós propostos em que cada um tem um volume **F:** e **G:** e executar o teste por 30 minutos:  
        
    ```PowerShell
    MD c:\temp  

    Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  
    ```

    > [!IMPORTANT]
      > Ao usar um servidor de teste com nenhuma carga de gravação de E/S no volume de origem especificado durante o período de avaliação, adicione uma carga de trabalho ou o servidor não gerará um relatório útil. Você deve testar com cargas de trabalho de produção para ver os números reais e os tamanhos de log recomendados. Como alternativa, basta copiar alguns arquivos no volume de origem durante o teste ou baixar e executar [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) para gerar a E/S de gravação. Por exemplo, uma amostra com uma carga de trabalho de E/S de gravação baixa por dez minutos para o volume D:  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 d:\test` 

10. Examine os **Testsrtopologyreport** relatório mostrado na Figura 2 para garantir que você atenda aos requisitos de réplica de armazenamento.  

    ![Tela que mostra o relatório de topologia](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)

    **Figura 2: Relatório de topologia de replicação de armazenamento**

## <a name="step-3-set-up-server-to-server-replication"></a>Etapa 3: Configurar a replicação de servidor para servidor
### <a name="using-windows-admin-center"></a>Usando o Windows Admin Center

1. Adicione o servidor de origem.
    1. Selecione o **adicionar** botão.
    2. Selecione **Adicionar conexão de servidor**.
    3. Digite o nome do servidor e, em seguida, selecione **enviar**.
2. Sobre o **todas as conexões** , selecione o servidor de origem.
3. Selecione **a réplica de armazenamento** do painel Ferramentas.
4. Selecione **New** para criar uma nova parceria.
5. Forneça os detalhes da parceria e, em seguida, selecione **criar**. <br>
![A tela de nova parceria mostrando detalhes de parceria, como um tamanho de log de 8 GB.](media\Storage-Replica-UI\Honolulu_SR_Create_Partnership.png)

    **Figura 3: Criar uma nova parceria**

> [!NOTE]
> Remover da parceria de réplica de armazenamento no Windows Admin Center não remove o nome do grupo de replicação.

### <a name="using-windows-powershell"></a>Usando o Windows PowerShell

Agora você configurará a replicação de servidor para servidor usando o Windows PowerShell. Você deve executar todas as etapas abaixo nos nós diretamente ou de um computador de gerenciamento remoto que contém as ferramentas de gerenciamento RSAT do Windows Server 2016.  

1. Verifique se você está usando um console do Powershell com privilégios elevados como um administrador.  
2. Configure a replicação de servidor para servidor, especificando os discos de origem e destino, os logs de origem e destino, os nós de origem e destino e o tamanho do log.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   Saída:
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
    Saída:

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
        ```
        Veja a seguir um exemplo de saída:
        ```
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

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Como um exemplo de andamento (que não será encerrado):  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  No servidor de destino, execute o seguinte comando e examine os eventos 5009, 1237, 5001, 5015, 5005 e 2200 para compreender o andamento do processamento. Não deve haver nenhum aviso de erro nessa sequência. Haverá muitos eventos 1237; eles indicam o progresso.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="step-4-manage-replication"></a>Etapa 4: Gerenciar a replicação

Agora você gerenciará e operará sua infra-estrutura replicada de servidor para servidor. Você pode executar todas as etapas abaixo diretamente nos nós ou de um computador de gerenciamento remoto que contém as ferramentas de gerenciamento RSAT do Windows Server 2016.  

1.  Use `Get-SRPartnership` e `Get-SRGroup` para determinar a origem e o destino atuais da replicação e seus status.  

2.  Para medir o desempenho da replicação, use o cmdlet `Get-Counter` nos nós de origem e destino. Os nomes de contador são:  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de vezes que a liberação pausou  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de E/S com liberação pendente  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de solicitações da última gravação de log  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho Médio da Fila de Liberação  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho da Fila de Liberação Atual  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Número de Solicitações de Gravação de Aplicativo  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho Médio de solicitações por gravação de log  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho Média de Gravação de Aplicativo  

    -   \Estatísticas de E/S da Partição de Réplica de Armazenamento(*)\Tamanho Média de Leitura de Aplicativo  

    -   \Estatísticas de Réplica de Armazenamento(*)\RPO de Destino  

    -   \Estatísticas de Réplica de Armazenamento(*)\RPO Atual  

    -   \Estatísticas de Réplica de Armazenamento(*)\Tamanho Médio da Fila de Log  

    -   \Estatísticas de Réplica de Armazenamento(*)\Comprimento da Fila de Log Atual  

    -   \Estatísticas de Réplica de Armazenamento(*)\Total de Bytes Recebidos  

    -   \Estatísticas de Réplica de Armazenamento(*)\Total de Bytes Enviados  

    -   \Estatísticas de Réplica de Armazenamento(*)\Tamanho Média de Envio de Rede  

    -   \Estatísticas de Réplica de Armazenamento(*)\Estado da Replicação  

    -   \Estatísticas de Réplica de Armazenamento(*)\Tamanho Média de Viagem de Ida e Volta da Mensagem  

    -   \Estatísticas de Réplica de Armazenamento(*)\Tempo Decorrido da Última Recuperação  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Recuperação Liberadas  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Recuperação  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Replicação Liberadas  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Transações de Replicação  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número Máximo de Sequência de Log  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Mensagens Recebidas  

    -   \Estatísticas de Réplica de Armazenamento(*)\Número de Mensagens Enviadas  

    Para saber mais sobre contadores de desempenho no Windows PowerShell, confira [Get-Counter](https://technet.microsoft.com/library/hh849685.aspx).  

3.  Para mover a direção da replicação de um site, use o cmdlet `Set-SRPartnership`.  

    ```PowerShell  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > O Windows Server 2016 impede a troca de função quando a sincronização inicial está em andamento, o que pode levar à perda de dados se você tentar alternar antes de permitir que a replicação inicial seja concluída. Não force a alternação de direções até que a sincronização inicial seja concluída.  

    Verifique os logs de evento para ver a mudança da direção de replicação e a ocorrência do modo de recuperação e reconcilie. A gravação de E/Ss pode gravar no armazenamento pertencente ao novo servidor de origem. Alterar a direção da replicação bloqueará a gravação de E/Ss no computador de origem anterior.  

4.  Para remover a replicação, use `Get-SRGroup`, `Get-SRPartnership`, `Remove-SRGroup` e `Remove-SRPartnership` em cada nó. Execute o cmdlet `Remove-SRPartnership` na origem atual apenas na origem atual da replicação, não no servidor de destino. Execute o `Remove-Group` nos dois servidores. Por exemplo, para remover toda a replicação de dois servidores:  

    ```powershell
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>Substituir a replicação DFS pela Réplica de Armazenamento  
Muitos clientes da Microsoft implantam a replicação DFS como uma solução de recuperação de desastres para dados de usuário não estruturados como pastas base e compartilhamentos de departamento. A Replicação DFS acompanha o Windows Server 2003 R2 e todos os sistemas operacionais posteriores e opera em redes de baixa largura de banda, tornando-o atraente para ambientes de alta latência e baixa alteração com vários nós. No entanto, a Replicação DFS tem limitações importantes como solução de replicação de dados:  
* Ele não replica arquivos abertos ou em uso.  
* Ele não replica de forma síncrona.  
* Sua latência de replicação assíncrona pode levar muitos minutos, horas ou até mesmo dias.  
* Ele se baseia em um banco de dados que pode exigir verificações de consistência demoradas após uma queda de energia.  
* Em geral, ele é configurado como vários mestres, que permite que alterações fluam em ambas as direções, possivelmente substituindo dados mais recentes.  

A Réplica de Armazenamento não tem nenhuma dessas limitações. No entanto, ela tem várias que podem torná-la menos interessante em alguns ambientes:  

* Só permite replicação individual entre volumes. É possível replicar volumes diferentes entre vários servidores.  
* Enquanto ele dá suporte a replicação assíncrona, ela não foi projetada para baixa largura de banda, redes de alta latência.  
* Ele não permite acesso de usuário aos dados protegidos no destino enquanto a replicação está em andamento  

Se esses não forem fatores de bloqueio, a Réplica de Armazenamento permite substituir servidores de Replicação DFS por essa tecnologia mais recente.   
O processo é, em um alto nível:  

1.  Instalar o Windows Server 2016 em dois servidores e configurar o armazenamento. Isso pode significar atualizar um conjunto existente de servidores ou uma nova instalação.  
2.  Verifique se os dados que você deseja replicar existem em um ou mais volumes de dados e não na unidade C:.   
a.  Você também pode propagar os dados em outro servidor para economizar tempo, usando um backup ou cópias de arquivos, além de usar o armazenamento de provisionamento dinâmico. Não é necessário corresponder perfeitamente a segurança dos metadados, ao contrário da Replicação DFS.  
3.  Compartilhar os dados no servidor de origem e torná-lo acessível por meio de um namespace do DFS. Isso é importante para garantir que os usuários possam ainda acessá-lo se o nome do servidor for alterado em um local de desastre.  
a.  Você pode criar compartilhamentos correspondentes no servidor de destino, que estará indisponível durante as operações normais,   
b.  Não adicione o servidor de destino ao namespace DFS Namespaces ou se você fizer isso, certifique-se de que todos os seus destinos de pasta estão desabilitados.  
4.  Habilite a replicação de Réplica de Armazenamento e a sincronização inicial completa. A replicação pode ser síncrona ou assíncrona.   
a.  No entanto, síncrona é recomendada para garantir a consistência de dados de E/S no servidor de destino.   
b.  É altamente recomendável habilitar Cópias de Sombra de Volume e periodicamente fazer instantâneos com VSSADMIN ou outras ferramentas de escolha. Isso garantirá que os aplicativos liberem os seus arquivos de dados em disco de forma consistente. Em caso de desastre, você poderá recuperar arquivos de instantâneos no servidor de destino que pode ter sido parcialmente replicado de forma assíncrona. Os instantâneos são replicados junto com os arquivos.  
5.  Opere normalmente até que haja um desastre.  
6.  Alterne o servidor de destino para ser a nova origem, que expõe seus volumes replicados para os usuários.  
7.  Se usar a replicação síncrona, nenhuma restauração de dados será necessária a menos que o usuário esteja usando um aplicativo que estava gravando dados sem proteção de transações (isso independe da replicação) durante a perda do servidor de origem. Se usar a replicação assíncrona, a necessidade de uma montagem de instantâneo de VSS será maior, mas use o VSS em todas as circunstâncias para instantâneos de aplicativo consistentes.  
8.  Adicione o servidor e seus compartilhamentos como um destino de pasta de Namespaces do DFS.   
9.  Os usuários podem acessar seus dados.  

 > [!NOTE]
 > O planejamento de Recuperação de Desastres é um assunto complexo e requer grande atenção aos detalhes. A criação de runbooks e o desempenho de rotinas anuais de failover dinâmico é altamente recomendável. Quando ocorre um desastre real, o caos reina e os funcionários experientes podem ficar permanentemente indisponíveis.  

## <a name="add-azure-vm-expressroute"></a>Adição de uma VM do Azure conectada à sua rede por meio do ExpressRoute

1. [Criar um ExpressRoute no portal do Azure](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager).<br>Depois que o ExpressRoute for aprovado, um grupo de recursos é adicionado à assinatura – navegue até **grupos de recursos** para exibir esse novo grupo. Anote o nome da rede virtual.
![Portal do Azure mostrando o grupo de recursos adicionado com o ExpressRoute](media/Server-to-Server-Storage-Replication/express-route-resource-group.png)
    
    **Figura 4: Os recursos associados com um ExpressRoute - tome nota do nome da rede virtual**
1. [Criar um novo grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
1. [Adicione um grupo de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal). Ao criá-lo, selecione a ID da assinatura associada com o ExpressRoute, você criou e selecione o grupo de recursos que você acabou de criar também.
<br><br>Adicione quaisquer regras de segurança de entrada e saída que você precisa para o grupo de segurança de rede. Por exemplo, você talvez queira permitir o acesso de área de trabalho remota à VM.
1. [Criar uma VM do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) com as seguintes configurações (mostradas na Figura 5):
    - **Endereço IP público**: Nenhuma
    - **Rede virtual**: Selecione a rede virtual que você observou do grupo de recursos adicionado com o ExpressRoute.
    - **Grupo de segurança de rede (firewall)**: Selecione o grupo de segurança de rede que você criou anteriormente.
    ![Criar a máquina virtual que mostra as configurações de rede de ExpressRoute](media/Server-to-Server-Storage-Replication/azure-vm-express-route.png)
    **Figura 5: Criar uma VM enquanto seleciona as configurações de rede de ExpressRoute**
1. Depois que a VM é criada, consulte [etapa 2: Provisionar o sistema operacional, recursos, funções, armazenamento e rede](#provision-os).


## <a name="related-topics"></a>Tópicos relacionados  
- [Visão geral da réplica de armazenamento](storage-replica-overview.md)  
- [Replicação de Cluster estendido usando armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)  
- [Replicação de armazenamento de Cluster para cluster](cluster-to-cluster-storage-replication.md)
- [Réplica de armazenamento: Problemas conhecidos](storage-replica-known-issues.md)  
- [Réplica de armazenamento: Perguntas frequentes](storage-replica-frequently-asked-questions.md)
- [Espaços de armazenamento diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
