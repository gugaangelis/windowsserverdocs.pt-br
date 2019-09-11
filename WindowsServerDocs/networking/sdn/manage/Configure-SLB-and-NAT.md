---
title: Configurar o balanceador de carga de software para balanceamento de carga e Conversão de endereços de rede (NAT)
description: Este tópico faz parte do guia de rede definido pelo software sobre como gerenciar cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
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
ms.openlocfilehash: 70bc6aa6a1276506d81b56520b7e127cd271cc83
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867154"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurar o balanceador de carga de software para balanceamento de carga e Conversão de endereços de rede (NAT)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar o \( \(SLB\) do balanceador de\) carga de software de rede definido pelo software Sdn para fornecer \(NAT\)de conversão de endereços de rede de saída, NAT de entrada ou balanceamento de carga entre várias instâncias de um aplicativo.

## <a name="software-load-balancer-overview"></a>Visão geral do software Load Balancer

O software Sdn Load Balancer \(SLB\) oferece alta disponibilidade e desempenho de rede para seus aplicativos. É um TCP de camada \(4, balanceador de carga UDP\) que distribui o tráfego de entrada entre instâncias de serviço íntegro em serviços de nuvem ou máquinas virtuais definidas em um conjunto de balanceadores de carga.

Configure o SLB para fazer o seguinte:

- Balancear a carga do tráfego de entrada externo a uma rede virtual \(para\)VMs de máquinas virtuais, também chamado de balanceamento de carga VIP público.
- Balancear a carga do tráfego de entrada entre VMs em uma rede virtual, entre VMs em serviços de nuvem ou entre computadores locais e VMs em uma rede virtual entre locais. 
- Encaminhe o tráfego de rede VM da rede virtual para destinos externos usando a NAT (conversão de endereços de rede), também chamada de NAT de saída.
- Encaminhe o tráfego externo para uma VM específica, também chamada de NAT de entrada.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Exemplo: Criar um VIP público para balanceamento de carga de um pool de duas VMs em uma rede virtual

Neste exemplo, você cria um objeto de balanceador de carga com um VIP público e duas VMs como membros do pool para atender às solicitações para o VIP. Este exemplo de código também adiciona uma investigação de integridade HTTP para detectar se um dos membros do pool se torna não responsivo.

1. Prepare o objeto do balanceador de carga.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Atribua um endereço IP de front-end, comumente conhecido como VIP (IP virtual).<p>O VIP deve ser de um IP não utilizado em um dos pools de IP de rede lógica fornecidos ao Gerenciador de balanceadores de carga. 

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

3. Aloque um pool de endereços de back-end, que contém os DIPs (IPs dinâmicos) que compõem os membros do conjunto de VMs com balanceamento de carga. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Defina uma investigação de integridade que o balanceador de carga usa para determinar o estado de integridade dos membros do pool de back-end.<p>Neste exemplo, você define uma investigação HTTP que consulta o RequestPath de "/Health.htm."  A consulta é executada a cada 5 segundos, conforme especificado pela propriedade IntervalInSeconds.<p>A investigação de integridade deve receber um código de resposta HTTP 200 para 11 consultas consecutivas para que a investigação considere o IP de back-end a ser íntegro. Se o IP de back-end não estiver íntegro, ele não receberá o tráfego do balanceador de carga.

   >[!IMPORTANT]
   >Não bloqueie o tráfego de ou para o primeiro IP na sub-rede para quaisquer ACLs (listas de controle de acesso) que você aplicar ao IP de back-end porque esse é o ponto de origem para as investigações.

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

5. Defina uma regra de balanceamento de carga para enviar o tráfego que chega ao IP de front-end para o IP de back-end.  Neste exemplo, o pool de back-end recebe o tráfego TCP para a porta 80.<p>Use o exemplo a seguir para definir uma regra de balanceamento de carga:

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

6. Adicione a configuração do balanceador de carga ao controlador de rede.<p>Use o exemplo a seguir para adicionar a configuração do balanceador de carga ao controlador de rede:

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Siga o exemplo a seguir para adicionar as interfaces de rede a esse pool de back-ends.


## <a name="example-use-slb-for-outbound-nat"></a>Exemplo: Usar SLB para NAT de saída

Neste exemplo, você configura o SLB com um pool de back-end para fornecer o recurso de NAT de saída para uma VM no espaço de endereço privado de uma rede virtual para alcançar a saída para a Internet. 

1. Crie as propriedades do balanceador de carga, o IP de front-end e o pool de back-end.

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

3. Adicione o objeto de balanceador de carga no controlador de rede.

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. Siga o exemplo a seguir para adicionar as interfaces de rede às quais você deseja fornecer acesso à Internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Exemplo: Adicionar interfaces de rede ao pool de back-ends
Neste exemplo, você adiciona interfaces de rede ao pool de back-end.  Você deve repetir essa etapa para cada interface de rede que pode processar solicitações feitas ao VIP. 

Você também pode repetir esse processo em uma única interface de rede para adicioná-lo a vários objetos do balanceador de carga. Por exemplo, se você tiver um objeto de balanceador de carga para um VIP de servidor Web e um objeto de balanceador de carga separado para fornecer NAT de saída.
    
1. Obtenha o objeto do balanceador de carga que contém o pool de back-ends para adicionar uma interface de rede.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. Obtenha a interface de rede e adicione o pool backendaddress à matriz loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Coloque a interface de rede para aplicar a alteração. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Exemplo: Usar o Load Balancer de software para o tráfego de encaminhamento
Se você precisar mapear um IP virtual para uma única interface de rede em uma rede virtual sem definir portas individuais, poderá criar uma regra de encaminhamento L3.  Essa regra encaminha todo o tráfego de e para a VM por meio do VIP atribuído contido em um objeto PublicIPAddress.

Se você tiver definido o VIP e o DIP como a mesma sub-rede, isso será equivalente a executar o encaminhamento L3 sem NAT.

>[!NOTE]
>Esse processo não exige que você crie um objeto de balanceador de carga.  A atribuição do PublicIPAddress à interface de rede é suficiente para que o software Load Balancer execute sua configuração.


1. Crie um objeto IP público para conter o VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Atribua o PublicIPAddress a uma interface de rede.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Exemplo: Usar o Load Balancer de software para encaminhar o tráfego com um VIP alocado dinamicamente
Este exemplo repete a mesma ação do exemplo anterior, mas aloca automaticamente o VIP do pool de VIPs disponível no balanceador de carga em vez de especificar um endereço IP específico. 

1. Crie um objeto IP público para conter o VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Consulte o recurso PublicIPAddress para determinar qual endereço IP foi atribuído.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   A propriedade IpAddress contém o endereço atribuído.  A saída terá uma aparência semelhante a esta:
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
 
3. Atribua o PublicIPAddress a uma interface de rede.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Exemplo: Remover um endereço PublicIP que está sendo usado para encaminhar o tráfego e retorná-lo para o pool VIP
   Este exemplo remove o recurso PublicIPAddress que foi criado pelos exemplos anteriores.  Depois que o PublicIPAddress for removido, a referência ao PublicIPAddress será automaticamente removida do adaptador de rede, o tráfego deixará de ser encaminhado e o endereço IP será retornado para o pool de VIP público para reutilização.  

4. Remover o PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 