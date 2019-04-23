---
title: Desempenho do Gateway do Windows Server de 2019
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845627"
---
# <a name="windows-server-2019-gateway-performance"></a>Desempenho do Gateway do Windows Server de 2019

>Aplica-se a: Windows Server


No Windows Server 2016, uma das preocupações cliente era a incapacidade de atender aos requisitos de taxa de transferência de redes modernos do gateway SDN. A taxa de transferência de rede dos túneis de IPsec e GRE tinha limitações com a taxa de transferência de conexão única para conectividade IPsec sendo aproximadamente 300 Mbps em conectividade GRE sendo aproximadamente 2,5 Gbps.

Aprimoramos significativamente no Windows Server de 2019, com os números de planagem 1,8 Gbps e 15 Gbps para conexões IPsec e GRE, respectivamente. Tudo isso, com uma reduções significativas em ciclos da CPU / por byte, fornecendo assim a taxa de transferência de alto desempenho com muito menor utilização da CPU.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Habilitar o alto desempenho com os gateways no Windows Server 2019

Para **conexões GRE**, quando você implantação/atualização para compilações de 2019 do Windows Server nas VMs de gateway, você deverá ver automaticamente o melhor desempenho. Nenhuma etapa manual está envolvida.

Para **conexões IPsec**, por padrão, quando você cria a conexão para suas redes virtuais, você pode obter os números de desempenho e o caminho de dados do Windows Server 2016. Para habilitar o caminho de dados do Windows Server 2019, faça o seguinte:

   1. Em um gateway SDN VM, vá para **Services** console (Services. msc).
   2. Localize o serviço nomeado **serviço de Gateway do Azure**e defina o tipo de inicialização **automático**.
   3. Reinicie a VM do gateway.
      As conexões ativas nesse failover de gateway para um gateway com redundância de VM.
   4. Repita as etapas anteriores para o restante das VMs de gateway.

>[!TIP]
>Para obter melhores resultados de desempenho, verifique se o cipherTransformationConstant e authenticationTransformConstant nas configurações do modo rápido dos usos de conexão IPsec a **GCMAES256** conjunto de codificação.
>
>Máximo desempenho, o hardware do gateway host deve dar suporte a AES-NI e conjuntos de instruções de CPU PCLMULQDQ. Eles estão disponíveis em qualquer Westmere (32nm) e posterior CPU Intel, exceto em modelos em que o AES-NI foi desabilitada. Você pode examinar a documentação do fornecedor de hardware para ver se a CPU dá suporte a AES-NI e CPU PCLMULQDQ instrução define.

Abaixo está um exemplo REST de conexão IPsec com algoritmos de segurança ideal:

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>Os resultados de testes

Fizemos testes para os gateways SDN em nossos laboratórios de teste de desempenho abrangente. Nos testes, comparamos desempenho de rede do gateway com Windows Server 2019 em cenários de não SDN e cenários SDN. Você pode encontrar os resultados e detalhes de instalação capturadas no artigo de blog de teste [aqui](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---