---
title: Usar a política DNS para um DNS no Active Directory
description: Você pode usar este tópico para aproveitar o tráfego de recursos de gerenciamento de políticas DNS para uma implantações com o Active Directory integrado zonas DNS no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usar a política DNS para um DNS no Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para aproveitar os recursos de gerenciamento de tráfego de políticas DNS para implantações de split\ cérebro com o Active Directory integradas DNS zonas no Windows Server 2016.

No Windows Server 2016, suporte a políticas DNS é estendido para o Active Directory zonas DNS integradas. Integração do Active Directory fornece recursos de multi\ mestre alta disponibilidade para o servidor DNS. 

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada fornecendo serviços para cada conjunto de usuários, internos e externos. Se apenas alguns registros interior da região foram brained split\ ou ambas as instâncias da zona (interna e externa) foram recebidas no mesmo domínio pai, isso se tornou um enigma de gerenciamento.

>[!NOTE]
> - Implantações de DNS são split\ cérebro quando há duas versões de uma única zona, uma versão para usuários internos na intranet da organização e uma versão para usuários externos – quem são, em geral, os usuários na Internet.
> - O tópico [usar política de DNS para Split-Brain DNS implantação](split-brain-DNS-deployment.md) explica como você pode usar políticas DNS e zona escopos para implantar um sistema DNS cérebro split\ em um único servidor DNS do Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Exemplo Split\ cérebro DNS no Active Directory

Este exemplo usa uma empresa fictícia Contoso, que mantém um site de carreiras em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos onde vagas internas estão disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39. 

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP 65.55.39.10 público.

Na ausência de política DNS, o administrador é necessária para hospedar estas duas regiões em servidores DNS do Windows Server separados e gerenciá-las separadamente. 

Usando políticas DNS essas zonas podem agora ser hospedadas no mesmo servidor DNS.

Se o servidor DNS para contoso.com é integrada ao Active Directory e está escutando duas interfaces de rede, o administrador de DNS de Contoso pode seguir as etapas neste tópico para obter uma implantação split\ cérebro.

O administrador de DNS configura as interfaces do servidor DNS com os seguintes endereços IP.

- O adaptador de rede direcionado ao Internet está configurado com um endereço IP público de 208.84.0.53 para consultas externas.
- O adaptador de rede oposto Intranet está configurado com um endereço IP privado 10.0.0.56 para consultas internas.

A ilustração a seguir ilustra esse cenário.

![Split-Brain Implantação de DNS integrado ao AD](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Como funciona a política DNS para Split\ cérebro DNS no Active Directory

Quando o servidor DNS é configurado com as políticas DNS necessárias, cada solicitação de resolução de nome é avaliada em relação as políticas no servidor DNS.

O servidor Interface é usado neste exemplo, como os critérios para diferenciar entre os clientes internos e externos.

Se a interface de servidor no qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo da região associado é usado para responder à consulta. 

Assim, em nosso exemplo, as consultas do DNS para www.career.contoso.com que são recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas do DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo zona padrão (Isso é igual a resolução de consulta normal).  

Suporte para atualizações dinâmicas DNS \(DDNS\) e eliminação é compatível apenas com o escopo da região padrão. Como os clientes internos são atendidos pelo escopo da região padrão, os administradores de DNS de Contoso pode continuar usando os mecanismos existentes (dinâmica DNS ou estático) para atualizar os registros em contoso.com. Para escopos de zona padrão non\ \ (por exemplo, o escopo externo neste example\), DDNS ou eliminação de suporte não está disponível.

### <a name="high-availability-of-policies"></a>Alta disponibilidade de políticas

As políticas DNS não são integrada ao Active Directory. Por isso, políticas DNS não são duplicadas para os outros servidores DNS que hospedam a mesmo zona integrada do Active Directory. 

Políticas DNS são armazenadas no servidor DNS local. Você pode facilmente exportar políticas DNS de um servidor para outro, usando os seguintes comandos do Windows PowerShell de exemplo.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Para obter mais informações, consulte os seguintes tópicos de referência do Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Como configurar a política DNS para Split\ cérebro DNS no Active Directory

Para configurar o DNS Split-Brain implantação usando a política de DNS, você deve usar as seções a seguir, que fornecem instruções de configuração detalhadas.

### <a name="add-the-active-directory-integrated-zone"></a>Adicionar a zona integrada do Active Directory

Você pode usar o comando de exemplo a seguir para adicionar a zona de contoso.com integrada do Active Directory para o servidor DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Para obter mais informações, consulte [adicionar DnsServerPrimaryZone](https://technet.microsoft.com/library/jj649876.aspx).

### <a name="create-the-scopes-of-the-zone"></a>Criar os escopos da zona

Você pode usar esta seção para a zona contoso.com para criar um escopo externo zona de partição.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP. 

Como você está adicionando esse novo escopo de zona em uma zona integrada do Active Directory, o escopo da região e os registros dentro dele replicará por meio do Active Directory para outros réplica servidores no domínio.

Por padrão, um escopo zona existe em cada zona DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. Este escopo de zona padrão irá hospedar a versão interna do www.career.contoso.com.

Você pode usar o comando de exemplo a seguir para criar o escopo de zona no servidor DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Para obter mais informações, consulte [adicionar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx).

### <a name="add-records-to-the-zone-scopes"></a>Adicionar registros os escopos de zona

A próxima etapa é adicionar os registros que representa o host do servidor da web para os dois escopos externa da zona e padrão \(for internal clients\). 

No escopo zona interno padrão, o registro www.career.contoso.com é adicionado com o endereço IP 10.0.0.39, que é um endereço IP privado; e no escopo externo zona, a mesma \(www.career.contoso.com\) registro é adicionado com o endereço IP público 65.55.39.10. 

Os registros \ (ambas no escopo da região interno padrão e o scope\ zona externo) replicará automaticamente em todo o domínio com seus escopos respectivo fuso.

Você pode usar o comando de exemplo a seguir para adicionar registros os escopos de zona no servidor DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>O **– ZoneScope** parâmetro não está incluído quando o registro é adicionado para o escopo da região padrão. Essa ação é igual a adição de registros para uma zona normal.

Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

### <a name="create-the-dns-policies"></a>Criar as políticas DNS

Depois que você identificou as interfaces do servidor para a rede externa e a rede interna, e você criou os escopos de zona, você deve criar políticas DNS que conectam os escopos de zona internas e externas.

>[!NOTE]
>Este exemplo usa a interface de servidor \ (o parâmetro - ServerInterface o below\ de comando de exemplo) como os critérios para diferenciar entre os clientes internos e externos. Outro método para diferenciar entre clientes externos e internos é usando sub-redes do cliente como um critério. Se você pode identificar as sub-redes ao qual pertencem os clientes internos, você pode configurar política DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de sub-rede do cliente, consulte [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](primary-geo-location.md).

Depois de configurar as políticas, quando uma consulta DNS é recebida na interface pública, a resposta é retornada do escopo externo da zona. 

>[!NOTE]
>Não há políticas são necessárias para mapear o escopo da região interno padrão. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 é o endereço IP na interface de rede pública.

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Agora o servidor DNS esteja configurado com as políticas DNS necessárias para um servidor de nome de uma com um Active Directory integrado DNS zona.

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas. 
