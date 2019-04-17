---
title: Visão geral das políticas DNS
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>Visão geral das políticas DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre a política de DNS, que é novo no Windows Server 2016. Você pode usar a política de DNS para gerenciamento de tráfego com base em localização geográfica, inteligentes respostas DNS com base na hora do dia, para gerenciar um único servidor DNS configurado para implantação split\ cérebro, a aplicação de filtros em consultas do DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você implantou várias instâncias de um aplicativo em locais diferentes, você pode usar a política DNS para equilibrar a carga de tráfego entre as instâncias de aplicativo diferente, alocar dinamicamente a carga do tráfego para o aplicativo.

-   **Geo\ local com base em gerenciamento de tráfego.** Você pode usar política de DNS para permitir que os servidores DNS primário e secundário responder a consultas do cliente DNS com base na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o endereço IP do recurso mais próximo. 

-   **Divida cérebro DNS.** Com o cérebro split\ DNS, registros DNS são divididos em escopos zona diferentes no mesmo servidor DNS e clientes DNS recebem uma resposta com base em se os clientes são clientes internos ou externos. Você pode configurar o cérebro split\ DNS para zonas integrada ao Active Directory ou para zonas em servidores DNS autônomo.

-   **Filtragem.** Você pode configurar a política DNS para criar filtros de consulta que são baseados em critérios que você fornecer. Filtros de consulta na política DNS permitem que você configure o servidor DNS para responder de forma personalizada com base na consulta de DNS e cliente DNS que envia a consulta DNS. 
-   **Perícia.** Você pode usar a política DNS para redirecionar mal-intencionado clientes DNS para um endereço IP non\ existente em vez de direcionando-os para o computador que está tentando acessar.

-   **Hora do dia com base em redirecionamento.** Você pode usar a política DNS para distribuir o tráfego do aplicativo entre diferentes instâncias geograficamente distribuídas de um aplicativo usando políticas DNS que se baseiam na hora do dia.

## <a name="new-concepts"></a>Novos conceitos  
Para criar políticas para dar suporte os cenários listados acima, é necessário ser capaz de identificar grupos de registros em uma zona, grupos de clientes em uma rede, entre outros elementos. Esses elementos são representados pelos novos objetos DNS a seguir:  

- **Sub-rede cliente:** um objeto de sub-rede cliente representa uma sub-rede IPv4 ou IPv6 do qual consultas são enviadas para um servidor DNS. Você pode criar sub-redes para definir mais tarde políticas sejam aplicadas com base na qual as solicitações são provenientes de sub-rede. Por exemplo, em um cenário DNS do cérebro de divisão, a solicitação para resolução de um nome como *www.microsoft.com* possa ser respondida com um endereço IP interno para clientes de sub-redes internas e um endereço IP diferente para clientes de sub-redes externos.

- **Escopo de recursão:** recursão escopos são exclusivas instâncias de um grupo de configurações que controlam recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se recursão está habilitada. Um servidor DNS pode ter vários escopos de recursão. Políticas de recursão de servidor DNS permitem que você escolha um escopo recursão para um conjunto de consultas. Se o servidor DNS não está autorizado para determinadas consultas, políticas de recursão de servidor DNS permitem que você controle como resolver esses consultas. Você pode especificar quais encaminhadores usar e se você deseja usar recursão.

- **Zona escopos:** uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com endereços IP diferentes. Além disso, as transferências de zona são feitas no nível de zona de escopo. Isso significa que registros de um escopo de zona em uma zona primária serão transferidos para o mesmo escopo de zona em uma zona secundária.

## <a name="types-of-policy"></a>Tipos de política

Políticas de DNS são divididas por nível e tipo. Você pode usar políticas de resolução de consulta para definir como consultas são processadas e políticas de transferência de zona para definir como transferências de zona ocorrem. Você pode aplicar a cada tipo de política no nível do servidor ou o nível de zona.
  
### <a name="query-resolution-policies"></a>Políticas de resolução de consulta

Você pode usar políticas de resolução de consulta de DNS para especificar como entrada resolução consultas são manipuladas por um servidor DNS. Cada política de resolução de consulta de DNS contém os seguintes elementos:  
  
|Campo|Descrição|Valores possíveis|  
|---------|---------------|-------------------|  
|**Nome**|Nome da política|-Até 256 caracteres<br />-Pode conter qualquer caractere válido para um nome de arquivo|  
|**Estado**|Estado de política|-Habilitar (padrão)<br />-Desabilitada|  
|**Nível**|Nível de política|-Server<br />-Zone|  
|**Ordem de processamento**|Depois que uma consulta é classificada por nível e se aplica a, o servidor localiza a primeira diretiva para os quais a consulta corresponde aos critérios e aplica-la para consultar|-Valor numérico<br />-Valor exclusivo por política que contém o mesmo nível e aplica-se no valor|  
|**Ação**|Ação a ser realizada pelo servidor DNS|-Permitir (padrão para o nível de região)<br />-Negar (padrão no nível de servidor)<br />-Ignorar|  
|**Critérios**|Condição de política (e/ou) e lista de critério a serem atendidos para política a ser aplicado|-Operador condição (e/ou)<br />– Lista de critérios (consulte a tabela de critério abaixo)|  
|**Escopo**|Lista de escopos de zona e valores ponderadas por escopo. Ponderada valores são usados para distribuição de balanceamento de carga. Por exemplo, se essa lista inclui datacenter1 com uma espessura de 3 e datacenter2 com uma espessura de 5 o servidor responderá com um registro de datacentre1 três vezes fora oito solicitações|– Lista de escopos de zona (por nome) e espessuras|  

