---
title: Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com implantações primárias e secundárias
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b11064e6b3bd2590d5712afdb7afc69de1ed83f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889697"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com implantações primárias e secundárias

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a criar política de DNS para o gerenciamento de tráfego com base em localização geográfica, quando sua implantação de DNS inclui servidores DNS primário e secundários.  

No cenário anterior, [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](primary-geo-location.md), fornecidas instruções para configurar a política de DNS para o gerenciamento de tráfego com base em localização geográfica em um servidor DNS primário. Na infraestrutura de Internet, no entanto, os servidores DNS são amplamente implantados em um modelo de primárias e secundárias, onde a cópia gravável de uma zona é armazenada nos servidores primários select e seguras, e cópias somente leitura da zona são mantidas em vários servidores secundários.   
  
Os servidores secundários usam os protocolos de transferência de zona transferência autoritativo (AXFR) e a transferência de zona Incremental (IXFR) para solicitar e receber atualizações de zona que incluem novas alterações para as zonas nos servidores DNS primários.   
  
>[!NOTE]
>Para obter mais informações sobre transações AXFR, consulte Internet Engineering Task Force (IETF) [solicitação de comentários 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para obter mais informações sobre IXFR, consulte Internet Engineering Task Force (IETF) [solicitação de comentários de 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="bkmk_example"></a>Exemplo de gerenciamento de tráfego com base de localização geográfica primárias e secundárias  
A seguir está um exemplo de como você pode usar a política de DNS em uma implantação de primárias e secundárias para alcançar o redirecionamento do tráfego com base em um local físico do cliente que executa uma consulta DNS.  
  
Este exemplo usa duas empresas fictícias - serviços de nuvem da Contoso, que fornece web e domínio que hospeda as soluções; e Woodgrove Food Services, que fornece serviços de entrega de alimento em várias cidades em todo o mundo, e que tem um site da Web denominado woodgrove.com.  
  
Para garantir que os clientes de woodgrove.com obtenham uma experiência responsiva de seu site, o Woodgrove deseja os clientes europeus direcionados para o data center europeu e American direcionado para o datacenter dos EUA. Os clientes localizados em outro lugar no mundo podem ser direcionados para qualquer um dos data centers.  
  
Serviços de nuvem da Contoso tem dois data centers, um dos EUA e outro na Europa, no qual a Contoso hospeda o seu portal para woodgrove.com de ordenação de alimentos.  
  
A implantação do DNS da Contoso inclui dois servidores secundários: **SecondaryServer1**, com o endereço IP 10.0.0.2; e **SecondaryServer2**, com o endereço IP 10.0.0.3. Esses servidores secundários são que atuam como servidores de nome em duas regiões diferentes, com SecondaryServer1 localizado na Europa e SecondaryServer2 localizados nos Estados Unidos
  
Há uma cópia gravável de zona primária em **PrimaryServer** (endereço IP 10.0.0.1), em que as alterações de zona são feitas. Com transferências de zona regulares para os servidores secundários, os servidores secundários são sempre atualizados com todas as novas alterações à zona em que o PrimaryServer.
  
A ilustração a seguir ilustra esse cenário.
  
![Exemplo de gerenciamento de tráfego com base de localização geográfica primárias e secundárias](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>Como funciona o sistema de primárias e secundárias do DNS

Quando você implanta o gerenciamento de tráfego com base em localização geográfica em uma implantação de DNS primário secundário, é importante entender como normal zona primária-secundária transferências ocorrerem antes de aprender sobre transferências de nível de escopo de zona. As seções a seguir fornecem informações sobre a zona e transferências de nível de escopo de zona.  
  
- [Transferências de zona em uma implantação de primárias e secundárias do DNS](#bkmk_zone)  
- [Transfere o nível de escopo de zona em uma implantação de primárias e secundárias do DNS](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>Transferências de zona em uma implantação de primárias e secundárias do DNS

Você pode criar uma implantação de primárias e secundárias do DNS e sincronizar as zonas com as etapas a seguir.  
1. Quando você instala o DNS, a zona primária é criada no servidor DNS primário.  
2. No servidor secundário, crie as zonas e especifique os servidores primários.   
3. Nos servidores primários, você pode adicionar os servidores secundários como secundários confiáveis na zona primária.   
4. As zonas secundárias fazer uma solicitação de transferência de zona completa (AXFR) e recebam uma cópia da zona.   
5. Quando necessário, os servidores primários enviam notificações para os servidores secundários sobre atualizações de zona.  
6. Servidores secundários fazem uma solicitação de transferência de zona incremental (IXFR). Por isso, os servidores secundários permanecem sincronizados com o servidor primário.   
  
### <a name="bkmk_scope"></a>Transfere o nível de escopo de zona em uma implantação de primárias e secundárias do DNS

O cenário de gerenciamento de tráfego requer etapas adicionais para particionar as zonas em escopos de zona diferente. Por isso, etapas adicionais são necessárias para transferir os dados dentro de escopos de zona para os servidores secundários e para transferir as políticas e subredes de cliente DNS para os servidores secundários.   
  
Depois de configurar sua infraestrutura DNS com os servidores primário e secundário, transferências de nível de escopo de zona são executadas automaticamente pelo DNS, usando os seguintes processos.  
  
Para garantir a transferência de nível de escopo de zona, servidores DNS usam os mecanismos de extensão para DNS (EDNS0) aceitar RR. Todas as solicitações de transferência (AXFR ou IXFR) zona das zonas com escopos originam-se com um EDNS0 aceitar RR, cuja ID de opção é definida para "65433" por padrão. Para obter mais informações sobre EDNSO, consulte IETF [solicitação de comentários 6891](https://tools.ietf.org/html/rfc6891).  
  
O valor do RR otimizado é o nome do escopo de zona para o qual a solicitação é enviada. Quando um servidor DNS primário recebe esse pacote de um servidor secundário confiável, ele interpreta a solicitação como proveniente desse escopo de zona.   
  
Se o servidor primário tiver escopo zona ele responde com os dados de transferência (XFR) do escopo. A resposta contém um RR otimizado com a mesma ID de opção "65433" e o valor definido para o mesmo escopo de zona. Os servidores secundários recebem essa resposta, recuperar as informações de escopo na resposta e atualizar esse determinado escopo da zona.  
  
Após esse processo, o servidor primário mantém uma lista de secundários confiáveis que tal uma solicitação de escopo de zona para notificações enviados.   
  
Para qualquer outra atualização em um escopo de zona, uma notificação de IXFR é enviada para os servidores secundários, com o mesmo RR de aceitação. O escopo da região receber essa notificação faz a solicitação IXFR que contém esse RR otimizado e o mesmo processo conforme descrito acima segue.  
  
## <a name="bkmk_config"></a>Como configurar a política de DNS para o gerenciamento de tráfego de com base em localização geográfica primárias e secundárias

Antes de começar, certifique-se de que você tenha concluído todas as etapas no tópico [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), e seu servidor DNS primário é configurado com zonas, escopos de zona, o cliente DNS Subredes e a política DNS.  
  
>[!NOTE]
> As instruções neste tópico para copiar as sub-redes de cliente DNS, escopos de zona e políticas de DNS de servidores DNS primários para os servidores DNS secundários são para sua configuração DNS inicial e a validação. No futuro você talvez queira alterar a subredes de cliente DNS, escopos de zona e configurações de políticas no servidor primário. Nessa circunstância, você pode criar scripts de automação para manter os servidores secundários sincronizados com o servidor primário.  
  
Para configurar a política de DNS para respostas de consultas de primárias e secundárias localização geográfica com base, você deve executar as etapas a seguir.  
  
- [Criar as zonas secundário](#bkmk_secondary)  
- [Definir as configurações de transferência de zona na região primária](#bkmk_zonexfer)  
- [Copie as sub-redes de cliente DNS](#bkmk_client)  
- [Criar os escopos de zona no servidor secundário](#bkmk_zonescopes)  
- [Configurar a política de DNS](#bkmk_dnspolicy)  
  
As seções a seguir fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos.  
><br>Associação na **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
### <a name="bkmk_secondary"></a>Criar as zonas secundário

Você pode criar a cópia secundária da zona da qual você deseja replicar para SecondaryServer1 e SecondaryServer2 (supondo que os cmdlets estão sendo executados remotamente de um cliente de gerenciamento único).   
  
Por exemplo, você pode criar a cópia secundária de www.woodgrove.com em SecondaryServer1 e SecondarySesrver2.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar as zonas secundárias.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [DnsServerSecondaryZone adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="bkmk_zonexfer"></a>Definir as configurações de transferência de zona na região primária

Você deve definir as configurações de zona primária, de modo que:

1. Transferências de zona do servidor primário para os servidores secundários especificados são permitidas.  
2. Notificações de atualização de zona são enviadas pelo servidor primário para os servidores secundários.  
  
Você pode usar os seguintes comandos do Windows PowerShell para definir as configurações de transferência de zona na zona primária.
  
>[!NOTE]
>No seguinte exemplo de comando, o parâmetro **-notificar** Especifica que o servidor primário enviará notificações sobre atualizações à lista de seleção de secundários.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Para obter mais informações, consulte [Set-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="bkmk_client"></a>Copie as sub-redes de cliente DNS

Você deve copiar as sub-redes de cliente DNS do servidor primário para os servidores secundários.
  
Você pode usar os seguintes comandos do Windows PowerShell para copiar as sub-redes para os servidores secundários.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [DnsServerClientSubnet adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_zonescopes"></a>Criar os escopos de zona no servidor secundário

Você deve criar os escopos de zona nos servidores secundários. No DNS, os escopos de zona também iniciar a solicitação XFRs do servidor primário. Qualquer alteração feita nos escopos a zona no servidor primário, uma notificação que contém as informações de escopo de zona é enviada para os servidores secundários. Os servidores secundários, em seguida, podem atualizar seus escopos de zona com alteração incremental.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar os escopos de zona nos servidores secundários.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>Esses comandos de exemplo, o **- ErrorAction ignorar** parâmetro seja incluído, como um escopo de zona padrão existe em cada zona. O escopo da região padrão não pode ser criado ou excluído. O pipelining resultará em uma tentativa de criar o escopo e irá falhar. Como alternativa, você pode criar os escopos de zonas não padrão em duas zonas secundárias.  
  
Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_dnspolicy"></a>Configurar a política de DNS

Depois de ter criado as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que se conectam a sub-redes e partições, para que quando uma consulta vem de uma fonte em uma das sub-redes de cliente DNS, a resposta de consulta é retornada de o escopo correto da zona. Não há políticas são necessárias para o escopo da região padrão de mapeamento.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula-se as sub-redes de cliente DNS e os escopos de zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora, os servidores DNS secundários são configurados com as políticas DNS necessárias para redirecionar o tráfego com base em localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação de DNS com as diretivas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associada é usado para responder à consulta e o usuário é direcionado para o recurso que seja geograficamente mais próximo a eles.   
  
Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada.
