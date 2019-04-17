---
title: Usar a política de DNS para a implantação de uma DNS
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usar a política de DNS para a implantação de DNS Split\ cérebro

>Aplica-se a: Windows Server 2016

Você pode usar este tópico para saber como configurar a política DNS no Windows Server&reg; 2016 para uma implantações de DNS, onde há duas versões de um único da zona - um para os usuários internos na intranet organização e para os usuários externos, que geralmente são os usuários na Internet.

>[!NOTE]
>Para obter informações sobre como usar a política de DNS para zonas DNS integradas de implantação de DNS cérebro split\ com o Active Directory, consulte [usar política de DNS para Split-Brain DNS no Active Directory](dns-sb-with-ad.md).

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada fornecendo serviços para cada conjunto de usuários, internos e externos. Se apenas alguns registros interior da região foram brained split\ ou ambas as instâncias da zona (interna e externa) foram recebidas no mesmo domínio pai, isso se tornou um enigma de gerenciamento. 

Outro cenário de configuração para uma implantação é o controle de recursão seletiva DNS para resolução de nomes. Em algumas circunstâncias, os servidores DNS corporativos devem executar a resolução recursiva pela Internet para os usuários internos, enquanto eles também devem atuar como servidores de nomes de autorização para usuários externos e bloquear recursão para eles. 

Este tópico contém as seções a seguir.

