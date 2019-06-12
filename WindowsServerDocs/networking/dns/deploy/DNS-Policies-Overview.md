---
title: Visão geral das políticas de DNS
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c4d8f9bb6c56e8f90a90cd4e77565a39211f719
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446430"
---
# <a name="dns-policies-overview"></a>Visão geral das políticas de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a política de DNS, o que há de novo no Windows Server 2016. Você pode usar a política de DNS para o gerenciamento de tráfego, com base na hora do dia, para gerenciar um único servidor DNS configurado para dividir as respostas DNS inteligentes com base de localização geográfica\-implantação cérebro, aplicação de filtros em consultas DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você tiver implantado várias instâncias de um aplicativo em locais diferentes, você pode usar a política de DNS para balancear a carga de tráfego entre as instâncias de aplicativo diferente, alocar dinamicamente a carga de tráfego para o aplicativo.

-   **Geográfica\-o gerenciamento de tráfego baseados na localização.** Você pode usar a política de DNS para permitir que os servidores DNS primário e secundário responder a consultas de cliente DNS baseadas na localização geográfica do cliente e o recurso ao qual o cliente está tentando se conectar, fornecendo o cliente com o endereço IP do mais próximo recurso. 

-   **Divida o cérebro DNS.** Com divisão\-cérebro DNS, registros de DNS são divididos em escopos de zona diferente no mesmo servidor DNS e clientes DNS recebem uma resposta com base em se os clientes são clientes internos ou externos. Você pode configurar a divisão\-cérebro DNS para zonas integrada ao Active Directory ou para as zonas em servidores DNS autônomos.

-   **A filtragem.** Você pode configurar a política de DNS para criar filtros de consulta que são baseados em critérios fornecidos por você. Filtros de consulta na política de DNS permitem que você configure o servidor DNS para responder de maneira personalizada com base na consulta DNS e do cliente DNS que envia a consulta DNS. 
-   **Análise forense.** Você pode usar a política de DNS para redirecionar os clientes mal-intencionados de DNS para um não\-endereço IP existente em vez de direcioná-las para o computador que estão tentando alcançar.

-   **Hora do dia com base em redirecionamento.** Você pode usar a política de DNS para distribuir o tráfego de aplicativo entre diferentes instâncias distribuídas geograficamente de um aplicativo por meio de políticas DNS com base na hora do dia.

## <a name="new-concepts"></a>Novos conceitos  
Para criar políticas para dar suporte a cenários listados acima, é necessário ser capaz de identificar os grupos de registros em uma zona, grupos de clientes em uma rede, entre outros elementos. Esses elementos são representados pelos novos objetos DNS a seguir:  

- **Subrede de cliente:** um objeto de sub-rede do cliente representa uma sub-rede IPv4 ou IPv6 do qual as consultas são enviadas a um servidor DNS. Você pode criar sub-redes para definir posteriormente as diretivas sejam aplicadas com base na qual as solicitações são provenientes de sub-rede. Por exemplo, em um cenário DNS de dupla personalidade de divisão, a solicitação para a resolução de um nome como <em>www.microsoft.com</em> podem ser respondidas com um endereço IP interno para os clientes de sub-redes internas e um endereço IP diferente para os clientes na externa subredes.

- **Escopo de recursão:** escopos de recursão são instâncias exclusivas de um grupo de configurações que controlam a recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se a recursão está habilitada. Um servidor DNS pode ter vários escopos de recursão. Políticas de recursão de servidor DNS permitem que você escolha um escopo de recursão para um conjunto de consultas. Se o servidor DNS não está autorizado para determinadas consultas, políticas de recursão de servidor DNS permitem que você controle como resolver essas consultas. Você pode especificar quais encaminhadores para usar e se deseja usar a recursão.

- **Escopos da zona:** uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP. Além disso, as transferências de zona são feitas no nível de escopo de zona. Isso significa que registros de um escopo de zona em uma zona primária serão transferidos para o mesmo escopo de zona em uma zona secundária.

## <a name="types-of-policy"></a>Tipos de política

