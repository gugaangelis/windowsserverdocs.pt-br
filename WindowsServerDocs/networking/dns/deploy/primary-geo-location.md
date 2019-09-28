---
title: Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com servidores primários
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c313b88e2502a99baf5962a1f2eb224d67a38dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406174"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com servidores primários

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber como configurar a política DNS para permitir que servidores DNS primários respondam a consultas de cliente DNS com base na localização geográfica do cliente e do recurso ao qual o cliente está tentando se conectar, fornecendo ao cliente o AD IP a cômoda do recurso mais próximo.  
  
>[!IMPORTANT]  
>Este cenário ilustra como implantar a política DNS para o gerenciamento de tráfego baseado na localização geográfica quando você estiver usando somente servidores DNS primários. Você também pode realizar o gerenciamento de tráfego baseado na localização geográfica quando tiver servidores DNS primários e secundários. Se você tiver uma implantação primária-secundária, primeiro conclua as etapas neste tópico e, em seguida, conclua as etapas fornecidas no tópico [usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com as implantações primárias secundárias](primary-secondary-geo-location.md).

Com as novas políticas de DNS, você pode criar uma política DNS que permite que o servidor DNS responda a uma consulta de cliente solicitando o endereço IP de um servidor Web. As instâncias do servidor Web podem estar localizadas em data centers diferentes em locais físicos diferentes. O DNS pode avaliar os locais do cliente e do servidor Web e, em seguida, responder à solicitação do cliente, fornecendo ao cliente um endereço IP do servidor Web para um servidor Web localizado fisicamente mais próximo do cliente.  

Você pode usar os seguintes parâmetros de política DNS para controlar as respostas do servidor DNS para consultas de clientes DNS. 

- **Sub-rede do cliente**. Nome de uma sub-rede de cliente predefinida. Usado para verificar a sub-rede da qual a consulta foi enviada.
- **Protocolo de transporte**. Protocolo de transporte usado na consulta. As entradas possíveis são **UDP** e **TCP**.
- **Protocolo de Internet**. Protocolo de rede usado na consulta. As entradas possíveis são **IPv4** e **IPv6**.
- **Endereço IP da interface do servidor**. Endereço IP da interface de rede do servidor DNS que recebeu a solicitação DNS.
- **FQDN**. O FQDN (nome de domínio totalmente qualificado) do registro na consulta, com a possibilidade de usar um curinga.
- **Tipo de consulta**. Tipo de registro que está sendo consultado (A, SRV, TXT, etc.).
- **Hora do dia**. Hora do dia em que a consulta é recebida. 
  
Você pode combinar os critérios a seguir com um operador lógico (e/ou) para formular expressões de política. Quando essas expressões correspondem, espera-se que as políticas executem uma das ações a seguir.
 
- **Ignorar**. O servidor DNS descarta a consulta silenciosamente.          
- **Negar**. O servidor DNS responde a consulta com uma resposta de falha.          
- **Permitir**. O servidor DNS responde de volta com a resposta gerenciada de tráfego.          
  
##  <a name="bkmk_example"></a>Exemplo de gerenciamento de tráfego baseado na localização geográfica

Veja a seguir um exemplo de como você pode usar a política DNS para obter o redirecionamento de tráfego com base no local físico do cliente que executa uma consulta DNS.   
  
Este exemplo usa duas empresas fictícias: serviços de nuvem da Contoso, que fornecem soluções de Hospedagem de domínio e Web; e os serviços do Woodgrove Food, que fornecem serviços de entrega de alimentos em várias cidades em todo o mundo e que tem um site chamado woodgrove.com.  
  
Os serviços de nuvem da Contoso têm dois data centers, um nos EUA e outro na Europa. O datacenter Europeu hospeda um portal de pedidos de alimentos para woodgrove.com.   
  
Para garantir que os clientes do woodgrove.com tenham uma experiência responsiva de seu site, o Woodgrove quer clientes europeus direcionados para o datacenter Europeu e os clientes norte-americanos direcionados para o datacenter dos EUA. Os clientes localizados em outro lugar do mundo podem ser direcionados para um dos datacenters.   
  
A ilustração a seguir descreve esse cenário.  
  
