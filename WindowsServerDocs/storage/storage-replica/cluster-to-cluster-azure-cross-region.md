---
title: Réplica de armazenamento de cluster para Cluster cruzar região no Azure
description: Replicação de armazenamento de Cluster para cluster cross região no Azure
keywords: A réplica de armazenamento, o Gerenciador do servidor, o Windows Server, o Azure, Cluster, várias regiões, região diferente
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: d9999f786639ff4aa303ed34ade14849cda8feec
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475908"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Réplica de armazenamento de cluster para Cluster cruzar região no Azure

> Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

Você pode configurar réplicas de armazenamento de Cluster para Cluster para aplicativos entre regiões no Azure. Nos exemplos a seguir, usamos um cluster de dois nós, mas a réplica de armazenamento de Cluster para Cluster não está restrita a um cluster de dois nós. A ilustração a seguir é um cluster de espaço de armazenamento diretos de dois nós que pode se comunicar uns com os outros, está no mesmo domínio e entre regiões.

Assista ao vídeo abaixo para obter uma explicação completa do processo.
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![A arquitetura de diagrama apresentando SR C2C no Azure withing mesma região.](media\Cluster-to-cluster-azure-cross-region\architecture.png)
> [!IMPORTANT]
> Todos os exemplos de referência são específicos para a ilustração acima.


1. No portal do Azure, crie [grupos de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) em duas regiões diferentes.

    Por exemplo, **SR AZ2AZ** na **Oeste dos EUA 2** e **SR AZCROSS** no **Centro-Oeste dos EUA**, conforme mostrado acima.

2. Criar duas [conjuntos de disponibilidade](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), uma em cada grupo de recursos para cada cluster.
    - Conjunto de disponibilidade (**az2azAS1**) no (**AZ2AZ SR**)
    - Conjunto de disponibilidade (**azcross-AS**) no (**AZCROSS SR**)

3. Criar duas redes virtuais
   - Criar o [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**Vnet az2az**) no primeiro grupo de recursos (**SR AZ2AZ**), que tem uma sub-rede e uma sub-rede de Gateway.
   - Criar o [rede virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**VNET azcross**) no segundo grupo de recursos (**SR AZCROSS**), que tem uma sub-rede e uma sub-rede de Gateway.

4. Crie dois grupos de segurança de rede
   - Criar o [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) no primeiro grupo de recursos (**SR AZ2AZ**).
   - Criar o [grupo de segurança de rede](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) no segundo grupo de recursos (**SR AZCROSS**).

   Adicione uma regra de segurança de entrada para RDP:3389 a ambos os grupos de segurança de rede. Você pode optar por remover essa regra depois de concluir a instalação.

5. Criar o Windows Server [máquinas virtuais](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) nos grupos de recursos criado anteriormente.

   Domain Controller (**az2azDC**). Você pode optar por criar um conjunto de disponibilidade 3ª para seu controlador de domínio ou adicionar o controlador de domínio em um conjunto de disponibilidade de dois. Se você estiver adicionando isso ao conjunto de disponibilidade criado para os dois clusters, atribui-lo um endereço IP público padrão durante a criação da VM.
      - Instale o serviço de domínio do Active Directory.
      - Criar um domínio (contoso.com)
      - Criar um usuário com privilégios de administrador (contosoadmin)

   Crie duas máquinas virtuais (**az2az1**, **az2az2**) no grupo de recursos (**SR AZ2AZ**) usando a rede virtual (**az2az Vnet**) e grupo de segurança de rede (**az2az-NSG**) no conjunto de disponibilidade (**az2azAS1**). Atribua um endereço IP público padrão para cada máquina virtual durante a criação em si.
      - Adicionar pelo menos dois discos gerenciados para cada máquina
      - Instalar o Clustering de Failover e o recurso de réplica de armazenamento

   Crie duas máquinas virtuais (**azcross1**, **azcross2**) no grupo de recursos (**SR AZCROSS**) usando a rede virtual (**azcross-VNET**) e o grupo de segurança de rede (**azcross-NSG**) no conjunto de disponibilidade (**azcross-AS**). Atribuir o endereço IP público padrão para cada máquina virtual durante a criação em si
      - Adicionar pelo menos dois discos gerenciados para cada máquina
      - Instalar o Clustering de Failover e o recurso de réplica de armazenamento

   Conectar-se a todos os nós no domínio e fornecer os privilégios de administrador para o usuário criado anteriormente.

   Altere o servidor DNS da rede virtual para o endereço IP privado de controlador de domínio.
   - No exemplo, o controlador de domínio **az2azDC** tem o endereço IP privado (10.3.0.8). Na rede Virtual (**az2az-Vnet** e **VNET azcross**) alterar servidor DNS 10.3.0.8. 

    No exemplo, se conectar a todos os nós para "contoso.com" e forneça os privilégios de administrador para "contosoadmin".
   - Faça logon como contosoadmin de todos os nós. 
 
