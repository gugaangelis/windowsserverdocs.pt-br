---
title: Visão geral das políticas de DNS
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 687864619c981b3ab8d24ef540c759bc29314c90
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544668"
---
# <a name="dns-policies-overview"></a>Visão geral das políticas de DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a política DNS, que é nova no Windows Server 2016. Você pode usar a política DNS para gerenciamento de tráfego baseado em localização geográfica, respostas de DNS inteligente com base na hora do dia, para gerenciar um único servidor DNS\-configurado para implantação de divisão Brain, aplicação de filtros em consultas DNS e muito mais. Os itens a seguir fornecem mais detalhes sobre esses recursos.

-   **Balanceamento de carga do aplicativo.** Quando você implantou várias instâncias de um aplicativo em locais diferentes, você pode usar a política DNS para balancear a carga de tráfego entre as diferentes instâncias de aplicativo, alocando dinamicamente a carga de tráfego para o aplicativo.

-   **Gerenciamento\-de tráfego baseado na localização geográfica.** Você pode usar a política DNS para permitir que servidores DNS primários e secundários respondam a consultas de cliente DNS com base na localização geográfica do cliente e do recurso ao qual o cliente está tentando se conectar, fornecendo ao cliente o endereço IP do mais próximo Kit. 

-   **DNS de divisão de cérebro.** Com o\-DNS de divisão Brain, os registros DNS são divididos em escopos de zona diferentes no mesmo servidor DNS, e os clientes DNS recebem uma resposta com base no fato de os clientes serem clientes internos ou externos. Você pode configurar o\-DNS de divisão de cérebro para Active Directory zonas integradas ou para zonas em servidores DNS autônomos.

-   **Aplica.** Você pode configurar a política DNS para criar filtros de consulta baseados em critérios fornecidos por você. Os filtros de consulta na política DNS permitem configurar o servidor DNS para responder de uma maneira personalizada com base na consulta DNS e no cliente DNS que envia a consulta DNS. 
-   **Análise forense.** Você pode usar a política DNS para redirecionar clientes DNS mal-intencionados\-para um endereço IP não existente em vez de direcioná-los para o computador que estão tentando acessar.

-   **Hora do redirecionamento com base no dia.** Você pode usar a política DNS para distribuir o tráfego de aplicativos em diferentes instâncias distribuídas geograficamente de um aplicativo usando políticas de DNS com base na hora do dia.

## <a name="new-concepts"></a>Novos conceitos  
Para criar políticas para dar suporte aos cenários listados acima, é necessário ser capaz de identificar grupos de registros em uma zona, grupos de clientes em uma rede, entre outros elementos. Esses elementos são representados pelos seguintes novos objetos DNS:  

- **Sub-rede do cliente:** um objeto de sub-rede de cliente representa uma sub-rede IPv4 ou IPv6 da qual as consultas são enviadas a um servidor DNS. Você pode criar sub-redes para definir posteriormente as políticas a serem aplicadas com base em qual sub-rede as solicitações vêm. Por exemplo, em um cenário de DNS de divisão Brain, a solicitação de resolução para um nome como <em>www.Microsoft.com</em> pode ser respondida com um endereço IP interno para clientes de sub-redes internas e um endereço IP diferente para clientes em sub-redes externas.

- **Escopo da recursão:** os escopos de recursão são instâncias exclusivas de um grupo de configurações que controlam a recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se a recursão está habilitada. Um servidor DNS pode ter muitos escopos de recursão. As políticas de recursão do servidor DNS permitem que você escolha um escopo de recursão para um conjunto de consultas. Se o servidor DNS não for autoritativo para determinadas consultas, as políticas de recursão do servidor DNS permitirão que você controle como resolver essas consultas. Você pode especificar quais encaminhadores devem ser usados e se a recursão deve ser usada.

- Escopos de **zona:** uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes. Além disso, as transferências de zona são feitas no nível de escopo da zona. Isso significa que os registros de um escopo de zona em uma zona primária serão transferidos para o mesmo escopo de zona em uma zona secundária.

