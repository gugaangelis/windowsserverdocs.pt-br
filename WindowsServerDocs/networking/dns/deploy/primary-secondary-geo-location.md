---
title: Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com implantações primárias e secundárias
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a7836160fc7363ec3d7b2fb11e194db82970f9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406160"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com implantações primárias e secundárias

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a criar uma política DNS para o gerenciamento de tráfego baseado na localização geográfica quando a implantação do DNS inclui servidores DNS primários e secundários.  

O cenário anterior, [use a política DNS para o gerenciamento de tráfego baseado em localização geográfica com servidores primários](primary-geo-location.md), forneceu instruções para configurar a política DNS para o gerenciamento de tráfego baseado em localização geográfica em um servidor DNS primário. No entanto, na infra-estrutura de Internet, os servidores DNS são amplamente implantados em um modelo primário secundário, no qual a cópia gravável de uma zona é armazenada em servidores primários selecionados e seguros, e cópias somente leitura da zona são mantidas em vários servidores secundários.   
  
Os servidores secundários usam a transferência autoritativa de protocolos de transferência de zona (AXFR) e a transferência de zona incremental (IXFR) para solicitar e receber atualizações de zona que incluem novas alterações nas zonas nos servidores DNS primários.   
  
> [!NOTE]
> Para obter mais informações sobre o AXFR, consulte a solicitação IETF (Internet Engineering Task Force) [para comentários 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para obter mais informações sobre IXFR, consulte a solicitação IETF (Internet Engineering Task Force) [para comentários 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>Exemplo de gerenciamento de tráfego baseado em localização geográfica secundária principal  
A seguir, um exemplo de como você pode usar a política DNS em uma implantação primária-secundária para obter o redirecionamento de tráfego com base no local físico do cliente que executa uma consulta DNS.  
  
Este exemplo usa duas empresas fictícias: serviços de nuvem da Contoso, que fornecem soluções de Hospedagem de domínio e Web; e os serviços do Woodgrove Food, que fornecem serviços de entrega de alimentos em várias cidades em todo o mundo e que tem um site chamado woodgrove.com.  
  
Para garantir que os clientes do woodgrove.com tenham uma experiência responsiva de seu site, o Woodgrove quer clientes europeus direcionados para o datacenter Europeu e os clientes norte-americanos direcionados para o datacenter dos EUA. Os clientes localizados em outro lugar do mundo podem ser direcionados para um dos datacenters.  
  
Os serviços de nuvem da Contoso têm dois data centers, um nos EUA e outro na Europa, no qual a contoso hospeda seu portal de pedidos de alimentos para woodgrove.com.  
  
A implantação de DNS da Contoso inclui dois servidores secundários: **SecondaryServer1**, com o endereço IP 10.0.0.2; e **SecondaryServer2**, com o endereço IP 10.0.0.3. Esses servidores secundários estão agindo como servidores de nomes nas duas regiões diferentes, com SecondaryServer1 localizado na Europa e SecondaryServer2, localizados nos EUA.
  
Há uma cópia de zona primária gravável em **PrimaryServer** (endereço IP 10.0.0.1), em que as alterações de zona são feitas. Com transferências de zona regulares para os servidores secundários, os servidores secundários estão sempre atualizados com qualquer nova alteração na zona no PrimaryServer.
  
A ilustração a seguir descreve esse cenário.
  
![Exemplo de gerenciamento de tráfego baseado em localização geográfica secundária principal](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>Como o sistema DNS primário-secundário funciona

Quando você implanta o gerenciamento de tráfego baseado em localização geográfica em uma implantação de DNS primário-secundário, é importante entender como as transferências de zona primárias secundárias ocorrem antes de aprender sobre as transferências de nível de escopo de zona. As seções a seguir fornecem informações sobre transferências de zona e de nível de escopo de zona.  
  
- [Transferências de zona em uma implantação de DNS primário-secundário](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [Transferências de nível de escopo de zona em uma implantação de DNS primário-secundário](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>Transferências de zona em uma implantação de DNS primário-secundário

Você pode criar uma implantação de DNS primário-secundário e sincronizar as zonas com as etapas a seguir.  
1. Quando você instala o DNS, a zona primária é criada no servidor DNS primário.  
2. No servidor secundário, crie as zonas e especifique os servidores primários.   
3. Nos servidores primários, você pode adicionar os servidores secundários como secundários confiáveis na zona primária.   
4. As zonas secundárias fazem uma solicitação de transferência de zona completa (AXFR) e recebem a cópia da zona.   
5. Quando necessário, os servidores primários enviam notificações para os servidores secundários sobre atualizações de zona.  
6. Os servidores secundários fazem uma solicitação de transferência de zona incremental (IXFR). Por isso, os servidores secundários permanecem sincronizados com o servidor primário.   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>Transferências de nível de escopo de zona em uma implantação de DNS primário-secundário

O cenário de gerenciamento de tráfego requer etapas adicionais para particionar as zonas em escopos de zona diferentes. Por isso, são necessárias etapas adicionais para transferir os dados dentro dos escopos de zona para os servidores secundários e para transferir as políticas e as sub-redes de cliente DNS para os servidores secundários.   
  
Depois de configurar sua infraestrutura de DNS com servidores primários e secundários, as transferências de nível de escopo de zona são executadas automaticamente pelo DNS, usando os processos a seguir.  
  
Para garantir a transferência no nível de escopo de zona, os servidores DNS usam os mecanismos de extensão para o RR de consentimento DNS (EDNS0). Todas as solicitações de transferência de zona (AXFR ou IXFR) das zonas com escopos se originam com um RR de OPT de EDNS0, cuja ID de opção é definida como "65433" por padrão. Para obter mais informações sobre EDNSO, consulte a [solicitação da IETF para comentários 6891](https://tools.ietf.org/html/rfc6891).  
  
O valor do RR de consentimento é o nome do escopo de zona para o qual a solicitação está sendo enviada. Quando um servidor DNS primário recebe esse pacote de um servidor secundário confiável, ele interpreta a solicitação como recebida para esse escopo de zona.   
  
Se o servidor primário tiver esse escopo de zona, ele responderá com os dados de transferência (XFR) desse escopo. A resposta contém um RR opcional com a mesma ID de opção "65433" e o valor definido para o mesmo escopo de zona. Os servidores secundários recebem essa resposta, recuperam as informações de escopo da resposta e atualizam esse escopo específico da zona.  
  
Após esse processo, o servidor primário mantém uma lista de secundários confiáveis que enviaram uma solicitação de escopo de zona para notificações.   
  
Para qualquer atualização adicional em um escopo de zona, uma notificação IXFR é enviada para os servidores secundários, com o mesmo RR de consentimento. O escopo de zona que recebe essa notificação faz com que a solicitação IXFR contenha esse RR de consentimento e o mesmo processo descrito acima a seguir.  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>Como configurar a política de DNS para o gerenciamento de tráfego baseado em localização geográfica secundária principal

Antes de começar, verifique se você concluiu todas as etapas no tópico usar a [política DNS para o gerenciamento de tráfego baseado na localização geográfica com servidores primários](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), e seu servidor DNS primário está configurado com zonas, escopos de zona, sub-redes de cliente DNS e DNS regras.  
  
> [!NOTE]
> As instruções neste tópico para copiar sub-redes de cliente DNS, escopos de zona e políticas de DNS de servidores DNS primários para servidores DNS secundários são para a sua configuração e validação de DNS inicial. No futuro, você pode querer alterar as configurações de sub-rede do cliente DNS, escopos de zona e políticas no servidor primário. Nessa circunstância, você pode criar scripts de automação para manter os servidores secundários sincronizados com o servidor primário.  
  
Para configurar a política DNS para respostas de consulta com base em localização geográfica secundária primária, você deve executar as etapas a seguir.  
  
- [Criar as zonas secundárias](#create-the-secondary-zones)  
- [Definir as configurações de transferência de zona na zona primária](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [Copiar as sub-redes do cliente DNS](#copy-the-dns-client-subnets)  
- [Criar os escopos de zona no servidor secundário](#create-the-zone-scopes-on-the-secondary-server)  
- [Configurar política DNS](#configure-dns-policy)  
  
As seções a seguir fornecem instruções de configuração detalhadas.  
  
> [!IMPORTANT]
> As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.  
> 
> A associação em **DNSAdmins**, ou equivalente, é necessária para executar os procedimentos a seguir.  
  
### <a name="create-the-secondary-zones"></a>Criar as zonas secundárias

Você pode criar a cópia secundária da zona que deseja replicar para SecondaryServer1 e SecondaryServer2 (supondo que os cmdlets estejam sendo executados remotamente de um único cliente de gerenciamento).   
  
Por exemplo, você pode criar a cópia secundária de www.woodgrove.com em SecondaryServer1 e SecondarySesrver2.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar as zonas secundárias.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [Add-DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>Definir as configurações de transferência de zona na zona primária

Você deve definir as configurações de zona primária para que:

1. As transferências de zona do servidor primário para os servidores secundários especificados são permitidas.  
2. As notificações de atualização de zona são enviadas pelo servidor primário para os servidores secundários.  
  
Você pode usar os seguintes comandos do Windows PowerShell para definir as configurações de transferência de zona na zona primária.
  
> [!NOTE]
> No comando de exemplo a seguir, o parâmetro **-Notify** especifica que o servidor primário enviará notificações sobre atualizações para a lista de seleção de secundários.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Para obter mais informações, consulte [set-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="copy-the-dns-client-subnets"></a>Copiar as sub-redes do cliente DNS

Você deve copiar as sub-redes do cliente DNS do servidor primário para os servidores secundários.
  
Você pode usar os seguintes comandos do Windows PowerShell para copiar as sub-redes para os servidores secundários.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>Criar os escopos de zona no servidor secundário

Você deve criar os escopos de zona nos servidores secundários. No DNS, os escopos de zona também iniciam a solicitação de XFRs do servidor primário. Com qualquer alteração nos escopos de zona no servidor primário, uma notificação que contém as informações de escopo de zona é enviada aos servidores secundários. Os servidores secundários podem então atualizar seus escopos de zona com alteração incremental.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar os escopos de zona nos servidores secundários.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> Nesses comandos de exemplo, o parâmetro **-ErrorAction ignorar** é incluído, pois existe um escopo de zona padrão em cada zona. O escopo de zona padrão não pode ser criado ou excluído. O pipeline resultará em uma tentativa de criar esse escopo e ele falhará. Como alternativa, você pode criar escopos de zona não padrão em duas zonas secundárias.  
  
Para obter mais informações, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="configure-dns-policy"></a>Configurar política DNS

Depois de criar as sub-redes, as partições (escopos de zona) e adicionar registros, você deve criar políticas que conectam as sub-redes e as partições, de modo que quando uma consulta vier de uma origem em uma das sub-redes de cliente DNS, a resposta de consulta será retornada de o escopo correto da zona. Nenhuma política é necessária para mapear o escopo de zona padrão.  
  
Você pode usar os seguintes comandos do Windows PowerShell para criar uma política DNS que vincula as sub-redes do cliente DNS e os escopos de zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Agora, os servidores DNS secundários são configurados com as políticas de DNS necessárias para redirecionar o tráfego com base na localização geográfica.  
  
Quando o servidor DNS recebe consultas de resolução de nomes, o servidor DNS avalia os campos na solicitação DNS em relação às políticas de DNS configuradas. Se o endereço IP de origem na solicitação de resolução de nomes corresponder a qualquer uma das políticas, o escopo de zona associado será usado para responder à consulta e o usuário será direcionado para o recurso que está geograficamente mais próximo.   
  
Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.
