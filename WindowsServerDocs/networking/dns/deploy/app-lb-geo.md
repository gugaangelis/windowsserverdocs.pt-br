---
title: Usar a Política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 806c0cdeedb44db44fc0ec5218124f516a6f70e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852547"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usar a Política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS para um aplicativo de balanceamento de carga com reconhecimento de localização geográfica.

O tópico anterior neste guia [usar a política de DNS para balanceamento de carga do aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), usa um exemplo de uma empresa fictícia - serviços de brinde do Contoso - que fornece o universo on-line de serviços e que tem um site chamado contosogiftservices.com. O balanceamento de carga de serviços do Contoso presente seu aplicativo Web online entre os servidores em data centers na América do Norte localizado em Seattle, WA, Chicago, IL e Dallas, Texas.

>[!NOTE]
>É recomendável que você se familiarizar com o tópico [usar a política de DNS para balanceamento de carga do aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) antes de executar as instruções nesse cenário.

Este tópico usa a infraestrutura de rede e a empresa fictícia mesma como base para uma nova implantação de exemplo que inclui reconhecimento de localização geográfica.

Neste exemplo, Contoso presente serviços com êxito está expandindo sua presença em todo o mundo.

Semelhante da América do Norte, a empresa agora tem servidores web hospedados em data centers europeus.

Os administradores de DNS do Contoso presente serviços deseja configurar o aplicativo de balanceamento de carga para data centers europeus de maneira semelhante para a implementação de política DNS nos Estados Unidos, com o tráfego de aplicativo distribuído entre os servidores Web que estão localizados em Dublin, Irlanda, Amsterdã, Holanda e em outro lugar.

Os administradores de DNS também deseja que todas as consultas de outros locais no mundo distribuído igualmente entre todos os seus data centers.

Nas próximas seções, você pode aprender como atingir as metas semelhantes dos administradores do DNS do Contoso na sua própria rede.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Como configurar o aplicativo balanceamento de carga com reconhecimento de localização geográfica

As seções a seguir mostram como configurar a política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos.

###<a name="bkmk_clientsubnets"></a>Crie as sub-redes de cliente DNS

Primeiro, você deve identificar a sub-redes ou espaço de endereço IP das regiões América do Norte e Europa.

Você pode obter essas informações de mapas de Geo-IP. Com base nessas distribuições IP geográfico, você deve criar as sub-redes de cliente DNS.

Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas para um servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Para obter mais informações, consulte [DnsServerClientSubnet adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

###<a name="bkmk_zscopes2"></a>Criar os escopos de zona

Depois que as sub-redes de cliente estão em vigor, você deve particionar a zona contosogiftservices.com em escopos de zona diferente, cada um para um data center.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.

O cenário anterior sobre o balanceamento de carga do aplicativo demonstra como configurar três escopos de zona para data centers na América do Norte.

Com os comandos a seguir, você pode criar dois escopos mais de zona, cada uma para os datacenters Dublin e Amsterdã. 

Você pode adicionar esses escopos de zona sem quaisquer alterações para os três escopos de zona da América do Norte existentes na mesma região. Além disso, depois de criar esses escopos de zona, você não precisa reiniciar o servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

###<a name="bkmk_records2"></a>Adicionar registros para os escopos de zona

Agora você deve adicionar os registros que representa o host do servidor web para os escopos de zona.

Os registros para os datacenters América foram adicionados no cenário anterior. Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros para os escopos de zona para data centers europeus.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

###<a name="bkmk_policies2"></a>Crie as políticas de DNS

Depois que você criou as partições (escopos de zona) e adicionar registros, você deve criar políticas DNS que distribui as consultas de entrada nesses escopos.

Neste exemplo, distribuição de consultas em servidores de aplicativos em diferentes data centers atende aos critérios a seguir.

1. Quando a consulta DNS é proveniente de uma fonte em uma sub-rede de cliente na América do Norte, 50% das respostas DNS apontar para o Centro de dados de Seattle, 25% das respostas aponte para o data center de Chicago e 25% restantes de respostas de apontar para o datacenter de Dallas.
2. Quando a consulta DNS é proveniente de uma fonte em uma sub-rede de cliente europeu, 50% das respostas DNS apontar para o datacenter em Dublin e 50% das respostas DNS apontar para o datacenter Amsterdã.
3. Quando a consulta derivar de qualquer outro lugar no mundo, as respostas DNS são distribuídas entre todos os datacenters de cinco.

Você pode usar os seguintes comandos do Windows PowerShell para implementar essas políticas DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Você agora criou com êxito uma política DNS que fornece o aplicativo balanceamento de carga entre servidores Web que estão localizados em diferentes data cinco centers em vários continentes.

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.
