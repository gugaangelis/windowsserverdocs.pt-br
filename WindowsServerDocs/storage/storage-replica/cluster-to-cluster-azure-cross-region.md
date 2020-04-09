---
title: Réplica de armazenamento de cluster para Cluster cruzar região no Azure
description: Cluster para replicação entre regiões de armazenamento em cluster no Azure
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: ee4f508cf0a65b59c3253d6865c649cc9652c569
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856299"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Réplica de armazenamento de cluster para Cluster cruzar região no Azure

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Você pode configurar cluster para réplicas de armazenamento de cluster para aplicativos entre regiões no Azure. Nos exemplos a seguir, usamos um cluster de dois nós, mas cluster para réplica de armazenamento de cluster não é restrito a um cluster de dois nós. A ilustração a seguir é um cluster direto de espaço de armazenamento de dois nós que pode se comunicar entre si, está no mesmo domínio e é entre regiões.

Assista ao vídeo abaixo para obter um passo a passo completo do processo.
> [!video https://www.microsoft.com/videoplayer/embed/RE26xeW]

![O diagrama de arquitetura que apresenta o C2C SR no Azure com a mesma região.](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> Todos os exemplos referenciados são específicos para a ilustração acima.


1. No portal do Azure, crie [grupos de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) em duas regiões diferentes.

    Por exemplo, **Sr-AZ2AZ** no **oeste dos EUA 2** e **SR-AZCROSS** no **Oeste EUA Central**, conforme mostrado acima.

2. Crie dois [conjuntos de disponibilidade](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), um em cada grupo de recursos para cada cluster.
    - Conjunto de disponibilidade (**az2azAS1**) em (**Sr-AZ2AZ**)
    - Conjunto de disponibilidade (**azcross-as**) em (**Sr-azcross**)

3. Criar duas redes virtuais
   - Crie a [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-vnet**) no primeiro grupo de recursos (**Sr-az2az**), tendo uma sub-rede e uma sub-rede de gateway.
   - Crie a [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-VNET**) no segundo grupo de recursos (**Sr-azcross**), tendo uma sub-rede e uma sub-rede de gateway.

4. Criar dois grupos de segurança de rede
   - Crie o [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) no primeiro grupo de recursos (**Sr-az2az**).
   - Crie o [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) no segundo grupo de recursos (**Sr-azcross**).

   Adicione uma regra de segurança de entrada para RDP: 3389 a ambos os grupos de segurança de rede. Você pode optar por remover essa regra depois de concluir a configuração.

5. Crie [máquinas virtuais](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) do Windows Server nos grupos de recursos criados anteriormente.

   Controlador de domínio (**az2azDC**). Você pode optar por criar um terceiro conjunto de disponibilidade para seu controlador de domínio ou adicionar o controlador de domínio em um dos dois conjuntos de disponibilidade. Se você estiver adicionando isso ao conjunto de disponibilidade criado para os dois clusters, atribua a ele um endereço IP público padrão durante a criação da VM.
      - Instale o serviço Domínio do Active Directory.
      - Criar um domínio (contoso.com)
      - Criar um usuário com privilégios de administrador (contosoadmin)

   Crie duas máquinas virtuais (**az2az1**, **az2az2**) no grupo de recursos (**Sr-AZ2AZ**) usando a rede virtual (**AZ2AZ-vnet**) e o grupo de segurança de rede (**AZ2AZ-NSG**) no conjunto de disponibilidade (**az2azAS1**). Atribua um endereço IP público padrão a cada máquina virtual durante a própria criação.
      - Adicionar pelo menos dois discos gerenciados a cada computador
      - Instalar o clustering de failover e o recurso de réplica de armazenamento

   Crie duas máquinas virtuais (**azcross1**, **azcross2**) no grupo de recursos (**Sr-AZCROSS**) usando a rede virtual (**AZCROSS-VNET**) e o grupo de segurança de rede (**AZCROSS-NSG**) no conjunto de disponibilidade (**AZCROSS-as**). Atribuir o endereço IP público padrão a cada máquina virtual durante a própria criação
      - Adicionar pelo menos dois discos gerenciados a cada computador
      - Instalar o clustering de failover e o recurso de réplica de armazenamento

   Conecte todos os nós ao domínio e forneça privilégios de administrador para o usuário criado anteriormente.

   Altere o servidor DNS da rede virtual para o endereço IP privado do controlador de domínio.
   - No exemplo, o controlador de domínio **az2azDC** tem o 10.3.0.8 (endereço IP privado). Na rede virtual (**az2az-vnet** e **azcross-vnet),** altere o servidor DNS 10.3.0.8. 

     No exemplo, conecte todos os nós a "contoso.com" e forneça privilégios de administrador para "contosoadmin".
   - Faça logon como contosoadmin de todos os nós. 
 
6. Crie os clusters (**SRAZC1**, **SRAZCross**).

   Abaixo estão os comandos do PowerShell para o exemplo
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Habilitar espaços de armazenamento diretos.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Para cada cluster, crie um disco virtual e volume. Um para os dados e outro para o log.

8. Crie um [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) de SKU padrão interno para cada cluster (**azlbr1**, **azlbazcross**).

   Forneça o endereço IP do cluster como endereço IP privado estático para o balanceador de carga.
      - azlbr1 = > IP de front-end: 10.3.0.100 (pegue um endereço IP não utilizado da sub-rede da rede virtual (**az2az-vnet**))
      - Crie um pool de back-end para cada balanceador de carga. Adicione os nós de cluster associados.
      - Criar investigação de integridade: porta 59999
      - Criar regra de balanceamento de carga: permitir portas de HA, com IP flutuante habilitado.

   Forneça o endereço IP do cluster como endereço IP privado estático para o balanceador de carga. 
      - azlbazcross = > IP de front-end: 10.0.0.10 (pegue um endereço IP não utilizado da sub-rede da rede virtual (**azcross-VNET**))
      - Crie um pool de back-end para cada balanceador de carga. Adicione os nós de cluster associados.
      - Criar investigação de integridade: porta 59999
      - Criar regra de balanceamento de carga: permitir portas de HA, com IP flutuante habilitado. 

9. Crie um [Gateway de rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) para conectividade vnet a vnet.

   - Criar o primeiro gateway de rede virtual (**az2az-VNetGateway**) no primeiro grupo de recursos (**Sr-az2az**)
   - Tipo de gateway = VPN e tipo de VPN = baseado em rota

   - Criar o segundo gateway de rede virtual (**azcross-VNetGateway**) no segundo grupo de recursos (**Sr-azcross**)
   - Tipo de gateway = VPN e tipo de VPN = baseado em rota

   - Crie uma conexão de vnet para vnet do primeiro gateway de rede virtual para o segundo gateway de rede virtual. Fornecer uma chave compartilhada

   - Crie uma conexão de vnet para vnet do segundo gateway de rede virtual para o primeiro gateway de rede virtual. Forneça a mesma chave compartilhada, conforme fornecido na etapa anterior. 

10. Em cada nó de cluster, abra a porta 59999 (investigação de integridade).

    Execute o seguinte comando em cada nó:

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. Instrua o cluster a escutar mensagens de investigação de integridade na porta 59999 e responder do nó que atualmente possui esse recurso.

    Execute-o uma vez de qualquer nó do cluster, para cada cluster. 
    
    Em nosso exemplo, certifique-se de alterar o "ILBIP" de acordo com seus valores de configuração. Execute o seguinte comando de um nó **az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}  
    ```

12. Execute o seguinte comando de um nó **azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}  
    ```

    Certifique-se de que ambos os clusters podem se conectar/se comunicar entre si.

    Use o recurso "conectar-se ao cluster" no Gerenciador de cluster de failover para se conectar ao outro cluster ou verifique se outro cluster responde de um dos nós do cluster atual.

    Do exemplo que utilizamos:
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. Criar testemunha de nuvem para ambos os clusters. Crie duas [contas de armazenamento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) no Azure, uma para cada cluster em cada grupo de recursos (**Sr-AZ2AZ**, **Sr-AZCROSS**).
   
    - Copie o nome da conta de armazenamento e a chave de "chaves de acesso"
    - Crie a testemunha de nuvem de "Gerenciador de cluster de failover" e use o nome e a chave da conta acima para criá-la. 

14. Executar [testes de validação de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de passar para a próxima etapa

15. Inicie o Windows PowerShell e use o cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, assim como um modo de avaliação de desempenho de execução longa.
 
16. Configure a réplica de armazenamento de cluster para cluster.
    Conceder acesso de um cluster para outro cluster em ambas as direções:

    Em nosso exemplo:
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    Se você estiver usando o Windows Server 2016, execute também este comando:

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. Crie a parceria de SR para os dois clusters:</ol>

    - Para **SRAZC1** de cluster
      - Local do volume:-c:\ClusterStorage\DataDisk1
      - Local do log:-g:
    - Para **SRAZCross** de cluster
      - Local do volume:-c:\ClusterStorage\DataDiskCross
      - Local do log:-g:

Execute o comando:

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```