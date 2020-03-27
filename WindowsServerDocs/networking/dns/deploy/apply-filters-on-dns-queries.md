---
title: Usar a Política de DNS para a aplicação de filtros em consultas DNS
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 45dad1eb40caba7ac304fc640e3d56044254f08c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317837"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usar a Política de DNS para a aplicação de filtros em consultas DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS no Windows Server&reg; 2016 para criar filtros de consulta baseados em critérios fornecidos por você. 

Os filtros de consulta na política DNS permitem configurar o servidor DNS para responder de uma maneira personalizada com base na consulta DNS e no cliente DNS que envia a consulta DNS.

Por exemplo, você pode configurar a política DNS com a lista de blocos de filtro de consulta que bloqueia consultas DNS de domínios mal-intencionados conhecidos, o que impede que o DNS responda a consultas desses domínios. Como nenhuma resposta é enviada do servidor DNS, a consulta DNS do membro do domínio mal-intencionado atinge o tempo limite.

Outro exemplo é criar uma lista de permissões de filtro de consulta que permita que apenas um conjunto específico de clientes resolva determinados nomes.

## <a name="query-filter-criteria"></a><a name="bkmk_criteria"></a>Critérios de filtro de consulta
Você pode criar filtros de consulta com qualquer combinação lógica (e/ou/não) dos critérios a seguir.

|{1&gt;Nome&lt;1}|Descrição|
|-----------------|---------------------|
|Sub-rede do cliente|Nome de uma sub-rede de cliente predefinida. Usado para verificar a sub-rede da qual a consulta foi enviada.|
|Protocolo de transporte|Protocolo de transporte usado na consulta. Os valores possíveis são UDP e TCP.|
|Protocolo de Internet|Protocolo de rede usado na consulta. Os valores possíveis são IPv4 e IPv6.|
|Endereço IP da interface do servidor|Endereço IP da interface de rede do servidor DNS que recebeu a solicitação DNS.|
|FQDN|Nome de domínio totalmente qualificado do registro na consulta, com a possibilidade de usar um curinga.|
|Tipo de consulta|Tipo de registro que está sendo consultado \(um, SRV, TXT, etc.\).|
|Hora do dia|Hora do dia em que a consulta é recebida.|

Os exemplos a seguir mostram como criar filtros para a política DNS que bloqueiam ou permitem consultas de resolução de nomes DNS.

>[!NOTE]
>Os comandos de exemplo neste tópico usam o comando do Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

## <a name="block-queries-from-a-domain"></a><a name="bkmk_block1"></a>Bloquear consultas de um domínio

Em algumas circunstâncias, talvez você queira bloquear a resolução de nomes DNS para domínios identificados como mal-intencionados ou para domínios que não estão em conformidade com as diretrizes de uso de sua organização. Você pode executar consultas de bloqueio para domínios usando a política DNS.

A política que você configura neste exemplo não é criada em nenhuma zona específica – em vez disso, você cria uma política no nível do servidor que é aplicada a todas as zonas configuradas no servidor DNS. As políticas de nível de servidor são as primeiras a serem avaliadas e, portanto, devem ser correspondidas primeiro quando uma consulta é recebida pelo servidor DNS.

O comando de exemplo a seguir configura uma política de nível de servidor para bloquear qualquer consulta com o sufixo de domínio **contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando você configura o parâmetro **Action** com o valor **ignore**, o servidor DNS é configurado para descartar consultas sem nenhuma resposta. Isso faz com que o cliente DNS no domínio mal-intencionado expire.

## <a name="block-queries-from-a-subnet"></a><a name="bkmk_block2"></a>Bloquear consultas de uma sub-rede
Com este exemplo, você pode bloquear consultas de uma sub-rede se ela for considerada infectada por algum malware e estiver tentando entrar em contato com sites mal-intencionados usando seu servidor DNS. 

' Add-DnsServerClientSubnet-Name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

Add-DnsServerQueryResolutionPolicy-Name "BlockListPolicyMalicious06"-ação IGNORE-ClientSubnet "EQ, MaliciousSubnet06"-PassThru "

O exemplo a seguir demonstra como você pode usar os critérios de sub-rede em combinação com os critérios de FQDN para bloquear consultas para determinados domínios mal-intencionados de sub-redes infectadas.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="block-a-type-of-query"></a><a name="bkmk_block3"></a>Bloquear um tipo de consulta
Talvez seja necessário bloquear a resolução de nomes para determinados tipos de consultas nos servidores. Por exemplo, você pode bloquear a consulta ' ANY ', que pode ser usada maliciosamente para criar ataques de amplificação.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="allow-queries-only-from-a-domain"></a><a name="bkmk_allow1"></a>Permitir consultas somente de um domínio
Você não pode usar apenas a política DNS para bloquear consultas, você pode usá-las para aprovar automaticamente as consultas de domínios ou sub-redes específicas. Quando você configura listas de permissões, o servidor DNS processa apenas consultas de domínios permitidos, ao mesmo tempo que bloqueia todas as outras consultas de outros domínios.

O comando de exemplo a seguir permite que somente computadores e dispositivos nos domínios contoso.com e filho consultem o servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="allow-queries-only-from-a-subnet"></a><a name="bkmk_allow2"></a>Permitir consultas somente de uma sub-rede
Você também pode criar listas de permissões para sub-redes IP, para que todas as consultas que não se originam dessas sub-redes sejam ignoradas.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="allow-only-certain-qtypes"></a><a name="bkmk_allow3"></a>Permitir apenas determinados QTypes
Você pode aplicar listas de permissão a QTYPEs. 

Por exemplo, se você tiver clientes externos consultando a interface do servidor DNS 164.8.1.1, somente certas QTYPEs poderão ser consultadas, enquanto há outros QTYPEs, como registros SRV ou TXT, que são usados por servidores internos para resolução de nomes ou para fins de monitoramento.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada. 
