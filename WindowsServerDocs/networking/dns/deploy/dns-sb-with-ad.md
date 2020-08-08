---
title: Usar a política de DNS para DNS com partição de rede no Active Directory
description: Você pode usar este tópico para aproveitar os recursos de gerenciamento de tráfego das políticas de DNS para implantações de divisão-Brain com Active Directory zonas DNS integradas no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f6da8584f7a2b2221fb1a283b8ea4de842ddc58
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964132"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usar a política de DNS para DNS com partição de rede no Active Directory

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aproveitar os recursos de gerenciamento de tráfego das políticas de DNS para \- implantações de divisão Brain com Active Directory zonas DNS integradas no Windows Server 2016.

No Windows Server 2016, o suporte às políticas de DNS é estendido para Active Directory zonas DNS integradas. A integração do Active Directory fornece \- recursos de alta disponibilidade de vários mestres para o servidor DNS.

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada um fornecendo serviços a cada conjunto de usuários, interno e externo. Se apenas alguns registros dentro da zona tiverem sido reduzidos \- ou se ambas as instâncias da zona (internas e externas) tiverem sido delegadas para o mesmo domínio pai, isso se tornaria um enigma de gerenciamento.

> [!NOTE]
> - As implantações de DNS são mais \- inbrainas quando há duas versões de uma única zona, uma versão para usuários internos na intranet da organização e uma versão para usuários externos – que são, normalmente, usuários na Internet.
> - O tópico [usar a política DNS para implantação de DNS de divisão-Brain](split-brain-DNS-deployment.md) explica como você pode usar políticas de DNS e escopos de zona para implantar um \- sistema DNS de divisão de alto nível em um único servidor DNS do Windows Server 2016.

## <a name="example-split-brain-dns-in-active-directory"></a>Exemplo \- de DNS de divisão Brain no Active Directory

Este exemplo usa uma empresa fictícia, contoso, que mantém um site de carreira em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos em que as postagens de trabalho internas estão disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39.

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP público 65.55.39.10.

Na ausência da política DNS, o administrador precisa hospedar essas duas zonas em servidores DNS separados do Windows Server e gerenciá-las separadamente.

Usando políticas DNS essas zonas agora podem ser hospedadas no mesmo servidor DNS.

Se o servidor DNS para contoso.com estiver Active Directory integrado e estiver escutando em duas interfaces de rede, o administrador de DNS da Contoso poderá seguir as etapas neste tópico para obter uma implantação de divisão \- Brain.

O administrador de DNS configura as interfaces do servidor DNS com os seguintes endereços IP.

- O adaptador de rede voltado para a Internet é configurado com um endereço IP público de 208.84.0.53 para consultas externas.
- O adaptador de rede voltado para a intranet é configurado com um endereço IP privado de 10.0.0.56 para consultas internas.

A ilustração a seguir descreve esse cenário.

![Implantação de DNS integrado do AD de divisão-cérebro](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Como a política DNS para o DNS de divisão de \- cérebro no Active Directory funciona

Quando o servidor DNS é configurado com as políticas de DNS necessárias, cada solicitação de resolução de nome é avaliada em relação às políticas no servidor DNS.

A interface do servidor é usada neste exemplo como os critérios para diferenciar entre os clientes internos e externos.

Se a interface do servidor na qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo de zona associado é usado para responder à consulta.

Portanto, em nosso exemplo, as consultas DNS para www.career.contoso.com recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo de zona padrão (isso é o mesmo que a resolução de consulta normal).

\( \) Há suporte para atualizações e limpeza de DDNS DNS dinâmico apenas no escopo de zona padrão. Como os clientes internos são atendidos pelo escopo de zona padrão, os administradores de DNS da Contoso podem continuar usando os mecanismos existentes (DNS dinâmico ou estático) para atualizar os registros em contoso.com. Para \- escopos de zona não padrão \( , como o escopo externo neste exemplo \) , o suporte a DDNS ou de eliminação não está disponível.

### <a name="high-availability-of-policies"></a>Alta disponibilidade de políticas

As políticas de DNS não estão Active Directory integradas. Por isso, as políticas de DNS não são replicadas para os outros servidores DNS que hospedam a mesma zona integrada Active Directory.

As políticas de DNS são armazenadas no servidor DNS local. Você pode exportar facilmente as políticas de DNS de um servidor para outro usando os comandos do Windows PowerShell de exemplo a seguir.

```powershell
$policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
$policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02
```

Para obter mais informações, consulte os seguintes tópicos de referência do Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)

## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Como configurar a política DNS para o \- DNS de divisão Brain no Active Directory

Para configurar a implantação de divisão-Brain do DNS usando a política DNS, você deve usar as seções a seguir, que fornecem instruções de configuração detalhadas.

### <a name="add-the-active-directory-integrated-zone"></a>Adicionar a zona integrada Active Directory

Você pode usar o comando de exemplo a seguir para adicionar o Active Directory zona integrada do contoso.com ao servidor DNS.

```powershell
Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru
```

Para obter mais informações, consulte [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Criar os escopos da zona

Você pode usar esta seção para particionar a zona contoso.com para criar um escopo de zona externa.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.

Como você está adicionando esse novo escopo de zona em uma zona integrada Active Directory, o escopo de zona e os registros dentro dele serão replicados por meio de Active Directory para outros servidores de réplica no domínio.

Por padrão, um escopo de zona existe em todas as zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo. Esse escopo de zona padrão hospedará a versão interna do www.career.contoso.com.

Você pode usar o seguinte comando de exemplo para criar o escopo de zona no servidor DNS.

```powershell
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"
```

Para obter mais informações, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Adicionar registros aos escopos de zona

A próxima etapa é adicionar os registros que representam o host do servidor Web nos escopos de duas zonas-externo e padrão \( para clientes internos \) .

No escopo de zona interna padrão, o registro www.career.contoso.com é adicionado com o endereço IP 10.0.0.39, que é um endereço IP privado; e no escopo da zona externa, o mesmo registro \( www.Career.contoso.com \) é adicionado com o endereço IP público 65.55.39.10.

Os registros \( no escopo da zona interna padrão e no escopo da zona externa \) serão replicados automaticamente pelo domínio com seus respectivos escopos de zona.

Você pode usar o comando de exemplo a seguir para adicionar registros aos escopos de zona no servidor DNS.

```powershell
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”
```

> [!NOTE]
> O parâmetro **– ZoneScope** não é incluído quando o registro é adicionado ao escopo de zona padrão. Essa ação é igual à adição de registros a uma zona normal.

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Criar as políticas de DNS

Depois de identificar as interfaces de servidor para a rede externa e a rede interna e criar os escopos de zona, você deverá criar políticas de DNS que conectem os escopos de zona interna e externa.

> [!NOTE]
> Este exemplo usa a interface de servidor \( do parâmetro-ServerInterface no comando de exemplo abaixo \) como os critérios para diferenciar entre os clientes internos e externos. Outro método para diferenciar entre clientes externos e internos é usar sub-redes de cliente como um critério. Se você puder identificar as sub-redes às quais os clientes internos pertencem, você pode configurar a política DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de sub-rede do cliente, consulte [usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com servidores primários](primary-geo-location.md).

Depois de configurar as políticas, quando uma consulta DNS é recebida na interface pública, a resposta é retornada do escopo externo da zona.

> [!NOTE]
> Nenhuma política é necessária para mapear o escopo de zona interna padrão.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com
```

> [!NOTE]
> 208.84.0.53 é o endereço IP na interface de rede pública.

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora, o servidor DNS é configurado com as políticas de DNS necessárias para um servidor de nomes de divisão com uma zona DNS integrada Active Directory.

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.
