---
title: Usar a política DNS para balanceamento de carga do aplicativo
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usar a política DNS para balanceamento de carga do aplicativo

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar a política DNS para executar o balanceamento de carga do aplicativo.

Versões anteriores do Windows Server DNS só forneciam balanceamento usando rodízio respostas; mas, com o serviço DNS no Windows Server 2016, você pode configurar política de DNS de balanceamento de carga do aplicativo.

Quando você implantou várias instâncias de um aplicativo, você pode usar a política DNS para equilibrar a carga de tráfego entre as instâncias de aplicativo diferente, alocando assim dinamicamente a carga do tráfego para o aplicativo.

## <a name="example-of-application-load-balancing"></a>Exemplo de balanceamento de carga do aplicativo

Este é um exemplo de como você pode usar política DNS para balanceamento de carga do aplicativo.

Este exemplo usa um fictícia - Contoso presente serviços - que fornece os serviços online gifing, e que tem um site chamado **contosogiftservices.com**.

O site contosogiftservices.com está hospedado em vários data centers que cada um tem diferentes endereços IP.

Na América do Norte, que é o principal mercado para serviços de presente Contoso, o site está hospedado em três datacenters: Chicago, Illinois, EUA, Dallas, TX e Seattle, WA.

O servidor Web de Seattle tenha a melhor configuração de hardware e pode lidar com o dobro carga como os outros dois locais. Serviços de presente Contoso quer tráfego do aplicativo direcionado da seguinte maneira.

- Como o servidor Web de Seattle inclui mais recursos, metade de clientes do aplicativo é direcionada para esse servidor
- Um quarto de clientes do aplicativo são direcionadas para o datacenter Dallas, TX
- Um quarto de clientes do aplicativo são direcionadas para o datacenter Chicago, Illinois, EUA,

A ilustração a seguir ilustra esse cenário.

![DNS aplicativo balanceamento de carga com política DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Como aplicativo balanceamento de funciona

Depois de configurar o servidor DNS com a política DNS para aplicativo carregar balanceamento usando esta situação de exemplo, o servidor DNS responde 50% do tempo com o endereço do servidor Web de Seattle, 25% do tempo com o endereço do servidor Web Dallas e 25% do tempo com o endereço do servidor Web Chicago.

Assim, para todas as consultas de quatro que recebe o servidor DNS, ele responde com duas respostas de Seattle e um para cada Dallas e Chicago.

Um possível problema de balanceamento de carga com política DNS é o cache de registros DNS pelo cliente DNS e resolvedor/LDNS, que pode interferir com balanceamento de carga porque o cliente ou um resolvedor não envie uma consulta para o servidor DNS.

Você pode atenuar o efeito desse comportamento usando um valor baixo de \(TTL\) Time\-to\-Live para os registros DNS que devem ser balanceado.

### <a name="how-to-configure-application-load-balancing"></a>Como configurar o balanceamento de carga do aplicativo

As seções a seguir mostram como configurar a política DNS para balanceamento de carga do aplicativo.

#### <a name="create-the-zone-scopes"></a>Criar os escopos de zona

Você deve primeiro criar os escopos de contosogiftservices.com a zona para os datacenters onde eles são hospedados.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>Adicionar registros os escopos de zona

Agora você deve adicionar os registros que representa o host do servidor da web para os escopos de zona.

Em **SeattleZoneScope**, você pode adicionar o registro www.contosogiftservices.com com o endereço IP 192.0.0.1, que está localizado no datacenter Seattle.

Em **ChicagoZoneScope**, você pode adicionar a mesma \(www.contosogiftservices.com\) registro com o endereço IP 182.0.0.1 no datacenter Chicago.

Da mesma forma em **DallasZoneScope**, você pode adicionar \(www.contosogiftservices.com\) um registro com o endereço IP 162.0.0.1 no datacenter Chicago.

Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros os escopos de zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

####<a name="bkmk_policies"></a>Criar as políticas DNS

Depois que você criou as partições (escopos zona) e você adicionou registros, você deve criar políticas DNS que distribuir as consultas recebidas por estes escopos para que 50% de consultas para contosogiftservices.com são respondidas com o endereço IP do servidor Web no datacenter Seattle e o restante é distribuído uniformemente entre os datacenters Chicago e Dallas.

Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que equilibra o tráfego de aplicativo em todos esses três datacenters.

>[!NOTE]
>No comando de exemplo abaixo, a expressão – ZoneScope "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1" configura o servidor DNS com uma matriz que inclui a combinação de parâmetro \ < ZoneScope\ > \ < weight\ >.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  

Agora foi criado com êxito uma política DNS que fornece o aplicativo balanceamento de carga entre servidores Web em três diferentes centros de dados.

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.