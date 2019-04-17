---
title: Usar a política DNS para aplicação de filtros em consultas do DNS
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usar a política DNS para aplicação de filtros em consultas do DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar a política DNS no Windows Server&reg; 2016 para criar filtros de consulta que são baseados em critérios que você fornecer. 

Filtros de consulta na política DNS permitem que você configure o servidor DNS para responder de forma personalizada com base na consulta de DNS e cliente DNS que envia a consulta DNS.

Por exemplo, você pode configurar a política DNS com consulta filtro lista de bloqueios que bloqueia consultas do DNS de domínios mal-intencionados conhecidos, que impede a responder às consultas desses domínios DNS. Como nenhuma resposta é enviada do servidor DNS, consulta de DNS do membro do domínio mal-intencionado expira.

Outro exemplo é criar um filtro de consulta lista de permissões que permite que apenas um conjunto específico de clientes para resolver nomes de determinados.

## <a name="bkmk_criteria"></a>Critérios de filtro de consulta
Você pode criar filtros de consulta com qualquer combinação lógica (e/ou/não) dos seguintes critérios.

|Nome|Descrição|
|-----------------|---------------------|
|Sub-rede do cliente|Nome de uma sub-rede cliente predefinidos. Usado para verificar a sub-rede da qual a consulta foi enviada.|
|Protocolo de transporte|Usado na consulta de protocolo de transporte. Valores possíveis são UDP e TCP.|
|Protocolo de Internet|Protocolo de rede usado na consulta. Valores possíveis são IPv4 e IPv6.|
|Endereço IP de Interface do servidor|Endereço IP da interface de rede do servidor DNS que recebeu a solicitação DNS|
|FQDN|Nome totalmente qualificado domínio de registro na consulta, com a possibilidade de usar um caractere curinga.|
|Tipo de consulta|Tipo de registro que está sendo consultado \ (A, SRV, TXT, etc. \)|
|Hora do dia|Hora do dia que a consulta é recebida.|

Os exemplos a seguir mostram como criar filtros para política DNS que qualquer bloquear ou permitir consultas de resolução de nomes DNS.

>[!NOTE]
>Os comandos de exemplo neste tópico usam o comando do Windows PowerShell **adicionar DnsServerQueryResolutionPolicy**. Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx). 

##<a name="bkmk_block1"></a>Consultas de bloco de um domínio

Em algumas circunstâncias, convém bloquear a resolução de nomes DNS para domínios que você identificou como mal-intencionado ou para domínios que não são compatíveis com as diretrizes de uso da sua organização. Você pode fazer consultas de bloqueio para domínios usando a política DNS.

A política que você configurar neste exemplo não é criada em qualquer região específico – em vez disso, você pode criar uma política de nível de servidor que é aplicado a todas as regiões configuradas no servidor DNS. As políticas de nível de servidor são os primeiros a ser avaliado e, portanto, primeiro a ser correspondida quando uma consulta é recebida pelo servidor DNS.

O comando de exemplo a seguir configura uma política de nível de servidor para bloquear qualquer consultas com o domínio **sufixo contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando você configura o **ação** parâmetro com o valor **ignorar**, o servidor DNS está configurado para soltar consultas sem resposta em todos. Isso faz com que o cliente DNS no domínio mal-intencionado tempo limite.

##<a name="bkmk_block2"></a>Consultas de bloco de uma sub-rede
Com este exemplo, você pode bloquear consultas de uma sub-rede se ele for encontrado para ser infectado por alguns tipos de malware e está tentando entrar em contato com sites mal-intencionados usando seu servidor DNS. 

' DnsServerClientSubnet Adicionar-nome "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24 - passagem

Adicionar DnsServerQueryResolutionPolicy-nome "BlockListPolicyMalicious06"-ação ignorar - ClientSubnet "EQ, MaliciousSubnet06" - passagem '

O exemplo a seguir demonstra como você pode usar os critérios de sub-rede em combinação com os critérios FQDN para bloquear consultas para determinados domínios mal-intencionados de sub-redes infectados.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Bloquear um tipo de consulta
Talvez seja necessário bloquear a resolução de nomes para certos tipos de consultas em seus servidores. Por exemplo, você pode bloquear a consulta 'ANY', que pode ser usada de maneira mal-intencionada criem amplificação ataques.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Permitir consultas somente de um domínio
Você não pode usar política DNS para bloquear consultas apenas, você pode usá-los para aprovar automaticamente consultas de domínios específicos ou sub-redes. Quando você configura permitir listas, o servidor DNS apenas processa consultas de domínios permitidos, bloqueando todas as outras consultas de outros domínios.

O comando de exemplo a seguir permite que apenas os computadores e dispositivos nos domínios contoso.com e filho para consultar o servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Permitir consultas somente a partir de uma sub-rede
Você também pode criar listas de permissões para sub-redes IP, para que todas as consultas não originário essas sub-redes são ignoradas.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Permitir que apenas determinados QTypes
Você pode aplicar a listas de permissões para QTYPEs. 

Por exemplo, se você tiver clientes externos consultando a interface de servidor DNS 164.8.1.1, somente determinados QTYPEs podem ser consultados, apesar de existirem outras QTYPEs como registros SRV ou TXT que são usados pelos servidores internos para resolução de nomes ou para fins de monitoramento.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas. 
