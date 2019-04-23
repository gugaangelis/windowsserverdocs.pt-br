---
title: Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com servidores primários
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110014adf1e23be574f23efc01e8a4d69397e547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831977"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com servidores primários

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a configurar a política de DNS para permitir que os servidores DNS primários responder a consultas de cliente DNS baseadas na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o anúncio IP total do recurso mais próximo.  
  
>[!IMPORTANT]  
>Esse cenário ilustra como implantar política de DNS para o gerenciamento de tráfego com base em localização geográfica, quando você estiver usando servidores DNS primários apenas. Você também pode realizar o gerenciamento de tráfego com base em localização geográfica, quando você tiver servidores DNS primário e secundários. Se você tiver uma implantação de primárias e secundárias, primeiro conclua as etapas neste tópico e, em seguida, conclua as etapas que são fornecidas no tópico [usar política de DNS para o gerenciamento de tráfego com base em localização geográfica com implantações primárias e secundárias](primary-secondary-geo-location.md).

Com as novas políticas DNS, você pode criar uma política DNS que permite que o servidor DNS responder a uma consulta de cliente solicitando o endereço IP de um servidor Web. Instâncias do servidor Web podem estar localizadas em diferentes data centers em locais físicos diferentes. DNS pode avaliar o cliente e locais do servidor Web, em seguida, responder à solicitação do cliente, fornecendo o cliente com um servidor Web endereço IP para um servidor Web que se encontra fisicamente mais próximo ao cliente.  

Você pode usar os seguintes parâmetros de política DNS para controlar as respostas do servidor DNS a consultas de clientes DNS. 

- **Subrede de cliente**. Nome de uma sub-rede de cliente predefinidos. Usado para verificar a sub-rede da qual a consulta foi enviada.
- **Protocolo de transporte**. Transporte de protocolo usado na consulta. São entradas possíveis **UDP** e **TCP**.
- **Protocolo de Internet**. Protocolo de rede usado na consulta. São entradas possíveis **IPv4** e **IPv6**.
- **Endereço IP da Interface de servidor**. Endereço IP do adaptador de rede do servidor DNS que recebeu a solicitação DNS.
- **FQDN**. O domínio nome FQDN (totalmente qualificado) do registro na consulta, com a possibilidade de usar um caractere curinga.
- **Tipo de consulta**. Tipo de registro que está sendo consultado (A, SRV, TXT, etc.).
- **Hora do dia**. Hora do dia em que a consulta for recebida. 
  
Você pode combinar os critérios a seguir com um operador lógico (e/ou) para formular expressões de política. Quando essas expressões correspondem, as políticas devem executar uma das seguintes ações.
 
- **Ignorar**. O servidor DNS descarta silenciosamente a consulta.          
- **Negar**. O servidor DNS responde a essa consulta com uma resposta de falha.          
- **Permitir**. O servidor DNS responde de volta com o Gerenciador de tráfego de resposta.          
  
##  <a name="bkmk_example"></a>Exemplo de gerenciamento de tráfego com base de localização geográfica

A seguir está um exemplo de como você pode usar a política de DNS para alcançar o redirecionamento do tráfego com base em um local físico do cliente que executa uma consulta DNS.   
  
Este exemplo usa duas empresas fictícias - serviços de nuvem da Contoso, que fornece web e domínio que hospeda as soluções; e Woodgrove Food Services, que fornece serviços de entrega de alimento em várias cidades em todo o mundo, e que tem um site da Web denominado woodgrove.com.  
  
Serviços de nuvem da Contoso tem dois data centers, um dos EUA e outro na Europa. Data centers europeus hospeda um portal para woodgrove.com de ordenação de alimentos.   
  
Para garantir que os clientes de woodgrove.com obtenham uma experiência responsiva de seu site, o Woodgrove deseja os clientes europeus direcionados para o data center europeu e American direcionado para o datacenter dos EUA. Os clientes localizados em outro lugar no mundo podem ser direcionados para qualquer um dos data centers.   
  
A ilustração a seguir ilustra esse cenário.  
  
