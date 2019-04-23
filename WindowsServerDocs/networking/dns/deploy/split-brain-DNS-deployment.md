---
title: Usar a política de DNS para a implantação de DNS com partição de rede
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4ec4bc8e77e8411101b9a2b83a85ad5e1a0765b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873497"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usar a política DNS para divisão\-cérebro implantação de DNS

>Aplica-se a: Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS no Windows Server&reg; 2016 para implantações de DNS com partição de rede, em que há duas versões de uma única zona de-um para os usuários internos na sua intranet da organização e outro para usuários externos, Normalmente, quem são os usuários na Internet.

>[!NOTE]
>Para obter informações sobre como usar a política de DNS para divisão\-zonas DNS integradas ao cérebro implantação de DNS com o Active Directory, consulte [usar a política de DNS para o DNS Split-Brain no Active Directory](dns-sb-with-ad.md).

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada fornecendo serviços a cada conjunto de usuários, internos e externos. Se apenas alguns registros dentro da zona foram divididos\-brained ou ambas as instâncias da zona (interna e externa) foram delegadas ao mesmo domínio pai, isso se tornou um enigma de gerenciamento. 

Outro cenário de configuração para implantação de separação é seletivo controle de recursão para resolução de nomes DNS. Em algumas circunstâncias, os servidores DNS da empresa devem executar a resolução recursiva pela Internet para usuários internos, embora eles também devem atuar como servidores de nomes autoritativo para usuários externos e bloquear a recursão para eles. 

Este tópico contém as seguintes seções.

