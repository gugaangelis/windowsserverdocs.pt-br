---
title: O servidor do aplicativo de nuvem respostas DNS com base na hora do dia com um Azure
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>O servidor do aplicativo de nuvem respostas DNS com base na hora do dia com um Azure

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como distribuir o tráfego do aplicativo entre diferentes instâncias geograficamente distribuídas de um aplicativo usando políticas DNS que se baseiam na hora do dia. 

Este cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativos, como servidores Web são hospedados no Microsoft Azure, que estão localizados em outro fuso horário. Isso permite que você carregue o tráfego de equilíbrio entre instâncias do aplicativo durante o pico períodos quando os servidores principais são sobrecarregados com tráfego. 

>[!NOTE]
>Para saber como usar a política DNS para respostas DNS inteligentes sem o uso do Azure, consulte [usar política de DNS para inteligente DNS respostas com base no tempo do dia](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Exemplo de respostas DNS inteligente com base na hora do dia com o servidor de aplicativo de nuvem do Azure

Este é um exemplo de como você pode usar a política DNS ao saldo tráfego de aplicativo com base na hora do dia.

Este exemplo usa uma empresa fictícia Contoso presente serviços, que fornece soluções doar on-line em todo o mundo por meio de seu site, contosogiftservices.com. 

O site contosogiftservices.com é hospedado apenas em um datacenter local único em Seattle (com IP pública 192.68.30.2). 

O servidor DNS também está localizado no datacenter local. 

Com um aumento recente do business, contosogiftservices.com tem um número maior de visitantes todos os dias, e alguns clientes relataram problemas de disponibilidade de serviço. 

Serviços de presente Contoso executa uma análise de site e descobre que todas as noites entre 18: 00 e às 21H hora local, não existe um aumento no tráfego para o servidor Web de Seattle. O servidor Web não pode ser dimensionados para lidar com o aumento do tráfego nesses horários de pico, resultando em negação de serviço aos clientes. 

Para garantir que os clientes contosogiftservices.com obtém uma experiência responsiva do site, Contoso presente serviços decide que durante essas horas que ele será alugar uma virtual machine \(VM\) no Microsoft Azure para hospedar uma cópia do seu servidor Web.  

Serviços de presente Contoso obtém um endereço IP público do Azure para a VM (192.68.31.44) e desenvolve a automação para implantar o servidor Web todos os dias no Azure entre 5-10 horas, permitindo por um período de contingencial para uma hora.

>[!NOTE]
>Para saber mais sobre VMs do Azure, consulte [documentação de máquinas virtuais](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Os servidores DNS são configurados com políticas DNS e escopos de zona para que entre 5-9 PM todos os dias, 30% de consultas são enviadas para a instância do servidor Web que está em execução no Azure.

A ilustração a seguir ilustra esse cenário.

![Política de DNS para tempo de respostas de dia](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>Como respostas DNS inteligente com base na hora do dia com o Azure funciona o servidor do aplicativo
 
Este artigo demonstra como configurar o servidor DNS para responder a consultas do DNS com dois endereços IP do servidor diferente do aplicativo - um servidor web está em Seattle e o outro está em um datacenter do Azure.

Após a configuração de uma nova política DNS é baseada no horário de pico de PM 6 a 9 PM em Seattle, o servidor DNS envia setenta por cento das respostas DNS para clientes que contém o endereço IP do servidor Web de Seattle e trinta por cento das respostas DNS para clientes que contém o endereço IP do servidor Web do Azure, assim, direcionar o tráfego de cliente para o novo servidor Web do Azure e impedindo que o servidor Web de Seattle ficando sobrecarregado. 

Em todos os outros horas do dia, o processamento de consulta normal ocorrerá e respostas são enviadas de escopo de zona padrão que contém um registro para o servidor web no datacenter local. 

O TTL de 10 minutos sobre o registro Azure garante que o registro é expirado do cache LDNS antes da VM é removida do Azure. Um dos benefícios de tais dimensionamento é que você pode manter seu DNS dados no local e manter dimensionamento ao Azure conforme exige.

## <a name="bkmk_azureconfigure"></a>Como configurar a política DNS para respostas DNS inteligente com base na hora do dia com o servidor do aplicativo Azure
Para configurar a política DNS para respostas de consulta de tempo de balanceamento de carga do dia aplicativo com base, você deve executar as etapas a seguir.


- [Criar os escopos de zona](#bkmk_zscopes)
- [Adicionar registros os escopos de zona](#bkmk_records)
- [Criar as políticas DNS](#bkmk_policies)


>[!NOTE]
>Você deve executar estas etapas no servidor DNS autoritativo para a zona que você deseja configurar. A associação ao grupo DnsAdmins, ou equivalente, é necessária para executar esses procedimentos. 

As seguintes seções fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos. 


### <a name="bkmk_zscopes"></a>Criar os escopos de zona
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP. 

>[!NOTE]
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. 

Você pode usar o comando de exemplo a seguir para criar um escopo de zona para hospedar os registros do Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>Adicionar registros os escopos de zona
A próxima etapa é adicionar os registros que representa o host do servidor da Web para os escopos de zona. 

Em AzureZoneScope, o registro www.contosogiftservices.com é adicionado com o endereço IP 192.68.31.44, que está localizado na nuvem pública do Azure. 

Da mesma forma, o padrão zona escopo \(contosogiftservices.com\), \(www.contosogiftservices.com\) um registro é adicionado com o endereço IP 192.68.30.2 do servidor Web em execução no datacenter Seattle local.

Na segundo cmdlet abaixo, o parâmetro – ZoneScope não está incluído. Por isso, os registros são adicionados no padrão ZoneScope. 

Além disso, o TTL do registro para VMs do Azure é mantido em 600s (10 minutos) para que o LDNS não armazenar em cache-lo por mais tempo - que interfira balanceamento de carga. Além disso, VMs do Azure estão disponíveis para 1 hora extra como contingência para garantir que os clientes até mesmo com registros armazenados em cache são capazes de resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).  

### <a name="bkmk_policies"></a>Criar as políticas DNS 
Depois que os escopos de zona são criados, você pode criar políticas DNS que distribuir as consultas recebidas por estes escopos para que ocorra a seguir.

1. De 18: 00 PM 9 diariamente, 30% dos clientes recebem o endereço IP do servidor Web no Azure datacenter na resposta DNS, enquanto 70% de clientes recebem o endereço IP do servidor Web Seattle local.
2. Em outras circunstâncias, todos os clientes recebem o endereço IP do servidor Web Seattle local.

O tempo do dia deve ser expressos na hora local do servidor DNS.

Você pode usar o comando de exemplo a seguir para criar a política de DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego para o servidor de Web do Azure com base na hora do dia. 

Observe a expressão:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Essa expressão configura o servidor DNS com uma combinação de ZoneScope e peso que instrui o servidor DNS para enviar o endereço IP do servidor Web de Seattle setenta por cento do tempo, ao enviar o endereço IP do servidor Web Azure trinta por cento do tempo.

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.