6. Criar os clusters (**SRAZC1**, **SRAZCross**).

   Abaixo está os comandos do PowerShell para o exemplo
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Habilite espaços de armazenamento diretos.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Para cada cluster, crie volume e disco virtual. Um para os dados e outro para o log.

8. Criar uma SKU padrão interno [balanceador de carga](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada cluster (**azlbr1**, **azlbazcross**).

   Forneça o endereço IP de Cluster como o endereço IP privado estático para o balanceador de carga.
      - azlbr1 => Frontend IP: 10.3.0.100 (pegar um endereço IP não usado da rede Virtual (**az2az Vnet**) sub-rede)
      - Crie Pool de back-end para cada balanceador de carga. Adicione os nós de cluster associado.
      - Criar a investigação de integridade: porta 59999
      - Crie regra de balanceamento de carga: Permitir que as portas de alta disponibilidade, com habilitada IP flutuante.

   Forneça o endereço IP de Cluster como o endereço IP privado estático para o balanceador de carga. 
      - azlbazcross = > Frontend IP: 10.0.0.10 (pegar um endereço IP não usado da rede Virtual (**azcross VNET**) sub-rede)
      - Crie Pool de back-end para cada balanceador de carga. Adicione os nós de cluster associado.
      - Criar a investigação de integridade: porta 59999
      - Crie regra de balanceamento de carga: Permitir que as portas de alta disponibilidade, com habilitada IP flutuante. 

9. Crie [gateway de rede Virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) para conectividade de rede virtual para rede virtual.

 - Criar o primeiro gateway de rede virtual (**az2az VNetGateway**) no primeiro grupo de recursos (**AZ2AZ SR**)
 - Tipo de gateway = VPN e o tipo de VPN = baseada em rota

 - Criar o segundo gateway de rede Virtual (**azcross VNetGateway**) no segundo grupo de recursos (**AZCROSS SR**)
 - Tipo de gateway = VPN e o tipo de VPN = baseada em rota

 - Crie uma conexão de rede virtual para rede virtual do primeiro gateway de rede Virtual para o segundo gateway de rede Virtual. Fornecer uma chave compartilhada

 - Crie uma conexão de rede virtual para rede virtual do segundo gateway de rede Virtual para o primeiro gateway de rede Virtual. Forneça a mesma chave compartilhada como fornecido na etapa anterior. 

10. Em cada nó de cluster, abra a porta 59999 (investigação de integridade).

   Execute o seguinte comando em cada nó:

   ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
   ```

11. Instrua o cluster para escutar mensagens de investigação de integridade na porta 59999 e responder a partir do nó que atualmente possui esse recurso.

   Execute uma vez a partir de qualquer nó do cluster, para cada cluster. 
    
   Em nosso exemplo, certifique-se de alterar o "ILBIP" acordo com seus valores de configuração. Execute o comando a seguir em qualquer nó **az2az1**/**az2az2**

   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

12. Execute o comando a seguir em qualquer nó **azcross1**/**azcross2**
   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

   Verifique se ambos os clusters podem conectar-se / se comunicam entre si.

   A usar o recurso de "Conectar-se ao Cluster" no Gerenciador de cluster de Failover para se conectar ao outro cluster ou verificar a que outro cluster responde a partir de um de nós do cluster atual.

   Do exemplo estamos usando:
   ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
   ```
   ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
   ```

13. Crie a testemunha de nuvem para os dois clusters. Criar duas [contas de armazenamento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) no Azure, um para cada cluster em cada grupo de recursos (**SR AZ2AZ**,  **SR-AZCROSS**).
   
   - Copiar o nome da conta de armazenamento e a chave de "chaves de acesso"
   - Crie a testemunha de nuvem do "Gerenciador de cluster de failover" e use o nome da conta e a chave acima para criá-lo. 

14. Execute [testes de validação de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de passar para a próxima etapa

15. Inicie o Windows PowerShell e use o cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar se você atende a todos os requisitos de Réplica de Armazenamento. Você pode usar o cmdlet em um modo somente de requisitos para um teste rápido, assim como um modo de avaliação de desempenho de execução longa.
 
16. Configure a réplica de armazenamento de cluster para cluster.
Conceder acesso de um cluster para outro cluster em ambas as direções:

   Do nosso exemplo:
   ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
   ```
Se você estiver usando o Windows Server 2016, também execute este comando:

   ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
   ```

17. Crie SR parceria para os dois clusters:</ol>

   - Para o cluster **SRAZC1**
      - Local do volume:-c:\ClusterStorage\DataDisk1
      - Local do log:-g:
   - Para o cluster **SRAZCross**
      - Local do volume:-c:\ClusterStorage\DataDiskCross
      - Local do log:-g:

Execute o comando:

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```