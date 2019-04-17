---
title: Usar a política de DNS para respostas DNS inteligente com base na hora do dia
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Usar a política de DNS para respostas DNS inteligente com base na hora do dia

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como distribuir o tráfego do aplicativo entre diferentes instâncias geograficamente distribuídas de um aplicativo usando políticas DNS que se baseiam na hora do dia.  
  
Este cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativos, como servidores Web, que estão localizados em outro fuso horário. Isso permite que você carregue o tráfego de equilíbrio entre instâncias do aplicativo durante o pico períodos quando os servidores principais são sobrecarregados com tráfego.   
  
### <a name="bkmk_example1"></a>Exemplo de respostas DNS inteligente com base na hora do dia  
Este é um exemplo de como você pode usar a política DNS ao saldo tráfego de aplicativo com base na hora do dia.  
  
Este exemplo usa uma empresa fictícia Contoso presente serviços, que fornece soluções doar on-line em todo o mundo por meio de seu site, contosogiftservices.com.   
  
O site contosogiftservices.com está hospedado em dois centros de dados, um em Seattle (América do Norte) e outro em Dublin (Europa). Os servidores DNS são configurados para enviar respostas de reconhecimento usando a política de DNS de localização geográfica. Com um aumento recente do business, contosogiftservices.com tem um número maior de visitantes todos os dias, e alguns clientes relataram problemas de disponibilidade de serviço.  
  
Serviços de presente Contoso executa uma análise de site e descobre que todas as noites entre 18: 00 e às 21H hora local, não existe um aumento no tráfego para os servidores da Web. Os servidores Web não podem ser dimensionados para lidar com o aumento do tráfego nesses horários de pico, resultando em negação de serviço aos clientes. A sobrecarga de tráfego de horas de pico mesmo acontece em datacenters europeus e americano. Em outras horas do dia, os servidores de lidar com volumes de tráfego que estão bem abaixo sua capacidade máxima.  
  
Para garantir que os clientes contosogiftservices.com obtém uma experiência responsiva do site, os serviços de presente Contoso quer redirecionar algum tráfego Dublin aos servidores de aplicativos Seattle entre 18: 00 e às 21H em Dublin; e eles querem redirecionar algum tráfego de Seattle aos servidores de aplicativos Dublin entre 18: 00 e às 21H em Seattle.  
  
A ilustração a seguir ilustra esse cenário.  
  
