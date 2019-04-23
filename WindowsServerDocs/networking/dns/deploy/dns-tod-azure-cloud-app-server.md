---
title: Respostas de DNS com base na hora do dia com o Servidor de aplicativos da nuvem do Azure
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ed6ac2ebc8839d0e7ecee682d7644251f8a59381
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829067"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Respostas de DNS com base na hora do dia com o Servidor de aplicativos da nuvem do Azure

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a distribuir o tráfego de aplicativo entre diferentes instâncias distribuídas geograficamente de um aplicativo por meio de políticas DNS com base na hora do dia. 

Esse cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativo, como servidores Web são hospedados no Microsoft Azure, que estão localizados em outro fuso horário. Isso permite que você carregue balancear o tráfego entre as instâncias do aplicativo durante o horário de pico quando seus servidores primários são sobrecarregados com o tráfego de períodos de tempo. 

>[!NOTE]
>Para saber como usar a política DNS para respostas DNS inteligentes sem o uso do Azure, consulte [usar a política de DNS para inteligente DNS respostas com base na hora do dia](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Exemplo de respostas DNS inteligente com base na hora do dia com o servidor de aplicativos de nuvem do Azure

A seguir está um exemplo de como você pode usar a política de DNS para o tráfego de aplicativo de saldo com base na hora do dia.

Este exemplo usa uma empresa fictícia, Contoso presente serviços, que fornece soluções de doação on-line em todo o mundo por meio do site, contosogiftservices.com. 

Site da web contosogiftservices.com é hospedado somente em um único datacenter local em Seattle (com IP público 192.68.30.2). 

O servidor DNS também está localizado no datacenter local. 

Com um surto recente nos negócios, contosogiftservices.com tem um número maior de visitantes todos os dias e alguns clientes relataram problemas de disponibilidade do serviço. 

Os serviços de brinde Contoso realiza uma análise de site e descobre todas as noites entre a hora local às 18H e às 21H, há um aumento no tráfego para o servidor Web de Seattle. O servidor Web não pode dimensionar para lidar com o aumento no tráfego a estas horas de pico, resultando na negação de serviço aos clientes. 

Para garantir que os clientes contosogiftservices.com obtenham uma experiência responsiva do site, a Contoso presente serviços decide que, durante o horário, ele será alugar uma máquina virtual \(VM\) no Microsoft Azure para hospedar uma cópia do seu servidor Web .  

Os serviços de brinde do Contoso obtém um endereço IP público do Azure para a VM (192.68.31.44) e desenvolve a automação para implantar o servidor Web de todos os dias no Azure entre - 10 às 17H, permitindo um período de contingência de uma hora.

>[!NOTE]
>Para obter mais informações sobre as VMs do Azure, consulte [documentação das máquinas virtuais](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Os servidores DNS estão configurados com escopos de zona e políticas de DNS para que entre 5 a 9 horas diariamente, 30% das consultas são enviadas para a instância do servidor Web que está em execução no Azure.

A ilustração a seguir ilustra esse cenário.

![Política de DNS para o tempo de respostas do dia](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>Como as respostas DNS inteligente com base na hora do dia com o Azure do servidor de aplicativo funciona
 
Este artigo demonstra como configurar o servidor DNS para responder a consultas DNS com dois endereços IP de servidor de aplicativo diferentes - um servidor web está em Seattle e o outro está em um datacenter do Azure.

Após a configuração de uma nova política DNS se baseia no horário de pico às 18H às 21:00 em Seattle, o servidor DNS envia setenta por cento das respostas DNS para clientes que contém o endereço IP do servidor Web de Seattle e 30 por cento das respostas DNS para clie NTS que contém o endereço IP do servidor Web do Azure, assim, direcionar o tráfego de cliente para o novo servidor Web do Azure e impedindo que o servidor Web de Seattle ficando sobrecarregado. 

Em todos os outros momentos do dia, o processamento de consulta normal ocorre e as respostas são enviadas do escopo de padrão de zona que contém um registro para o servidor web no datacenter local. 

O TTL de 10 minutos no registro do Azure garante que o registro é expirado do cache LDNS antes que a VM seja removida do Azure. Um dos benefícios de dimensionamento de tal é que você pode manter seu DNS dados locais e manter escala horizontal para o Azure conforme a demanda.

## <a name="bkmk_azureconfigure"></a>Como configurar a política de DNS para respostas DNS inteligente com base na hora do dia com o servidor de aplicativo do Azure
Para configurar a política de DNS para respostas de consulta de tempo de balanceamento de carga de aplicativo dia com base, você deve executar as etapas a seguir.


- [Criar os escopos de zona](#bkmk_zscopes)
- [Adicionar registros para os escopos de zona](#bkmk_records)
- [Crie as políticas de DNS](#bkmk_policies)


>[!NOTE]
>Você deve executar essas etapas no servidor DNS autoritativo para a zona que você deseja configurar. Associação no grupo DnsAdmins, ou equivalente, é necessário para executar os procedimentos a seguir. 

As seções a seguir fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos. 


### <a name="bkmk_zscopes"></a>Criar os escopos de zona
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP. 

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. 

Você pode usar o comando de exemplo a seguir para criar um escopo de zona para hospedar os registros do Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Adicionar registros para os escopos de zona
A próxima etapa é adicionar os registros que representa o host do servidor Web para os escopos de zona. 

AzureZoneScope, www.contosogiftservices.com o registro é adicionado com o endereço IP 192.68.31.44, que está localizado na nuvem pública do Azure. 

Da mesma forma, no escopo de zona padrão \(contosogiftservices.com\), um registro \(www.contosogiftservices.com\) é adicionado com o endereço IP 192.68.30.2 do servidor Web em execução em Seattle no local Data Center.

No segundo cmdlet abaixo, o parâmetro – ZoneScope não está incluído. Por isso, os registros são adicionados no padrão ZoneScope. 

Além disso, o TTL do registro para VMs do Azure é mantido no 600s (10 minutos), para que o LDNS armazena em cache por um período maior - que poderia interferir com balanceamento de carga. Além disso, as VMs do Azure estão disponíveis por 1 hora extra como uma contingência para garantir que os clientes, mesmo com registros armazenados em cache sejam capazes de resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="bkmk_policies"></a>Crie as políticas de DNS 
Depois que os escopos de zona são criados, você pode criar políticas DNS que distribui as consultas de entrada nesses escopos para que ocorra o seguinte.

1. Do PM de 6 a 9 PM diariamente, 30% dos clientes recebem o endereço IP do servidor Web no datacenter do Azure na resposta DNS, enquanto os 70% dos clientes recebem o endereço IP do servidor Web de local de Seattle.
2. Em todos os outros horários, todos os clientes recebem o endereço IP do servidor Web de local de Seattle.

A hora do dia deve ser expressa em hora local do servidor DNS.

Você pode usar o comando de exemplo a seguir para criar a política DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego para o servidor Web do Azure com base na hora do dia. 

Observe a expressão:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Essa expressão configura o servidor DNS com uma combinação de ZoneScope e peso que instrui o servidor DNS para enviar o endereço IP do servidor Web de Seattle setenta por cento do tempo, ao enviar o endereço IP do servidor Web do Azure de 30 por cento do tempo.

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.
