---
title: Clustering convidado em uma rede virtual
description: Máquinas virtuais conectadas a uma rede virtual só têm permissão para usar os endereços IP que o controlador de rede atribuído a se comunicar pela rede.  Tecnologias de clustering que exigem um endereço IP flutuante, como o Clustering de Failover da Microsoft, exigem algumas etapas adicionais para funcionar corretamente.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: fcd37ebb3739f1d7118ce41dfc61764486c920d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844957"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering convidado em uma rede virtual

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Máquinas virtuais conectadas a uma rede virtual só têm permissão para usar os endereços IP que o controlador de rede atribuído a se comunicar pela rede.  Tecnologias de clustering que exigem um endereço IP flutuante, como o Clustering de Failover da Microsoft, exigem algumas etapas adicionais para funcionar corretamente.

O método para fazer o IP flutuante acessível é usar um balanceador de carga de Software \(SLB\) IP virtual \(VIP\).  O balanceador de carga de software deve ser configurado com uma investigação de integridade em uma porta em que IP para que o SLB direciona o tráfego para o computador que atualmente possui esse IP.


## <a name="example-load-balancer-configuration"></a>Exemplo: Configuração de Balanceador de carga

Este exemplo pressupõe que você já criou as VMs que se tornarão nós do cluster e anexada-los a uma rede Virtual.  Para obter diretrizes, consulte [criar uma VM e conectar-se a uma rede Virtual do locatário ou VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

Neste exemplo você criará um endereço IP virtual (192.168.2.100) para representar o endereço IP flutuante do cluster e configure uma investigação de integridade para monitorar a porta TCP 59999, para determinar qual nó está ativo.

1. Selecione o VIP.<p>Prepare-se atribuir um endereço IP VIP, que pode ser qualquer endereço não utilizado ou reservado na mesma sub-rede que os nós do cluster.  O VIP deve corresponder ao endereço flutuante do cluster.

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

3. Criar um front\-endereço IP final.

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

4. Crie um back\-terminar o pool para conter os nós do cluster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Adicione uma investigação para detectar o endereço flutuante está atualmente ativo no nó de cluster. 

   >[!NOTE]
   >A consulta de investigação em relação ao endereço de permanente da VM na porta definida abaixo.  A porta só deve responder no nó ativo. 

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

6. Adicione regras para a porta TCP 1433 de balanceamento de carga.<p>Você pode modificar o protocolo e porta, conforme necessário.  Você também pode repetir esta etapa várias vezes para portas adicionais e protcols neste VIP.  É importante que EnableFloatingIP está definido como $true, porque isso informa ao balanceador de carga para enviar o pacote para o nó com o VIP original em vigor.

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

8. Adicione nós do cluster para o pool de back-end.<p>Você pode adicionar nós ao pool de quantas forem necessárias para o cluster.

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

   Depois que você criou o balanceador de carga e adicionado os adaptadores de rede para o pool de back-end, você está pronto para configurar o cluster.  

9. (Opcional) Se você estiver usando um Cluster de Failover do Microsoft, continue com o exemplo a seguir. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Exemplo 2: Configurando um cluster de failover da Microsoft

Você pode usar as etapas a seguir para configurar um cluster de failover.

1. Instalar e configurar as propriedades de um cluster de failover.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

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

4. Defina o cluster IP, porta de investigação.<p>O endereço IP deve corresponder ao endereço ip de front-end usado no exemplo anterior, e a porta de investigação deve corresponder à porta de investigação no exemplo anterior.

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

_**O cluster está ativo.**_ O tráfego direcionado para o VIP na porta especificada é direcionado ao nó ativo.

---