## <a name="types-of-policy"></a>Tipos de política

As políticas de DNS são divididas por nível e tipo. Você pode usar as políticas de resolução de consulta para definir como as consultas são processadas e as políticas de transferência de zona para definir como as transferências de zona ocorrem. Você pode aplicar cada tipo de política no nível do servidor ou no nível da zona.

### <a name="query-resolution-policies"></a>Políticas de resolução de consulta

Você pode usar políticas de resolução de consulta DNS para especificar como as consultas de resolução de entrada são tratadas por um servidor DNS. Cada política de resolução de consulta DNS contém os seguintes elementos:  

|Campo|Descrição|Valores possíveis|  
|---------|---------------|-------------------|  
|**Name**|Nome da política|-Até 256 caracteres<br />-Pode conter qualquer caractere válido para um nome de arquivo|  
|**Estado**|Estado da política|-Habilitar (padrão)<br />-Desabilitado|  
|**Geral**|Nível de política|-Servidor<br />-Zona|  
|**Ordem de processamento**|Quando uma consulta é classificada por nível e aplica-se a, o servidor encontra a primeira política para a qual a consulta corresponde aos critérios e a aplica à consulta|-Valor numérico<br />-Valor exclusivo por política que contém o mesmo nível e aplica-se ao valor|  
|**Ação**|Ação a ser executada pelo servidor DNS|-Permitir (padrão para nível de zona)<br />-Deny (padrão no nível do servidor)<br />-Ignorar|  
|**Aos**|Condição de política (AND/OR) e lista de critérios a serem atendidos para que a política seja aplicada|-Operador-condição (AND/OR)<br />-Lista de critérios (consulte a tabela critério abaixo)|  
|**Escopo**|Lista de escopos de zona e valores ponderados por escopo. Valores ponderados são usados para distribuição de balanceamento de carga. Por exemplo, se essa lista incluir Datacenter1 com peso de 3 e datacenter2 com um peso de 5, o servidor responderá com um registro de datacentre1 três vezes em oito solicitações|-Lista de escopos de zona (por nome) e pesos|  

> [!NOTE]
> As políticas de nível de servidor só podem ter os valores **negar** ou **ignorar** como uma ação.

O campo de critérios de política DNS é composto por dois elementos:


|              Nome               |                                         Descrição                                          |                                                                                                                               Valores de exemplo                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Sub-rede do cliente**        | Nome de uma sub-rede de cliente predefinida. Usado para verificar a sub-rede da qual a consulta foi enviada. |                             -   **EQ, Espanha, França** -resolve para verdadeiro se a sub-rede for identificada como Espanha ou França<br />-   **Ne, Canadá, México** -resolve para verdadeiro se a sub-rede do cliente for qualquer sub-rede diferente do Canadá e do México                             |
|     **Protocolo de transporte**      |        Protocolo de transporte usado na consulta. As entradas possíveis são **UDP** e **TCP**        |                                                                                                                    -   **EQ, TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocolo de Internet**      |        Protocolo de rede usado na consulta. As entradas possíveis são **IPv4** e **IPv6**        |                                                                                                                   -   **EQ, IPv4**<br />-   **EQ, IPv6**                                                                                                                    |
| **Endereço IP da interface do servidor** |                   Endereço IP para a interface de rede do servidor DNS de entrada                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **FULLY**             |            FQDN do registro na consulta, com a possibilidade de usar um curinga            | -   **EQ, www. contoso. com** -resolve para verdadeiro somente se a consulta estiver tentando resolver o FQDN do <em>www.contoso.com</em><br />-   **EQ,\*. contoso.com,\*. Woodgrove.com** -resolve para verdadeiro se a consulta for para qualquer registro que termina em *contoso.com***ou***Woodgrove.com* |
|         **Tipo de consulta**          |                          Tipo de registro que está sendo consultado (A, SRV, TXT)                          |                                                  -   **EQ, txt, SRV** -resolve para verdadeiro se a consulta estiver solicitando um registro txt **ou** SRV<br />-   **EQ, MX** -resolve para verdadeiro se a consulta estiver solicitando um registro MX                                                   |
|         **Hora do dia**         |                              Hora do dia em que a consulta é recebida                               |                                                                    -   **EQ, 10:00-12:00, 22:00-23:00** -resolve para verdadeiro se a consulta for recebida entre 10 a.m. e meio-dia, **ou** entre 19:10 e 23h                                                                    |

