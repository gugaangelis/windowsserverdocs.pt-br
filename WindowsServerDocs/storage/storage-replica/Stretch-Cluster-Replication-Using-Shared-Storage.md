---
title: Replicação de cluster estendido usando Armazenamento Compartilhado
ms.prod: windows-server
manager: eldenc
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: e6dbe6ef618f989ed158382ef6c8bd063548d281
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950069"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>Replicação de cluster estendido usando Armazenamento Compartilhado

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Neste exemplo de avaliação, você irá configurar esses computadores e seu armazenamento em um único cluster estendido, em que dois nós compartilham um conjunto de armazenamento e dois nós compartilham outro. Em seguida, a replicação mantém os dois conjuntos de armazenamento espelhados no cluster para permitir o failover imediato. Esses nós e seu armazenamento devem estar localizados em locais físicos separados, embora isso não seja obrigatório. Há diferentes etapas para criar clusters do Hyper-V e servidor de arquivos como cenários de exemplo.  

> [!IMPORTANT]  
> Nesta avaliação, os servidores em locais diferentes devem ser capazes de se comunicar com outros servidores através de uma rede, mas não ter nenhuma conectividade física com o armazenamento compartilhado do outro local. Esse cenário não usa Espaços de Armazenamento Diretos.  

## <a name="terms"></a>Termos  
Este passo a passo usa o seguinte ambiente como exemplo:  

-   Quatro servidores, chamados **SR-SRV01**, **SR-SRV02**, **SR-SRV03** e **SR-SRV04** formados em um único cluster chamado **SR-SRVCLUS**.  

-   Um par de "locais" lógicos que representam dois data centers diferentes, com um chamado **Redmond** e o outro chamado **Bellevue.**  

> [!NOTE]  
> Você pode usar apenas dois nós, com um nó em cada local. No entanto, você não poderá realizar failover entre locais com apenas dois servidores. Você pode usar até 64 nós.

![Diagrama mostrando dois nós em Redmond sendo replicados com dois nós do mesmo cluster no site Bellevue](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)  

**Figura 1: replicação de armazenamento em um cluster de ampliação**  

## <a name="prerequisites"></a>Pré-requisitos  
-   Floresta dos Active Directory Domain Services (não precisa ser executada no Windows Server 2016).  
-   2-64 servidores que executam o Windows Server 2019 ou o Windows Server 2016, Datacenter Edition. Se estiver executando o Windows Server 2019, você poderá usar a Standard Edition se estiver OK replicando apenas um único volume de até 2 TB de tamanho. 
-   Dois conjuntos de armazenamento compartilhado, usando SAS JBODs (como os Espaços de Armazenamento), SAN Fibre Channel, VHDX compartilhado ou destino iSCSI. O armazenamento deve conter uma mistura de mídia de HDD e SSD e deve oferecer suporte a Reserva Persistente. Você disponibilizará cada conjunto de armazenamento apenas para dois servidores (assimétrico).  
-   Cada conjunto de armazenamento deve permitir a criação de pelo menos dois discos virtuais, um para dados replicados e outro para logs. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de dados. O armazenamento físico deve ter os mesmos tamanhos de setor em todos os discos de logs.  
-   Pelo menos uma conexão de 1 GbE em cada servidor para replicação síncrona, mas, preferencialmente, RDMA.   
-   Pelo menos 2GB de RAM e dois núcleos por servidor. Será necessário mais memória e núcleos para mais máquinas virtuais.  
-   Regras de firewall e roteador apropriadas para permitir ICMP, SMB (porta 445, além de 5445 para SMB Direct) e tráfego bidirecional de WS-MAN (porta 5985) entre todos os nós.  
-   Uma rede entre servidores com largura de banda suficiente para conter a carga de trabalho de gravação de E/S e uma média de =5 ms de latência de ida e vinda, para replicação síncrona. A replicação assíncrona não tem uma recomendação de latência.  
-   O armazenamento replicado não pode ser localizado na unidade que contém a pasta do sistema operacional Windows.

Muitos desses requisitos podem ser determinados usando o cmdlet `Test-SRTopology`. Você obtém acesso a essa ferramenta se instalar os recursos Ferramentas de Gerenciamento de Réplica de Armazenamento ou Réplica de Armazenamento em pelo menos um servidor. Não é necessário configurar a Réplica de Armazenamento para usar essa ferramenta, apenas para instalar o cmdlet. Mais informações estão incluídas nas seguintes etapas.  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Provisionar o sistema operacional, os recursos, as funções, o armazenamento e a rede  

1.  Instale o Windows Server em todos os nós de servidor, usando as opções de instalação Server Core ou Server with Desktop Experience.  
    > [!IMPORTANT]
    > Desse ponto em diante, sempre faça logon como um usuário de domínio que é membro do grupo de administradores internos em todos os servidores. Lembre-se sempre de elevar seus prompts de CMD e do PowerShell ao executar em uma instalação de servidor gráfico ou em um computador com Windows 10.

2.  Adicione as informações de rede e adicione os nós ao domínio e reinicie-os.  
    > [!NOTE]
    > A partir deste ponto, o guia supõe que você tenha dois pares de servidores para uso em um cluster estendido. Uma rede LAN ou WAN separa os servidores dos que pertencem a locais físicos ou lógicos. O guia considera **SR-SRV01** e **SR-SRV02** no local Redmond e **SR-SRV03** e **SRV04 SR** no local **Bellevue**.  

