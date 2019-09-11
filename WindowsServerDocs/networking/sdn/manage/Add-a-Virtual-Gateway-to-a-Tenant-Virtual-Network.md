---
title: Adicionar um gateway virtual a uma rede virtual de locatário
description: Saiba como usar os cmdlets e scripts do Windows PowerShell para fornecer conectividade site a site para as redes virtuais do seu locatário.
manager: dougkim
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
ms.date: 08/23/2018
ms.openlocfilehash: 39199a96b1f3cd5a62e60f676e8ab47ad4acb4a8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869952"
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>Adicionar um gateway virtual a uma rede virtual de locatário 

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016 

Saiba como usar os cmdlets e scripts do Windows PowerShell para fornecer conectividade site a site para as redes virtuais do seu locatário. Neste tópico, você adiciona gateways virtuais de locatário a instâncias do gateway de RAS que são membros de pools de gateways, usando o controlador de rede. O gateway RAS dá suporte a até 100 locatários, dependendo da largura de banda usada por cada locatário. O controlador de rede determina automaticamente o melhor gateway de RAS a ser usado quando você implanta um novo gateway virtual para seus locatários.  

Cada gateway virtual corresponde a um locatário específico e consiste em uma ou mais conexões de rede (túneis de VPN site a site) e, opcionalmente, conexões Border Gateway Protocol (BGP). Quando você fornece conectividade site a site, seus clientes podem conectar sua rede virtual de locatário a uma rede externa, como uma rede corporativa de locatário, uma rede de provedor de serviços ou a Internet.

**Ao implantar um gateway virtual de locatário, você tem as seguintes opções de configuração:**  


|                                                        Opções de conexão de rede                                                         |                                              Opções de configuração de BGP                                               |
|-------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| <ul><li>VPN (rede virtual privada) site a site do IPSec</li><li>Encapsulamento de roteamento genérico (GRE)</li><li>Encaminhamento de camada 3</li></ul> | <ul><li>Configuração do roteador BGP</li><li>Configuração de par BGP</li><li>Configuração de políticas de roteamento BGP</li></ul> |

---

Os scripts e comandos de exemplo do Windows PowerShell neste tópico demonstram como implantar um gateway virtual de locatário em um gateway RAS com cada uma dessas opções.  


>[!IMPORTANT]  
>Antes de executar qualquer um dos comandos e scripts do Windows PowerShell de exemplo fornecidos, você deve alterar todos os valores de variáveis para que os valores sejam apropriados para sua implantação.  

1.  Verifique se o objeto do pool de gateway existe no controlador de rede. 

    ```PowerShell
    $uri = "https://ncrest.contoso.com"   

    # Retrieve the Gateway Pool configuration  
    $gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  

    # Display in JSON format  
    $gwPool | ConvertTo-Json -Depth 2   

    ```  

2.  Verifique se a sub-rede usada para rotear pacotes da rede virtual do locatário existe no controlador de rede. Você também recupera a sub-rede virtual usada para roteamento entre o gateway de locatário e a rede virtual.  

    ```PowerShell 
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

3.  Crie um novo objeto para o gateway virtual de locatário e, em seguida, atualize a referência de pool de gateway.  Você também especifica a sub-rede virtual usada para roteamento entre o gateway e a rede virtual.  Depois de especificar a sub-rede virtual, você atualiza o restante das propriedades do objeto de gateway virtual e, em seguida, adiciona o novo gateway virtual para o locatário.

    ```PowerShell  
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

4. Crie uma conexão VPN site a site com o encaminhamento de IPsec, GRE ou camada 3 (L3).  

   >[!TIP]
   >Opcionalmente, você pode combinar todas as etapas anteriores e configurar um gateway virtual de locatário com todas as três opções de conexão.  Para obter mais detalhes, consulte [configurar um gateway com todos os três tipos de conexão (IPSec, GRE, L3) e BGP](#optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp).

   **Conexão de rede site a site VPN IPsec**

   ```PowerShell  
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

   **Conexão de rede site a site de VPN GRE**

   ```PowerShell  
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

   **Conexão de rede de encaminhamento L3**<p>
   Para que uma conexão de rede de encaminhamento L3 funcione corretamente, você deve configurar uma rede lógica correspondente.   

   1. Configure uma rede lógica para a conexão de rede de encaminhamento L3.  <br>

      ```PowerShell  
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

   2. Crie um objeto JSON de conexão de rede e adicione-o ao controlador de rede.  

      ```PowerShell 
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

5. Configure o gateway como um roteador BGP e adicione-o ao controlador de rede. 

   1. Adicione um roteador BGP para o locatário.  

      ```PowerShell  
      # Create a new object for the Tenant BGP Router  
      $bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   

      # Update the BGP Router properties  
      $bgpRouterproperties.ExtAsNumber = "0.64512"   
      $bgpRouterproperties.RouterId = "192.168.0.2"   
      $bgpRouterproperties.RouterIP = @("192.168.0.2")   

      # Add the new BGP Router for the tenant  
      $bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   

      ```  

   2. Adicione um par de BGP para esse locatário, que corresponde à conexão de rede VPN site a site adicionada acima.  

      ```PowerShell
      # Create a new object for Tenant BGP Peer  
      $bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   

      # Update the BGP Peer properties  
      $bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
      $bgpPeerProperties.AsNumber = 64521   
      $bgpPeerProperties.ExtAsNumber = "0.64521"   

      # Add the new BGP Peer for tenant  
      New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   

      ```  

## <a name="optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp"></a>(Etapa opcional) Configurar um gateway com todos os três tipos de conexão (IPsec, GRE, L3) e BGP  
Opcionalmente, você pode combinar todas as etapas anteriores e configurar um gateway virtual de locatário com todas as três opções de conexão:   

```PowerShell  
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

## <a name="modify-a-gateway-for-a-virtual-network"></a>Modificar um gateway para uma rede virtual  


**Recuperar a configuração do componente e armazená-la em uma variável**

```PowerShell  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  

**Navegar pela estrutura de variável para alcançar a propriedade necessária e defini-la para o valor de atualizações**

```PowerShell  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  

**Adicionar a configuração modificada para substituir a configuração mais antiga no controlador de rede**

```PowerShell  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  


## <a name="remove-a-gateway-from-a-virtual-network"></a>Remover um gateway de uma rede virtual 
Você pode usar os seguintes comandos do Windows PowerShell para remover recursos de gateway individuais ou todo o gateway.  

**Remover uma conexão de rede**  
```PowerShell  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  

**Remover um par de BGP** 
```PowerShell  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  

**Remover um roteador BGP**
```PowerShell  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```

**Remover um gateway**  
```PowerShell  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  

---