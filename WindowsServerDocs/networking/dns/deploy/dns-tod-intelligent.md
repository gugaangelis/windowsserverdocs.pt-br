---
title: Usar a política de DNS para respostas de DNS inteligente com base na hora do dia
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f371b91ecd87e07af90a7a2f7a0c802fb3d6c7e5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955933"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Usar a política de DNS para respostas de DNS inteligente com base na hora do dia

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a distribuir o tráfego do aplicativo em diferentes instâncias distribuídas geograficamente de um aplicativo usando políticas de DNS com base na hora do dia.

Esse cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativos, como servidores Web, que estão localizados em outro fuso horário. Isso permite balancear a carga de tráfego entre instâncias do aplicativo durante períodos de pico quando os servidores primários estão sobrecarregados com o tráfego.

### <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day"></a><a name="bkmk_example1"></a>Exemplo de respostas de DNS inteligente com base na hora do dia
Veja a seguir um exemplo de como você pode usar a política DNS para balancear o tráfego do aplicativo com base na hora do dia.

Este exemplo usa uma empresa fictícia, serviços de presente da Contoso, que fornece soluções de oferta online em todo o mundo por meio de seu site, contosogiftservices.com.

O site da contosogiftservices.com é hospedado em dois datacenters, um em Seattle (América do Norte) e outro em Dublin (Europa). Os servidores DNS são configurados para enviar respostas com reconhecimento de localização geográfica usando a política DNS. Com um surto recente nos negócios, o contosogiftservices.com tem um número maior de visitantes todos os dias, e alguns dos clientes relataram problemas de disponibilidade do serviço.

Os serviços de presentes da Contoso executam uma análise de site e descobre que todas as noites entre as horas locais, há um surto no tráfego para os servidores Web. Os servidores Web não podem ser dimensionados para lidar com o aumento do tráfego nesses horários de pico, resultando em negação de serviço aos clientes. A mesma sobrecarga de tráfego de horário de pico ocorre nos datacenters da Europa e da América. Em outras ocasiões do dia, os servidores lidam com os volumes de tráfego que estão bem abaixo do seu recurso máximo.

Para garantir que os clientes do contosogiftservices.com obtenham uma experiência responsiva do site, os serviços de presentes da Contoso querem redirecionar alguns tráfegos de Dublin para os servidores de aplicativos de Seattle entre 6 e 9 PM em Dublin; e eles desejam redirecionar alguns tráfegos de Seattle para os servidores de aplicativos de Dublin entre 6 e 9 PM em Seattle.

A ilustração a seguir descreve esse cenário.

![Exemplo de política de DNS de hora do dia](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)

### <a name="how-intelligent-dns-responses-based-on-time-of-day-works"></a><a name="bkmk_works1"></a>Como as respostas do DNS inteligente com base na hora do dia funcionam

Quando o servidor DNS é configurado com a política de DNS de hora do dia, entre 6 e 9 PM em cada local geográfico, o servidor DNS faz o seguinte.

- Responde às quatro primeiras consultas recebidas com o endereço IP do servidor Web no datacenter local.
- Responde à quinta consulta recebida com o endereço IP do servidor Web no datacenter remoto.

Esse comportamento baseado em políticas descarrega vinte por cento da carga de tráfego do servidor Web local para o servidor Web remoto, facilitando a sobrecarga no servidor de aplicativos local e melhorando o desempenho do site para os clientes.

Fora do horário de pico, os servidores DNS executam o gerenciamento de tráfego baseado em locais geográficos normais. Além disso, os clientes DNS que enviam consultas de locais diferentes de América do Norte ou Europa, a carga do servidor DNS equilibra o tráfego entre os data centers de Seattle e Dublin.

Quando várias políticas de DNS são configuradas no DNS, elas são um conjunto ordenado de regras e são processadas pelo DNS da prioridade mais alta para a prioridade mais baixa. O DNS usa a primeira política que corresponde às circunstâncias, incluindo a hora do dia. Por esse motivo, políticas mais específicas devem ter prioridade mais alta. Se você criar políticas de hora do dia e lhes conceder prioridade alta na lista de políticas, os processos DNS e usará essas políticas primeiro se corresponderem aos parâmetros da consulta do cliente DNS e aos critérios definidos na política. Se eles não corresponderem, o DNS se moverá para baixo na lista de políticas para processar as políticas padrão até encontrar uma correspondência.

Para obter mais informações sobre critérios e tipos de política, consulte [visão geral de políticas de DNS](../../dns/deploy/DNS-Policies-Overview.md).

