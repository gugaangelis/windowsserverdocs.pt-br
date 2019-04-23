---
title: Usar a Política de DNS para a aplicação de filtros em consultas DNS
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b00c773462569f005a73f535b1a872ae7b389db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859897"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usar a Política de DNS para a aplicação de filtros em consultas DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS no Windows Server&reg; 2016 para criar filtros de consulta que são baseados em critérios fornecidos por você. 

Filtros de consulta na política de DNS permitem que você configure o servidor DNS para responder de maneira personalizada com base na consulta DNS e do cliente DNS que envia a consulta DNS.

Por exemplo, você pode configurar a política de DNS com o filtro de consulta lista de bloqueios que bloqueia consultas DNS de domínios mal-intencionados conhecidos, que impede que o DNS respondendo a consultas desses domínios. Como nenhuma resposta é enviada do servidor DNS, do membro domínio mal-intencionado DNS tempo limite da consulta.

Outro exemplo é criar um filtro de consulta de lista de permissões que permite que apenas um conjunto específico de clientes para resolver nomes de determinados.

## <a name="bkmk_criteria"></a> Critérios de filtro de consulta
Você pode criar filtros de consulta com qualquer combinação lógica (e/ou/não) dos critérios a seguir.

|Nome|Descrição|
|-----------------|---------------------|
|Subrede de cliente|Nome de uma sub-rede de cliente predefinidos. Usado para verificar a sub-rede da qual a consulta foi enviada.|
|Protocolo de transporte|Transporte de protocolo usado na consulta. Os valores possíveis são TCP e UDP.|
|Protocolo de Internet|Protocolo de rede usado na consulta. Os valores possíveis são IPv4 e IPv6.|
|Endereço IP da Interface do servidor|Endereço IP do adaptador de rede do servidor DNS que recebeu a solicitação DNS.|
|FQDN|Nome totalmente qualificado domínio do registro na consulta, com a possibilidade de usar um caractere curinga.|
|Tipo de Consulta|Tipo de registro que está sendo consultado \(A, SRV, TXT, etc.\).|
|Hora do dia|Hora do dia em que a consulta for recebida.|

Os exemplos a seguir mostram como criar filtros para a política DNS que qualquer bloco ou permitir consultas de resolução de nomes DNS.

>[!NOTE]
>Os comandos de exemplo neste tópico usam o comando do Windows PowerShell **DnsServerQueryResolutionPolicy adicionar**. Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

##<a name="bkmk_block1"></a>Consultas de bloco de um domínio

Em algumas circunstâncias, você talvez queira bloquear a resolução de nomes DNS para domínios que você identificou como mal-intencionado, ou para domínios que não são compatíveis com as diretrizes de uso da sua organização. Você pode realizar consultas de bloqueio para domínios por meio da política DNS.

A política que você configura neste exemplo não é criada em qualquer região específica – em vez disso, você pode criar uma política de nível de servidor que é aplicado a todas as regiões configuradas no servidor DNS. Políticas no nível do servidor são a primeira a ser avaliada e, portanto, primeiro a ser correspondido quando uma consulta é recebido pelo servidor DNS.

O comando de exemplo a seguir configura uma política de nível de servidor para bloquear todas as consultas com o domínio **sufixo contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quando você configura a **ação** parâmetro com o valor **ignorar**, o servidor DNS está configurado para soltar consultas sem resposta em todos os. Isso faz com que o cliente DNS no domínio mal-intencionado para atingir o tempo limite.

##<a name="bkmk_block2"></a>Consultas de bloco de uma sub-rede
Com este exemplo, você pode bloquear a consultas de uma sub-rede se ela for encontrada para serem infectados por algum malware e está tentando entrar em contato com os sites mal-intencionados usando seu servidor DNS. 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

Adicionar-DnsServerQueryResolutionPolicy-nome "BlockListPolicyMalicious06"-ação ignorar - ClientSubnet "MaliciousSubnet06 EQ" - PassThru '

O exemplo a seguir demonstra como você pode usar os critérios de sub-rede em combinação com os critérios FQDN para bloqueie as consultas para determinados domínios mal-intencionados de sub-redes infectados.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Bloquear um tipo de consulta
Talvez seja necessário bloquear a resolução de nomes de determinados tipos de consultas em seus servidores. Por exemplo, você pode bloquear a consulta 'ANY', que pode ser usada de maneira mal-intencionada para criar ataques de amplificação.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Permitir consultas somente de um domínio
Você só não pode usar a política de DNS a consultas de bloco, você pode usá-los para aprovar automaticamente as consultas de domínios específicos ou sub-redes. Quando você configura a listas de permissões, o servidor DNS só processa consultas de domínios permitidos, e o bloqueio de todas as outras consultas de outros domínios.

O comando de exemplo a seguir permite que somente os computadores e dispositivos nos domínios contoso.com e filho para consultar o servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Permitir consultas apenas de uma sub-rede
Você também pode criar listas de permissões para sub-redes IP, para que todas as consultas não provenientes dessas sub-redes são ignoradas.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Permitir que somente determinados QTypes
Você pode aplicar a listas de permissões para QTYPEs. 

Por exemplo, se você tiver clientes externos, consultando a interface de servidor DNS 164.8.1.1, somente determinados QTYPEs são permitidas para serem consultados, embora haja outras QTYPEs como os registros SRV ou TXT, que são usados por servidores internos para resolução de nomes ou para fins de monitoramento.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada. 
