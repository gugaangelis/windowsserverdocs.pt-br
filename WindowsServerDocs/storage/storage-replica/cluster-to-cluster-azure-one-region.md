---
title: Cluster para cluster de réplica de armazenamento na mesma região no Azure
description: Cluster para cluster de replicação de armazenamento na mesma região no Azure
keywords: A réplica de armazenamento, Gerenciador de servidores, Windows Server, Azure, Cluster, a mesma região
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 9cf998087e23f45fe5981aef6d1ff5b7b4e85b9b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447612"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Cluster para cluster de réplica de armazenamento na mesma região no Azure

> Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

Você pode configurar a replicação de armazenamento de cluster para cluster na mesma região no Azure. Nos exemplos a seguir, usamos um cluster de dois nós, mas a réplica de armazenamento de cluster para cluster não está restrita a um cluster de dois nós. A ilustração a seguir é um cluster de espaço de armazenamento diretos de dois nós que pode se comunicar uns com os outros estão no mesmo domínio e na mesma região.

Assista aos vídeos abaixo para obter uma explicação completa do processo.

Parte um
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Parte dois
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![O diagrama da arquitetura mostrando a réplica de armazenamento de cluster para Cluster no Azure na mesma região.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Todos os exemplos de referência são específicos para a ilustração acima.

1. Criar uma [grupo de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) no portal do Azure em uma região (**AZ2AZ SR** na **Oeste dos EUA 2**). 
2. Criar duas [conjuntos de disponibilidade](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) no grupo de recursos (**AZ2AZ SR**) criado acima, um para cada cluster. 
    a. Conjunto de disponibilidade (**az2azAS1**) b. Conjunto de disponibilidade (**az2azAS2**)
3. Criar uma [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**Vnet az2az**) no grupo de recursos criado anteriormente (**SR AZ2AZ**), tendo pelo menos uma sub-rede. 
4. Criar uma [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**NSG az2az**) e adicione uma regra de segurança de entrada para RDP:3389. Você pode optar por remover essa regra depois de concluir a instalação. 
5. Criar o Windows Server [máquinas virtuais](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) no grupo de recursos criado anteriormente (**SR AZ2AZ**). Usar a rede virtual criada anteriormente (**az2az-Vnet**) e o grupo de segurança de rede (**NSG az2az**). 
   
   Domain Controller (**az2azDC**). Você pode optar por criar um terceiro conjunto de disponibilidade para seu controlador de domínio ou adicionar o controlador de domínio em um dos dois conjuntos de disponibilidade. Se você estiver adicionando isso ao conjunto de disponibilidade criado para os dois clusters, atribui-lo um endereço IP público padrão durante a criação da VM. 
   - Instale o serviço de domínio do Active Directory.
   - Criar um domínio (Contoso.com)
   - Criar um usuário com privilégios de administrador (contosoadmin) 
   - Crie duas máquinas virtuais (**az2az1**, **az2az2**) no conjunto de disponibilidade primeiro (**az2azAS1**). Atribua um endereço IP público padrão para cada máquina virtual durante a criação em si.
   - Adicionar pelo menos 2 discos gerenciados para cada máquina
   - Instalar o recurso de Clustering de Failover e a réplica de armazenamento
   - Crie duas máquinas virtuais (**az2az3**, **az2az4**) no conjunto de disponibilidade de segundo (**az2azAS2**). Atribua o endereço IP público padrão para cada máquina virtual durante a criação em si. 
   - Adicione pelo menos 2 discos gerenciados para cada máquina. 
   - Instale o recurso de Clustering de Failover e a réplica de armazenamento. 
   
6. Conectar-se a todos os nós no domínio e fornecer os privilégios de administrador para o usuário criado anteriormente. 

7. Altere o servidor DNS da rede virtual para o endereço IP privado de controlador de domínio. 
8. Em nosso exemplo, o controlador de domínio **az2azDC** tem o endereço IP privado (10.3.0.8). Na rede Virtual (**az2az Vnet**) alterar servidor DNS 10.3.0.8. Conecte todos os nós para "Contoso.com" e forneça os privilégios de administrador para "contosoadmin".
   - Faça logon como contosoadmin de todos os nós. 
    
9. Criar os clusters (**SRAZC1**, **SRAZC2**). 
   Abaixo está os comandos do PowerShell para o nosso exemplo
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
   
    Para cada cluster, crie volume e disco virtual. Um para os dados e outro para o log. 
   
11. Criar uma SKU padrão interno [balanceador de carga](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada cluster (**azlbr1**,**azlbr2**). 
   
    Forneça o endereço IP de Cluster como o endereço IP privado estático para o balanceador de carga.
    - azlbr1 => Frontend IP: 10.3.0.100 (pegar um endereço IP não usado da rede Virtual (**az2az Vnet**) sub-rede)
    - Crie Pool de back-end para cada balanceador de carga. Adicione os nós de cluster associado.
    - Criar a investigação de integridade: porta 59999
    - Crie regra de balanceamento de carga: Permitir que as portas de alta disponibilidade, com habilitada IP flutuante. 
   
    Forneça o endereço IP de Cluster como o endereço IP privado estático para o balanceador de carga.
    - azlbr2 => Frontend IP: 10.3.0.101 (pegar um endereço IP não usado da rede Virtual (**az2az Vnet**) sub-rede)
    - Crie Pool de back-end para cada balanceador de carga. Adicione os nós de cluster associado.
    - Criar a investigação de integridade: porta 59999
    - Crie regra de balanceamento de carga: Permitir que as portas de alta disponibilidade, com habilitada IP flutuante. 
   
12. Em cada nó de cluster, abra a porta 59999 (investigação de integridade). 
   
    Execute o seguinte comando em cada nó:
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Instrua o cluster para escutar mensagens de investigação de integridade na porta 59999 e responder a partir do nó que atualmente possui esse recurso. 
    Execute uma vez a partir de qualquer nó do cluster, para cada cluster. 
    
    Em nosso exemplo, certifique-se de alterar o "ILBIP" acordo com seus valores de configuração. Execute o comando a seguir em qualquer nó **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Execute o comando a seguir em qualquer nó **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    Verifique se ambos os clusters podem conectar-se / se comunicam entre si. 

    A usar o recurso de "Conectar-se ao Cluster" no Gerenciador de cluster de Failover para se conectar ao outro cluster ou verificar a que outro cluster responde a partir de um de nós do cluster atual.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Crie as testemunhas de nuvem para os dois clusters. Criar duas [contas de armazenamento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) no azure para cada cluster no mesmo grupo de recursos (**SR AZ2AZ**).

    - Copiar o nome da conta de armazenamento e a chave de "chaves de acesso"
    - Crie a testemunha de nuvem do "Gerenciador de cluster de failover" e use o nome da conta e a chave acima para criá-lo.

16. Execute [testes de validação de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de passar para a próxima etapa.

17. Inicie o Windows PowerShell e use o cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, bem como um modo de avaliação de desempenho de execução longa.

18. Configure a réplica de armazenamento de cluster para cluster.
   
    Conceder acesso de um cluster para outro cluster em ambas as direções:

    Em nosso exemplo:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Se você estiver usando o Windows Server 2016, em seguida, também, execute este comando:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Crie SRPartnership para os clusters:</ol>

    - Para o cluster **SRAZC1**.
    - Local do volume:-c:\ClusterStorage\DataDisk1
    - Local do log:-g:
    - Para o cluster **SRAZC2**
    - Local do volume:-c:\ClusterStorage\DataDisk2
    - Local do log:-g:

Execute o seguinte comando:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```