Políticas de DNS sejam divididas por nível e tipo. Você pode usar políticas de resolução de consulta para definir como as consultas são processadas e políticas de transferência de zona para definir como as transferências de zona ocorrem. Você pode aplicar a cada tipo de política no nível do servidor ou no nível de zona.

### <a name="query-resolution-policies"></a>Políticas de resolução de consulta

Você pode usar políticas de resolução de consulta de DNS para especificar a resolução de entrada como consultas são tratadas por um servidor DNS. Cada política de resolução DNS consulta contém os seguintes elementos:  

|Campo|Descrição|Valores possíveis|  
|---------|---------------|-------------------|  
|**Nome**|Nome da política|-Até 256 caracteres<br />-Podem conter qualquer caractere válido para um nome de arquivo|  
|**Estado**|Estado da política|-Habilitar (padrão)<br />-Desabilitado|  
|**Nível**|Nível de política|-Servidor<br />-Zona|  
|**Ordem de processamento**|Depois que uma consulta é classificada por nível e aplica-se no, o servidor localiza a primeira diretiva para a qual a consulta corresponde aos critérios e aplica-se a consulta|-Valor numérico<br />-Valor exclusivo por política que contém o mesmo nível e aplica-se no valor|  
|**Ação**|Ação a ser executada pelo servidor DNS|-Permitir (padrão para o nível de zona)<br />-Deny (padrão no nível do servidor)<br />-   Ignore|  
|**Critérios**|Condição da política (e/ou) e a lista de critério a ser atendida para a política seja aplicada|-Operador de condição (e/ou)<br />-Lista de critérios (consulte a tabela de critério abaixo)|  
|**Escopo**|Lista de escopos de zona e valores ponderados por escopo. Valores ponderados são usados para distribuição de balanceamento de carga. Por exemplo, se essa lista inclui datacenter1 com um peso de 3 e datacenter2 com um peso de 5 o servidor responderá com um registro de datacentre1 três vezes fora de oito solicitações|-Lista de pesos e escopos de zona (por nome)|  

> [!NOTE]
> Políticas de nível de servidor só podem ter os valores **Deny** ou **ignorar** como uma ação.

O campo de critérios da política DNS é composto de dois elementos:


|              Nome               |                                         Descrição                                          |                                                                                                                               Valores de exemplo                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Subrede de cliente**        | Nome de uma sub-rede de cliente predefinidos. Usado para verificar a sub-rede da qual a consulta foi enviada. |                             -   **EQ, Espanha, França** -resolve como true se a sub-rede é identificada como Espanha ou França<br />-   **NE, Canadá, México** -resolve como true se a sub-rede do cliente é qualquer sub-rede e que não seja o Canadá e México.                             |
|     **Protocolo de transporte**      |        Transporte de protocolo usado na consulta. São entradas possíveis **UDP** e **TCP**        |                                                                                                                    -   **EQ,TCP**<br />-   **EQ,UDP**                                                                                                                     |
|      **Protocolo de Internet**      |        Protocolo de rede usado na consulta. São entradas possíveis **IPv4** e **IPv6**        |                                                                                                                   -   **EQ,IPv4**<br />-   **EQ,IPv6**                                                                                                                    |
| **Endereço IP da Interface do servidor** |                   Endereço IP para a interface de rede de servidor DNS de entrada                   |                                                                                                              -   **EQ,10.0.0.1**<br />-   **EQ,192.168.1.1**                                                                                                              |
|            **FQDN**             |            FQDN do registro na consulta, com a possibilidade de usar um caractere curinga            | -   **EQ, www.contoso.com** -total de resolve rue somente se a consulta está tentando resolver os <em>www.contoso.com</em> FQDN<br />-   **EQ,\*. contoso.com\*. woodgrove.com** -resolve como true se a consulta é para qualquer registro que terminam em *contoso.com***ou***woodgrove.com* |
|         **Tipo de consulta**          |                          Tipo de registro que está sendo consultado (A, SVR, TXT)                          |                                                  -   **EQ, TXT, SRV** -é resolvida como true se a consulta está solicitando um TXT **ou** registro SRV<br />-   **EQ, MX** -resolve como true se a consulta está solicitando um registro MX                                                   |
|         **Hora do dia**         |                              Hora do dia em que a consulta for recebida                               |                                                                    -   **EQ, 12-10:00:00, 23-22:00:00** -é resolvida como true se a consulta for recebida entre 10h e meio-dia, **ou** entre 22H00 e 11 PM                                                                    |

