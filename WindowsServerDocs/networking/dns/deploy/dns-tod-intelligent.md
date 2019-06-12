---
title: Usar a política de DNS para respostas de DNS inteligente com base na hora do dia
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c36475dacb8664352f4ab270878357118d281c60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446425"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Usar a política de DNS para respostas de DNS inteligente com base na hora do dia

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a distribuir o tráfego de aplicativo entre diferentes instâncias distribuídas geograficamente de um aplicativo por meio de políticas DNS com base na hora do dia.  
  
Esse cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativo, como servidores Web, que estão localizados em outro fuso horário. Isso permite que você carregue balancear o tráfego entre as instâncias do aplicativo durante o horário de pico quando seus servidores primários são sobrecarregados com o tráfego de períodos de tempo.   
  
### <a name="bkmk_example1"></a>Exemplo de respostas DNS inteligente com base na hora do dia  
A seguir está um exemplo de como você pode usar a política de DNS para o tráfego de aplicativo de saldo com base na hora do dia.  
  
Este exemplo usa uma empresa fictícia, Contoso presente serviços, que fornece soluções de doação on-line em todo o mundo por meio do site, contosogiftservices.com.   
  
Site da Web contosogiftservices.com é hospedado em dois datacenters, em Seattle (América do Norte) e outro em Dublin (Europa). Os servidores DNS estão configurados para o envio de localização geográfica com suporte a respostas usando a política de DNS. Com um surto recente nos negócios, contosogiftservices.com tem um número maior de visitantes todos os dias e alguns clientes relataram problemas de disponibilidade do serviço.  
  
Os serviços de brinde Contoso realiza uma análise de site e descobre todas as noites entre a hora local às 18H e às 21H, há um aumento no tráfego para os servidores Web. Os servidores Web não podem dimensionar para lidar com o aumento no tráfego a estas horas de pico, resultando na negação de serviço aos clientes. A sobrecarga de tráfego de horas de pico mesmo acontece em datacenters europeus e América. Em outros momentos do dia, os servidores de lidar com volumes de tráfego que são bem abaixo da sua capacidade máxima.  
  
Para garantir que os clientes contosogiftservices.com obtenham uma experiência responsiva do site da Web, serviços de brinde Contoso deseja redirecionar algum tráfego Dublin para os servidores de aplicativos de Seattle entre às 18H e 9 PM em Dublin; e eles desejam redirecionar algum tráfego de Seattle para os servidores de aplicativos de Dublin entre às 18H e às 21 horas em Seattle.  
  
A ilustração a seguir ilustra esse cenário.  
  
