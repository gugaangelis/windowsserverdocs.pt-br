---
title: Configurar o Software balanceador balanceamento de carga e de rede NAT (Address Translation)
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurar o Software balanceador balanceamento de carga e de rede NAT (Address Translation)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como usar o balanceador de carga do Software de rede definidos \(SDN\) software \(SLB\) para fornecer saída NAT network address translation NAT, entrada, ou balanceamento de carga entre várias instâncias de um aplicativo.

Este tópico contém as seções a seguir.

- [Visão geral de Balanceador de carga de software](#bkmk_slbover)
- [Exemplo: Criar um VIP público para um pool de duas VMs em uma rede virtual de balanceamento de carga](#bkmk_publicvip)
- [Exemplo: Use SLB para saída NAT](#bkmk_obnat)
- [Exemplo: Adicionar interfaces de rede ao pool de back-end](#bkmk_backend)
- [Exemplo: Use o Software balanceador pelo encaminhamento de tráfego](#bkmk_forward)

## <a name="bkmk_slbover"></a>Visão geral de Balanceador de carga de software

O \(SLB\) SDN Software balanceador oferece alto desempenho de rede e a disponibilidade para seus aplicativos. É uma camada 4 \ balanceador que distribui o tráfego entre as instâncias de serviço saudável em serviços de nuvem ou máquinas virtuais definidas em um conjunto de Balanceador de carga de carga (TCP, UDP\).

Você pode configurar SLB para fazer o seguinte.

* Carregar o saldo tráfego externo a uma rede virtual para máquinas virtuais \(VMs\). Isso é chamado de balanceamento de carga VIP pública.
* Carregar o saldo tráfego entre VMs em uma rede virtual, entre VMs nos serviços em nuvem ou entre computadores locais e VMs em uma rede virtual entre locais. 
* Encaminhe o tráfego de rede VM da rede virtual para destinos externos usando a conversão de endereços de rede (NAT).  Isso é chamado de saída NAT.
* Encaminhar tráfego externo a uma VM específica.  Isso é chamado de entrada NAT.

>[!IMPORTANT]
>Um problema conhecido impede que os objetos balanceador no módulo NetworkController Windows PowerShell está funcionando corretamente no Windows Server 2016 5. A solução alternativa é usar tabelas dinâmicas hash e Invoke-WebRequest em vez disso. Esse método é demonstrado nos exemplos a seguir.


## <a name="bkmk_publicvip"></a>Exemplo: Criar um VIP público para um pool de duas VMs em uma rede virtual de balanceamento de carga

Você pode usar este exemplo para criar um objeto de Balanceador de carga com um VIP público e duas VMs como membros de pool para atender às solicitações para o VIP.  Este exemplo de código também adiciona um teste de integridade HTTP para detectar se um dos membros do pool se torna não responde.

###<a name="step-1-prepare-the-load-balancer-object"></a>Etapa 1: Preparar o objeto de Balanceador de carga
Você pode usar o exemplo a seguir para preparar o objeto de Balanceador de carga.

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>Etapa 2: Atribuir um IP front-end
O front-end IP é conhecido como um IP Virtual (VIP).  O VIP deve ser extraído de um IP não utilizado em um Pool de IP que tenha recebido anteriormente para o Gerenciador de Balanceador de carga de rede lógica.

Você pode usar o exemplo a seguir para atribuir um endereço IP front-end.

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>Etapa 3: Alocar um pool de endereços de back-end
O pool de endereços de back-end contém IPs dinâmico (DIPs) que compõem os membros do conjunto de balanceamento de carga de VMs. Nesta etapa você alocar somente o pool; as configurações de IP são adicionadas em uma etapa posterior.

Você pode usar o exemplo a seguir para alocar um pool de endereços de back-end.
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>Etapa 4: Definir um teste de integridade
Testes de integridade são usados pelo Balanceador de carga para determinar o estado de integridade dos membros do pool de back-end. Com este exemplo, você define um teste HTTP que consultas para RequestPath de "/ health.htm".  A consulta é executada a cada 5 segundos, conforme especificado pela propriedade IntervalInSeconds.

O teste de integridade deve receber um código de resposta HTTP de 200 para 11 consecutivas consultas para o teste a serem consideradas IP back-end para ser comprovada. Se o IP de back-end não é adequado, o balanceador não enviará tráfego ao IP.

>[!Note]
>É importante que quaisquer listas de controle de acesso que você aplica ao back-end IP não bloquear o tráfego de ou para a primeira IP na sub-rede, porque é o ponto de origem para os testes.

Você pode usar o exemplo a seguir para definir um teste de integridade.
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>Etapa 5: Definir uma regra de balanceamento de carga
Essa regra de balanceamento de carga define como o tráfego chega ao quarto de IP front-end é a serem enviadas para o IP de back-end.  Neste exemplo, o tráfego TCP na porta 80 é enviado ao pool de back-end.

Você pode usar o exemplo a seguir para definir uma regra de balanceamento de carga.

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>Etapa 6: Adicionar a configuração de Balanceador de carga ao controlador de rede
Até agora neste exemplo, todos os objetos criados estão na memória da sessão do Windows PowerShell. Esta etapa adiciona os objetos ao controlador de rede.

Você pode usar o exemplo a seguir para adicionar a configuração de Balanceador de carga ao controlador de rede.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Após essa etapa, você precisará seguir o exemplo a seguir para adicionar as interfaces de rede para este pool de back-end.

## <a name="bkmk_obnat"></a>Exemplo: Use SLB para saída NAT

Você pode usar este exemplo configurar SLB com um pool de back-end para fornecer funcionalidade NAT saída para uma VM no espaço de endereço particular de uma rede virtual alcançar saída à internet.

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>Etapa 1: Crie as propriedades loadbalancer, front-end IP e Pool de back-end.
Você pode usar o exemplo a seguir para criar as propriedades loadbalancer, front-end IP e Pool de back-end.

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>Etapa 2: Definir a regra de saída de NAT
Você pode usar o exemplo a seguir para definir a regra de saída de NAT. 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>Etapa 3: Adicionar o objeto de Balanceador de carga no controlador de rede
Você pode usar o exemplo a seguir para adicionar o objeto de Balanceador de carga no controlador de rede.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Na próxima etapa, você pode adicionar as interfaces de rede à qual você deseja fornecer acesso à internet.

## <a name="bkmk_backend"></a>Exemplo: Adicionar interfaces de rede ao pool de back-end
Você pode usar este exemplo adicionar interfaces de rede ao pool de back-end.

Você deve repetir esta etapa para cada interface de rede que pode processar solicitações que são feitas para o VIP. Você também pode repetir esse processo em uma interface de rede para adicioná-lo a vários objetos de Balanceador de carga. Por exemplo, se você tiver um objeto de Balanceador de carga para um servidor Web VIP e um objeto de Balanceador de carga separada para fornecer saída NAT.
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>Etapa 1: Obter o objeto de Balanceador de carga que contém o pool de back-end para o qual você irá adicionar uma interface de rede
Você pode usar o exemplo a seguir para recuperar o objeto de Balanceador de carga.

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>Etapa 2: Obtenha a interface de rede e adicione o pool de backendaddress à matriz loadbalancerbackendaddresspools.
Você pode usar o exemplo a seguir para obter a interface de rede e adicione o pool de backendaddress à matriz loadbalancerbackendaddresspools.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>Etapa 3: Coloque a interface de rede para aplicar a alteração
Você pode usar o exemplo a seguir para colocar a interface de rede para aplicar a alteração.

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>Exemplo: Use o Software balanceador pelo encaminhamento de tráfego
Se você precisa para mapear um IP Virtual para uma interface de rede em uma rede virtual sem definir portas individuais, você pode criar uma regra de encaminhamento L3.  Essa regra encaminha todo o tráfego de e para a VM via VIP atribuído, que deve estar contido em um objeto PublicIPAddress.

Se o VIP e DIP são definidas como a mesma sub-rede, em seguida, isso equivale a executar L3 encaminhamento sem NAT.

>[!NOTE]
>Esse processo não exige que você crie um objeto de Balanceador de carga.  Atribuir o PublicIPAddress à interface de rede é informações suficientes para o balanceador de carga de Software executar sua configuração.

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>Etapa 1: Criar um objeto IP público para conter o VIP
Você pode usar o exemplo a seguir para criar um objeto IP público.

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>Etapa 2: Atribuir o PublicIPAddress para uma interface de rede
Você pode usar o exemplo a seguir para atribuir o PublicIPAddress a uma interface de rede.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 