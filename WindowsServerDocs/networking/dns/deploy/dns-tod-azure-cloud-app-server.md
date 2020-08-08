---
title: Respostas de DNS com base na hora do dia com o Servidor de aplicativos da nuvem do Azure
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d7df84e26ef86f553d57b2019d4d46581d7c17fa
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996893"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Respostas de DNS com base na hora do dia com o Servidor de aplicativos da nuvem do Azure

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a distribuir o tráfego do aplicativo em diferentes instâncias distribuídas geograficamente de um aplicativo usando políticas de DNS com base na hora do dia.

Esse cenário é útil em situações em que você deseja direcionar o tráfego em um fuso horário para servidores de aplicativos alternativos, como servidores Web hospedados em Microsoft Azure, que estão localizados em outro fuso horário. Isso permite balancear a carga de tráfego entre instâncias do aplicativo durante períodos de pico quando os servidores primários estão sobrecarregados com o tráfego.

> [!NOTE]
> Para saber como usar a política DNS para respostas de DNS inteligentes sem usar o Azure, consulte [usar a política DNS para respostas de DNS inteligente com base na hora do dia](./dns-tod-intelligent.md).

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Exemplo de respostas de DNS inteligente com base na hora do dia com o servidor de aplicativos de nuvem do Azure

Veja a seguir um exemplo de como você pode usar a política DNS para balancear o tráfego do aplicativo com base na hora do dia.

Este exemplo usa uma empresa fictícia, serviços de presente da Contoso, que fornece soluções de oferta online em todo o mundo por meio de seu site, contosogiftservices.com.

O site da contosogiftservices.com é hospedado somente em um datacenter local único em Seattle (com IP público 192.68.30.2).

O servidor DNS também está localizado no datacenter local.

Com um surto recente nos negócios, o contosogiftservices.com tem um número maior de visitantes todos os dias, e alguns dos clientes relataram problemas de disponibilidade do serviço.

Os serviços de presentes da Contoso executam uma análise de site e descobre que todas as noites entre as horas locais, há um surto no tráfego para o servidor Web de Seattle. O servidor Web não pode ser dimensionado para lidar com o aumento de tráfego nesses horários de pico, resultando em negação de serviço aos clientes.

Para garantir que os clientes do contosogiftservices.com tenham uma experiência responsiva do site, os serviços de presentes da Contoso decidem que, durante essas horas, ele alugará uma \( VM de máquina virtual \) em Microsoft Azure para hospedar uma cópia de seu servidor Web.

Os serviços de presentes da Contoso recebem um endereço IP público do Azure para a VM (192.68.31.44) e desenvolve a automação para implantar o servidor Web todos os dias no Azure entre 5-10 PM, permitindo um período de contingência de uma hora.

> [!NOTE]
> Para obter mais informações sobre VMs do Azure, consulte [documentação de máquinas virtuais](https://azure.microsoft.com/documentation/services/virtual-machines/)

Os servidores DNS são configurados com escopos de zona e políticas de DNS para que entre 5-9 PM todos os dias, 30% das consultas são enviadas para a instância do servidor Web em execução no Azure.

A ilustração a seguir descreve esse cenário.

![Política de DNS para respostas de hora do dia](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Como as respostas do DNS inteligente com base na hora do dia com o Azure App Server funcionam

Este artigo demonstra como configurar o servidor DNS para responder a consultas DNS com dois endereços IP de servidor de aplicativos diferentes: um servidor Web está em Seattle e o outro está em um datacenter do Azure.

Após a configuração de uma nova política de DNS com base nas horas de pico de 6 a 9 PM em Seattle, o servidor DNS envia 70 por cento das respostas DNS para clientes que contêm o endereço IP do servidor Web de Seattle e trinta por cento das respostas DNS aos clientes que contêm o endereço IP do servidor Web do Azure , direcionando assim o tráfego do cliente para o novo servidor Web do Azure e impedindo que o servidor Web de Seattle fique sobrecarregado.

Em todas as outras ocasiões do dia, o processamento de consulta normal ocorre e as respostas são enviadas do escopo de zona padrão que contém um registro para o servidor Web no datacenter local.

O TTL de 10 minutos no registro do Azure garante que o registro tenha expirado do cache LDNS antes que a VM seja removida do Azure. Um dos benefícios desse dimensionamento é que você pode manter os dados do DNS no local e manter a expansão para o Azure conforme a demanda exige.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Como configurar a política DNS para respostas de DNS inteligente com base na hora do dia com o Azure App Server

Para configurar a política DNS para as respostas de consulta com base no balanceamento de carga do aplicativo de hora do dia, você deve executar as etapas a seguir.

- [Criar os escopos de zona](#create-the-zone-scopes)
- [Adicionar registros aos escopos de zona](#add-records-to-the-zone-scopes)
- [Criar as políticas de DNS](#create-the-dns-policies)

> [!NOTE]
> Você deve executar essas etapas no servidor DNS que é autoritativo para a zona que deseja configurar. A associação em DnsAdmins, ou equivalente, é necessária para executar os procedimentos a seguir.

As seções a seguir fornecem instruções de configuração detalhadas.

> [!IMPORTANT]
> As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.


### <a name="create-the-zone-scopes"></a>Criar os escopos de zona

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.

> [!NOTE]
> Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo.

Você pode usar o seguinte comando de exemplo para criar um escopo de zona para hospedar os registros do Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obter mais informações, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>Adicionar registros aos escopos de zona
A próxima etapa é adicionar os registros que representam o host do servidor Web nos escopos de zona.

No AzureZoneScope, o registro www.contosogiftservices.com é adicionado com o endereço IP 192.68.31.44, que está localizado na nuvem pública do Azure.

Da mesma forma, no escopo de zona padrão \( contosogiftservices.com \) , um registro \( www.contosogiftservices.com \) é adicionado com o endereço IP 192.68.30.2 do servidor Web em execução no datacenter local de Seattle.

No segundo cmdlet abaixo, o parâmetro – ZoneScope não está incluído. Por isso, os registros são adicionados no ZoneScope padrão.

Além disso, o TTL do registro para VMs do Azure é mantido em 600s (10 minutos) para que o LDNS não o armazene em cache por um tempo maior, o que iria interferir no balanceamento de carga. Além disso, as VMs do Azure estão disponíveis por 1 hora extra como contingência para garantir que até mesmo clientes com registros em cache sejam capazes de resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obter mais informações, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Criar as políticas de DNS
Depois que os escopos de zona são criados, você pode criar políticas de DNS que distribuem as consultas de entrada entre esses escopos para que ocorra o seguinte.

1. De 6 a 9 PM diariamente, 30% dos clientes recebem o endereço IP do servidor Web no datacenter do Azure na resposta DNS, enquanto 70% dos clientes recebem o endereço IP do servidor Web local de Seattle.
2. Em outras ocasiões, todos os clientes recebem o endereço IP do servidor Web local de Seattle.

A hora do dia deve ser expressa na hora local do servidor DNS.

Você pode usar o comando de exemplo a seguir para criar a política DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora, o servidor DNS é configurado com as políticas de DNS necessárias para redirecionar o tráfego para o servidor Web do Azure com base na hora do dia.

Observe a expressão:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00”
`

Essa expressão configura o servidor DNS com uma combinação de ZoneScope e peso que instrui o servidor DNS a enviar o endereço IP do servidor Web de Seattle 70 por cento do tempo, enquanto envia o endereço IP do servidor Web do Azure trinta por cento do tempo.

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.