- [Exemplo de uma implantação do DNS](#bkmk_sbexample)
- [Exemplo de controle de recursão seletiva DNS](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>Exemplo de uma implantação do DNS
Este é um exemplo de como você pode usar a política DNS para realizar o cenário descrito anteriormente de uma DNS.

Esta seção contém os tópicos a seguir.

- [Como funciona a implantação de uma DNS](#bkmk_sbhow)
- [Como configurar uma implantação do DNS](#bkmk_sbconfigure)


Este exemplo usa uma empresa fictícia Contoso, que mantém um site de carreiras em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos onde vagas internas estão disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39. 

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP 65.55.39.10 público.

Na ausência de política DNS, o administrador é necessária para hospedar estas duas regiões em servidores DNS do Windows Server separados e gerenciá-las separadamente. 

Usando políticas DNS essas zonas podem agora ser hospedadas no mesmo servidor DNS.  

A ilustração a seguir ilustra esse cenário.

![Implantação de uma DNS](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>Como funciona a implantação de uma DNS

Quando o servidor DNS é configurado com as políticas DNS necessárias, cada solicitação de resolução de nome é avaliada em relação as políticas no servidor DNS.

O servidor Interface é usado neste exemplo, como os critérios para diferenciar entre os clientes internos e externos.

Se a interface de servidor no qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo da região associado é usado para responder à consulta. 

Assim, em nosso exemplo, as consultas do DNS para www.career.contoso.com que são recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas do DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo zona padrão (Isso é igual a resolução de consulta normal).  

##<a name="bkmk_sbconfigure"></a>Como configurar uma implantação do DNS
Para configurar o DNS Split-Brain implantação usando a política de DNS, você deve usar as etapas a seguir.

- [Criar os escopos de zona](#bkmk_zscopes)  
- [Adicionar registros os escopos de zona](#bkmk_records)  
- [Criar as políticas DNS](#bkmk_policies)

As seguintes seções fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos. 

###<a name="bkmk_zscopes"></a>Criar os escopos de zona

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona que contém seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em múltiplos escopos, com diferentes endereços IP ou os mesmos endereços IP. 

>[!NOTE]
>Por padrão, um escopo zona existe nas zonas DNS. Este escopo zona tem o mesmo nome que a zona e operações de DNS herdadas funcionam neste escopo. Este escopo de zona padrão irá hospedar a versão externa do www.career.contoso.com.

Você pode usar o comando de exemplo a seguir para particionar o contoso.com de escopo de zona para criar um escopo de região interna. O escopo de zona interno será usado para manter a versão interna do www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obter mais informações, consulte [DnsServerZoneScope adicionar](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>Adicionar registros os escopos de zona

A próxima etapa é adicionar os registros que representa o host do servidor da Web para os escopos de duas zona - internos e o padrão (para clientes externos). 

No escopo zona interno, o registro **www.career.contoso.com** é adicionado com o endereço IP 10.0.0.39, que é um IP privado; e no escopo de zona padrão mesmo registro, **www.career.contoso.com**, é adicionado com o endereço IP 65.55.39.10.

Não **– ZoneScope** parâmetro é fornecido nos comandos de exemplo a seguir, quando o registro está sendo adicionado o escopo de zona padrão. Isso é semelhante a adição de registros para uma zona de baunilha.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obter mais informações, consulte [adicionar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies"></a>Criar as políticas DNS

Depois que você identificou as interfaces do servidor para a rede externa e a rede interna, e você criou os escopos de zona, você deve criar políticas DNS que conectam os escopos de zona internas e externas.

>[!NOTE]
>Este exemplo usa a interface de servidor como os critérios para diferenciar entre os clientes internos e externos. Outro método para diferenciar entre clientes externos e internos é usando sub-redes do cliente como um critério. Se você pode identificar as sub-redes ao qual pertencem os clientes internos, você pode configurar política DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de sub-rede do cliente, consulte [usar a política de DNS para o gerenciamento de tráfego com base em localização geográfica com servidores primários](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Quando o servidor DNS recebe uma consulta na interface privada, a resposta da consulta DNS é retornada do escopo região interna.

>[!NOTE]
>Não há políticas são necessárias para mapear o escopo da região padrão. 

No comando de exemplo a seguir, 10.0.0.56 é o endereço IP na interface de rede privada, conforme mostrado na ilustração anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  


## <a name="bkmk_recursion"></a>Exemplo de controle de recursão seletiva DNS

Este é um exemplo de como você pode usar a política DNS para realizar o cenário descrito anteriormente do controle de recursão seletiva de DNS.

Esta seção contém os tópicos a seguir.

- [Como funciona o controle DNS recursão seletiva](#bkmk_recursionhow)
- [Como configurar o controle de recursão seletiva DNS](#bkmk_recursionconfigure)

Este exemplo usa a mesma empresa fictícia como no exemplo anterior, Contoso, que mantém um site de carreiras em www.career.contoso.com.

O exemplo de uma implantação de DNS, o mesmo servidor DNS responde para os clientes internos e externos e fornece respostas diferentes. 

Algumas implantações de DNS podem exigir o mesmo servidor DNS para realizar a resolução recursiva de nomes de clientes internos além atuando como o servidor de nomes de autorização para clientes externos. Neste caso é chamado de controle de recursão seletiva de DNS.

Em versões anteriores do Windows Server, permitindo recursão significava que ele foi habilitado em todo o servidor DNS para todas as regiões. Porque o servidor DNS também está ouvindo consultas externas, recursão está habilitada para clientes internos e externos, fazendo o servidor DNS um resolvedor aberto. 

Um servidor DNS que está configurado como um resolvedor aberto poderá ficar vulnerável a esgotamento de recursos e pode ser usada por clientes mal intencionados para criar ataques de reflexão. 

Por isso, os administradores de Contoso DNS não deseja que o servidor DNS para contoso.com realizar a resolução recursiva de nomes de clientes externos. Há apenas uma necessidade de controle de recursão para clientes internos, enquanto o controle de recursão pode ser bloqueado para clientes externos. 

A ilustração a seguir ilustra esse cenário.

![Controle de recursão seletiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Como funciona o controle DNS recursão seletiva

Se for recebida uma consulta para os quais o servidor DNS da Contoso é não-autorizada, como para www.microsoft.com, em seguida, a solicitação de resolução de nome é avaliada contra as políticas no servidor DNS. 

Porque essas consultas não se enquadram em qualquer zona, a zona nível políticas \ (conforme definido na uma example\) não são avaliadas. 

O servidor DNS avalia as políticas de recursão e as consultas que são recebidas na correspondência interface privada a **SplitBrainRecursionPolicy**. Esta configuração de política aponta para um escopo de recursão onde recursão está habilitada.

O servidor DNS, em seguida, executa recursão para obter a resposta para www.microsoft.com da Internet e armazena em cache localmente a resposta. 

Se a consulta for recebida em interface externa, nenhuma correspondência de políticas DNS e a configuração de recursão do padrão - neste caso é **desabilitada** -é aplicada.

Isso impede que o servidor atuando como um resolvedor aberto para clientes externos, enquanto ele está atuando como um resolvedor de cache para os clientes internos. 

### <a name="bkmk_recursionconfigure"></a>Como configurar o controle de recursão seletiva DNS

Para configurar o controle de recursão seletiva DNS usando a política de DNS, você deve usar as etapas a seguir.

- [Criar escopos de recursão DNS](#bkmk_recscopes)
- [Criar políticas de recursão DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Criar escopos de recursão DNS

Escopos de recursão são exclusivas instâncias de um grupo de configurações que controlam recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se recursão está habilitada. Um servidor DNS pode ter vários escopos de recursão. 

A configuração de recursão herdados e lista de encaminhadores são chamados do escopo de recursão padrão. Você não pode adicionar ou remover o escopo de recursão padrão, identificado pelo ponto de nome \("." \).

Neste exemplo, a configuração de recursão padrão está desabilitada, enquanto um novo escopo de recursão para clientes internos é criado em que recursão está habilitado.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Para obter mais informações, consulte [DnsServerRecursionScope adicionar](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>Criar políticas de recursão DNS

Você pode criar políticas de recursão para escolher um escopo recursão para um conjunto de consultas que correspondam a critérios específicos do servidor DNS. 

Se o servidor DNS não for autoritativo para algumas consultas, políticas de recursão de servidor DNS permitem que você controle como resolver as consultas. 

Neste exemplo, o escopo de recursão interno com recursão habilitado está associado com a interface de rede privada

Você pode usar o comando de exemplo a seguir para configurar políticas de recursão DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

Para obter mais informações, consulte [adicionar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Agora o servidor DNS é configurado com as políticas DNS necessárias para um servidor de nome de uma ou um servidor DNS com o controle de recursão seletiva habilitado para os clientes internos.

Você pode criar milhares de políticas de DNS de acordo com o tráfego requisitos de gerenciamento e todas as novas políticas são aplicadas dinamicamente - sem reiniciar o servidor DNS - em consultas recebidas. 

Para obter mais informações, consulte [guia cenário da política de DNS](DNS-Policy-Scenario-Guide.md).
