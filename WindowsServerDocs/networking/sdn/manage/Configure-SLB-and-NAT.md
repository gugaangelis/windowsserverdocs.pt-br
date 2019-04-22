---
title: Configurar o balanceador de carga de software para balanceamento de carga e Conversão de endereços de rede (NAT)
description: Este tópico faz parte do guia rede definida pelo Software sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 55847bfbc0362887497514009f6efe1312d79906
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819347"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurar o balanceador de carga de software para balanceamento de carga e Conversão de endereços de rede (NAT)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar a rede definida pelo Software \(SDN\) balanceador de carga de software \(SLB\) para fornecer conversão de endereços de rede de saída \(NAT\), NAT de entrada ou balanceamento de carga entre várias instâncias de um aplicativo.

## <a name="software-load-balancer-overview"></a>Visão geral do balanceador de carga de software

O balanceador de carga de Software de SDN \(SLB\) oferece alta disponibilidade e desempenho de rede para seus aplicativos. É uma camada 4 \(TCP, UDP\) que distribui o tráfego de entrada entre instâncias de serviço íntegros em serviços de nuvem ou máquinas virtuais definidas em um conjunto de Balanceador de carga balanceador de carga.

Configure o SLB para fazer o seguinte:

- Balancear o tráfego entrada externa para uma rede virtual para máquinas virtuais \(VMs\), também chamado de balanceamento de carga de VIP público.
- Entrada balancear a carga entre VMs em uma rede virtual, entre as VMs nos serviços de nuvem ou entre computadores locais e VMs em uma rede de virtual entre locais. 
- Encaminhar o tráfego de rede VM da rede virtual para destinos externos usando a conversão de endereços de rede (NAT), também chamado de NAT de saída.
- Encaminhar tráfego externo para uma VM específica, também chamada de NAT de entrada.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Exemplo: Criar um VIP público para um pool de duas VMs em uma rede virtual de balanceamento de carga

Neste exemplo, você cria um objeto de Balanceador de carga com um VIP público e duas VMs como membros do pool para atender às solicitações para o VIP. Esse código de exemplo também adiciona uma investigação de integridade HTTP para detectar se um dos membros do pool de ficar sem resposta.

