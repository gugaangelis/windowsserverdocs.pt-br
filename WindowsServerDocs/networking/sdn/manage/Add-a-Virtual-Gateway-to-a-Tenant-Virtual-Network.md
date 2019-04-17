---
title: Adicionar um Gateway Virtual para uma rede Virtual locatário
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 351cafd1466c4b3c20e907ea221c9f58129b3443
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>Adicionar um Gateway Virtual para uma rede Virtual locatário

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar locatário Gateways virtuais, usando os cmdlets do Windows PowerShell e scripts, para fornecer redes virtuais seu locatários com conectividade to-site para os sites da organização e à Internet.   
  
Gateway RAS dá suporte a até cem locatários, dependendo da largura de banda usada por cada locatário. Você usar o controlador de rede para adicionar locatário Gateways Virtual às instâncias do Gateway RAS que são membros de grupos de gateway. Controlador de rede determina automaticamente o Gateway RAS recomendadas para usar quando você implanta um novo Gateway Virtual para seu locatários.  
  
Cada Gateway Virtual corresponde a um locatário específico e consiste em uma ou mais conexões de rede (-to-site VPN túneis) e, opcionalmente, conexões de Border Gateway Protocol (BGP). Isso permite que os clientes a conectar seu locatário rede Virtual a uma rede externa, como uma rede corporativa de locatário, uma rede de provedor de serviço ou a internet.  
  
Quando você implanta um Gateway Virtual de locatário, você tem as seguintes opções de configuração:  
  
**Opções de conexão de rede**  
- IPSec-to-site rede virtual privada (VPN)   
- Encapsulamento de roteamento genérico (GRE)   
- Encaminhamento de camada 3  
  
**Opções de configuração de BGP**  
- Configuração do roteador BGP  
- Configuração de par BGP  
- Configuração de políticas de roteamento de BGP  
  
Os scripts de exemplo do Windows PowerShell e comandos neste tópico demonstram como implantar um gateway de locatário virtual em um Gateway RAS com cada uma dessas opções.  
  
Este tópico contém as seções a seguir.  
  