![Exemplo de gerenciamento de tráfego com base de localização geográfica](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Como o processo de resolução de nomes do DNS funciona  
  
Durante o processo de resolução de nome, o usuário tenta se conectar ao www.woodgrove.com. Isso resulta em uma solicitação de resolução de nome DNS é enviada para o servidor DNS configurado nas propriedades de Conexão de rede no computador do usuário. Normalmente, isso é o servidor DNS fornecido pelo ISP local que atua como um resolvedor de cache e é conhecido como o LDNS.   
  
Se o nome DNS não estiver presente no cache local de LDNS, o servidor LDNS encaminha a consulta para o servidor DNS autoritativo para woodgrove.com. O servidor DNS autoritativo responde com o registro solicitado (www.woodgrove.com) para o servidor LDNS, que por sua vez armazena em cache o registro localmente antes de enviá-la para o computador do usuário.  
  
Como os serviços de nuvem Contoso usa políticas de servidor DNS, o servidor DNS autoritativo que hospeda o contoso.com é configurado para retornar respostas de Gerenciador de tráfego com base em localização geográfica. Isso resulta na direção dos clientes europeus Europeu datacenter e a direção de clientes na América para o datacenter dos EUA, conforme mostrado na ilustração.  
  
Nesse cenário, o servidor DNS autoritativo geralmente vê a solicitação de resolução de nome provenientes do servidor LDNS e, muito raramente, de computador do usuário. Por isso, o endereço IP de origem na solicitação de resolução de nome, como visto pelo servidor DNS autoritativo é do servidor LDNS e não do computador do usuário. No entanto, usar o endereço IP do servidor LDNS quando você configura a localização geográfica com base em consulta respostas fornece uma estimativa razoável de localização geográfica do usuário, porque o usuário está consultando o servidor DNS do seu ISP local.  
  
>[!NOTE]  
>Políticas de DNS utilizam o IP do remetente no pacote TCP/UDP que contém a consulta DNS. Se a consulta atinge o servidor primário por meio de vários saltos de resolvedor/LDNS, a política de considerar somente o IP do resolvedor último do qual o servidor DNS recebe a consulta.  
  
##  <a name="bkmk_config"></a>Como configurar a política de DNS para respostas de consultas com base em localização geográfica  
Para configurar a política de DNS para respostas de consultas com base em localização geográfica, você deve executar as etapas a seguir.  
  
1. [Crie as sub-redes de cliente DNS](#bkmk_subnets)  
2. [Criar os escopos da zona](#bkmk_scopes)  
3. [Adicionar registros para os escopos de zona](#bkmk_records)  
4. [Crie as políticas](#bkmk_policies)  
  
>[!NOTE]  
>Você deve executar essas etapas no servidor DNS autoritativo para a zona que você deseja configurar. Associação na **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
As seções a seguir fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]  
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos.  
  
### <a name="bkmk_subnets"></a>Crie as sub-redes de cliente DNS  
  
A primeira etapa é identificar o espaço de endereço IP das regiões para o qual você deseja redirecionar o tráfego ou sub-redes. Por exemplo, se você deseja redirecionar o tráfego para os EUA e Europa, você precisa identificar as sub-redes ou espaços de endereço IP dessas regiões.  
  
Você pode obter essas informações de mapas de Geo-IP. Com base nessas distribuições IP geográfico, você deve criar as "sub-redes de cliente DNS". Uma sub-rede de cliente DNS é um agrupamento lógico de sub-redes IPv4 ou IPv6 do qual as consultas são enviadas para um servidor DNS.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar sub-redes de cliente DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Para obter mais informações, consulte [DnsServerClientSubnet adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Criar escopos de zona  
Depois que as sub-redes de cliente são configuradas, você deve particionar a zona cujo tráfego você deseja redirecionar em dois escopos de zona diferente, um escopo para cada uma das sub-redes cliente DNS que você configurou.   
  
Por exemplo, se você deseja redirecionar o tráfego para o DNS nome www.woodgrove.com, você deve criar dois escopos de zona diferente na zona woodgrove.com, um para os EUA e outro para Europa.  
  
Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP.  
  
>[!NOTE]  
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar escopos de zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Adicionar registros para os escopos de zona  
Agora você deve adicionar os registros que representa o host do servidor web para os escopos de zona de dois.   
  
Por exemplo, **USZoneScope** e **EuropeZoneScope**. No USZoneScope, você pode adicionar www.woodgrove.com o registro com o endereço IP 192.0.0.1, que está localizado em um datacenter dos EUA; e, em EuropeZoneScope você pode adicionar o mesmo registro (www.woodgrove.com) com o endereço IP 141.1.0.1 no data center europeu.   
  
Você pode usar os seguintes comandos do Windows PowerShell para adicionar registros para os escopos de zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Neste exemplo, você também deve usar os seguintes comandos do Windows PowerShell para adicionar registros em que o escopo da região padrão para garantir que o resto do mundo ainda pode acessar o servidor de web woodgrove.com de qualquer um dos dois datacenters.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
O **ZoneScope** parâmetro não for incluído quando você adiciona um registro no escopo padrão. Isso é o mesmo que adicionar registros a uma zona DNS padrão.  
  
Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Crie as políticas  
Depois de ter criado as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que se conectam a sub-redes e partições, para que quando uma consulta vem de uma fonte em uma das sub-redes de cliente DNS, a resposta de consulta é retornada de o escopo correto da zona. Não há políticas são necessárias para o escopo da região padrão de mapeamento.   
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula-se as sub-redes de cliente DNS e os escopos de zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora o servidor DNS é configurado com as políticas DNS necessárias para redirecionar o tráfego com base em localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação de DNS com as diretivas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associada é usado para responder à consulta e o usuário é direcionado para o recurso que seja geograficamente mais próximo a eles.   
  
Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.  
  
  