![Hora do exemplo de política de DNS dia](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Como inteligentes respostas DNS com base na hora do dia funciona  
  
Quando o servidor DNS é configurado com tempo de política DNS do dia, entre 18: 00 e às 21H em cada localização geográfica, o servidor DNS faz o seguinte.  
  
- Respostas as consultas de quatro primeiros que ele recebe com o endereço IP do servidor Web no datacenter local.  
- Respostas a consulta quinta que ele recebe com o endereço IP do servidor Web no datacenter remoto.   
  
Esse comportamento com base em política descarrega vinte por cento da carga de tráfego do servidor Web local para o servidor Web remoto, facilitando a sobrecarga no servidor de aplicativo local e melhorar o desempenho de site para os clientes.  
  
Fora do horário de pico, os servidores DNS realizar o gerenciamento de tráfego normal locais de localização geográfica com base. Além disso, os clientes DNS que enviar consultas de locais diferentes na América do Norte ou Europa, a carga do servidor DNS equilibra o tráfego entre os datacenters de Seattle e Dublin.  
  
Quando várias políticas DNS são configuradas no DNS, eles são um conjunto ordenado de regras, e elas são processadas pelo DNS de prioridade mais alta para a menor prioridade. DNS usa a política primeiro que atenda as circunstâncias, incluindo a hora do dia. Por esse motivo, políticas mais específicas devem ter prioridade mais alta. Se você criar tempo das políticas de dia e dê a eles de alta prioridade na lista de políticas, DNS processa e usa essas políticas primeiro se eles correspondem aos parâmetros de consulta DNS cliente e os critérios definidos na política. Se eles não corresponderem, DNS move para baixo na lista de políticas para processar as políticas padrão até encontrar uma correspondência.  
  
Para obter mais informações sobre tipos de política e critérios, consulte [visão geral das políticas de DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Como configurar a política DNS para respostas DNS inteligente com base na hora do dia  
Para configurar a política DNS para respostas de consulta de tempo de balanceamento de carga do dia aplicativo com base, você deve executar as etapas a seguir.  
  
- [Criar as sub-redes do cliente DNS](#bkmk_subnets)  
- [Criar os escopos de zona](#bkmk_zscopes)  
- [Adicionar registros os escopos de zona](#bkmk_records)  
- [Criar as políticas DNS](#bkmk_policies)  
  
>[!NOTE]
>Você deve executar estas etapas no servidor DNS autoritativo para a zona que você deseja configurar. A associação ao grupo **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
As seguintes seções fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos.  
  
#### <a name="bkmk_subnets"></a>Criar as sub-redes do cliente DNS  
A primeira etapa é identificar as sub-redes ou espaço de endereço IP das regiões para o qual você quer redirecionar o tráfego. Por exemplo, se você quer redirecionar o tráfego para os EUA e Europa, você precisa identificar as sub-redes ou espaços de endereço IP dessas regiões.  
  
Você pode obter essas informações de mapas de IP de localização geográfica. De acordo com essas distribuições de IP de localização geográfica, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 da qual as consultas são enviadas para um servidor DNS.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes do cliente DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Para obter mais informações, consulte [adicionar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Criar os escopos de zona  
Depois que as sub-redes de cliente são configuradas, você deve particionar zona da qual você quer redirecionar em dois escopos de zona diferente, o tráfego um escopo para cada uma das sub-redes cliente DNS que você configurou.  
  
Por exemplo, se você quer redirecionar o tráfego para o nome DNS www.contosogiftservices.com, você deve criar dois escopos de zona diferentes na zona da contosogiftservices.com, um para os EUA e outra para Europa.  
  
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP.  
  
>[!NOTE]
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Para obter mais informações, consulte [adicionar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Adicionar registros os escopos de zona  
Agora você deve adicionar os registros que representa o host do servidor da web para os escopos de dois zona.  
  
Por exemplo, em **SeattleZoneScope**, o registro **www.contosogiftservices.com** é adicionado ao endereço IP 192.0.0.1, que está localizado em um datacenter de Seattle. Da mesma forma, em **DublinZoneScope**, o registro **www.contosogiftservices.com** é adicionado com o endereço IP 141.1.0.3 no datacenter Dublin  
  
Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros os escopos de zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
O parâmetro ZoneScope não está incluído quando você adiciona um registro no escopo padrão. Isso equivale a adição de registros para uma zona DNS padrão.  
  
Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Criar as políticas DNS  
Depois que você criou as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que conectam as sub-redes e partições, para que quando uma consulta vem de uma fonte em um das sub-redes do cliente DNS, a resposta da consulta é retornada do escopo da zona correto. Não há políticas são necessárias para mapear o escopo da região padrão.  
  
Depois de definir essas políticas DNS, o comportamento do servidor DNS é o seguinte:  
  
1. Os clientes DNS Europeu recebem o endereço IP do servidor Web no datacenter Dublin em sua resposta da consulta DNS.  
2. Os clientes DNS americano recebem o endereço IP do servidor Web no datacenter Seattle em sua resposta da consulta DNS.  
3. Entre 18: 00 e às 21H em Dublin, 20% das consultas de clientes na Europa recebem o endereço IP do servidor Web no datacenter Seattle em sua resposta da consulta DNS.  
4. Entre 18: 00 e às 21H em Seattle, 20% das consultas dos clientes americano recebem o endereço IP do servidor Web no datacenter Dublin em sua resposta da consulta DNS.  
5. Metade das consultas do resto do mundo recebem o endereço IP do datacenter Seattle e a outra metade recebe o endereço IP do datacenter Dublin.  
  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.  
  
>[!NOTE]
>Neste exemplo, o servidor DNS é no fuso horário GMT, portanto, o horário de pico períodos de tempo devem ser expressos na hora GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego com base na localização geográfica e a hora do dia.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação DNS contra as políticas DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associado é usado para responder à consulta e o usuário será direcionado para o recurso que é geograficamente closest-los.  
  
Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.


