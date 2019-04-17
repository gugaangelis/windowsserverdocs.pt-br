---
title: Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com servidores primários
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com servidores primários

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar a política de DNS para permitir que os servidores DNS primários responder a consultas do cliente DNS com base na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o endereço IP do recurso mais próximo.  
  
>[!IMPORTANT]  
>Esse cenário ilustra como implantar política DNS para gerenciamento de tráfego com base em localização geográfica quando você estiver usando servidores DNS primários apenas. Você também pode realizar o gerenciamento de tráfego com base em localização geográfica quando você tem servidores DNS primário e secundários. Se você tiver uma implantação primário secundário, primeiro, conclua as etapas neste tópico e, em seguida, conclua as etapas que são fornecidas no tópico [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com implantações de primário secundário](primary-secondary-geo-location.md).

Com as novas políticas DNS, você pode criar uma política DNS que permite que o servidor DNS responder a uma consulta de cliente solicita o endereço IP de um servidor Web. Instâncias do servidor Web podem estar localizadas em datacenters diferentes em diferentes locais físicos. DNS pode avaliar o cliente e locais do servidor Web, e depois responder à solicitação do cliente, fornecendo o cliente com um servidor Web endereço IP para um servidor Web que encontra-se fisicamente mais próxima ao cliente.  

Você pode usar os seguintes parâmetros de política DNS para controlar as respostas do servidor DNS para consultas de clientes DNS. 

- **Cliente sub-rede**. Nome de uma sub-rede cliente predefinidos. Usado para verificar a sub-rede da qual a consulta foi enviada.
- **Protocolo de transporte**. Usado na consulta de protocolo de transporte. Possíveis entradas são **UDP** e **TCP**.
- **Protocolo de Internet**. Protocolo de rede usado na consulta. Possíveis entradas são **IPv4** e **IPv6**.
- **Endereço de IP da Interface de servidor**. Endereço IP da interface de rede do servidor DNS que recebeu a solicitação DNS.
- **FQDN**. O totalmente qualificado domínio Name (FQDN) do registro na consulta, com a possibilidade de usar um caractere curinga.
- **Tipo de consulta**. Tipo de registro está sendo consultado (A, SRV, TXT, etc.).
- **Hora do dia**. Hora do dia que a consulta é recebida. 
  
Você pode combinar os seguintes critérios com um operador lógico (e/ou) para formular expressões de política. Quando esses expressões corresponderem, as políticas devem realizar uma das seguintes ações.
 
- **Ignorar**. O servidor DNS silenciosamente solta a consulta.          
- **Negar**. O servidor DNS responde a essa consulta com uma resposta de falha.          
- **Permitir**. O servidor DNS responde novamente com a resposta de tráfego gerenciado.          
  
##  <a name="bkmk_example"></a>Localização geográfica com base em exemplo de gerenciamento de tráfego

Este é um exemplo de como você pode usar política DNS para obter o redirecionamento do tráfego com base na localização física do cliente que executa uma consulta DNS.   
  
Este exemplo usa duas empresas fictícias - Contoso serviços em nuvem, que fornece a web e domínio que hospeda soluções; e serviços de comida Woodgrove, que fornece serviços de entrega de comida em várias cidades em todo o mundo, e que tem um site denominado woodgrove.com.  
  
Serviços de nuvem Contoso tem dois centros de dados, um nos EUA e outro na Europa. O datacenter Europeu hospeda um alimento ordenação portal para woodgrove.com.   
  
Para garantir que os clientes de woodgrove.com tenham uma experiência responsiva de seu site, Woodgrove deseja europeus clientes direcionados para o datacenter Europeia e da América direcionado para o datacenter dos EUA. Clientes localizados em outro lugar do mundo podem ser direcionados a ambos os datacenters.   
  
A ilustração a seguir ilustra esse cenário.  
  