3.  Conecte o primeiro conjunto de compartimento de armazenamento JBOD compartilhado, VHDX compartilhado, destino iSCSI ou SAN FC aos servidores **Redmond** no local.  

4.  Conecte o segundo conjunto de armazenamento ao servidor no local **Bellevue**.  

5.  Conforme for apropriado, instale o firmware e os drivers mais recentes de fornecedor de armazenamento e compartimento, os drivers mais recentes de fornecedor de HBA, o firmware mais recente do fornecedor de UEFI/BIOS, os drivers mais recentes de fornecedor de rede e os drivers mais recentes de chipsets da placa-mãe em todos os quatro nós. Reinicie os nós conforme necessário.  

    > [!NOTE]
    > confira a documentação do fornecedor de hardware para configurar o armazenamento compartilhado e o hardware de rede.  

6.  Verifique se as configurações de BIOS/UEFI para servidores permitem o alto desempenho, como desabilitar C-State, definir a velocidade de QPI, habilitar NUMA e definir a frequência de memória mais alta. Verifique se o gerenciamento de energia no Windows Server está definido como alto desempenho. Reinicie conforme necessário.  

7.  Configure as funções da seguinte maneira:  

    -   **Método gráfico**  

        Execute **ServerManager.exe** e adicione todos os nós de servidor clicando em **Gerenciar** e **Adicionar servidores**.  

        > [!IMPORTANT]
        > Instale as funções e os recursos de **Clustering de Failover** e **Réplica de Armazenamento** em cada um dos nós e reinicie-os. Se estiver planejando usar outras funções como o Hyper-V, servidor de arquivos etc., instale-os agora também.  

    -   **Usando o método do Windows PowerShell**  

        No **SR-SRV04** ou em um computador de gerenciamento remoto, execute o seguinte comando em um console do Windows PowerShell para instalar os recursos e as funções necessárias para um cluster estendido nos quatro nós e reinicie-os:  

        ```PowerShell  
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  

        ```  

        Para saber mais sobre essas etapas, confira [Instalar ou desinstalar funções, Serviços de função ou Recursos](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md).  


8. Configure o armazenamento da seguinte maneira:  

    > [!IMPORTANT]  
    > -   Você deve criar dois volumes em cada compartimento: um para dados e outro para logs.  
    > -   Os discos de dados e log devem ser inicializados como GPT, não MBR.  
    > -   Os dois volumes de dados devem ser de tamanho idêntico.  
    > -   Os dois volumes de log devem ser de tamanho idêntico.  
    > -   Todos os discos de dados replicados devem ter os mesmos tamanhos de setor.  
    > -   Todos os discos de log devem ter os mesmos tamanhos de setor.  
    > -   Os volumes de log devem usar armazenamento baseado em flash e configurações de resiliência de alto desempenho. A Microsoft recomenda que o armazenamento de log seja mais rápido do que o armazenamento de dados. Volumes de log nunca devem ser usados para outras cargas de trabalho. 
    > -   Os discos de dados podem usar HDD, SSD ou uma combinação em camadas e podem usar os espaços espelhados ou de paridade, ou RAID 1 ou 10, RAID 5 ou RAID 50.  
    > -  O volume do log deve ter pelo menos 9 GB por padrão e pode ser maior ou menor dependendo dos requisitos de log.  
    > - Os volumes podem ser formatados com NTFS ou ReFS.
    > - A função de Servidor de Arquivo só é necessária para Test-SRTopology funcionar, pois abre as portas de firewall necessárias para teste.  

    -   **Para compartimentos JBOD:**  

        1.  Verifique se cada conjunto de nós de servidor emparelhados pode ver apenas os compartimentos de armazenamento do local (ou seja, armazenamento assimétrico) e se as conexões SAS estão configuradas corretamente.  

        2.  Provisione o armazenamento usando Espaços de Armazenamento seguindo as **etapas 1 a 3** fornecidas em [Implantar espaços de armazenamento em um servidor autônomo](../storage-spaces/deploy-standalone-storage-spaces.md) usando o Windows PowerShell ou Gerenciador do Servidor.  

    -   **Para o armazenamento iSCSI:**  

        1.  Verifique se cada conjunto de nós de servidor emparelhados pode ver apenas os compartimentos de armazenamento do local (ou seja, armazenamento assimétrico). Você deverá usar mais de um único adaptador de rede se usar iSCSI.  

        2.  Provisione o armazenamento usando a documentação do fornecedor. Se estiver usando o destino iSCSI baseado em Windows, confira [Armazenamento em bloco de destino iSCSI, Como](../iscsi/iscsi-target-server.md).  

    -   **Para armazenamento SAN FC:**  

        1.  Verifique se cada conjunto de nós de servidor emparelhados pode ver apenas os compartimentos de armazenamento do local (ou seja, armazenamento assimétrico) e se você definiu corretamente as zonas dos hosts.  

        2.  Provisione o armazenamento usando a documentação do fornecedor.  

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>Configurar um Cluster de Failover de Hyper-V ou um Servidor de arquivos para um Cluster de uso geral

