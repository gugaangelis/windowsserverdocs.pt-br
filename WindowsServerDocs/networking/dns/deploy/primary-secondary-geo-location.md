---
title: Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com implantações de primário secundário
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c78a0198e29fb59f30fd8ad776c7f200312d014
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com implantações de primário secundário

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como criar a política DNS para gerenciamento de tráfego com base em localização geográfica quando a implantação do DNS inclui servidores DNS primário e secundários.  

No cenário anterior, [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](primary-geo-location.md), fornecidas instruções para configurar a política DNS para gerenciamento de tráfego de localização geográfica com base em um servidor DNS primário. A infraestrutura de Internet, no entanto, os servidores DNS são amplamente implantados em um modelo primário secundário, onde a cópia de uma zona gravável é armazenada em servidores principais select e seguras e cópias somente leitura da zona são mantidas em vários servidores secundários.   
  
Os servidores secundários usam os protocolos de transferência de zona de transferência (AXFR autoritativo) e transferência de zona Incremental (IXFR) para solicitar e receber atualizações de zona que incluem novas alterações para as zonas nos servidores DNS primários.   
  
>[!NOTE]
>Para saber mais sobre AXFR, consulte Internet Engineering Task Force (IETF) [solicitar para comentários 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para saber mais sobre IXFR, consulte Internet Engineering Task Force (IETF) [solicitar para comentários 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="bkmk_example"></a>Localização geográfica primário secundário com base em exemplo de gerenciamento de tráfego  
Este é um exemplo de como você pode usar a política DNS em uma implantação primário secundário para alcançar o redirecionamento do tráfego com base na localização física do cliente que executa uma consulta DNS.  
  
Este exemplo usa duas empresas fictícias - Contoso serviços em nuvem, que fornece a web e domínio que hospeda soluções; e serviços de comida Woodgrove, que fornece serviços de entrega de comida em várias cidades em todo o mundo, e que tem um site denominado woodgrove.com.  
  
Para garantir que os clientes de woodgrove.com tenham uma experiência responsiva de seu site, Woodgrove deseja europeus clientes direcionados para o datacenter Europeia e da América direcionado para o datacenter dos EUA. Clientes localizados em outro lugar do mundo podem ser direcionados a ambos os datacenters.  
  
Serviços de nuvem Contoso tem dois centros de dados, um nos EUA e outro na Europa, na qual a Contoso hospeda sua comida ordenação portal para woodgrove.com.  
  
A implantação de DNS da Contoso inclui dois servidores secundários: **SecondaryServer1**, com o endereço IP 10.0.0.2; e **SecondaryServer2**, com o endereço IP 10.0.0.3. Esses servidores secundários estiver atuando como servidores de nomes nas duas regiões diferentes, com SecondaryServer1 localizada na Europa e SecondaryServer2 localizado nos EUA
  
Há uma cópia da zona primária gravável em **entradas PrimaryServer** (endereço IP 10.0.0.1), onde as alterações de zona são feitas. Com transferências de zona normal para os servidores secundários, os servidores secundários sempre estão atualizados com todas as novas alterações à zona sobre as entradas de PrimaryServer.
  
A ilustração a seguir ilustra esse cenário.
  
![Localização geográfica primário secundário com base em exemplo de gerenciamento de tráfego](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>Como funciona o sistema do DNS primário secundário

Quando você implanta o gerenciamento de tráfego de localização geográfica com base em uma implantação de DNS primário secundário, é importante compreender como normal zona primário secundário transferências ocorrem antes de ter conhecimento sobre transferências de nível de escopo de zona. As seções a seguir fornecem informações sobre zona e transferências de nível de escopo de zona.  
  
- [Transferências de zona em uma implantação do DNS primário secundário](#bkmk_zone)  
- [Transferências de nível de escopo zona em uma implantação do DNS primário secundário](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>Transferências de zona em uma implantação do DNS primário secundário

Você pode criar uma implantação do DNS primário secundário e sincronizar zonas com as etapas a seguir.  
1. Quando você instala o DNS, a zona principal é criada no servidor DNS primário.  
2. No servidor secundário, crie as zonas e especifique os servidores primários.   
3. Nos servidores do principais, você pode adicionar os servidores secundários como confiáveis secundários na zona primária.   
4. As zonas secundárias faça uma solicitação de transferência de zona plena (AXFR) e recebam uma cópia da zona.   
5. Quando necessário, os servidores primários enviam notificações para os servidores secundários sobre atualizações de zona.  
6. Servidores secundários fazer uma solicitação de transferência de zona incremental (IXFR). Por isso, os servidores secundários permanecerão sincronizados com o servidor primário.   
  
### <a name="bkmk_scope"></a>Transferências de nível de escopo zona em uma implantação do DNS primário secundário

O cenário de gerenciamento de tráfego requer etapas adicionais para particionar as zonas em escopos de zona diferentes. Por isso, etapas adicionais são necessárias para transferir os dados dentro os escopos de zona para os servidores secundários e transferir as políticas e sub-redes do cliente DNS para os servidores secundários.   
  
Depois de configurar sua infraestrutura DNS com servidores primário e secundário, transferências de nível de escopo de zona são executadas automaticamente pelo DNS, usando os seguintes processos.  
  
Para garantir a transferência de nível de escopo de zona, servidores DNS usam os mecanismos de extensão para RR de aceitação de DNS (EDNS0). Todas as solicitações de transferência (AXFR ou IXFR) de zona das regiões com escopos são originadas com um EDNS0 aceitar registro recurso, cuja ID de opção é definida como "65433" por padrão. Para saber mais sobre EDNSO, consulte IETF [solicitar para comentários 6891](https://tools.ietf.org/html/rfc6891).  
  
O valor do registro de recurso de aceitação é o nome de escopo de zona para o qual a solicitação está sendo enviada. Quando um servidor DNS primário recebe esse pacote em um servidor secundário confiável, ele interpreta a solicitação como vindo para esse escopo de zona.   
  
Se o servidor primário tem esse escopo de zona ele responde com os dados de transferência (XFR) do escopo. A resposta contém um aceitar RR com a mesma ID de opção "65433" e o valor definido para o mesmo escopo da região. Os servidores secundários recebem essa resposta, recuperar as informações de escopo da resposta e atualizar esse escopo específico da zona.  
  
Após esse processo, o servidor primário mantém uma lista de secundários confiáveis que enviaram tal uma solicitação de escopo de zona de notificações.   
  
Para qualquer atualização adicional em um escopo de zona, uma notificação de IXFR é enviada para os servidores secundários, com o registro de recurso mesmo aceitar. O escopo de zona recebendo essa notificação faz a solicitação IXFR contendo esse RR aceitação e o mesmo processo conforme descrito acima segue.  
  
## <a name="bkmk_config"></a>Como configurar a política de DNS primário secundário localização geográfica com base em gerenciamento de tráfego

Antes de começar, certifique-se de que você concluiu todas as etapas no tópico [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), e o servidor DNS primário está configurado com zonas, escopos da zona, sub-redes do cliente DNS e política DNS.  
  
>[!NOTE]
> As instruções neste tópico para copiar sub-redes do cliente DNS, escopos da zona e políticas DNS dos servidores DNS primários para os servidores DNS secundários destinam-se a sua instalação inicial do DNS e validação. No futuro, convém alterar as sub-redes do cliente DNS, escopos da zona e configurações de políticas no servidor primário. Neste caso, você pode criar scripts de automação para manter os servidores secundários sincronizados com o servidor principal.  
  
Para configurar a política DNS para respostas de consulta primário secundário localização geográfica com base, você deve executar as etapas a seguir.  
  
- [Criar as zonas secundário](#bkmk_secondary)  
- [Definir as configurações de transferência de zona na zona principal](#bkmk_zonexfer)  
- [Copie as sub-redes do cliente DNS](#bkmk_client)  
- [Criar os escopos de zona no servidor secundário](#bkmk_zonescopes)  
- [Configurar a política DNS](#bkmk_dnspolicy)  
  
As seguintes seções fornecem instruções de configuração detalhadas.  
  
>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos.  
><br>A associação ao grupo **DnsAdmins**, ou equivalente, é necessário para executar os procedimentos a seguir.  
  
### <a name="bkmk_secondary"></a>Criar as zonas secundário

Você pode criar a cópia secundária da zona deseja duplicar SecondaryServer1 e SecondaryServer2 (supondo que os cmdlets estão sendo executadas remotamente de um cliente de gerenciamento único).   
  
Por exemplo, você pode criar a cópia secundária de www.woodgrove.com em SecondaryServer1 e SecondarySesrver2.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar as zonas secundárias.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [adicionar DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="bkmk_zonexfer"></a>Definir as configurações de transferência de zona na zona principal

Você deve definir as configurações de zona primária para que:

1. São permitidas transferências de zona do servidor principal para os servidores secundários especificados.  
2. Notificações de atualização de zona são enviadas pelo servidor principal para os servidores secundários.  
  
Você pode usar os seguintes comandos do Windows PowerShell para definir as configurações de transferência de zona na zona primária.
  
>[!NOTE]
>No seguinte exemplo de comando, o parâmetro **-notificar** Especifica que o servidor primário enviará notificações sobre atualizações na lista Selecionar de secundários.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Para obter mais informações, consulte [conjunto DnsServerPrimaryZone](https://https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="bkmk_client"></a>Copie as sub-redes do cliente DNS

Você deve copiar as sub-redes do cliente DNS do servidor principal para os servidores secundários.
  
Você pode usar os seguintes comandos do Windows PowerShell para copiar as sub-redes para os servidores secundários.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [adicionar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_zonescopes"></a>Criar os escopos de zona no servidor secundário

Você deve criar os escopos de zona nos servidores secundários. No DNS, os escopos de zona também iniciar a solicitação XFRs do servidor principal. Qualquer alteração realizada nos escopos de zona no servidor primário, uma notificação que contém as informações de escopo de zona é enviada para os servidores secundários. Os servidores secundários, em seguida, podem atualizar seus escopos zona com alteração incremental.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar os escopos de zona nos servidores secundários.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>Esses comandos de exemplo, o **- ErrorAction ignorar** parâmetro está incluído, como um escopo de zona padrão existe em cada região. O escopo da região padrão não pode ser criado ou excluído. Pipelining resultará em uma tentativa de criar esse escopo e ele irá falhar. Como alternativa, você pode criar os escopos de zona de não padrão em dois fusos secundários.  
  
Para obter mais informações, consulte [adicionar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_dnspolicy"></a>Configurar a política DNS

Depois que você criou as sub-redes, as partições (escopos de zona) e você adicionou registros, você deve criar políticas que conectam as sub-redes e partições, para que quando uma consulta vem de uma fonte em um das sub-redes do cliente DNS, a resposta da consulta é retornada do escopo da zona correto. Não há políticas são necessárias para mapear o escopo da região padrão.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora os servidores DNS secundários são configurados com as políticas DNS necessárias para redirecionar o tráfego com base na localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nome, o servidor DNS avalia os campos na solicitação DNS contra as políticas DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nome corresponde a qualquer uma das políticas, o escopo da região associado é usado para responder à consulta e o usuário será direcionado para o recurso que é geograficamente closest-los.   
  
Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas.