![Tempo de exemplo de política de DNS do dia](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Respostas DNS como inteligentes com base na hora do dia funciona  
  
Quando o servidor DNS é configurado com o tempo da política DNS do dia, entre às 18H e às 21 horas em cada localização geográfica, o servidor DNS faz o seguinte.  
  
- Responde as consultas de quatro primeiros com o endereço IP do servidor Web no datacenter local.  
- Responde à consulta quinta recebe com o endereço IP do servidor Web no data center remoto.   
  
Esse comportamento baseado em políticas descarrega vinte por cento de carga de tráfego do servidor Web local para o servidor Web remoto, facilitando a carga no servidor de aplicativos locais e melhorando o desempenho do site para os clientes.  
  
Fora do horário de pico, os servidores DNS realizar o gerenciamento de tráfego normal locais geográficos com base. Além disso, os clientes DNS que enviam consultas de locais diferentes da América do Norte ou na Europa, a carga do servidor DNS equilibra o tráfego entre os datacenters de Seattle e Dublin.  
  
Quando várias políticas DNS são configuradas no DNS, eles são um conjunto ordenado de regras, e eles são processados pelo DNS de prioridade mais alta a prioridade mais baixa. O DNS usa a primeira diretiva que coincide com as circunstâncias, incluindo a hora do dia. Por esse motivo, políticas mais específicas devem ter prioridade mais alta. Se você criar o tempo das políticas de dia e lhes dá prioridade alta na lista de políticas, o DNS processa e usa essas políticas primeiro se eles correspondem aos parâmetros de consulta de cliente DNS e os critérios definidos na política. Se elas não corresponderem, o DNS move para baixo na lista de políticas para processar as políticas padrão até que ele encontra uma correspondência.  
  
Para obter mais informações sobre tipos de política e critérios, consulte [visão geral das políticas de DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Como configurar a política de DNS para respostas de DNS inteligente com base na hora do dia  
Para configurar a política de DNS para respostas de consulta de tempo de balanceamento de carga de aplicativo dia com base, você deve executar as etapas a seguir.  
  
- [Crie as sub-redes de cliente DNS](#bkmk_subnets)  
- [Criar os escopos de zona](#bkmk_zscopes)  
- [Adicionar registros para os escopos de zona](#bkmk_records)  
- [Crie as políticas de DNS](#bkmk_policies)  
  
>[!NOTE]
>Você deve executar essas etapas no servidor DNS autoritativo para a zona que você deseja configurar. Associação na **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
As seções a seguir fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos.  
  
#### <a name="bkmk_subnets"></a>Crie as sub-redes de cliente DNS  
A primeira etapa é identificar o espaço de endereço IP das regiões para o qual você deseja redirecionar o tráfego ou sub-redes. Por exemplo, se você deseja redirecionar o tráfego para os EUA e Europa, você precisa identificar as sub-redes ou espaços de endereço IP dessas regiões.  
  
Você pode obter essas informações de mapas de Geo-IP. Com base nessas distribuições IP geográfico, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas para um servidor DNS.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Para obter mais informações, consulte [DnsServerClientSubnet adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Criar os escopos de zona  
Depois que as sub-redes de cliente são configuradas, você deve particionar a zona cujo tráfego você deseja redirecionar em dois escopos de zona diferente, um escopo para cada uma das sub-redes cliente DNS que você configurou.  
  
Por exemplo, se você deseja redirecionar o tráfego para o DNS nome www.contosogiftservices.com, você deve criar dois escopos de zona diferente na zona contosogiftservices.com, um para os EUA e outro para Europa.  
  
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP.  
  
>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Adicionar registros para os escopos de zona  
Agora você deve adicionar os registros que representa o host do servidor web para os escopos de zona de dois.  
  
Por exemplo, na **SeattleZoneScope**, o registro <strong>www.contosogiftservices.com</strong> é adicionado com o endereço IP 192.0.0.1, que está localizado em um datacenter de Seattle. Da mesma forma, na **DublinZoneScope**, o registro <strong>www.contosogiftservices.com</strong> é adicionado com o endereço IP 141.1.0.3 no datacenter Dublin  
  
Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros para os escopos de zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
O parâmetro ZoneScope não está incluído quando você adiciona um registro no escopo padrão. Isso é o mesmo que adicionar registros a uma zona DNS padrão.  
  
Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Crie as políticas de DNS  
Depois de ter criado as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que se conectam a sub-redes e partições, para que quando uma consulta vem de uma fonte em uma das sub-redes de cliente DNS, a resposta de consulta é retornada de o escopo correto da zona. Não há políticas são necessárias para o escopo da região padrão de mapeamento.  
  
Depois de configurar essas políticas DNS, o comportamento do servidor DNS é o seguinte:  
  
1. Os clientes DNS europeus recebem o endereço IP do servidor Web no datacenter Dublin na sua resposta da consulta DNS.  
2. Os clientes DNS American recebem o endereço IP do servidor Web no datacenter Seattle em sua resposta de consulta DNS.  
3. Entre às 18H e 9 PM em Dublin, 20% das consultas dos clientes europeus receber o endereço IP do servidor Web no datacenter Seattle em sua resposta de consulta DNS.  
4. Entre às 18H e às 21 horas em Seattle, 20% das consultas dos clientes a American receber o endereço IP do servidor Web no datacenter Dublin na sua resposta da consulta DNS.  
5. Metade das consultas do restante do mundo receber o endereço IP do datacenter Seattle e a outra metade receber o endereço IP do datacenter Dublin.  
  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula-se as sub-redes de cliente DNS e os escopos de zona.  
  
>[!NOTE]
>Neste exemplo, o servidor DNS está no fuso horário GMT, para que o horário de pico períodos de tempo devem ser expressa na hora GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego com base em localização geográfica e a hora do dia.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação de DNS com as diretivas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associada é usado para responder à consulta e o usuário é direcionado para o recurso que seja geograficamente mais próximo a eles.  
  
Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.


