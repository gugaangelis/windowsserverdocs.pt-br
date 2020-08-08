---
title: Clustering convidado em uma rede virtual
description: As máquinas virtuais conectadas a uma rede virtual só têm permissão para usar os endereços IP que o controlador de rede atribuiu para se comunicar na rede.  As tecnologias de clustering que exigem um endereço IP flutuante, como o clustering de failover da Microsoft, exigem algumas etapas adicionais para funcionar corretamente.
manager: grcusanz
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 6d597d4ced923c751e54ed4678ffb2d956a7b471
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994765"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering convidado em uma rede virtual

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

As máquinas virtuais conectadas a uma rede virtual só têm permissão para usar os endereços IP que o controlador de rede atribuiu para se comunicar na rede.  As tecnologias de clustering que exigem um endereço IP flutuante, como o clustering de failover da Microsoft, exigem algumas etapas adicionais para funcionar corretamente.

O método para tornar o IP flutuante acessível é usar um software Load Balancer \( VIP de \) IP virtual \( SLB \) .  O balanceador de carga de software deve ser configurado com uma investigação de integridade em uma porta nesse IP para que o SLB direcione o tráfego para o computador que atualmente tem esse IP.


## <a name="example-load-balancer-configuration"></a>Exemplo: configuração do balanceador de carga

Este exemplo pressupõe que você já criou as VMs que se tornarão nós de cluster e as anexará a uma rede virtual.  Para obter diretrizes, consulte [criar uma VM e conectar-se a uma rede virtual de locatário ou VLAN](./create-a-tenant-vm.md).

Neste exemplo, você criará um endereço IP virtual (192.168.2.100) para representar o endereço IP flutuante do cluster e configurará uma investigação de integridade para monitorar a porta TCP 59999 para determinar qual nó é o ativo.

1. Selecione o VIP.<p>Prepare atribuindo um endereço IP VIP, que pode ser qualquer endereço não usado ou reservado na mesma sub-rede que os nós do cluster.  O VIP deve corresponder ao endereço flutuante do cluster.

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. Crie o objeto de propriedades do balanceador de carga.

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. Crie um endereço IP de front- \- end.

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. Crie um pool de back- \- end para conter os nós de cluster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Adicione uma investigação para detectar em qual nó de cluster o endereço flutuante está ativo no momento.

   >[!NOTE]
   >A consulta de investigação em relação ao endereço permanente da VM na porta definida abaixo.  A porta deve responder somente no nó ativo.

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. Adicione as regras de balanceamento de carga para a porta TCP 1433.<p>Você pode modificar o protocolo e a porta conforme necessário.  Você também pode repetir essa etapa várias vezes para portas adicionais e protcols nesse VIP.  É importante que EnableFloatingIP esteja definido como $true porque isso diz ao balanceador de carga para enviar o pacote para o nó com o VIP original em vigor.

   ```PowerShell
   $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $lbrule.ResourceId = "Rules1"

   $lbrule.properties.frontendipconfigurations += $FrontEnd
   $lbrule.properties.backendaddresspool = $BackEnd
   $lbrule.properties.protocol = "TCP"
   $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433
   $lbrule.properties.IdleTimeoutInMinutes = 4
   $lbrule.properties.EnableFloatingIP = $true
   $lbrule.properties.Probe = $lbprobe
   ```

7. Crie o balanceador de carga no controlador de rede.

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. Adicione os nós de cluster ao pool de back-end.<p>Você pode adicionar tantos nós ao pool quanto necessário para o cluster.

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   Depois de criar o balanceador de carga e adicionar as interfaces de rede ao pool de back-end, você estará pronto para configurar o cluster.

9. Adicional Se você estiver usando um cluster de failover da Microsoft, continue com o próximo exemplo.

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Exemplo 2: Configurando um cluster de failover da Microsoft

Você pode usar as etapas a seguir para configurar um cluster de failover do.

1. Instale e configure as propriedades de um cluster de failover.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"

   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =
   $ILBIP = "192.168.2.100"

   $nodes = @("DB1", "DB2")
   ```

2. Crie o cluster em um nó.

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. Pare o recurso de cluster.

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. Defina o IP do cluster e a porta de investigação.<p>O endereço IP deve corresponder ao endereço IP de front-end usado no exemplo anterior, e a porta de investigação deve corresponder à porta de investigação no exemplo anterior.

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. Inicie os recursos de cluster.

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. Adicione os nós restantes.

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**O cluster está ativo.**_ O tráfego que vai para o VIP na porta especificada é direcionado para o nó ativo.

---