- [Adicione um gateway virtual para um locatário](#bkmk_addgwy)  
- [Adicionar uma Conexão de rede VPN-to-site para um locatário (IPsec, GRE ou L3)](#bkmk_s2s1)  
- [Configurar o gateway como um roteador BGP](#bkmk_bgp1)  
- [Configurar um gateway com todos os tipos de conexão três (IPsec, GRE, L3) e BGP](#bkmk_all3)  
- [Modificar ou remover um gateway para uma rede Virtual](#bkmk_modify)  
  
   
>[!IMPORTANT]  
>Antes de executar qualquer um dos comandos do Windows PowerShell de exemplo e scripts que são fornecidos neste tópico, você deve alterar todos os valores de variáveis para que os valores são apropriados para a implantação.  
  
## <a name="bkmk_addgwy"></a>Adicione um gateway virtual para um locatário  
  
Etapa 1: Verifique se o objeto de Pool de Gateway existe no controlador de rede.  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Gateway Pool configuration  
$gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  
  
# Display in JSON format  
$gwPool | ConvertTo-Json -Depth 2   
  
  
```  
Etapa 2: Verificar a existência de sub-rede a ser usado para roteamento de pacotes de rede Virtual do locatário no controlador de rede; e recupere a sub-rede virtual que deve ser usado para roteamento entre o gateway de locatário e rede virtual.  
  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Tenant Virtual Network configuration  
$Vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri  -ResourceId "Contoso_Vnet1"   
  
# Display in JSON format  
$Vnet | ConvertTo-Json -Depth 4   
  
# Retrieve the Tenant Virtual Subnet configuration  
$RoutingSubnet = Get-NetworkControllerVirtualSubnet -ConnectionUri $uri  -ResourceId "Contoso_WebTier" -VirtualNetworkID $vnet.ResourceId   
  
# Display in JSON format  
$RoutingSubnet | ConvertTo-Json -Depth 4   
  
```  
Etapa 3: Criar um objeto JSON de gateway virtual e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for Tenant Virtual Gateway  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties   
  
# Update Gateway Pool reference  
$VirtualGWProperties.GatewayPools = @()   
$VirtualGWProperties.GatewayPools += $gwPool   
  
# Specify the Virtual Subnet that is to be used for routing between the gateway and Virtual Network   
$VirtualGWProperties.GatewaySubnets = @()   
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet   
  
# Update the rest of the Virtual Gateway object properties  
$VirtualGWProperties.RoutingType = "Dynamic"   
$VirtualGWProperties.NetworkConnections = @()   
$VirtualGWProperties.BgpRouters = @()   
  
# Add the new Virtual Gateway for tenant   
$virtualGW = New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force   
  
```  
  
## <a name="bkmk_s2s1"></a>Adicionar uma Conexão de rede VPN-to-site para um locatário (IPsec, GRE ou L3)  
  
Você pode criar uma conexão de VPN-to-site com IPsec, GRE ou Layer 3 (L3) usando os exemplos a seguir para cada tipo de gateway de encaminhamento.  
  
### <a name="ipsec-vpn-site-to-site-network-connection"></a>Conexão de rede-to-site VPN IPsec  
  
Crie um objeto de JSON de Conexão de rede e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "P@ssw0rd"   
  
$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   
  
$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   
  
# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.1.10.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.121"   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force   
  
  
```  
### <a name="gre-vpn-site-to-site-network-connection"></a>Conexão de rede-to-site GRE VPN  
  
Crie um objeto de JSON de Conexão de rede e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "GRE"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
$nwConnectionProperties.GreConfiguration.GreKey = 1234   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.122"   
  
# L3 specific configuration (leave blank for GRE)  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_GreGW" -Properties $nwConnectionProperties -Force   
  
```  
### <a name="l3-forwarding-network-connection"></a>L3 Conexão de rede de encaminhamento  
  
Para configurar uma Conexão de rede de encaminhamento L3, você também deve configurar uma rede de lógica correspondente.   
  
Etapa 1: Configure uma rede lógica para o L3 encaminhando a Conexão de rede.  
  
```  
# Create a new object for the Logical Network to be used for L3 Forwarding  
$lnProperties = New-Object Microsoft.Windows.NetworkController.LogicalNetworkProperties  
  
$lnProperties.NetworkVirtualizationEnabled = $false  
$lnProperties.Subnets = @()  
  
# Create a new object for the Logical Subnet to be used for L3 Forwarding and update properties  
$logicalsubnet = New-Object Microsoft.Windows.NetworkController.LogicalSubnet  
$logicalsubnet.ResourceId = "Contoso_L3_Subnet"  
$logicalsubnet.Properties = New-Object Microsoft.Windows.NetworkController.LogicalSubnetProperties  
$logicalsubnet.Properties.VlanID = 1001  
$logicalsubnet.Properties.AddressPrefix = "10.127.134.0/25"  
$logicalsubnet.Properties.DefaultGateways = "10.127.134.1"  
  
$lnProperties.Subnets += $logicalsubnet  
  
# Add the new Logical Network to Network Controller  
$vlanNetwork = New-NetworkControllerLogicalNetwork -ConnectionUri $uri -ResourceId "Contoso_L3_Network" -Properties $lnProperties -Force  
  
```  
Etapa 2: Criar um objeto de JSON de Conexão de rede e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "L3"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# GRE specific configuration (leave blank for L3)  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]   
  
$nwConnectionProperties.IPAddresses = @()   
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress   
$localIPAddress.IPAddress = "10.127.134.55"   
$localIPAddress.PrefixLength = 25   
$nwConnectionProperties.IPAddresses += $localIPAddress   
  
$nwConnectionProperties.PeerIPAddresses = @("10.127.134.65")   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_L3GW" -Properties $nwConnectionProperties -Force   
  
```  
  
## <a name="bkmk_bgp1"></a>Configurar o gateway como um roteador BGP  
  
Você pode usar os scripts de exemplo a seguir para configurar o gateway como um roteador Border Gateway Protocol (BGP).  
  
### <a name="add-a-bgp-router-for-the-tenant"></a>Adicionar um roteador BGP do locatário  
  
Crie um objeto de JSON BGP roteador e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for the Tenant BGP Router  
$bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   
  
# Update the BGP Router properties  
$bgpRouterproperties.ExtAsNumber = "0.64512"   
$bgpRouterproperties.RouterId = "192.168.0.2"   
$bgpRouterproperties.RouterIP = @("192.168.0.2")   
  
# Add the new BGP Router for the tenant  
$bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   
   
  
```  
### <a name="add-a-bgp-peer-for-this-tenant-corresponding-to-the-site-to-site-vpn-network-connection-added-above"></a>Adicione um par de BGP para esse locatário correspondente para a Conexão de rede de VPN-to-site adicionou acima  
  
Crie um objeto de JSON BGP par e adicioná-lo ao controlador de rede.  
  
```  
# Create a new object for Tenant BGP Peer  
$bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   
  
# Update the BGP Peer properties  
$bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
$bgpPeerProperties.AsNumber = 64521   
$bgpPeerProperties.ExtAsNumber = "0.64521"   
  
# Add the new BGP Peer for tenant  
New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   
  
```  
## <a name="bkmk_all3"></a>Configurar um gateway com todos os tipos de conexão três (IPsec, GRE, L3) e BGP  
Opcionalmente, você pode combinar todas as etapas anteriores e configurar um gateway virtual locatário com todas as opções de conexão três:   
  
```  
# Create a new Virtual Gateway Properties type object  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties  
  
# Update GatewayPool reference  
$VirtualGWProperties.GatewayPools = @()  
$VirtualGWProperties.GatewayPools += $gwPool  
  
# Specify the Virtual Subnet that is to be used for routing between GW and VNET  
$VirtualGWProperties.GatewaySubnets = @()  
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet  
  
# Update some basic properties  
$VirtualGWProperties.RoutingType = "Dynamic"  
  
# Update Network Connection object(s)  
$VirtualGWProperties.NetworkConnections = @()  
  
# IPSec Connection configuration  
$ipSecConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$ipSecConnection.ResourceId = "Contoso_IPSecGW"  
$ipSecConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$ipSecConnection.Properties.ConnectionType = "IPSec"  
$ipSecConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$ipSecConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$ipSecConnection.Properties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration  
  
$ipSecConnection.Properties.IpSecConfiguration.AuthenticationMethod = "PSK"  
$ipSecConnection.Properties.IpSecConfiguration.SharedSecret = "P@ssw0rd"  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IPAddresses = @()  
$ipSecConnection.Properties.PeerIPAddresses = @()  
  
$ipSecConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.1.10.1/32"  
$ipv4Route.metric = 10  
$ipSecConnection.Properties.Routes += $ipv4Route  
  
$ipSecConnection.Properties.DestinationIPAddress = "10.127.134.121"  
  
# GRE Connection configuration  
$greConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$greConnection.ResourceId = "Contoso_GreGW"  
  
$greConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$greConnection.Properties.ConnectionType = "GRE"  
$greConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$greConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$greConnection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$greConnection.Properties.GreConfiguration.GreKey = 1234  
  
$greConnection.Properties.IPAddresses = @()  
$greConnection.Properties.PeerIPAddresses = @()  
  
$greConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$greConnection.Properties.Routes += $ipv4Route  
  
$greConnection.Properties.DestinationIPAddress = "10.127.134.122"  
  
$greConnection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
  
# L3 Forwarding connection configuration  
$l3Connection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$l3Connection.ResourceId = "Contoso_L3GW"  
  
$l3Connection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$l3Connection.Properties.ConnectionType = "L3"  
$l3Connection.Properties.OutboundKiloBitsPerSecond = 10000  
$l3Connection.Properties.InboundKiloBitsPerSecond = 10000  
  
$l3Connection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$l3Connection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
$l3Connection.Properties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]  
  
$l3Connection.Properties.IPAddresses = @()  
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress  
$localIPAddress.IPAddress = "10.127.134.55"  
$localIPAddress.PrefixLength = 25  
$l3Connection.Properties.IPAddresses += $localIPAddress  
  
$l3Connection.Properties.PeerIPAddresses = @("10.127.134.65")  
  
$l3Connection.Properties.Routes = @()  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$l3Connection.Properties.Routes += $ipv4Route  
  
# Update BGP Router Object  
$VirtualGWProperties.BgpRouters = @()  
  
$bgpRouter = New-Object Microsoft.Windows.NetworkController.VGwBgpRouter  
$bgpRouter.ResourceId = "Contoso_BgpRouter1"  
$bgpRouter.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties  
  
$bgpRouter.Properties.ExtAsNumber = "0.64512"  
$bgpRouter.Properties.RouterId = "192.168.0.2"  
$bgpRouter.Properties.RouterIP = @("192.168.0.2")  
  
$bgpRouter.Properties.BgpPeers = @()  
  
# Create BGP Peer Object(s)  
# BGP Peer for IPSec Connection  
$bgpPeer_IPSec = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_IPSec.ResourceId = "Contoso_IPSec_Peer"  
  
$bgpPeer_IPSec.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_IPSec.Properties.PeerIpAddress = "14.1.10.1"  
$bgpPeer_IPSec.Properties.AsNumber = 64521  
$bgpPeer_IPSec.Properties.ExtAsNumber = "0.64521"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_IPSec  
  
# BGP Peer for GRE Connection  
$bgpPeer_Gre = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_Gre.ResourceId = "Contoso_Gre_Peer"  
  
$bgpPeer_Gre.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_Gre.Properties.PeerIpAddress = "14.2.20.1"  
$bgpPeer_Gre.Properties.AsNumber = 64522  
$bgpPeer_Gre.Properties.ExtAsNumber = "0.64522"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_Gre  
  
# BGP Peer for L3 Connection  
$bgpPeer_L3 = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_L3.ResourceId = "Contoso_L3_Peer"  
  
$bgpPeer_L3.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_L3.Properties.PeerIpAddress = "14.3.30.1"  
$bgpPeer_L3.Properties.AsNumber = 64523  
$bgpPeer_L3.Properties.ExtAsNumber = "0.64523"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_L3  
  
$VirtualGWProperties.BgpRouters += $bgpRouter  
  
# Finally Add the new Virtual Gateway for tenant  
New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force  
  
```  
## <a name="bkmk_modify"></a>Modificar ou remover um gateway para uma rede Virtual  
  
Você pode usar os scripts de exemplo a seguir para modificar ou remover um gateway existente.  
  
### <a name="modify-the-configuration-of-an-existing-gateway"></a>Modificar a configuração de um gateway existente  
Você pode usar os seguintes comandos para modificar um gateway existente.  
  
Etapa 1: Recuperar a configuração do componente e armazená-la em uma variável  
  
```  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  
Etapa 2: Navegar pela estrutura de variável para acessar a propriedade necessária e configurá-lo para o valor de atualizações  
  
```  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  
Etapa 3: Adicionar a configuração modificada para substituir a configuração mais antiga no controlador de rede  
  
```  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  
### <a name="remove-a-gateway"></a>Remover um gateway  
Você pode usar os seguintes comandos do Windows PowerShell para remover recursos do gateway individuais ou todo o gateway.  
  
#### <a name="remove-a-network-connection"></a>Remover uma conexão de rede  
```  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  
#### <a name="remove-a-bgp-peer"></a>Remover um par BGP  
```  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  
#### <a name="remove-a-bgp-router"></a>Remover um roteador BGP  
```  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```  
#### <a name="remove-a-gateway"></a>Remover um gateway  
```  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  
  
  


