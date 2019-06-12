---
title: Usar a política de DNS para DNS com partição de rede no Active Directory
description: Você pode usar este tópico para tráfego de aproveitar os recursos de gerenciamento de políticas DNS para implantações com partição de rede com o Active Directory integrado as zonas DNS no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 66931d2196b741e469cb726929f7b58985b8d0cd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812144"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usar a política de DNS para DNS com partição de rede no Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aproveitar os recursos de gerenciamento de tráfego das políticas de DNS para divisão\-implantações de dupla personalidade com o Active Directory integrado as zonas DNS no Windows Server 2016.

No Windows Server 2016, suporte para políticas DNS é estendido para o Active Directory zonas DNS integradas. Integração do Active Directory fornece várias\-dominar os recursos de alta disponibilidade para o servidor DNS. 

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada fornecendo serviços a cada conjunto de usuários, internos e externos. Se apenas alguns registros dentro da zona foram divididos\-brained ou ambas as instâncias da zona (interna e externa) foram delegadas ao mesmo domínio pai, isso se tornou um enigma de gerenciamento.

> [!NOTE]
> - Implantações de DNS são divididas\-cérebro quando há duas versões de uma única zona, uma versão para usuários internos na intranet da organização e uma versão para usuários externos – que são, normalmente, os usuários na Internet.
> - O tópico [usar a política de DNS para a implantação de DNS Split-Brain](split-brain-DNS-deployment.md) explica como você pode usar políticas de DNS e escopos de zona para implantar uma divisão\-cérebro sistema DNS em um único servidor DNS do Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Exemplo de divisão\-cérebro DNS no Active Directory

Este exemplo usa uma empresa fictícia, Contoso, que mantém um site de carreira em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos, onde o emprego internos está disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39. 

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP público 65.55.39.10.

Na ausência de política de DNS, o administrador é necessária para hospedar essas duas zonas em servidores separados do DNS do Windows Server e gerenciá-los separadamente. 

Usando políticas DNS essas zonas podem agora ser hospedadas no mesmo servidor DNS.

Se o servidor DNS de contoso.com é integrada ao Active Directory e está escutando em duas interfaces de rede, o administrador de DNS da Contoso pode seguir as etapas neste tópico para obter uma divisão\-cérebro implantação.

O administrador de DNS configura as interfaces do servidor DNS com os seguintes endereços IP.

- O adaptador de rede voltado para a Internet está configurado com um endereço IP público de 208.84.0.53 para consultas externas.
- O adaptador de rede voltado para a Intranet é configurado com um endereço IP privado de 10.0.0.56 para consultas internas.

A ilustração a seguir ilustra esse cenário.

![Implantação de DNS integrado ao AD de separação](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Como a política de DNS para dividir\-cérebro DNS no Active Directory funciona

Quando o servidor DNS é configurado com as políticas DNS necessárias, cada solicitação de resolução de nome é avaliada em relação as políticas no servidor DNS.

O Interface de servidor é usado neste exemplo, como os critérios para diferenciar entre os clientes internos e externos.

Se a interface de servidor no qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo da região associada é usado para responder à consulta. 

Portanto, em nosso exemplo, as consultas DNS para www.career.contoso.com que são recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo de zona padrão (Isso é o mesmo que a resolução de consulta normal).  

Suporte para o DNS dinâmico \(DDNS\) atualizações e eliminação tem suporte apenas no escopo de zona padrão. Porque os clientes internos são atendidos pelo escopo de zona padrão, os administradores de DNS do Contoso pode continuar usando os mecanismos existentes (dinâmico DNS ou estático) para atualizar os registros em contoso.com. Para não\-padrão de escopos de zona \(como o escopo externo neste exemplo\), suporte de eliminação ou DDNS não está disponível.

### <a name="high-availability-of-policies"></a>Alta disponibilidade das políticas

Políticas de DNS não são integrados do Active Directory. Por isso, as políticas de DNS não são replicadas para os outros servidores DNS que hospedam a mesma zona integrada do Active Directory. 

As políticas DNS são armazenadas no servidor DNS local. Você pode facilmente exportar políticas de DNS de um servidor para outro usando os seguintes comandos do Windows PowerShell de exemplo.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Para obter mais informações, consulte os seguintes tópicos de referência do Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Como configurar a política de DNS para a divisão\-cérebro DNS no Active Directory

Para configurar o DNS Split-Brain implantação por meio da política de DNS, você deve usar as seções a seguir, que fornecem instruções detalhadas de configuração.

### <a name="add-the-active-directory-integrated-zone"></a>Adicionar a zona integrada ao Active Directory

Você pode usar o comando de exemplo a seguir para adicionar a zona contoso.com integrado ao Active Directory para o servidor DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Para obter mais informações, consulte [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Criar os escopos da zona

Você pode usar esta seção para particionar a zona contoso.com para criar um escopo externo de zona.

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP. 

Como você está adicionando este novo escopo de zona em uma zona integrada ao Active Directory, o escopo de zona e os registros dentro dela serão replicadas por meio do Active Directory para outros servidores de réplica no domínio.

Por padrão, um escopo de zona existe em cada zona DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. Esse escopo de zona padrão hospedará a versão interna do www.career.contoso.com.

Você pode usar o comando de exemplo a seguir para criar o escopo de zona no servidor DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Adicionar registros para os escopos de zona

A próxima etapa é adicionar os registros que representa o host do servidor web para os dois escopos externos de zona e padrão \(para clientes internos\). 

No escopo interno zona padrão, o registro www.career.contoso.com é adicionado com o endereço IP 10.0.0.39, que é um endereço IP privado; e no escopo externo de zona, o mesmo registro \(www.career.contoso.com\) é adicionada com o endereço IP público 65.55.39.10. 

Os registros \(tanto no padrão interno de zona escopo e o escopo de zona externo\) serão replicadas automaticamente em todo o domínio com seus escopos respectiva zona.

Você pode usar o comando de exemplo a seguir para adicionar registros para os escopos de zona no servidor DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> O **ZoneScope –** parâmetro não for incluído quando o registro é adicionado ao escopo de zona padrão. Essa ação é igual a adicionar registros a uma zona normal.

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Crie as políticas de DNS

Depois que você identificou as interfaces de servidor para a rede externa e a rede interna e você tiver criado os escopos de zona, você deve criar políticas DNS que conectam os escopos de zona internos e externos.

> [!NOTE]
> Este exemplo usa a interface de servidor \(o parâmetro - ServerInterface no comando de exemplo a seguir\) como critérios de diferenciar entre os clientes internos e externos. Outro método para diferenciar entre os clientes internos e externos é por meio de subredes de cliente como um critério. Se você puder identificar as sub-redes aos quais pertencem os clientes internos, você pode configurar a política de DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de subrede de cliente, consulte [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](primary-geo-location.md).

Depois de configurar políticas, quando uma consulta DNS é recebida na interface pública, a resposta é retornada do escopo externo da zona. 

> [!NOTE]
> Não há políticas são necessárias para o escopo da região interna padrão de mapeamento. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 é o endereço IP no adaptador de rede pública.

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora o servidor DNS é configurado com as políticas DNS necessárias para um servidor de nomes com partição de rede com um Active Directory integrado DNS zona.

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada. 
