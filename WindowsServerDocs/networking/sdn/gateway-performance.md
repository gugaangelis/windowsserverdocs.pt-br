---
title: Desempenho do gateway do Windows Server 2019
manager: grcusanz
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: d7ca57b9cb1013d1e6c1081bdf7c5c50fa6a918d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969533"
---
# <a name="windows-server-2019-gateway-performance"></a>Desempenho do gateway do Windows Server 2019

>Aplica-se a: Windows Server


No Windows Server 2016, uma das preocupações dos clientes foi a incapacidade do gateway de SDN de atender aos requisitos de produtividade das redes modernas. A taxa de transferência de rede de túneis IPsec e GRE teve limitações com a taxa de transferência de conexão única para a conectividade IPsec de cerca de 300 Mbps e para a conectividade do GRE sendo cerca de 2,5 Gbps.

Melhoramos significativamente no Windows Server 2019, com os números de soaring a 1,8 Gbps e 15 Gbps para conexões IPsec e GRE, respectivamente. Tudo isso, com reduções significativas nos ciclos de CPU/por byte, fornecendo, assim, uma taxa de transferência de alto desempenho com muito menos utilização da CPU.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Habilitar alto desempenho com gateways no Windows Server 2019

Para **conexões de GRE**, depois que você implantar/atualizar para o Windows Server 2019 se basear nas VMs de gateway, você deverá ver automaticamente o desempenho aprimorado. Nenhuma etapa manual está envolvida.

Para **conexões IPSec**, por padrão, ao criar a conexão para suas redes virtuais, você obtém o caminho de dados do Windows Server 2016 e os números de desempenho. Para habilitar o caminho de dados do Windows Server 2019, faça o seguinte:

   1. Em uma VM de gateway de SDN, vá para console de **Serviços** (Services. msc).
   2. Localize o serviço chamado **serviço de gateway do Azure**e defina o tipo de inicialização como **automático**.
   3. Reinicie a VM do gateway.
      As conexões ativas neste failover de gateway para uma VM de gateway redundante.
   4. Repita as etapas anteriores para o restante das VMs de gateway.

>[!TIP]
>Para obter os melhores resultados de desempenho, verifique se cipherTransformationConstant e authenticationTransformConstant nas configurações do modo rápido da conexão IPsec usam o **GCMAES256** Cipher Suite.
>
>Para obter o máximo de desempenho, o hardware do host do gateway deve dar suporte a conjuntos de instruções de CPU AES-NI e PCLMULQDQ. Eles estão disponíveis em qualquer Westmere (32nm) e na CPU da Intel posterior, exceto nos modelos em que o AES-NI foi desabilitado. Você pode examinar a documentação do fornecedor de hardware para ver se a CPU dá suporte a conjuntos de instruções de CPU AES-NI e PCLMULQDQ.

Abaixo está um exemplo REST de conexão IPsec com algoritmos de segurança ideais:

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

## <a name="testing-results"></a>Testando resultados

Fizemos testes de desempenho extensivos para os gateways de SDN em nossos laboratórios de teste. Nos testes, comparamos o desempenho de rede do gateway com o Windows Server 2019 em cenários de SDN e cenários que não são SDN. Você pode encontrar os resultados e os detalhes de configuração de teste capturados no artigo do blog [aqui](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---