Usando a tabela acima como um ponto de partida, a tabela a seguir pode ser usada para definir um critério que é usado para corresponder consultas para qualquer tipo de registros, mas os registros SRV no domínio contoso.com proveniente de um cliente na sub-rede 10.0.0.0/24 via TCP entre 8 e 10 PM por meio de eu nterface 10.0.0.3:  

|Nome|Valor|  
|--------|---------|  
|Subrede de cliente|EQ,10.0.0.0/24|  
|Protocolo de transporte|EQ,TCP|  
|Endereço IP da Interface do servidor|EQ,10.0.0.3|  
|FQDN|EQ,*.contoso.com|  
|Tipo de Consulta|NE,SRV|  
|Hora do dia|EQ,20:00-22:00|  

Você pode criar consulta de várias políticas de resolução de mesmo nível, desde que eles têm um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa as consultas de entrada da seguinte maneira:  

![Processamento da política DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Políticas de recursão  
Políticas de recursão são um especial **tipo** das políticas de nível de servidor. As diretivas de recursão controlam como o servidor DNS executa recursão para uma consulta. Políticas de recursão aplicam-se somente quando o processamento de consulta atinge o caminho de recursão. Você pode escolher um valor de negar ou em Ignorar para recursão para um conjunto de consultas. Como alternativa, você pode escolher um conjunto de encaminhadores para um conjunto de consultas.  

Você pode usar políticas de recursão para implementar uma configuração de DNS Split-brain. Nessa configuração, o servidor DNS executa recursão para um conjunto de clientes para uma consulta, enquanto o servidor DNS não executa recursão para outros clientes para a consulta.  

Políticas de recursão contém os mesmos elementos que contém uma política de resolução de consulta DNS regular, juntamente com os elementos na tabela a seguir:  

|Nome|Descrição|  
|--------|---------------|  
|**Aplicar em recursão**|Especifica que essa política só deve ser usada para recursão.|  
|**Escopo de recursão**|Nome do escopo de recursão.|  

> [!NOTE]  
> Políticas de recursão só podem ser criadas no nível do servidor.  

### <a name="zone-transfer-policies"></a>Políticas de transferência de zona  
As políticas de transferência de zona controlam se uma transferência de zona é permitida ou não pelo seu servidor DNS. Você pode criar políticas para transferência de zona no nível do servidor ou o nível de zona. Aplicam políticas de nível de servidor em cada consulta de transferência de zona que ocorre no servidor DNS. Políticas de nível de zona se aplicam apenas as consultas em uma zona hospedada no servidor DNS. O uso mais comum para as políticas de nível de zona é implementar listas bloqueadas ou seguras.  

> [!NOTE]  
> As políticas de transferência de zona só podem usar DENY ou ignorar como ações.  

Você pode usar a política de transferência de zona de nível de servidor abaixo para negar uma transferência de zona para o domínio contoso.com de uma determinada sub-rede:  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

Você pode criar a transferência de zona várias políticas de mesmo nível, desde que eles têm um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa as consultas de entrada da seguinte maneira:  

![Processo de DNS para várias políticas de transferência de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>Gerenciando políticas DNS  
Você pode criar e gerenciar políticas de DNS usando o PowerShell. Os exemplos a seguir percorrer os cenários de exemplo diferentes que podem ser configuradas por meio de políticas de DNS:  

### <a name="traffic-management"></a>Gerenciamento de tráfego  
Você pode direcionar o tráfego com base em um FQDN para servidores diferentes, dependendo do local do cliente DNS. O exemplo a seguir mostra como criar políticas de gerenciamento para direcionar os clientes de uma determinada sub-rede para um datacenter na América do Norte e de outra sub-rede para um data center europeu de tráfego.  

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

As duas primeiras linhas do script criam cliente objetos de sub-rede para a América do Norte e Europa. As duas linhas depois que criar um escopo de zona dentro do domínio contoso.com, um para cada região. As duas linhas depois que criam um registro em cada zona que associa ww.contoso.com para endereço IP diferente, um para a Europa, outra para a América do Norte. Por fim, as últimas linhas do script criam duas DNS consulta políticas de resolução, um para ser aplicado à sub-rede da América do Norte, outro para a sub-rede na Europa.  

### <a name="block-queries-for-a-domain"></a>Consultas de bloco para um domínio  
Você pode usar uma política de resolução de consulta de DNS a consultas de bloco em um domínio. O exemplo a seguir bloqueia todas as consultas treyresearch.net:  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Consultas de bloco de uma sub-rede  
Você também pode bloquear consultas provenientes de uma sub-rede específica. O script a seguir cria uma sub-rede para 172.0.33.0/24 e, em seguida, cria uma política para ignorar todas as consultas proveniente da sub-rede:  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Permitir a recursão para clientes internos  
Você pode controlar a recursão usando uma política de resolução de consulta de DNS. O exemplo a seguir pode ser usado para habilitar a recursão para clientes internos, ao mesmo tempo, desabilitá-lo para clientes externos em um cenário de divisão de dupla personalidade.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

A primeira linha no script altera o escopo de recursão padrão, simplesmente nomeado como "." (ponto) para desabilitar a recursão. A segunda linha cria um escopo de recursão chamado *InternalClients* com a recursão habilitada. E a terceira linha cria uma política se aplique os recém Criar escopo de recursão a todas as consultas entrando por meio de uma interface de servidor que tenha 10.0.0.34 como um endereço IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Criar uma política de transferência de zona de nível de servidor  
Você pode controlar a transferência de zona em um formulário mais granular por meio de políticas de transferência de zona DNS. O script de exemplo a seguir pode ser usado para permitir transferências de zona para qualquer servidor em uma determinada sub-rede:  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

A primeira linha no script cria um objeto de sub-rede chamado *AllowedSubnet* com o IP bloquear 172.21.33.0/24. A segunda linha cria uma política de transferência de zona para permitir transferências de zona para qualquer servidor DNS na sub-rede criada anteriormente.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Criar uma política de transferência de zona de nível de zona  
Você também pode criar políticas de transferência de zona de nível de zona. O exemplo a seguir ignora qualquer solicitação para uma transferência de zona para contoso.com proveniente de uma interface de servidor que tenha um endereço IP de 10.0.0.33:  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Cenários de política DNS

Para obter informações sobre como usar a política de DNS para cenários específicos, consulte os tópicos a seguir neste guia.

- [Usar a política de DNS para a localização geográfica com base em gerenciamento de tráfego com servidores primários](primary-geo-location.md)  
- [Usar a política de DNS para a localização geográfica com base em gerenciamento de tráfego com implantações primárias e secundárias](primary-secondary-geo-location.md)  
- [Usar política de DNS para respostas DNS inteligente com base na hora do dia](dns-tod-intelligent.md)
- [Servidor de aplicativos de nuvem de respostas DNS com base na hora do dia com o Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política de DNS para a implantação de DNS com partição de rede](split-brain-DNS-deployment.md)
- [Usar a política de DNS para o DNS com partição de rede no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para a aplicação de filtros em consultas DNS](apply-filters-on-dns-queries.md)
- [Usar a política de DNS para balanceamento de carga do aplicativo](app-lb.md)
- [Usar a política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Usando a política de DNS em controladores de domínio somente leitura

Política de DNS é compatível com os controladores de domínio somente leitura. Observe que uma reinicialização do serviço servidor DNS é necessária para novas políticas de DNS a ser carregado em controladores de domínio somente leitura. Isso não é necessário em controladores de domínio gravável.