> [!NOTE]
> Políticas de nível de servidor só podem ter os valores **negar** ou **ignorar** como uma ação.

O campo de critérios de política DNS é composto de dois elementos:

|Nome|Descrição|Valores de exemplo|
|--------|---------------|-----------------|
|**Sub-rede do cliente**|Usado na consulta de protocolo de transporte. Possíveis entradas são **UDP** e **TCP**|-   **EQ, Espanha, França** -será resolvido como true se a sub-rede é identificada como Espanha ou França<br />-   **NE, Canadá, México** -será resolvido como true se a sub-rede do cliente é uma sub-rede diferentes Canadá e México|  
|**Protocolo de transporte**|Usado na consulta de protocolo de transporte. Possíveis entradas são **UDP** e **TCP**|-   **EQ, TCP**<br />-   **EQ, UDP**|  
|**Protocolo de Internet**|Protocolo de rede usado na consulta. Possíveis entradas são **IPv4** e **IPv6**|-   **EQ, IPv4**<br />-   **EQ, IPv6**|  
|**Endereço IP de Interface do servidor**|Endereço IP de interface de rede recebida do servidor DNS|-   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**|  
|**FQDN**|FQDN do registro na consulta, com a possibilidade de usar um caractere curinga|-   **EQ, www.contoso.com** -resolve tot rue somente se a consulta é tentar resolver o *www.contoso.com* FQDN<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** -será resolvido como true se a consulta é para qualquer registro termina em *contoso.com***ou***woodgrove.com*|  
|**Tipo de consulta**|Tipo de registro está sendo consultado (A, SVR, TXT)|-   **EQ, TXT, SRV** -resolve tot rue se a consulta está solicitando um TXT **ou** registro SRV<br />-   **EQ, MX** -resolve tot rue se a consulta está solicitando um registro MX|  
|**Hora do dia**|Hora do dia que a consulta é recebida|-   **EQ, 10:00-12:00, 23-22:00:00** -resolve tot rue se a consulta for recebida entre 10 AM e meio-dia, **ou** entre PM de 10 e 11 PM|  
  
Usando a tabela acima como um ponto de partida, a tabela a seguir pode ser usada para definir um critério que é usado para combinar com consultas para qualquer tipo de registros mas registros SRV no domínio contoso.com provenientes de um cliente na sub-rede 10.0.0.0/24 via TCP entre 8 e 10 PM pela interface 10.0.0.3:  
  
|Nome|Valor|  
|--------|---------|  
|Sub-rede do cliente|EQ, 10.0.0.0/24|  
|Protocolo de transporte|EQ, TCP|  
|Endereço IP de Interface do servidor|EQ, 10.0.0.3|  
|FQDN|EQ, *. contoso.com|  
|Tipo de consulta|NE, SRV|  
|Hora do dia|EQ, 22-20:00:00|  
  
Você pode criar consultas várias políticas de resolução do mesmo nível, desde que eles têm um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa consultas de entrada da seguinte maneira:  
  