![Exemplo de gerenciamento de tráfego baseado na localização geográfica](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Como funciona o processo de resolução de nomes DNS  
  
Durante o processo de resolução de nomes, o usuário tenta se conectar ao www.woodgrove.com. Isso resulta em uma solicitação de resolução de nomes DNS que é enviada para o servidor DNS que é configurado nas propriedades de conexão de rede no computador do usuário. Normalmente, esse é o servidor DNS fornecido pelo ISP local atuando como um resolvedor de cache e é chamado de LDNS.   
  
Se o nome DNS não estiver presente no cache local de LDNS, o servidor LDNS encaminhará a consulta para o servidor DNS autoritativo para woodgrove.com. O servidor DNS autoritativo responde com o registro solicitado (www.woodgrove.com) ao servidor LDNS, que, por sua vez, armazena em cache o registro localmente antes de enviá-lo ao computador do usuário.  
  
Como os serviços de nuvem da Contoso usam políticas de servidor DNS, o servidor DNS autoritativo que hospeda o contoso.com é configurado para retornar respostas gerenciadas de tráfego com base na localização geográfica. Isso resulta na direção dos clientes europeus para o datacenter Europeu e na direção dos clientes norte-americanos para o datacenter dos EUA, conforme ilustrado na ilustração.  
  
Nesse cenário, o servidor DNS autoritativo geralmente vê a solicitação de resolução de nomes proveniente do servidor LDNS e, muito raramente, do computador do usuário. Por isso, o endereço IP de origem na solicitação de resolução de nomes, como visto pelo servidor DNS autoritativo, é o do servidor LDNS e não o do computador do usuário. No entanto, o uso do endereço IP do servidor LDNS quando você configura as respostas de consulta baseadas na localização geográfica fornece uma estimativa justa da localização geográfica do usuário, pois o usuário está consultando o servidor DNS de seu ISP local.  
  
>[!NOTE]  
>As políticas de DNS utilizam o IP do remetente no pacote UDP/TCP que contém a consulta DNS. Se a consulta atingir o servidor primário por meio de vários saltos resolvedor/LDNS, a política considerará apenas o IP do último resolvedor do qual o servidor DNS recebe a consulta.  
  
##  <a name="bkmk_config"></a>Como configurar a política DNS para respostas de consulta com base na localização geográfica  
Para configurar a política DNS para respostas de consulta baseadas em localização geográfica, você deve executar as etapas a seguir.  
  
1. [Criar as sub-redes do cliente DNS](#bkmk_subnets)  
2. [Criar os escopos da zona](#bkmk_scopes)  
3. [Adicionar registros aos escopos de zona](#bkmk_records)  
4. [Criar as políticas](#bkmk_policies)  
  
>[!NOTE]  
>Você deve executar essas etapas no servidor DNS que é autoritativo para a zona que deseja configurar. A associação em **DNSAdmins**, ou equivalente, é necessária para executar os procedimentos a seguir.  
  
As seções a seguir fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]  
>As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.  
  
### <a name="bkmk_subnets"></a>Criar as sub-redes do cliente DNS  
  
A primeira etapa é identificar as sub-redes ou o espaço de endereço IP das regiões para as quais você deseja redirecionar o tráfego. Por exemplo, se você quiser redirecionar o tráfego para os EUA e Europa, precisará identificar as sub-redes ou os espaços de endereço IP dessas regiões.  
  
Você pode obter essas informações de mapas de IP geográfico. Com base nessas distribuições de IP geográfico, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas a um servidor DNS.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Para obter mais informações, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Criar escopos de zona  
Depois que as sub-redes de cliente são configuradas, você deve particionar a zona cujo tráfego você deseja redirecionar em dois escopos de zona diferentes, um escopo para cada uma das sub-redes de cliente DNS que você configurou.   
  
Por exemplo, se você quiser redirecionar o tráfego para o nome DNS www.woodgrove.com, deverá criar dois escopos de zona diferentes na zona woodgrove.com, um para os EUA e outro para a Europa.  
  
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.  
  
>[!NOTE]  
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Para obter mais informações, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Adicionar registros aos escopos de zona  
Agora você deve adicionar os registros que representam o host do servidor Web nos escopos de duas zonas.   
  
Por exemplo, **USZoneScope** e **EuropeZoneScope**. No USZoneScope, você pode adicionar o registro www.woodgrove.com com o endereço IP 192.0.0.1, que está localizado em um datacenter dos EUA; e, no EuropeZoneScope, você pode adicionar o mesmo registro (www.woodgrove.com) com o endereço IP 141.1.0.1 no datacenter Europeu.   
  
Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros aos escopos de zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Neste exemplo, você também deve usar os comandos do Windows PowerShell a seguir para adicionar registros ao escopo de zona padrão para garantir que o restante do mundo ainda possa acessar o servidor Web woodgrove.com de qualquer um dos dois data centers.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
O parâmetro **ZoneScope** não é incluído quando você adiciona um registro no escopo padrão. Isso é o mesmo que adicionar registros a uma zona DNS padrão.  
  
Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Criar as políticas  
Depois de criar as sub-redes, as partições (escopos de zona) e adicionar registros, você deve criar políticas que conectam as sub-redes e as partições, de modo que quando uma consulta vier de uma origem em uma das sub-redes de cliente DNS, a resposta de consulta será retornada de o escopo correto da zona. Nenhuma política é necessária para mapear o escopo de zona padrão.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora, o servidor DNS é configurado com as políticas de DNS necessárias para redirecionar o tráfego com base na localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nomes, o servidor DNS avalia os campos na solicitação DNS em relação às políticas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nomes corresponder a qualquer uma das políticas, o escopo de zona associado será usado para responder à consulta e o usuário será direcionado para o recurso que está geograficamente mais próximo.   
  
Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.  
  
  