Usando a tabela acima como ponto de partida, a tabela a seguir pode ser usada para definir um critério que é usado para fazer a correspondência de consultas para qualquer tipo de registro, mas os registros SRV no domínio contoso.com provenientes de um cliente na sub-rede 10.0.0.0/24 via TCP entre 8 e 10 horas por meio de i 10.0.0.3 nterface:  

|Nome|Valor|  
|--------|---------|  
|Sub-rede do cliente|EQ, 10.0.0.0/24|  
|Protocolo de transporte|EQ, TCP|  
|Endereço IP da interface do servidor|EQ, 10.0.0.3|  
|FQDN|EQ, *. contoso. com|  
|Tipo de Consulta|NE, SRV|  
|Hora do dia|EQ, 20:00-22:00|  

Você pode criar várias políticas de resolução de consulta do mesmo nível, desde que elas tenham um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa as consultas de entrada da seguinte maneira:  

![Processamento de política DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Políticas de recursão  
As políticas de recursão são um **tipo** especial de políticas de nível de servidor. As políticas de recursão controlam como o servidor DNS executa a recursão para uma consulta. As políticas de recursão se aplicam somente quando o processamento de consultas atinge o caminho de recursão Você pode escolher um valor de negar ou ignorar para recursão para um conjunto de consultas. Como alternativa, você pode escolher um conjunto de encaminhadores para um conjunto de consultas.  

Você pode usar políticas de recursão para implementar uma configuração de DNS de divisão-cérebro. Nessa configuração, o servidor DNS executa a recursão para um conjunto de clientes para uma consulta, enquanto o servidor DNS não executa recursão para outros clientes para essa consulta.  

As políticas de recursão contêm os mesmos elementos que uma política de resolução de consulta DNS regular contém, juntamente com os elementos na tabela abaixo:  

|Nome|Descrição|  
|--------|---------------|  
|**Aplicar na recursão**|Especifica que essa política só deve ser usada para recursão.|  
|**Escopo da recursão**|Nome do escopo de recursão.|  

> [!NOTE]  
> As políticas de recursão só podem ser criadas no nível do servidor.  

### <a name="zone-transfer-policies"></a>Políticas de transferência de zona  
As políticas de transferência de zona controlam se uma transferência de zona é permitida ou não pelo seu servidor DNS. Você pode criar políticas para transferência de zona no nível do servidor ou no nível da zona. As políticas de nível de servidor se aplicam a cada consulta de transferência de zona que ocorre no servidor DNS. As políticas de nível de zona se aplicam somente às consultas em uma zona hospedada no servidor DNS. O uso mais comum para políticas de nível de zona é implementar listas bloqueadas ou seguras.  

> [!NOTE]  
> As políticas de transferência de zona só podem usar DENY ou IGNORE como ações.  

Você pode usar a política de transferência de zona de nível de servidor abaixo para negar uma transferência de zona para o domínio contoso.com de uma determinada sub-rede:  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

Você pode criar várias políticas de transferência de zona do mesmo nível, desde que elas tenham um valor diferente para a ordem de processamento. Quando várias políticas estão disponíveis, o servidor DNS processa as consultas de entrada da seguinte maneira:  

![Processo DNS para várias políticas de transferência de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>Gerenciando políticas DNS  
Você pode criar e gerenciar políticas DNS usando o PowerShell. Os exemplos a seguir passam por diferentes cenários de exemplo que você pode configurar por meio de políticas de DNS:  

### <a name="traffic-management"></a>Gerenciamento de tráfego  
Você pode direcionar o tráfego com base em um FQDN para servidores diferentes, dependendo do local do cliente DNS. O exemplo a seguir mostra como criar políticas de gerenciamento de tráfego para direcionar os clientes de uma determinada sub-rede para um datacenter da América do Norte e de outra sub-rede para um datacenter Europeu.  

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

As duas primeiras linhas do script criam objetos de sub-rede de cliente para América do Norte e Europa. As duas linhas depois dessa criam um escopo de zona dentro do domínio contoso.com, uma para cada região. As duas linhas depois dessa criam um registro em cada zona que associa ww.contoso.com a um endereço IP diferente, um para a Europa, outro para América do Norte. Por fim, as últimas linhas do script criam duas políticas de resolução de consulta DNS, uma para ser aplicada à sub-rede América do Norte, outra para a sub-rede da Europa.  

### <a name="block-queries-for-a-domain"></a>Bloquear consultas para um domínio  
Você pode usar uma política de resolução de consulta DNS para bloquear consultas a um domínio. O exemplo a seguir bloqueia todas as consultas para treyresearch.net:  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Bloquear consultas de uma sub-rede  
Você também pode bloquear consultas provenientes de uma sub-rede específica. O script a seguir cria uma sub-rede para 172.0.33.0/24 e, em seguida, cria uma política para ignorar todas as consultas provenientes dessa sub-rede:  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Permitir recursão para clientes internos  
Você pode controlar a recursão usando uma política de resolução de consulta DNS. O exemplo a seguir pode ser usado para habilitar a recursão para clientes internos, ao mesmo tempo em que é desabilitado para clientes externos em um cenário de divisão Brain.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

A primeira linha no script altera o escopo de recursão padrão, simplesmente chamado de "." (ponto) para desabilitar a recursão. A segunda linha cria um escopo de recursão chamado *InternalClients* com recursão habilitada. E a terceira linha cria uma política para aplicar o escopo de recursão recém-criado a quaisquer consultas provenientes por meio de uma interface de servidor que tenha 10.0.0.34 como um endereço IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Criar uma política de transferência de zona no nível do servidor  
Você pode controlar a transferência de zona em uma forma mais granular usando políticas de transferência de zona DNS. O script de exemplo abaixo pode ser usado para permitir transferências de zona para qualquer servidor em uma determinada sub-rede:  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

A primeira linha no script cria um objeto de sub-rede chamado *AllowedSubnet* com o bloqueio de IP 172.21.33.0/24. A segunda linha cria uma política de transferência de zona para permitir transferências de zona para qualquer servidor DNS na sub-rede criada anteriormente.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Criar uma política de transferência de zona no nível da zona  
Você também pode criar políticas de transferência de zona no nível da zona. O exemplo a seguir ignora qualquer solicitação de transferência de zona para contoso.com provenientes de uma interface de servidor que tenha um endereço IP de 10.0.0.33:  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Cenários de política DNS

Para obter informações sobre como usar a política DNS para cenários específicos, consulte os tópicos a seguir neste guia.

- [Usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com servidores primários](primary-geo-location.md)  
- [Usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com implantações primárias secundárias](primary-secondary-geo-location.md)  
- [Usar a política DNS para respostas de DNS inteligentes com base na hora do dia](dns-tod-intelligent.md)
- [Respostas DNS com base na hora do dia com um servidor de aplicativos de nuvem do Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política DNS para implantação de DNS de divisão-Brain](split-brain-DNS-deployment.md)
- [Usar a política DNS para o DNS de divisão de cérebro no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para aplicar filtros em consultas DNS](apply-filters-on-dns-queries.md)
- [Usar política DNS para balanceamento de carga de aplicativo](app-lb.md)
- [Usar política DNS para balanceamento de carga de aplicativo com reconhecimento de localização geográfica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Usando a política DNS em controladores de domínio somente leitura

A política DNS é compatível com controladores de domínio somente leitura. Observe que uma reinicialização do serviço do servidor DNS é necessária para que novas políticas de DNS sejam carregadas em controladores de domínio somente leitura. Isso não é necessário em controladores de domínio graváveis.
