---
title: Usar a política DNS para aplicativo balanceamento de carga com reconhecimento de localização geográfica
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usar a política DNS para aplicativo balanceamento de carga com reconhecimento de localização geográfica

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar a política DNS para carregar o saldo de um aplicativo com reconhecimento de localização geográfica.

O tópico anterior neste guia, [usar a política de DNS para balanceamento de carga do aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), usa um exemplo de uma empresa fictícia - Contoso presente serviços - que fornece doar online services e que tem um site chamado contosogiftservices.com. Balanceamento de carga de serviços de presente Contoso seus aplicativos Web online entre servidores na América do Norte datacenters localizados em Seattle, WA, Chicago, Illinois, EUA, and Dallas, Texas.

>[!NOTE]
>É recomendável que você se familiarize com o tópico [usar a política de DNS para balanceamento de carga do aplicativo](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) antes de executar as instruções neste cenário.

Este tópico usa o mesmo empresa fictícia e infraestrutura de rede como base para uma nova implantação de exemplo que inclui o reconhecimento de localização geográfica.

Neste exemplo, Contoso presente serviços com êxito é expandindo sua presença em todo o mundo.

Assim como na América do Norte, a empresa agora tem servidores web hospedados em datacenters europeus.

Os administradores da Contoso presente serviços DNS deseja configurar aplicativo balanceamento de carga para datacenters europeus de maneira semelhante para a implementação de política DNS nos Estados Unidos, com tráfego de aplicativo distribuído entre servidores Web que estão localizados em Dublin, Irlanda, Amsterdã, Holanda e em outros lugares.

Os administradores de DNS também querem todas as consultas de outros locais no mundo distribuído igualmente entre todos os seus centros de dados.

Nas próximas seções você pode aprender a atingir objetivos semelhantes dos administradores de DNS de Contoso em sua própria rede.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Como configurar o aplicativo balanceamento de carga com reconhecimento de localização geográfica

As seções a seguir mostram como configurar a política DNS aplicativo balanceamento de carga com reconhecimento de localização geográfica.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos.

###<a name="bkmk_clientsubnets"></a>Criar as sub-redes do cliente DNS

Primeiro, você deve identificar as sub-redes ou espaço de endereço IP das regiões da América do Norte e Europa.

Você pode obter essas informações de mapas de IP de localização geográfica. De acordo com essas distribuições de IP de localização geográfica, você deve criar as sub-redes do cliente DNS.

Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 da qual as consultas são enviadas para um servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes do cliente DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Para obter mais informações, consulte [adicionar DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx).

###<a name="bkmk_zscopes2"></a>Criar os escopos de zona

Depois que as sub-redes clientes estão no lugar, você deve particionar o contosogiftservices.com zona em escopos de zona diferentes, cada um de um data center.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.

O cenário anterior em balanceamento de carga do aplicativo demonstra como configurar três escopos de zona de data centers na América do Norte.

Com os comandos a seguir, você pode criar dois escopos zona mais, um para os datacenters Dublin e Amsterdã. 

Você pode adicionar estes escopos zona sem qualquer alteração três escopos de zona existente na América do Norte na mesma região. Além disso, depois de criar estes escopos zona, você não precisa reiniciar o servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>Adicionar registros os escopos de zona

Agora você deve adicionar os registros que representa o host do servidor da web para os escopos de zona.

Os registros para os datacenters América foram adicionados no cenário anterior. Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros para os escopos de datacenters europeus zona.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies2"></a>Criar as políticas DNS

Depois que você criou as partições (escopos zona) e você adicionou registros, você deve criar políticas DNS que distribuir as consultas recebidas por estes escopos.

Neste exemplo, a distribuição de consulta em todos os servidores de aplicativos em diferentes datacenters atenda aos seguintes critérios.

1. Quando a consulta DNS é recebida de uma fonte em uma sub-rede de cliente da América do Norte, 50% das respostas DNS apontar para o Centro de dados de Seattle, 25% das respostas aponte para o datacenter Chicago e o restante 25% das respostas aponte para o datacenter Dallas.
2. Quando a consulta DNS é recebida de uma fonte em uma sub-rede cliente Europeia, 50% das respostas DNS aponte para o datacenter Dublin e 50% das respostas DNS aponte para o datacenter Amsterdã.
3. Quando a consulta vem de outro lugar do mundo, as respostas DNS são distribuídas por todos os cinco datacenters.

Você pode usar os seguintes comandos do Windows PowerShell para implementar essas diretivas DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Agora foi criado com êxito uma política DNS que fornece o aplicativo balanceamento de carga entre servidores Web que estão localizados em cinco datacenters diferentes em vários continentes.

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.
