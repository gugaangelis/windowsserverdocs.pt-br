---
title: Convidado Clustering em uma rede Virtual
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>Convidado Clustering em uma rede Virtual

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Máquinas virtuais que estão conectadas a uma rede virtual só têm permissão para usar os endereços IP que atribuiu controlador de rede para se comunicar na rede.  Isso significa clustering tecnologias que exigem um endereço IP flutuante, como Clustering de Failover da Microsoft, exigem algumas etapas extras para funcionar corretamente.

O método para tornar o IP flutuante acessível é usar um Software balanceador \(SLB\) virtual \(VIP\) IP.  O balanceador de carga de software deve ser configurado com um teste de integridade em uma porta em que IP para que SLB irá direcionar o tráfego na máquina que atualmente tem que IP.

## <a name="example-load-balancer-configuration"></a>Exemplo: Configuração de Balanceador de carga

Este exemplo pressupõe que você já criou VMs do que se tornarão nós de cluster e anexado-los a uma rede Virtual.  Para obter diretrizes, consulte [cria uma VM e conectar-se a uma rede Virtual de locatário ou VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

Neste exemplo você irá criar um endereço IP virtual (192.168.2.100) para representar o endereço IP flutuante do cluster e configurar um teste de integridade para monitorar a porta TCP 59999 para determinar qual nó está ativo.

### <a name="step-1-select-the-vip"></a>Etapa 1: Selecione o VIP
Prepare atribuindo um endereço IP VIP.  Esse endereço pode ser qualquer endereço reservado ou não utilizado na mesma sub-rede como nós de cluster.  O VIP deve corresponder ao endereço flutuante do cluster.

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>Etapa 2: Criar o objeto de propriedades de Balanceador de carga

Você pode usar o comando de exemplo a seguir para criar o objeto de propriedades de Balanceador de carga.

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>Etapa 3: Criar um endereço IP front\-end

Você pode usar o comando de exemplo a seguir para criar um endereço IP front\-end.

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>Etapa 4: Criar um pool back\-end para conter os nós de cluster

Você pode usar o comando de exemplo a seguir para criar um pool back\-end

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>Etapa 5: Adicionar um teste
O teste é necessário para detectar o endereço flutuante está atualmente ativo no nó de cluster.

>[!NOTE]
>A consulta de teste contra endereço permanente da VM na porta definido abaixo.  A porta deve responder somente no nó ativo. 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>Etapa 5: Adicionar as regras de balanceamento de carga
Esta etapa cria uma regra para a porta TCP 1433 balanceamento de carga.  Você pode modificar o protocolo e a porta conforme necessário.  Você também pode repetir esta etapa várias vezes para portas extras e os protocolos neste VIP.  É importante que EnableFloatingIP é definido como $true porque isso informa o balanceador para enviar o pacote para o nó com o VIP original no lugar.

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

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>Etapa 5: Criar o balanceador no controlador de rede

Você pode usar o comando de exemplo a seguir para criar o balanceador de carga.

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>Etapa 6: Adicionar os nós de cluster ao pool de back-end

Este exemplo mostra a adição de dois membros de pool, mas você pode adicionar nós ao pool de quantos forem necessários para o cluster.

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

Depois que você criou o balanceador de carga e adicionado as interfaces de rede ao pool de back-end, você estará pronto para configurar o cluster.  Se você estiver usando um Cluster de Failover Microsoft, você pode continuar com o próximo exemplo. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Exemplo 2: Configurando um Cluster de Failover da Microsoft

Você pode usar as etapas a seguir para configurar um cluster de failover.

### <a name="step-1-install-failover-clustering"></a>Etapa 1: Instalar o cluster de failover

Você pode usar os comandos de exemplo a seguir para instalar e configurar propriedades para um cluster de failover.

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>Etapa 2: Criar o cluster em um nó

Você pode usar o comando de exemplo a seguir para criar o cluster em um nó.

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>Etapa 3: Interromper o recurso de cluster

Você pode usar o comando de exemplo a seguir para parar o recurso de cluster.

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>Etapa 4: Definir o cluster porta IP e teste
O endereço IP deve coincidir com o endereço ip front-end usado no exemplo anterior, e a porta de teste deve corresponder a porta de teste no exemplo anterior.

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>Etapa 5: Iniciar os recursos de cluster

Você pode usar o comando de exemplo a seguir para iniciar os recursos de cluster.

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>Etapa 6: Adicionar os nós restantes

Você pode usar o comando de exemplo a seguir para adicionar nós de cluster.

    Add-ClusterNode $nodes[1]

Após a conclusão da última etapa, o cluster está ativo. Tráfego indo para o VIP na porta especificada será direcionado no nó active.