Depois de instalar os nós de servidor, a próxima etapa é criar um dos seguintes tipos de clusters:  
*  [Cluster de failover do Hyper-V](#BKMK_HyperV)  
*  [Servidor de arquivos para o cluster de uso geral](#BKMK_FileServer)  

### <a name="BKMK_HyperV"></a>Configurar um cluster de failover do Hyper-V  

>[!NOTE]
> Ignore esta seção e vá para a seção [Configurar um servidor de arquivos para cluster de uso geral](#BKMK_FileServer), se quiser criar um cluster de servidor de arquivos e não um cluster de Hyper-V.  

Agora você criará um cluster de failover normal. Após a configuração, a validação e o teste, você irá estendê-lo usando a Réplica de Armazenamento. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.  

#### <a name="graphical-method"></a>Método gráfico  

1. Execute **cluadmin.msc**.  

2. Valide o cluster proposto e analise os resultados para garantir que você possa continuar.  

   > [!NOTE]  
   > Você deve esperar erros de armazenamento da validação do cluster, devido ao uso de armazenamento assimétrico.  

3. Crie o cluster de cálculo do Hyper-V. Verifique se o nome do cluster tem no máximo 15 caracteres. O exemplo usado abaixo é SR-SRVCLUS. Se os nós estiverem residindo em sub-redes diferentes, você deverá criar um endereço IP para o nome do cluster para cada sub-rede e usar a dependência "OR".  Mais informações podem ser encontradas em [Configurando endereços IP e dependências para clusters de várias sub-redes – parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configure uma Testemunha de compartilhamento de arquivos ou Testemunha de nuvem para fornecer o quorum em caso de perda no local.  

   > [!NOTE]  
   > O WIndows Server agora inclui uma opção de testemunha baseada em nuvem (Azure). Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.  

   > [!WARNING]  
   > Para saber mais sobre a configuração de quorum, confira [Configurar e gerenciar o quorum em uma configuração de testemunha do guia do Cluster de failover do Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Para saber mais sobre o cmdlet `Set-ClusterQuorum`, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

5. Revise as [Recomendações de rede para um cluster de Hyper-V no Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) e verifique se você configurou corretamente as redes de cluster.  

6. Adicione um disco no local de Redmond ao CSV de cluster. Para fazer isso, clique com botão direito em um disco de origem no nó **Discos** da seção **Armazenamento** e, em seguida, clique em **Adicionar aos Volumes Compartilhados Clusterizados**.  

7. Usando o guia [Implantar um cluster do Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), execute as etapas 7 a 10 no local de **Redmond** para criar uma máquina virtual de teste apenas para garantir que o cluster esteja funcionando normalmente dentro dos dois nós que compartilham o armazenamento no primeiro local de teste.  

8. Se você estiver criando um cluster estendido de dois nós, deverá adicionar todo o armazenamento antes de continuar. Para fazer isso, abra uma sessão do PowerShell com permissões administrativas em nós de cluster e execute o seguinte comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este é o comportamento padrão no Windows Server 2016.

9. Inicie o Windows PowerShell e use o cmdlet `Test-SRTopology` para determinar se você atende a todos os requisitos de Réplica de Armazenamento.  

    Por exemplo, para validar dois dos nós de cluster estendido propostos, em que cada um tem um volume **D:** e **E:** , execute o teste por 30 minutos:
   1. Mova todo o armazenamento disponível para **SR-SRV01**.
   2. Clique em **Criar Função Vazia** na seção **Funções** do Gerenciador de Cluster de Failover.
   3. Adicione o armazenamento online à função vazia chamada **Nova Função**.
   4. Mova todo o armazenamento disponível para **SR-SRV03**.
   5. Clique em **Criar Função Vazia** na seção **Funções** do Gerenciador de Cluster de Failover.
   6. Mova a **Nova Função (2)** vazia para **SR-SRV03**.
   7. Adicione o armazenamento online à função vazia chamada **Nova Função (2)** .
   8. Agora você montou todo o seu armazenamento com letras de unidade e pode avaliar o cluster com `Test-SRTopology`.

       Por exemplo:

           MD c:\temp  

           Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp        

      > [!IMPORTANT]
      > Ao usar um servidor de teste com nenhuma carga de gravação de E/S no volume de origem especificado durante o período de avaliação, adicionar uma carga de trabalho ou Test-SRTopology não gerará um relatório útil. Você deve testar com cargas de trabalho de produção para ver os números reais e os tamanhos de log recomendados. Como alternativa, basta copiar alguns arquivos no volume de origem durante o teste ou baixar e executar DISKSPD para gerar a E/S de gravação. Por exemplo, uma amostra com uma carga de trabalho de E/S de gravação baixa por dez minutos para o volume D:   
       `Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`  

10. Examine o relatório **TestSrTopologyReport-< data >.html** para verificar que você atendeu aos requisitos de Réplica de Armazenamento e observe as recomendações de log e previsão de tempo a sincronização inicial.  

      ![Tela mostrando o relatório de replicação](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

11. Retorne os discos para o Armazenamento disponível e remova as funções vazias temporárias.

12. Quando estiver satisfeito, remova a máquina virtual de teste. Adicione as máquinas virtuais de teste reais necessárias para avaliação adicional para um nó de origem proposto.  

13. Configure o reconhecimento de sites do cluster estendido para que os servidores **SR-SRV01** e **SR-SRV02** estejam no local **Redmond**, o **SR-SRV03** e o **SRV04 SR** no local **Bellevue** e **Redmond** é preferencial para a propriedade do nó de armazenamento de origem e VMs:  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  
   
    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  
   
    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > Não há nenhuma opção para configurar o reconhecimento de locais usando o Gerenciador de Cluster de Failover no Windows Server 2016.  

14. **(Opcional)** Configure redes de cluster e o Active Directory para o failover do site DNS mais rápido. Você pode usar redes definidas pelo software do Hyper-V, VLANs estendidas, dispositivos de abstração de rede, TTL de DNS reduzida e outras técnicas comuns.

    Para obter mais informações, examine a sessão do Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Alongando Clusters de Failover e usando a réplica de armazenamento no Windows Server vNext)](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) e o post do blog [Enable Change Notifications between Sites – How and Why (Habilitar as notificações de alteração entre sites – como e por que)](https://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx).  

15. **(Opcional)** Configure a resiliência de VM para que os convidados não pausem por muitos períodos durante falhas do nó. Em vez disso, eles realizam failover para o novo armazenamento de origem de replicação em 10 segundos.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]
    >  Não há nenhuma opção para configurar a resiliência da VM usando o Gerenciador de Cluster de Failover no Windows Server 2016.  

#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  

1. Teste o cluster proposto e analise os resultados para garantir que você possa continuar:  

   ```PowerShell  
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04  
   ```  

   > [!NOTE]
   >  Você deve esperar erros de armazenamento da validação do cluster, devido ao uso de armazenamento assimétrico.  

2. Crie o servidor de arquivos para uso geral cluster de armazenamento (você deve especificar seu próprio endereço IP estático que o cluster usará). Verifique se o nome do cluster tem no máximo 15 caracteres.  Se os nós residirem em sub-redes diferentes, o endereço IP do site adicional deverá ser criado usando a dependência "OR". Mais informações podem ser encontradas em [Configurando endereços IP e dependências para clusters de várias sub-redes – parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).
   ```PowerShell  
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>  
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```  

3. Configure uma Testemunha de compartilhamento de arquivos ou Testemunha de nuvem (Azure) no cluster que aponta para um compartilhamento hospedado no controlador de domínio ou outro servidor independente. Por exemplo:  

   ```PowerShell  
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
   ```  

   > [!NOTE]
   > O WIndows Server agora inclui uma opção de testemunha baseada em nuvem (Azure). Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.  
    
   Para saber mais sobre a configuração de quorum, confira [Configurar e gerenciar o quorum em uma configuração de testemunha do guia do Cluster de failover do Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Para saber mais sobre o cmdlet `Set-ClusterQuorum`, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

4. Revise as [Recomendações de rede para um cluster de Hyper-V no Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) e verifique se você configurou corretamente as redes de cluster.  

5. Se você estiver criando um cluster estendido de dois nós, deverá adicionar todo o armazenamento antes de continuar. Para fazer isso, abra uma sessão do PowerShell com permissões administrativas em nós de cluster e execute o seguinte comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este é o comportamento padrão no Windows Server 2016.

6. Usando o guia [Implantar um cluster do Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), execute as etapas 7 a 10 no local de **Redmond** para criar uma máquina virtual de teste apenas para garantir que o cluster esteja funcionando normalmente dentro dos dois nós que compartilham o armazenamento no primeiro local de teste.  

7. Quando estiver satisfeito, remova a máquina virtual de teste. Adicione as máquinas virtuais de teste reais necessárias para avaliação adicional para um nó de origem proposto.  

8. Configure o reconhecimento de sites do cluster estendido para que os servidores **SR-SRV01** e **SR-SRV02** estejam no local **Redmond**, o **SR-SRV03** e o **SRV04 SR** no local **Bellevue** e **Redmond** é preferencial para a propriedade do nó de armazenamento de origem e máquinas virtuais:  

   ```PowerShell  
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

   (Get-Cluster).PreferredSite="Seattle"  
   ```  

9. **(Opcional)** Configure redes de cluster e o Active Directory para o failover do site DNS mais rápido. Você pode usar redes definidas pelo software do Hyper-V, VLANs estendidas, dispositivos de abstração de rede, TTL de DNS reduzida e outras técnicas comuns.  

   Para obter mais informações, examine a sessão do Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Alongando Clusters de Failover e usando a réplica de armazenamento no Windows Server vNext)](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) e [Enable Change Notifications between Sites – How and Why (Habilitar as notificações de alteração entre sites – como e por que)](https://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx).  

10. **(Opcional)** Configure a resiliência de VM para que os convidados não pausem por muitos períodos durante falhas do nó. Em vez disso, eles realizam failover para o novo armazenamento de origem de replicação em 10 segundos.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]  
    > Não há nenhuma opção para a resiliência da VM usando o Gerenciador de Cluster de Failover no Windows Server 2016.  



### <a name="BKMK_FileServer"></a>Configurar um servidor de arquivos para o cluster de uso geral  

>[!NOTE]
> Ignore esta seção se você já tiver configurado um cluster de Failover de Hyper-V, conforme descrito em [Configurar um cluster de failover de Hyper-V](#BKMK_HyperV).  

Agora você criará um cluster de failover normal. Após a configuração, a validação e o teste, você irá estendê-lo usando a Réplica de Armazenamento. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.  

#### <a name="graphical-method"></a>Método gráfico  

1. Execute cluadmin.msc.  

2. Valide o cluster proposto e analise os resultados para garantir que você possa continuar.  
   >[!NOTE]
   >Você deve esperar erros de armazenamento da validação do cluster, devido ao uso de armazenamento assimétrico.   
3. Crie o servidor de arquivos para o cluster de armazenamento de uso geral. Verifique se o nome do cluster tem no máximo 15 caracteres. O exemplo usado abaixo é SR-SRVCLUS.  Se os nós estiverem residindo em sub-redes diferentes, você deverá criar um endereço IP para o nome do cluster para cada sub-rede e usar a dependência "OR".  Mais informações podem ser encontradas em [Configurando endereços IP e dependências para clusters de várias sub-redes – parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configure uma Testemunha de compartilhamento de arquivos ou Testemunha de nuvem para fornecer o quorum em caso de perda no local.  
   >[!NOTE]
   > O WIndows Server agora inclui uma opção de testemunha baseada em nuvem (Azure). Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.                                                                                                                                                                             
   >[!NOTE]
   >  Para saber mais sobre a configuração de quorum, confira [Configurar e gerenciar o quorum em uma configuração de testemunha do guia do Cluster de failover do Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Para saber mais sobre o cmdlet Set-ClusterQuorum, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum). 

5. Se você estiver criando um cluster estendido de dois nós, deverá adicionar todo o armazenamento antes de continuar. Para fazer isso, abra uma sessão do PowerShell com permissões administrativas em nós de cluster e execute o seguinte comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este é o comportamento padrão no Windows Server 2016.

6. Verifique se você configurou corretamente as redes de cluster.  
    >[!NOTE]
    > A função de servidor de arquivos deve ser instalada em todos os nós antes de prosseguir para a próxima etapa.   |  

7. Em **Funções**, clique em **Configurar Função**. Revise **Antes de Começar** e clique em **Avançar**.  

8. Selecione **Servidor de Arquivos** e clique em **Avançar**.  

9. Deixe **Servidor de Arquivos para uso geral** selecionado e clique em **Avançar**.  

10. Forneça um nome **Ponto de Acesso para Cliente** (15 caracteres ou menos) e clique em **Avançar**.  

11. Selecione um disco para ser seu volume de dados e clique em **Avançar**.  

12. Revise as configurações e clique em **Avançar**. Clique em **concluir**.  

13. Clique com o botão direito do mouse na sua nova função de Servidor de Arquivos e clique em **Adicionar Compartilhamento de Arquivos**. Continue com o assistente para configurar compartilhamentos.  

14. Opcional: adicione outra função de Servidor de Arquivos que usa o outro armazenamento neste site.  

15. Configure o reconhecimento de sites do cluster estendido para que os servidores SR-SRV01 e SR-SRV02 estejam no local Redmond, o SR-SRV03 e o SRV04 SR no local Bellevue e Redmond é preferencial para a propriedade do nó de armazenamento de origem e VMs:  

    ```PowerShell  
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```  

      >[!NOTE]
      > Não há nenhuma opção para configurar o reconhecimento de locais usando o Gerenciador de Cluster de Failover no Windows Server 2016.  

16. (Opcional) Configure redes de cluster e o Active Directory para o failover do site DNS mais rápido. Você pode usar VLANs estendidas, dispositivos de abstração de rede, TTL de DNS reduzida e outras técnicas comuns.  

Para obter mais informações, examine a sessão do Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Alongando Clusters de Failover e usando a réplica de armazenamento no Windows Server vNext)](https://channel9.msdn.com/events/ignite/2015/brk3487) e o post do blog [Enable Change Notifications between Sites – How and Why (Habilitar as notificações de alteração entre sites – como e por que)](https://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx).    

#### <a name="powershell-method"></a>Método do PowerShell

1. Teste o cluster proposto e analise os resultados para garantir que você possa continuar:    

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  Você deve esperar erros de armazenamento da validação do cluster, devido ao uso de armazenamento assimétrico.   

2.  Crie o cluster de cálculo do Hyper-V (você deve especificar seu próprio endereço IP estático que o cluster usará). Verifique se o nome do cluster tem no máximo 15 caracteres.  Se os nós residirem em sub-redes diferentes, o endereço IP do site adicional deverá ser criado usando a dependência "OR". Mais informações podem ser encontradas em [Configurando endereços IP e dependências para clusters de várias sub-redes – parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here> 

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. Configure uma Testemunha de compartilhamento de arquivos ou Testemunha de nuvem (Azure) no cluster que aponta para um compartilhamento hospedado no controlador de domínio ou outro servidor independente. Por exemplo:  

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > O Windows Server agora inclui uma opção para a testemunha de nuvem usando o Azure. Você pode escolher essa opção de quorum em vez da testemunha de compartilhamento de arquivos.  

   Para obter mais informações sobre a configuração de quorum, consulte o artigo [noções básicas sobre o cluster e o quorum do pool](../storage-spaces/understand-quorum.md). Para saber mais sobre o cmdlet Set-ClusterQuorum, confira [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).

4.  Se você estiver criando um cluster estendido de dois nós, deverá adicionar todo o armazenamento antes de continuar. Para fazer isso, abra uma sessão do PowerShell com permissões administrativas em nós de cluster e execute o seguinte comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

    Este é o comportamento padrão no Windows Server 2016.

5. Verifique se você configurou corretamente as redes de cluster.  

6.  Configure a função de Servidor de Arquivos. Por exemplo:

    ```PowerShell  
    Get-ClusterResource  
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"  

    MD e:\share01  

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false  
    ```

7. Configure o reconhecimento de sites do cluster estendido para que os servidores SR-SRV01 e SR-SRV02 estejam no local Redmond, o SR-SRV03 e o SRV04 SR no local Bellevue e Redmond é preferencial para a propriedade do nó de armazenamento de origem e máquinas virtuais:  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```

8.  (Opcional) Configure redes de cluster e o Active Directory para o failover do site DNS mais rápido. Você pode usar VLANs estendidas, dispositivos de abstração de rede, TTL de DNS reduzida e outras técnicas comuns.  
    
    Para obter mais informações, examine a sessão do Microsoft Ignite [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Alongando Clusters de Failover e usando a réplica de armazenamento no Windows Server vNext)](https://channel9.msdn.com/events/ignite/2015/brk3487) e o post do blog [Enable Change Notifications between Sites – How and Why (Habilitar as notificações de alteração entre sites – como e por que)](https://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx).

### <a name="configure-a-stretch-cluster"></a>Configurar um cluster estendido  
Agora você irá configurar o cluster estendido, usando o Gerenciador de Cluster de Failover ou Windows PowerShell. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.  

#### <a name="failover-cluster-manager-method"></a>Método do Gerenciador de Cluster de Failover  

1.  Para cargas de trabalho do Hyper-V, em um nó em que você tem os dados dos quais deseja replicar, adicione o disco de dados de origem de seus discos disponíveis nos volumes compartilhados clusterizados se já não estiver configurado. Não adicione todos os discos; basta adicionar o disco único. Neste ponto, metade dos discos mostrará offline porque este é um armazenamento assimétrico.  
Se estiver replicando uma carga de trabalho de recurso de disco físico (PDR) como Servidor de Arquivos para uso geral, você já terá um disco conectado à função pronto para usar.  

    ![Tela mostrando o Gerenciador de Cluster de Failover](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)  

2.  Clique com o botão direito no disco CSV ou disco conectado à função, clique em **Replicação** e, em seguida, clique em **Habilitar**.  

3.  Selecione o volume de dados de destino apropriado e clique em **Avançar**. Os discos de destino mostrados terão um volume do mesmo tamanho que o disco de origem selecionado. Durante a movimentação entre essas caixas de diálogo do assistente, o armazenamento disponível automaticamente moverá e ficará online em segundo plano conforme o necessário.  

    ![Tela mostrando a página Selecionar disco de destino do assistente Configurar réplica de armazenamento](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)  

4.  Selecione o disco de log de origem apropriado e clique em **Avançar**. O volume do log de origem deve estar em um disco que usa SSD ou mídia rápida semelhante, não discos rotativos.  

5.  Selecione o volume de log de destino apropriado e clique em Avançar. Os discos de log de destino mostrados terão um volume do mesmo tamanho que o volume de disco de log de origem selecionado.  

6.  Deixe o valor **Substituir volume** em **Substituir volume de destino** se o volume de destino não contiver uma cópia anterior dos dados do servidor de origem. Se o destino contiver dados semelhantes, de um backup recente ou replicação anterior, selecione **Disco de destino de propagação** e, em seguida, clique em **Avançar**.  

7.  Deixe o valor **Modo de Replicação** em **Replicação Síncrona** se quiser usar a replicação zero de RPO. Altere-a para **Replicação Assíncrona** se quiser estender seu cluster em redes de latência mais alta ou precisar de menor latência de E/S nos nós de site primário.  
8.  Deixe o valor **Grupo de Consistência** em **Desempenho Mais Alto** se não quiser usar a ordenação de gravação posterior com pares de disco adicional no grupo de replicação. Se você planeja adicionar mais discos a este grupo de replicação e precisar de ordenação de gravação, selecione **Habilitar Ordenação de Gravação** e, em seguida, clique em **Avançar**.  

9.  Clique em **Avançar** para configurar a replicação e a formação do cluster estendido.  

    ![Tela mostrando a página Selecionar confirmação do assistente Configurar réplica de armazenamento](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)  

10. Na tela de Resumo, observe os resultados da caixa de diálogo de conclusão. Você pode exibir o relatório em um navegador da Web.  

11. Neste ponto, você configurou uma parceria de Réplica de Armazenamento entre as duas metades do cluster, mas a replicação está em andamento. Há várias maneiras de ver o estado de replicação por meio de uma ferramenta gráfica.  

    1.  Use a coluna **Função de Replicação** e a guia **Replicação**. Quando terminar a sincronização inicial, os discos de origem e de destino terão um Status de Replicação **Replicando continuamente**.   

        ![Tela mostrando a guia Replicação de um disco no Gerenciador de Cluster de Failover](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)  

    2.  Inicie **eventvwr.exe**.  

        1.  No servidor de origem, navegue até **Aplicativos e Serviços \ Microsoft \ Windows \ StorageReplica \ Administrador** e examine os eventos 5015, 5002, 5004, 1237, 5001 e 2200.  

        2.  No servidor de destino, navegue até **Aplicativos e Serviços \ Microsoft \ Windows \ StorageReplica \ Operacional** e aguarde o evento 1215. Esse evento indica o número de bytes copiados e o tempo gasto. Exemplo:  

            ```  
            Log Name:      Microsoft-Windows-StorageReplica/Operational  
            Source:        Microsoft-Windows-StorageReplica  
            Date:          4/6/2016 4:52:23 PM  
            Event ID:      1215  
            Task Category: (1)  
            Level:         Information  
            Keywords:      (1)  
            User:          SYSTEM  
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com  
            Description:  
            Block copy completed for replica.  

            ReplicationGroupName: Replication 2  
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:   
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (ms): 140  
            ```  

        3.  No servidor de destino, navegue até **Aplicativos e Serviços\Microsoft\Windows\StorageReplica\Administrador** e examine os eventos 5009, 1237, 5001, 5015, 5005 e 2200 para compreender o andamento do processamento. Não deve haver nenhum aviso de erro nessa sequência. Haverá muitos eventos 1237; eles indicam o progresso.  

            > [!WARNING]
            > A CPU e o uso de memória são provavelmente maiores que o normal até a conclusão da sincronização inicial.  

#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  

1.  Verifique se o console do Powershell está em execução com uma conta de administrador com privilégios elevados.  
2.  Adicione o armazenamento de dados de origem somente no cluster como CSV. Para obter o tamanho, a partição e o layout do volume dos discos disponíveis, use os seguintes comandos:  

    ```PowerShell  
    Move-ClusterGroup -Name "available storage" -Node sr-srv01  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  

    Move-ClusterGroup -Name "available storage" -Node sr-srv03  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  
    ```  

4.  Defina o disco correto para CSV com:  

    ```PowerShell  
    Add-ClusterSharedVolume -Name "Cluster Disk 4"  
    Get-ClusterSharedVolume  
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01  
    ```  

5.  Configure o cluster estendido, especificando o seguinte:  

    -   Nós de origem e de destino (onde os dados de origem são um disco CSV e todos os outros discos não são).  

    -   Nomes de grupos de replicação de origem e destino.  

    -   Discos de origem e destino, onde há correspondência entre os tamanhos das partições.  

    -   Volumes de log de origem e destino, onde não há espaço livre suficiente para conter o tamanho do log em ambos os discos e o armazenamento é SSD ou mídia rápida semelhante.  

    -   Volumes de log de origem e destino, onde não há espaço livre suficiente para conter o tamanho do log em ambos os discos e o armazenamento é SSD ou mídia rápida semelhante.  

    -   Tamanho do log.  

    -   O volume do log de origem deve estar em um disco que usa SSD ou mídia rápida semelhante, não discos rotativos.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:  
    ```  

    > [!NOTE]  
    > Você também pode usar `New-SRGroup` em um nó em cada site e `New-SRPartnership` para criar a replicação em estágios, em vez de todos ao mesmo tempo.  

6.  Determine o andamento da replicação.  

    1.  No servidor de origem, execute o comando a seguir e examine os eventos 5015, 5002, 5004, 1237, 5001 e 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  No servidor de destino, execute o seguinte comando para ver os eventos de Réplica de Armazenamento que mostram a criação da parceria. Esse evento indica o número de bytes copiados e o tempo gasto. Exemplo:  

            Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

            TimeCreated  : 4/6/2016 4:52:23 PM  
            ProviderName : Microsoft-Windows-StorageReplica  
            Id           : 1215  
            Message      : Block copy completed for replica.  

               ReplicationGroupName: Replication 2  
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}  
               ReplicaId: {00000000-0000-0000-0000-000000000000}  
               End LSN in bitmap:   
               LogGeneration: {00000000-0000-0000-0000-000000000000}  
               LogFileId: 0  
               CLSFLsn: 0xFFFFFFFF  
               Number of Bytes Recovered: 68583161856  
               Elapsed Time (ms): 140  

    3.  No servidor de destino, execute o seguinte comando e examine os eventos 5009, 1237, 5001, 5015, 5005 e 2200 para compreender o andamento do processamento. Não deve haver nenhum aviso de erro nessa sequência. Haverá muitos eventos 1237; eles indicam o progresso.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

    4.  Como alternativa, o grupo de servidores de destino para a réplica informa o número de bytes restantes para copiar todo o tempo e pode ser consultado por meio do PowerShell. Por exemplo:  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Como um exemplo de andamento (que não será encerrado):  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

7.  Para obter o estado de origem e destino de replicação dentro do cluster estendido, use `Get-SRGroup` e `Get-SRPartnership` para ver o estado configurado de replicação no cluster estendido.  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

### <a name="manage-stretched-cluster-replication"></a>Gerenciar a replicação de cluster estendido  
Agora você irá gerenciar e operar o cluster estendido. Você pode executar todas as etapas abaixo nos nós de cluster diretamente ou em um computador de gerenciamento remoto que contenha o Windows Server Ferramentas de Administração de Servidor Remoto.  

#### <a name="graphical-tools-method"></a>Método de ferramentas gráficas  

1.  Use o Gerenciador de Cluster de Failover para determinar a origem e o destino atuais da replicação e seus status.  

2.  Para medir o desempenho de replicação, execute **Perfmon.exe** nos nós de origem e destino.  

    1.  No nó de destino:  

        1.  Adicione os objetos **Estatísticas de Réplica de Armazenamento** com todos os seus contadores de desempenho para o volume de dados.  

        2.  Examine os resultados.  

    2.  No nó de origem:  

        1.  Adicione os objetos **Estatísticas de Réplica de Armazenamento** e **Estatísticas de E/S da Partição da Réplica do Armazenamento** com todos os seus contadores de desempenho para o volume de dados (a última opção só está disponível com os dados no servidor de origem atual).  

        2.  Examine os resultados.  

3.  Para alterar a origem e o destino de replicação dentro do cluster estendido, use os seguintes métodos:  

    1.  Para mover a replicação de origem entre os nós no mesmo site: clique com o botão direito no CSV de origem, clique em **Mover Armazenamento**, clique em **Selecionar Nó** e, em seguida, selecione um nó no mesmo site. Se estiver usando o armazenamento não CSV para um disco atribuído a uma função, você poderá mover a função.  

    2.  Para mover a replicação de origem de um site para outro: clique com o botão direito no CSV de origem, clique em **Mover Armazenamento**, clique em **Selecionar Nó** e, em seguida, selecione um nó no outro site. Se você tiver configurado um site preferido, poderá usar o melhor nó possível para sempre mover o armazenamento de origem para um nó no site preferido. Se estiver usando o armazenamento não CSV para um disco atribuído a uma função, você poderá mover a função.  

    3.  Para executar o failover planejado da direção da replicação de um local para outro: desligue ambos os nós em um local usando **ServerManager.exe** ou **SConfig**.  

    4.  Para executar o failover não planejado da direção da replicação de um local para outro: corte a alimentação de ambos os nós em um local.  

        > [!NOTE]
        > No Windows Server 2016, você talvez precise usar o Gerenciador de Cluster de Failover ou Move-ClusterGroup para mover os discos de destino de volta para o outro site manualmente depois que os nós voltarem a ficar online.  

        > [!NOTE]
        > Armazenamento réplica desmonta os volumes de destino. Isso ocorre por design.  

4.  Para alterar o tamanho do log do padrão de 8 GB, clique com o botão direito do mouse nos discos de log de origem e de destino, clique na guia **log de replicação** e altere os tamanhos nos dois discos para correspondência.  

    > [!NOTE]  
    > O tamanho do log padrão é 8 GB. Dependendo dos resultados do cmdlet `Test-SRTopology`, você pode optar por usar `-LogSizeInBytes` com um valor maior ou menor.  

5.  Para adicionar outro par de discos replicados ao grupo de replicação existente, você deverá garantir que haja pelo menos um disco extra no armazenamento disponível. Você pode, em seguida, clicar com o botão direito no disco de origem e selecionar **Adicionar uma parceria de replicação**.  
    > [!NOTE]  
    > Essa necessidade de um disco adicional 'fictício' no armazenamento disponível é devido a uma regressão e não é intencional. O Gerenciador de Cluster de Failover anteriormente suportava a adição de mais discos normalmente e isso ocorrerá novamente em uma versão posterior.  


6.  Para remover a replicação existente:  

    1.  Inicie **cluadmin.msc**.  

    2.  Clique com o botão direito no disco CSV de origem e clique em **Replicação**, em seguida, clique em **Remover**. Aceite a mensagem de aviso.  

    3.  Opcionalmente, remova o armazenamento do CSV para retorná-lo ao armazenamento disponível para outros testes.  

        > [!NOTE]  
        > Você talvez precise usar **DiskMgmt.msc** ou **ServerManager.exe** para adicionar de volta as letras de unidade aos volumes após retornar ao armazenamento disponível.  

#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  

1.  Use **Get-SRGroup** e **(Get-SRGroup).Replicas** para determinar a origem e o destino atuais da replicação e seus status.  

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

    Para saber mais sobre contadores de desempenho no Windows PowerShell, confira [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter).  

3.  Para alterar a origem e o destino de replicação dentro do cluster estendido, use os seguintes métodos:  

    1.  Para mover a origem da replicação de um nó para outro no local de **Redmond**, mova o recurso de CSV usando o cmdlet Move-ClusterSharedVolume.  

        ```PowerShell  
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02  
        ```  

    2.  Para mover a direção da replicação de um site para outro "planejado", mova o recurso de CSV usando o cmdlet **Move-ClusterSharedVolume**.  

        ```PowerShell 
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04  
        ```  

        Isso também moverá os logs e os dados corretamente para os outros sites e nós.  

    3.  Para executar o failover não planejado da direção da replicação de um local para outro: corte a alimentação de ambos os nós em um local.  

        > [!NOTE]  
        > Armazenamento réplica desmonta os volumes de destino. Isso ocorre por design.  

4.  Para alterar o tamanho do log do padrão de 8 GB, use **set-SRGroup** nos grupos de réplica de armazenamento de origem e de destino.   Por exemplo, para definir todos os logs para 2 GB:  

    ```PowerShell  
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB  
    Get-SRGroup  
    ```  

5.  Para adicionar outro par de discos replicados ao grupo de replicação existente, você deverá garantir que haja pelo menos um disco extra no armazenamento disponível. Você pode, em seguida, clicar com o botão direito no disco de origem e selecionar Adicionar uma parceria de replicação.  
       >[!NOTE]
       >Essa necessidade de um disco adicional 'fictício' no armazenamento disponível é devido a uma regressão e não é intencional. O Gerenciador de Cluster de Failover anteriormente suportava a adição de mais discos normalmente e isso ocorrerá novamente em uma versão posterior.  

    Use o cmdlet **Set-SRPartnership** com os parâmetros **-SourceAddVolumePartnership** e **-DestinationAddVolumePartnership**.  
6.  Para remover a replicação, use `Get-SRGroup`, Get-`SRPartnership`, `Remove-SRGroup` e `Remove-SRPartnership` em qualquer nó.  

    ```PowerShell  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]
    > Se usar um computador de gerenciamento remoto, você precisará especificar o nome do cluster para esses cmdlets e fornecer os dois nomes de RG.  

### <a name="related-topics"></a>Tópicos relacionados  
- [Visão geral da réplica de armazenamento](storage-replica-overview.md)  
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)  
- [Cluster para replicação de armazenamento de cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de armazenamento: problemas conhecidos](storage-replica-known-issues.md) 
- [Réplica de armazenamento: perguntas frequentes](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>Veja também  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