1. Prepare-se o objeto de Balanceador de carga.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Atribua um endereço IP front-end, normalmente conhecido como um IP Virtual (VIP).<p>O VIP deve ser de um IP não utilizado em um dos pools IP de rede lógica fornecidos para o Gerenciador de Balanceador de carga. 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. Alocar um pool de endereços de back-end, que contém o IPs dinâmicos (DIPs) que compõem os membros do conjunto de VMs com balanceamento de carga. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Defina uma investigação de integridade que o balanceador de carga usa para determinar o estado de integridade de membros do pool de back-end.<p>Neste exemplo, você define uma investigação HTTP que consulta para o RequestPath de "/ health.htm."  A consulta é executada a cada 5 segundos, conforme especificado pela propriedade IntervalInSeconds.<p>A investigação de integridade deve receber um código de resposta HTTP de 200 para consultas consecutivas 11 para a investigação de considerar o IP de back-end íntegras. Se o IP de back-end não está íntegro, ele não recebe tráfego do balanceador de carga.

   >[!IMPORTANT]
   >Não bloqueie o tráfego de ou para o primeiro IP na sub-rede de qualquer controle para listas de acesso (ACLs) que você se aplicam para o IP de back-end porque esse é o ponto de origem para os testes.

   Use o exemplo a seguir para definir uma investigação de integridade.

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5.  Defina uma regra para enviar o tráfego que chega no IP de front-end para o IP de back-end de balanceamento de carga.  Neste exemplo, o pool de back-end recebe o tráfego TCP para a porta 80.<p>Use o exemplo a seguir para definir uma regra de balanceamento de carga:

   ```PowerShell
    $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
    $Rule.ResourceId = "webserver1"

    $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
    $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
    $Rule.Properties.backendaddresspool = $BackEndAddressPool 
    $Rule.Properties.protocol = "TCP"
    $Rule.Properties.FrontEndPort = 80
    $Rule.Properties.BackEndPort = 80
    $Rule.Properties.IdleTimeoutInMinutes = 4
    $Rule.Properties.Probe = $Probe

    $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. Adicione a configuração de Balanceador de carga no controlador de rede.<p>Use o exemplo a seguir para adicionar a configuração de Balanceador de carga no controlador de rede:

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Siga o exemplo a seguir para adicionar as interfaces de rede para este pool de back-end.


## <a name="example-use-slb-for-outbound-nat"></a>Exemplo: Usar o SLB para NAT de saída

Neste exemplo, você pode configurar SLB com um pool de back-end para fornecer funcionalidade NAT de saída para uma VM no espaço de endereço privado da rede virtual alcançar a saída para a internet. 

1. Crie as propriedades do balanceador de carga, IP de front-end e pool de back-end.

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. Defina a regra NAT de saída.

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. Adicione o objeto de Balanceador de carga no controlador de rede.

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. Siga o exemplo a seguir para adicionar as interfaces de rede ao qual você deseja fornecer acesso à internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Exemplo: Adicionar adaptadores de rede para o pool de back-end
Neste exemplo, você pode adicionar adaptadores de rede para o pool de back-end.  Você deve repetir esta etapa para cada interface de rede que pode processar as solicitações feitas para o VIP. 

Você também pode repetir esse processo em uma interface de rede para adicioná-lo a vários objetos de Balanceador de carga. Por exemplo, se você tiver um objeto de Balanceador de carga para um VIP de servidor web e um objeto de Balanceador de carga separado para fornecer NAT de saída.
    
1. Obtenha o objeto de Balanceador de carga que contém o pool de back-end para adicionar um adaptador de rede.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
  ```

2. Obtenha a interface de rede e adicione o pool backendaddress à matriz loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Coloque o adaptador de rede para aplicar a alteração. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Exemplo: Usar o balanceador de carga de Software para encaminhar o tráfego
Se você precisar mapear um IP Virtual para uma interface de rede em uma rede virtual sem definir portas individuais, você pode criar uma regra de encaminhamento L3.  Essa regra encaminha todo o tráfego de e para a VM por meio do VIP atribuído contido em um objeto de PublicIPAddress.

Se você tiver definido o VIP e DIP como a mesma sub-rede, em seguida, isso é equivalente a executar o encaminhamento de L3 sem NAT.

>[!NOTE]
>Esse processo não requer que você crie um objeto de Balanceador de carga.  Atribuir o PublicIPAddress ao adaptador de rede é informações suficientes para que o balanceador de carga de Software executar sua configuração.


1. Crie um objeto IP público para conter o VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Atribua o PublicIPAddress a um adaptador de rede.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Exemplo: Usar o balanceador de carga de Software para encaminhar o tráfego com um VIP alocado dinamicamente
Este exemplo repete a mesma ação que o exemplo anterior, mas ele aloca automaticamente o VIP do pool disponível de VIPs no balanceador de carga em vez de especificar um endereço IP específico. 

1. Crie um objeto IP público para conter o VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Consulte o recurso de PublicIPAddress para determinar qual endereço IP foi atribuído.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   A propriedade IpAddress contém o endereço atribuído.  A saída será semelhante a este:
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
1. Atribua o PublicIPAddress a um adaptador de rede.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Exemplo: Remover um endereço de PublicIP que está sendo usado para encaminhar o tráfego e retorná-lo ao pool de VIP
Este exemplo remove o recurso de PublicIPAddress que foi criado por um dos exemplos anteriores.  Depois que o PublicIPAddress for removido, a referência para o PublicIPAddress será removida automaticamente da interface de rede, interromperá o tráfego seja encaminhado e o endereço IP será retornado ao pool de VIP público para reutilização.  

1. Remover o PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 