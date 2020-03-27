---
title: Usar a Política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d4e005e65a3ff645ed91f488820435aff5173390
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317896"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usar a Política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS para balancear a carga de um aplicativo com reconhecimento de localização geográfica.

O tópico anterior neste guia, [usar política de DNS para balanceamento de carga de aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), usa um exemplo de uma empresa fictícia-serviços de presente da Contoso, que fornece serviços de oferta online e que tem um site chamado contosogiftservices.com. Os serviços de presentes da Contoso balanceam seu aplicativo Web online entre os servidores nos data centers da América do Norte localizados em Seattle, WA, Chicago, IL e Dallas, TX.

>[!NOTE]
>É recomendável que você se familiarize com o tópico [usar a política DNS para balanceamento de carga do aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) antes de executar as instruções neste cenário.

Este tópico usa a mesma infraestrutura de rede e empresa fictícia como base para uma nova implantação de exemplo que inclui reconhecimento de localização geográfica.

Neste exemplo, os serviços de presentes da Contoso estão expandindo com êxito sua presença em todo o mundo.

Semelhante à América do Norte, a empresa agora tem servidores Web hospedados em data centers europeus.

Os administradores de DNS dos serviços de presentes da Contoso desejam configurar o balanceamento de carga de aplicativos para data centers europeus de maneira semelhante à implementação da política DNS na Estados Unidos, com o tráfego de aplicativos distribuído entre os servidores Web que estão localizados no Dublin, Irlanda, Amsterdã, Holanda e em outro lugar.

Os administradores de DNS também desejam que todas as consultas de outros locais no mundo sejam distribuídas igualmente entre todos os seus data centers.

Nas próximas seções, você pode aprender a atingir metas semelhantes às dos administradores de DNS da Contoso em sua própria rede.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Como configurar o balanceamento de carga de aplicativo com reconhecimento de localização geográfica

As seções a seguir mostram como configurar a política DNS para balanceamento de carga de aplicativo com reconhecimento de localização geográfica.

>[!IMPORTANT]
>As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_clientsubnets"></a>Criar as sub-redes do cliente DNS

Você deve primeiro identificar as sub-redes ou o espaço de endereço IP das regiões América do Norte e Europa.

Você pode obter essas informações de mapas de IP geográfico. Com base nessas distribuições de IP geográfico, você deve criar as sub-redes de cliente DNS.

Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas a um servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Para obter mais informações, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes2"></a>Criar os escopos de zona

Depois que as sub-redes do cliente estiverem em vigor, você deverá particionar a zona contosogiftservices.com em escopos de zona diferentes, cada uma para um datacenter.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo.

O cenário anterior sobre balanceamento de carga de aplicativo demonstra como configurar três escopos de zona para datacenters em América do Norte.

Com os comandos a seguir, você pode criar mais dois escopos de zona, um para os data centers Dublin e Amsterdã. 

Você pode adicionar esses escopos de zona sem qualquer alteração nos três escopos de zona de América do Norte existentes na mesma zona. Além disso, depois de criar esses escopos de zona, você não precisa reiniciar o servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Para obter mais informações, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records2"></a>Adicionar registros aos escopos de zona

Agora você deve adicionar os registros que representam o host do servidor Web nos escopos de zona.

Os registros dos data centers da América foram adicionados no cenário anterior. Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros aos escopos de zona para data centers europeus.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a><a name="bkmk_policies2"></a>Criar as políticas de DNS

Depois de criar as partições (escopos de zona) e adicionar registros, você deve criar políticas de DNS que distribuem as consultas de entrada entre esses escopos.

Para este exemplo, a distribuição de consultas entre servidores de aplicativos em data centers diferentes atende aos critérios a seguir.

1. Quando a consulta DNS é recebida de uma origem em uma sub-rede do cliente norte-americano, 50% das respostas DNS apontam para o data center de Seattle, 25% das respostas apontam para o datacenter de Chicago e os 25% de respostas restantes apontam para o datacenter de Dallas.
2. Quando a consulta DNS é recebida de uma origem em uma sub-rede do cliente Europeu, 50% das respostas DNS apontam para o datacenter Dublin e 50% das respostas DNS apontam para o datacenter Amsterdã.
3. Quando a consulta é proveniente de qualquer outro lugar do mundo, as respostas DNS são distribuídas entre todos os cinco data centers.

Você pode usar os seguintes comandos do Windows PowerShell para implementar essas políticas de DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora você criou com êxito uma política de DNS que fornece balanceamento de carga de aplicativo entre servidores Web localizados em cinco data centers diferentes em vários continentes.

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.