### <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day"></a><a name="bkmk_how1"></a>Como configurar a política DNS para respostas de DNS inteligentes com base na hora do dia
Para configurar a política DNS para as respostas de consulta com base no balanceamento de carga do aplicativo de hora do dia, você deve executar as etapas a seguir.

- [Criar as sub-redes do cliente DNS](#bkmk_subnets)
- [Criar os escopos de zona](#bkmk_zscopes)
- [Adicionar registros aos escopos de zona](#bkmk_records)
- [Criar as políticas de DNS](#bkmk_policies)

>[!NOTE]
>Você deve executar essas etapas no servidor DNS que é autoritativo para a zona que deseja configurar. A associação em **DNSAdmins**, ou equivalente, é necessária para executar os procedimentos a seguir.

As seções a seguir fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.

#### <a name="create-the-dns-client-subnets"></a><a name="bkmk_subnets"></a>Criar as sub-redes do cliente DNS
A primeira etapa é identificar as sub-redes ou o espaço de endereço IP das regiões para as quais você deseja redirecionar o tráfego. Por exemplo, se você quiser redirecionar o tráfego para os EUA e Europa, precisará identificar as sub-redes ou os espaços de endereço IP dessas regiões.

Você pode obter essas informações de mapas de IP geográfico. Com base nessas distribuições de IP geográfico, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas a um servidor DNS.

Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS.

```
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"

Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"

```
Para obter mais informações, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

#### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes"></a>Criar os escopos de zona
Depois que as sub-redes de cliente são configuradas, você deve particionar a zona cujo tráfego você deseja redirecionar em dois escopos de zona diferentes, um escopo para cada uma das sub-redes de cliente DNS que você configurou.

Por exemplo, se você quiser redirecionar o tráfego para o nome DNS www.contosogiftservices.com, deverá criar dois escopos de zona diferentes na zona contosogiftservices.com, um para os EUA e outro para a Europa.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo.

Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"

Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"

```
Para obter mais informações, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

#### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Adicionar registros aos escopos de zona
Agora você deve adicionar os registros que representam o host do servidor Web nos escopos de duas zonas.

Por exemplo, em **SeattleZoneScope**, o registro <strong>www.contosogiftservices.com</strong> é adicionado com o endereço IP 192.0.0.1, que está localizado em um datacenter de Seattle. Da mesma forma, no **DublinZoneScope**, o registro <strong>www.contosogiftservices.com</strong> é adicionado com o endereço IP 141.1.0.3 no datacenter Dublin

Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros aos escopos de zona.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"

```
O parâmetro ZoneScope não é incluído quando você adiciona um registro no escopo padrão. Isso é o mesmo que adicionar registros a uma zona DNS padrão.

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

#### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>Criar as políticas de DNS
Depois de criar as sub-redes, as partições (escopos de zona) e adicionar registros, você deve criar políticas que conectam as sub-redes e as partições, de modo que quando uma consulta vier de uma origem em uma das sub-redes de cliente DNS, a resposta de consulta será retornada do escopo correto da zona. Nenhuma política é necessária para mapear o escopo de zona padrão.

Depois de configurar essas políticas de DNS, o comportamento do servidor DNS é o seguinte:

1. Os clientes DNS europeus recebem o endereço IP do servidor Web no datacenter Dublin em sua resposta de consulta DNS.
2. Os clientes de DNS American recebem o endereço IP do servidor Web no datacenter de Seattle em sua resposta de consulta DNS.
3. Entre 6 e 9 PM em Dublin, 20% das consultas de clientes europeus recebem o endereço IP do servidor Web no datacenter de Seattle em sua resposta de consulta DNS.
4. Entre 6 pm e 9h em Seattle, 20% das consultas dos clientes norte-americanos recebem o endereço IP do servidor Web no datacenter Dublin em sua resposta de consulta DNS.
5. Metade das consultas do restante do mundo recebe o endereço IP do datacenter de Seattle e a outra metade recebe o endereço IP do datacenter Dublin.


Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.

>[!NOTE]
>Neste exemplo, o servidor DNS está no fuso horário GMT, portanto, os períodos de hora de pico devem ser expressos no tempo de Greenwich equivalente.

```
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1

Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2

Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3

Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4

Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5

```
Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora, o servidor DNS é configurado com as políticas de DNS necessárias para redirecionar o tráfego com base na localização geográfica e na hora do dia.

Quando o servidor DNS recebe consultas de resolução de nomes, o servidor DNS avalia os campos na solicitação DNS em relação às políticas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nomes corresponder a qualquer uma das políticas, o escopo de zona associado será usado para responder à consulta e o usuário será direcionado para o recurso que está geograficamente mais próximo.

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.


