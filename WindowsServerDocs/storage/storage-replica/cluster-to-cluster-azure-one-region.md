---
title: Cluster para réplica de armazenamento de cluster na mesma região no Azure
description: Cluster para replicação de armazenamento de cluster na mesma região no Azure
keywords: Réplica de armazenamento, Gerenciador do Servidor, Windows Server, Azure, cluster, a mesma região
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 55d9c600c86b6b64efdb5c7d4437697539f887ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402951"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Cluster para réplica de armazenamento de cluster na mesma região no Azure

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Você pode configurar o cluster para replicação de armazenamento de cluster na mesma região no Azure. Nos exemplos a seguir, usamos um cluster de dois nós, mas cluster para réplica de armazenamento de cluster não é restrito a um cluster de dois nós. A ilustração a seguir é um cluster direto de espaço de armazenamento de dois nós que pode se comunicar entre si, está no mesmo domínio e na mesma região.

Assista aos vídeos abaixo para ver um passo a passo completo do processo.

Parte um
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Parte dois
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![O diagrama de arquitetura que está mostrando a réplica de armazenamento de cluster para cluster no Azure na mesma região.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Todos os exemplos referenciados são específicos para a ilustração acima.

1. Crie um [grupo de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) no portal do Azure em uma região (**Sr-AZ2AZ** no **oeste dos EUA 2**). 
2. Crie dois [conjuntos de disponibilidade](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) no grupo de recursos (**Sr-AZ2AZ**) criados acima, um para cada cluster. 
    a. Conjunto de disponibilidade (**az2azAS1**) b. Conjunto de disponibilidade (**az2azAS2**)
3. Crie uma [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-vnet**) no grupo de recursos criado anteriormente (**Sr-az2az**), com pelo menos uma sub-rede. 
4. Crie um [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) e adicione uma regra de segurança de entrada para RDP: 3389. Você pode optar por remover essa regra depois de concluir a configuração. 
5. Crie [máquinas virtuais](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) do Windows Server no grupo de recursos criado anteriormente (**Sr-AZ2AZ**). Use a rede virtual criada anteriormente (**az2az-vnet**) e o grupo de segurança de rede (**az2az-NSG**). 
   
   Controlador de domínio (**az2azDC**). Você pode optar por criar um terceiro conjunto de disponibilidade para seu controlador de domínio ou adicionar o controlador de domínio em um dos dois conjuntos de disponibilidade. Se você estiver adicionando isso ao conjunto de disponibilidade criado para os dois clusters, atribua a ele um endereço IP público padrão durante a criação da VM. 
   - Instale o serviço Domínio do Active Directory.
   - Criar um domínio (Contoso.com)
   - Criar um usuário com privilégios de administrador (contosoadmin) 
   - Crie duas máquinas virtuais (**az2az1**, **az2az2**) no primeiro conjunto de disponibilidade (**az2azAS1**). Atribua um endereço IP público padrão a cada máquina virtual durante a própria criação.
   - Adicionar pelo menos 2 discos gerenciados a cada computador
   - Instalar o recurso de clustering de failover e de réplica de armazenamento
   - Crie duas máquinas virtuais (**az2az3**, **az2az4**) no segundo conjunto de disponibilidade (**az2azAS2**). Atribua o endereço IP público padrão a cada máquina virtual durante a própria criação. 
   - Adicione pelo menos 2 discos gerenciados a cada computador. 
   - Instale o recurso de clustering de failover e de réplica de armazenamento. 
   
6. Conecte todos os nós ao domínio e forneça privilégios de administrador para o usuário criado anteriormente. 

7. Altere o servidor DNS da rede virtual para o endereço IP privado do controlador de domínio. 
8. Em nosso exemplo, o controlador de domínio **az2azDC** tem o 10.3.0.8 (endereço IP privado). Na rede virtual (**az2az-vnet),** altere 10.3.0.8 do servidor DNS. Conecte todos os nós a "Contoso.com" e forneça privilégios de administrador para "contosoadmin".
   - Faça logon como contosoadmin de todos os nós. 
    
9. Crie os clusters (**SRAZC1**, **SRAZC2**). 
   Abaixo estão os comandos do PowerShell para nosso exemplo
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. Habilitar espaços de armazenamento diretos
    ```PowerShell
    Enable-clusterS2D
    ```   
   
    Para cada cluster, crie um disco virtual e volume. Um para os dados e outro para o log. 
   
11. Crie um [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) de SKU padrão interno para cada cluster (**azlbr1**,**azlbr2**). 
   
    Forneça o endereço IP do cluster como endereço IP privado estático para o balanceador de carga.
    - azlbr1 = > IP de front-end: 10.3.0.100 (pegue um endereço IP não utilizado da sub-rede da rede virtual (**az2az**))
    - Crie um pool de back-end para cada balanceador de carga. Adicione os nós de cluster associados.
    - Criar investigação de integridade: porta 59999
    - Criar regra de balanceamento de carga: Permitir portas de alta disponibilidade, com IP flutuante habilitado. 
   
    Forneça o endereço IP do cluster como endereço IP privado estático para o balanceador de carga.
    - azlbr2 = > IP de front-end: 10.3.0.101 (pegue um endereço IP não utilizado da sub-rede da rede virtual (**az2az**))
    - Crie um pool de back-end para cada balanceador de carga. Adicione os nós de cluster associados.
    - Criar investigação de integridade: porta 59999
    - Criar regra de balanceamento de carga: Permitir portas de alta disponibilidade, com IP flutuante habilitado. 
   
12. Em cada nó de cluster, abra a porta 59999 (investigação de integridade). 
   
    Execute o seguinte comando em cada nó:
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Instrua o cluster a escutar mensagens de investigação de integridade na porta 59999 e responder do nó que atualmente possui esse recurso. 
    Execute-o uma vez de qualquer nó do cluster, para cada cluster. 
    
    Em nosso exemplo, certifique-se de alterar o "ILBIP" de acordo com seus valores de configuração. Execute o seguinte comando de um nó **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Execute o seguinte comando de um nó **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    Certifique-se de que ambos os clusters podem se conectar/se comunicar entre si. 

    Use o recurso "conectar-se ao cluster" no Gerenciador de cluster de failover para se conectar ao outro cluster ou verifique se outro cluster responde de um dos nós do cluster atual.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Crie testemunhas de nuvem para ambos os clusters. Crie duas [contas de armazenamento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) no Azure uma para cada cluster no mesmo grupo de recursos (**Sr-AZ2AZ**).

    - Copie o nome da conta de armazenamento e a chave de "chaves de acesso"
    - Crie a testemunha de nuvem de "Gerenciador de cluster de failover" e use o nome e a chave da conta acima para criá-la.

16. Execute [testes de validação de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de passar para a próxima etapa.

17. Inicie o Windows PowerShell e use o cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, bem como um modo de avaliação de desempenho de execução longa.

18. Configure a réplica de armazenamento de cluster para cluster.
   
    Conceder acesso de um cluster para outro cluster em ambas as direções:

    Em nosso exemplo:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Se você estiver usando o Windows Server 2016, execute também este comando:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Crie SRPartnership para os clusters:</ol>

    - Para **SRAZC1**de cluster.
    - Local do volume:-c:\ClusterStorage\DataDisk1
    - Local do log:-g:
    - Para **SRAZC2** de cluster
    - Local do volume:-c:\ClusterStorage\DataDisk2
    - Local do log:-g:

Execute o seguinte comando:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```