![Processamento da política de DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>Políticas de recursão  
As políticas de recursão são especial **tipo** das políticas de nível de servidor. As políticas de recursão controlam como o servidor DNS realiza recursão para uma consulta. Políticas de recursão se aplicam somente quando o processamento de consulta atinge o caminho de recursão. Você pode escolher um valor de negação ou em Ignorar para recursão para um conjunto de consultas. Como alternativa, você pode escolher um conjunto de encaminhadores para um conjunto de consultas.  
  
Você pode usar políticas de recursão para implementar uma Split-brain configuração DNS. Nesta configuração, o servidor DNS executa recursão para um conjunto de clientes para uma consulta, enquanto o servidor DNS não executa recursão para outros clientes para a consulta.  
  
Políticas de recursão contém os mesmos elementos que contém uma política de resolução de consulta DNS normal, juntamente com os elementos na tabela a seguir:  
  
|Nome|Descrição|  
|--------|---------------|  
|**Se aplicam a recursão**|Especifica que essa política deve ser usada apenas para recursão.|  
|**Escopo de recursão**|Nome do escopo recursão.|  
  
> [!NOTE]  
> Políticas de recursão só podem ser criadas no nível do servidor.  
  
### <a name="zone-transfer-policies"></a>Políticas de transferência de zona  
Políticas de transferência de zona controlam se uma transferência de zona é permitida ou não pelo servidor DNS. Você pode criar políticas para transferência de zona no nível do servidor ou o nível de zona. Aplicam políticas de nível de servidor em cada consulta de transferência de zona que ocorre no servidor DNS. Políticas de nível de zona se aplicam apenas em consultas em uma zona hospedado no servidor DNS. O uso mais comum para políticas de nível de zona é implementar listas bloqueadas ou seguras.  
  
> [!NOTE]  
> Políticas de transferência de zona só podem usar negar ou ignorar como ações.  
  
Você pode usar a política de transferência de zona de nível de servidor abaixo para negar uma transferência de zona para o domínio contoso.com de uma determinada sub-rede:  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
Você pode criar vários transferência de zona políticas de mesmo nível, desde que eles têm um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa consultas de entrada da seguinte maneira:  
  
![Processo DNS para várias políticas de transferência de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>Gerenciamento de políticas de DNS  
Você pode criar e gerenciar políticas de DNS usando o PowerShell. Consulte os exemplos a seguir por meio de cenários de exemplo diferentes que você pode configurar por meio de políticas de DNS:  
  
### <a name="traffic-management"></a>Gerenciamento de tráfego  
Você pode direcionar o tráfego com base em um FQDN para servidores diferentes dependendo da localização do cliente DNS. O exemplo a seguir mostra como criar políticas de gerenciamento para direcionar os clientes de uma determinada sub-rede para um datacenter da América do Norte e de sub-rede outra para um datacenter europeu de tráfego.  
  
```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  
  
As duas primeiras linhas do script criam objetos de sub-rede para América do Norte e Europa de cliente. As duas linhas depois que criar um escopo de zona dentro do domínio contoso.com, um para cada região. As duas linhas depois que criam um registro em cada região que associa ww.contoso.com outro endereço IP, um para Europa, outra América do Norte. Por fim, as últimas linhas do script criam dois DNS consulta resolução políticas, um para ser aplicadas à sub-rede na América do Norte, outro para a sub-rede da Europa.  
  
### <a name="block-queries-for-a-domain"></a>Consultas de bloco para um domínio  
Você pode usar uma política de resolução de consulta de DNS para consultas de bloco em um domínio. O exemplo a seguir bloqueia todas as consultas de treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>Consultas de bloco de uma sub-rede  
Você também pode bloquear consultas provenientes de uma sub-rede específica. O script a seguir cria uma sub-rede para 172.0.33.0/24 e, em seguida, cria uma política para ignorar todas as consultas provenientes de sub-rede:  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>Permitir recursão para os clientes internos  
Você pode controlar recursão usando uma política de resolução de consulta de DNS. O exemplo abaixo pode ser usado para habilitar recursão para clientes internos, enquanto desabilitá-lo para clientes externos em um cenário de cérebro dividido.  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
A primeira linha no script altera o escopo de recursão padrão, chamado simplesmente como "." (ponto) para desabilitar a recursão. A segunda linha cria um escopo de recursão chamado *InternalClients* com recursão habilitado. E a terceira linha cria uma política para aplicar o recentemente criar o escopo de recursão para qualquer consulta cair uma interface de servidor que tenha 10.0.0.34 como um endereço IP.  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>Criar uma política de transferência de zona de nível de servidor  
Você pode controlar a transferência de zona em um formulário mais granular usando políticas de transferência de zona de DNS. O script de exemplo abaixo pode ser usado para permitir transferências de zona para qualquer servidor em uma determinada sub-rede:  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
A primeira linha no script cria um objeto de sub-rede chamado *AllowedSubnet* com IP bloquear 172.21.33.0/24. A segunda linha cria uma política de transferência de zona para permitir transferências de zona para qualquer servidor DNS na sub-rede criado anteriormente.  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>Criar uma política de transferência de zona de nível de zona  
Você também pode criar políticas de transferência de zona de nível de zona. O exemplo a seguir ignora qualquer solicitação de uma transferência de zona contoso.com proveniente de uma interface de servidor que tenha um endereço IP 10.0.0.33:  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>Cenários de política DNS

Para obter informações sobre como usar a política DNS para cenários específicos, consulte os tópicos a seguir neste guia.

- [Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com servidores primários](primary-geo-location.md)  
- [Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com implantações de primário secundário](primary-secondary-geo-location.md)  
- [Usar a política de DNS para respostas DNS inteligente com base na hora do dia](dns-tod-intelligent.md)
- [O servidor do aplicativo de nuvem respostas DNS com base na hora do dia com um Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política de DNS para a implantação de uma DNS](split-brain-DNS-deployment.md)
- [Usar a política DNS para um DNS no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para aplicação de filtros em consultas do DNS](apply-filters-on-dns-queries.md)
- [Usar a política DNS para balanceamento de carga do aplicativo](app-lb.md)
- [Usar a política DNS para aplicativo balanceamento de carga com reconhecimento de localização geográfica](app-lb-geo.md)