![Localização geográfica com base em exemplo de gerenciamento de tráfego](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Como o DNS nomeia o processo de resolução funciona  
  
Durante o processo de resolução de nome, o usuário tenta se conectar a www.woodgrove.com. Isso resulta em uma solicitação de resolução de nome DNS que é enviada ao servidor DNS que está configurado nas propriedades da Conexão de rede no computador do usuário. Normalmente, isso é o servidor DNS fornecido pelo provedor local atuando como um resolvedor de cache e é conhecido como o LDNS.   
  
Se o nome DNS não estiver presente no cache local de LDNS, o servidor LDNS encaminha a consulta para o servidor DNS autoritativo para woodgrove.com. O servidor DNS autoritativo responde com o registro solicitado (www.woodgrove.com) para o servidor LDNS, que por sua vez armazena em cache o registro localmente antes de enviá-lo para o computador do usuário.  
  
Como os serviços de nuvem Contoso usa políticas de servidor DNS, o servidor DNS autoritativo que hospeda contoso.com está configurado para retornar a localização geográfica com base tráfego gerenciado respostas. Isso resulta em direção de clientes na Europa datacenter europeu e a direção de clientes americano para datacenter dos EUA, conforme mostrado na ilustração.  
  
Nesse cenário, o servidor DNS autoritativo geralmente vê a solicitação de resolução de nome vindo do servidor LDNS e, raramente, do computador do usuário. Por isso, o endereço IP de origem na solicitação de resolução de nome, como visto pelo servidor DNS autoritativo é que o servidor LDNS e não do que o computador do usuário. No entanto, usando o endereço IP do servidor LDNS quando você configura a localização geográfica com base em consulta respostas fornece uma estimativa razoável de localização geográfica do usuário, porque o usuário está consultando o servidor DNS de seu provedor local.  
  
>[!NOTE]  
>Políticas DNS utilizam IP o remetente no pacote TCP/UDP que contém a consulta DNS. Se a consulta atinge o servidor principal por meio de vários saltos resolvedor/LDNS, a política considerará apenas IP do último resolvedor do qual o servidor DNS recebe a consulta.  
  
##  <a name="bkmk_config"></a>Como configurar a política de DNS para respostas de consulta com base em localização geográfica  
Para configurar a política DNS para respostas de consulta com base em localização geográfica, você deve executar as etapas a seguir.  
  
1. [Criar as sub-redes do cliente DNS](#bkmk_subnets)  
2. [Criar os escopos da zona](#bkmk_scopes)  
3. [Adicionar registros os escopos de zona](#bkmk_records)  
4. [Criar as políticas](#bkmk_policies)  
  
>[!NOTE]  
>Você deve executar estas etapas no servidor DNS autoritativo para a zona que você deseja configurar. A associação ao grupo **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
As seguintes seções fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]  
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos.  
  
### <a name="bkmk_subnets"></a>Criar as sub-redes do cliente DNS  
  
A primeira etapa é identificar as sub-redes ou espaço de endereço IP das regiões para o qual você quer redirecionar o tráfego. Por exemplo, se você quer redirecionar o tráfego para os EUA e Europa, você precisa identificar as sub-redes ou espaços de endereço IP dessas regiões.  
  
Você pode obter essas informações de mapas de IP de localização geográfica. De acordo com essas distribuições de IP de localização geográfica, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 da qual as consultas são enviadas para um servidor DNS.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes do cliente DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Para obter mais informações, consulte [adicionar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Criar escopos de zona  
Depois que as sub-redes de cliente são configuradas, você deve particionar zona da qual você quer redirecionar em dois escopos de zona diferente, o tráfego um escopo para cada uma das sub-redes cliente DNS que você configurou.   
  
Por exemplo, se você quer redirecionar o tráfego para o nome DNS www.woodgrove.com, você deve criar dois escopos de zona diferentes na zona da woodgrove.com, um para os EUA e outra para Europa.  
  
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP.  
  
>[!NOTE]  
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Para obter mais informações, consulte [adicionar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Adicionar registros os escopos de zona  
Agora você deve adicionar os registros que representa o host do servidor da web para os escopos de dois zona.   
  
Por exemplo, **USZoneScope** e **EuropeZoneScope**. Em USZoneScope, você pode adicionar o registro www.woodgrove.com com o endereço IP 192.0.0.1, que está localizado em um datacenter EUA; e, em EuropeZoneScope você pode adicionar o mesmo registro (www.woodgrove.com) com o endereço IP 141.1.0.1 no datacenter europeu.   
  
Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros os escopos de zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Neste exemplo, você também deve usar os seguintes comandos do Windows PowerShell para adicionar registros no escopo da região padrão para garantir que o resto do mundo ainda pode servidor de web do acesso a woodgrove.com partir destes dois centros de dados.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
O **ZoneScope** parâmetro não está incluído quando você adiciona um registro no escopo padrão. Isso equivale a adição de registros para uma zona DNS padrão.  
  
Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Criar as políticas  
Depois que você criou as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que conectam as sub-redes e partições, para que quando uma consulta vem de uma fonte em um das sub-redes do cliente DNS, a resposta da consulta é retornada do escopo da zona correto. Não há políticas são necessárias para mapear o escopo da região padrão.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego com base na localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação DNS contra as políticas DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associado é usado para responder à consulta e o usuário será direcionado para o recurso que é geograficamente closest-los.   
  
Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.  
  
  
