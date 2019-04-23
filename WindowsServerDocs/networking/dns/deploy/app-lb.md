---
title: Usar a Política de DNS para balanceamento de carga de aplicativo
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bb3e6695a7ec8fc7d950873403df023b4def3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881607"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usar a Política de DNS para balanceamento de carga de aplicativo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política de DNS para realizar o balanceamento de carga do aplicativo.

As versões anteriores do Windows Server DNS forneciam apenas por meio de respostas de round robin; de balanceamento de carga mas com o DNS no Windows Server 2016, você pode configurar a política de DNS para balanceamento de carga do aplicativo.

Quando você tiver implantado várias instâncias de um aplicativo, você pode usar a política de DNS para balancear a carga de tráfego entre as instâncias de aplicativo diferentes, alocando dinamicamente, assim, a carga de tráfego para o aplicativo.

## <a name="example-of-application-load-balancing"></a>Exemplo de balanceamento de carga do aplicativo

A seguir está um exemplo de como você pode usar a política de DNS para balanceamento de carga do aplicativo.

Este exemplo usa uma organização fictícia - serviços de brinde do Contoso - que fornece serviços online gifing, e que tem um site da Web denominado **contosogiftservices.com**.

O site contosogiftservices.com é hospedado em vários data centers que possuem endereços IP diferentes.

Na América do Norte, o que é o mercado primário para os serviços do Contoso presente, o site da Web está hospedado em três datacenters: Chicago, IL, Dallas, Texas e Seattle, WA.

O servidor Web de Seattle tem a melhor configuração de hardware e pode lidar com duas vezes mais carga como os dois sites. Serviços de brinde Contoso deseja tráfego de aplicativo direcionado da seguinte maneira.

- Como o servidor Web de Seattle inclui mais recursos, metade dos clientes do aplicativo é direcionada para este servidor
- Um quarto de clientes do aplicativo são direcionadas para o datacenter de Dallas, Texas
- Um quarto de clientes do aplicativo são direcionadas para o data center de Chicago, IL

A ilustração a seguir ilustra esse cenário.

![Com a política DNS de balanceamento de carga de aplicativos de DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Funciona de balanceamento de carga de aplicativo como

Depois de configurar o servidor DNS com a política de DNS para o aplicativo carregar balanceamento usando esse cenário de exemplo, o servidor DNS responde a 50% do tempo com o endereço do servidor Web de Seattle, 25% do tempo com o endereço do servidor Web de Dallas e 25% do tempo com o endereço do servidor Web de Chicago.

Assim, para cada quatro consultas que o servidor DNS recebe, ele responde com duas respostas para Seattle e um para Dallas e Chicago.

Um possível problema com o balanceamento de carga com a política de DNS é o cache de registros DNS, o cliente DNS e o resolvedor/LDNS, que podem interferir com balanceamento de carga porque o cliente ou um resolvedor não enviar uma consulta para o servidor DNS.

Você pode reduzir o efeito desse comportamento por meio de uma hora de baixa\-à\-Live \(TTL\) valor para registros de DNS que deve ser com balanceamento de carga.

### <a name="how-to-configure-application-load-balancing"></a>Como configurar o balanceamento de carga do aplicativo

As seções a seguir mostram como configurar a política de DNS para balanceamento de carga do aplicativo.

#### <a name="create-the-zone-scopes"></a>Criar os escopos de zona

Você deve primeiro criar os escopos de contosogiftservices.com a zona para os data centers onde eles estão hospedados.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

####<a name="bkmk_records"></a>Adicionar registros para os escopos de zona

Agora você deve adicionar os registros que representa o host do servidor web para os escopos de zona.

Na **SeattleZoneScope**, você pode adicionar o registro www.contosogiftservices.com com endereço IP 192.0.0.1, que está localizado no datacenter de Seattle.

Na **ChicagoZoneScope**, você pode adicionar o mesmo registro \(www.contosogiftservices.com\) com endereço IP 182.0.0.1 no data center de Chicago.

Da mesma forma em **DallasZoneScope**, você pode adicionar um registro \(www.contosogiftservices.com\) com endereço IP 162.0.0.1 no data center de Chicago.

Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros para os escopos de zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

####<a name="bkmk_policies"></a>Crie as políticas de DNS

Depois que você criou as partições (escopos de zona) e adicionar registros, você deve criar políticas DNS que distribui as consultas de entrada nesses escopos, de modo que 50% de consultas para contosogiftservices.com são respondidas com o endereço IP para a Web servidor no datacenter Seattle e o restante são distribuídas igualmente entre os data centers de Chicago e Dallas.

Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que equilibra o tráfego de aplicativo entre esses três datacenters.

>[!NOTE]
>No exemplo abaixo, a expressão de comando – ZoneScope "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1" configura o servidor DNS com uma matriz que inclui a combinação de parâmetros \<ZoneScope\>,\<peso\>.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

Você agora criou com êxito uma política DNS que fornece em servidores Web em três diferentes data centers de balanceamento de carga do aplicativo.

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.