- [Exemplo de implantação de DNS com partição de rede](#bkmk_sbexample)
- [Exemplo de controle de recursão seletiva de DNS](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>Exemplo de implantação de DNS com partição de rede
A seguir está um exemplo de como você pode usar a política de DNS para realizar o cenário descrito anteriormente do DNS com partição de rede.

Esta seção contém os seguintes tópicos.

- [Como funciona a implantação de DNS com partição de rede](#bkmk_sbhow)
- [Como configurar a implantação de DNS com partição de rede](#bkmk_sbconfigure)


Este exemplo usa uma empresa fictícia, Contoso, que mantém um site de carreira em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos, onde o emprego internos está disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39. 

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP público 65.55.39.10.

Na ausência de política de DNS, o administrador é necessária para hospedar essas duas zonas em servidores separados do DNS do Windows Server e gerenciá-los separadamente. 

Usando políticas DNS essas zonas podem agora ser hospedadas no mesmo servidor DNS.  

A ilustração a seguir ilustra esse cenário.

![Implantação de DNS com partição de rede](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>Como funciona a implantação de DNS com partição de rede

Quando o servidor DNS é configurado com as políticas DNS necessárias, cada solicitação de resolução de nome é avaliada em relação as políticas no servidor DNS.

O Interface de servidor é usado neste exemplo, como os critérios para diferenciar entre os clientes internos e externos.

Se a interface de servidor no qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo da região associada é usado para responder à consulta. 

Portanto, em nosso exemplo, as consultas DNS para www.career.contoso.com que são recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo de zona padrão (Isso é o mesmo que a resolução de consulta normal).  

##<a name="bkmk_sbconfigure"></a>Como configurar a implantação de DNS com partição de rede
Para configurar o DNS Split-Brain implantação por meio da política de DNS, você deve usar as etapas a seguir.

- [Criar os escopos de zona](#bkmk_zscopes)  
- [Adicionar registros para os escopos de zona](#bkmk_records)  
- [Crie as políticas de DNS](#bkmk_policies)

As seções a seguir fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos. 

###<a name="bkmk_zscopes"></a>Criar os escopos de zona

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com diferentes endereços IP ou os mesmos endereços IP. 

>[!NOTE]
>Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. Esse escopo de zona padrão hospedará a versão externa do www.career.contoso.com.

Você pode usar o comando de exemplo a seguir para particionar o escopo de zona contoso.com para criar um escopo interno de zona. O escopo da região interno será usado para manter a versão interna do www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

###<a name="bkmk_records"></a>Adicionar registros para os escopos de zona

A próxima etapa é adicionar os registros que representa o host do servidor Web para os escopos de duas zona - internos e padrão (para clientes externos). 

No escopo interno de zona, o registro **www.career.contoso.com** é adicionado com o endereço IP 10.0.0.39, que é um endereço IP privado; e no escopo de zona padrão mesmo registro **www.career.contoso.com**, é adicionado com o endereço IP 65.55.39.10.

Não **ZoneScope –** parâmetro é fornecido nos comandos de exemplo a seguir, quando o registro está sendo adicionado ao escopo de zona padrão. Isso é semelhante para adicionar registros a uma zona de baunilha.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obter mais informações, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

###<a name="bkmk_policies"></a>Crie as políticas de DNS

Depois que você identificou as interfaces de servidor para a rede externa e a rede interna e você tiver criado os escopos de zona, você deve criar políticas DNS que conectam os escopos de zona internos e externos.

>[!NOTE]
>Este exemplo usa a interface de servidor como o critério para diferenciar entre os clientes internos e externos. Outro método para diferenciar entre os clientes internos e externos é por meio de subredes de cliente como um critério. Se você puder identificar as sub-redes aos quais pertencem os clientes internos, você pode configurar a política de DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de subrede de cliente, consulte [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Quando o servidor DNS recebe uma consulta na interface privada, a resposta da consulta DNS é retornada do escopo interno de zona.

>[!NOTE]
>Não há políticas são necessárias para o escopo da região padrão de mapeamento. 

No comando de exemplo a seguir, 10.0.0.56 é o endereço IP no adaptador de rede privada, conforme mostrado na ilustração anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Exemplo de controle de recursão seletiva de DNS

A seguir está um exemplo de como você pode usar a política de DNS para realizar o cenário descrito anteriormente do controle de recursão seletiva de DNS.

Esta seção contém os seguintes tópicos.

- [Como funciona o controle DNS recursão seletivo](#bkmk_recursionhow)
- [Como configurar o controle de recursão seletiva de DNS](#bkmk_recursionconfigure)

Este exemplo usa a mesma da empresa fictícia do exemplo anterior, Contoso, que mantém um site de carreira em www.career.contoso.com.

O exemplo de implantação de separação de DNS, o mesmo servidor DNS responde aos clientes internos e externos e fornece-os com respostas diferentes. 

Algumas implantações de DNS podem exigir o mesmo servidor DNS para executar a resolução de nomes recursivos para clientes internos, além de atuar como servidor de nomes autoritativo para clientes externos. Nessa circunstância é chamada de controle de recursão seletiva de DNS.

Nas versões anteriores do Windows Server, permitindo a recursão significava que ele foi habilitado em todo o servidor DNS para todas as regiões. Porque o servidor DNS também está escutando para consultas externas, recursão está habilitado para clientes internos e externos, tornando o servidor DNS um resolvedor de abrir. 

Um servidor DNS que está configurado como um resolvedor aberto pode ser vulnerável a exaustão de recursos e pode ser usado por clientes mal-intencionados para criar ataques de reflexão. 

Por isso, os administradores de DNS da Contoso não deseja que o servidor DNS de contoso.com para executar a resolução recursiva para clientes externos. É necessário para o controle de recursão para clientes internos, enquanto o controle de recursão pode ser bloqueado para clientes externos. 

A ilustração a seguir ilustra esse cenário.

![Controle de recursão seletivo](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Como funciona o controle DNS recursão seletivo

Se for recebida uma consulta para o qual o servidor DNS da Contoso é não-autoritativa, como www.microsoft.com, em seguida, a solicitação de resolução de nome é avaliada em relação as políticas no servidor DNS. 

Porque essas consultas não se enquadram em qualquer região, a zona políticas no nível \(conforme definido no exemplo de separação\) não são avaliadas. 

O servidor DNS avalia as políticas de recursão e as consultas que são recebidas na correspondência interface privada de **SplitBrainRecursionPolicy**. Essa política aponta para um escopo de recursão onde a recursão está habilitada.

O servidor DNS, em seguida, executa a recursão para obter a resposta para www.microsoft.com da Internet e armazena em cache a resposta localmente. 

Se a consulta for recebida na interface externa, nenhuma correspondência de políticas DNS e a configuração de recursão padrão – que nesse caso é **desabilitado** -é aplicado.

Isso impede que o servidor que atua como um resolvedor aberto para clientes externos, enquanto ele está atuando como um resolvedor de cache para os clientes internos. 

### <a name="bkmk_recursionconfigure"></a>Como configurar o controle de recursão seletiva de DNS

Para configurar o controle de recursão seletiva de DNS usando a política de DNS, você deve usar as etapas a seguir.

- [Criar escopos de recursão DNS](#bkmk_recscopes)
- [Criar políticas de recursão DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Criar escopos de recursão DNS

Escopos de recursão são instâncias exclusivas de um grupo de configurações que controlam a recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se a recursão está habilitada. Um servidor DNS pode ter vários escopos de recursão. 

A configuração de recursão herdado e a lista de encaminhadores são chamados de escopo padrão de recursão. Você não pode adicionar ou remover o escopo padrão recursão, identificado pelo ponto nome \("." \).

Neste exemplo, a configuração de recursão padrão é desabilitada, enquanto um novo escopo de recursão para clientes internos é criado, onde a recursão está habilitada.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Para obter mais informações, consulte [DnsServerRecursionScope adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Criar políticas de recursão DNS

Você pode criar políticas de recursão para escolher um escopo de recursão para um conjunto de consultas que correspondem a critérios específicos do servidor DNS. 

Se o servidor DNS não autoritativo para algumas consultas, políticas de recursão de servidor DNS permitem que você controle como resolver as consultas. 

Neste exemplo, o escopo de recursão interno com a recursão habilitada é associado com a interface de rede privada.

Você pode usar o comando de exemplo a seguir para configurar as políticas de recursão DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Para obter mais informações, consulte [DnsServerQueryResolutionPolicy adicionar](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora o servidor DNS é configurado com as políticas DNS necessárias para um servidor de nomes com partição de rede ou um servidor DNS com o controle de recursão seletivo habilitado para clientes internos.

Você pode criar milhares de políticas DNS de acordo com seu tráfego de requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - nas consultas de entrada. 

Para obter mais informações, consulte [guia de cenário de política de DNS](DNS-Policy-Scenario-Guide.md).
