---
title: Usar a política de DNS para a implantação de DNS com partição de rede
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e8b19df2313bd0f3f6599aae8a23a18233f469e7
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766919"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usar a política DNS para a implantação de DNS de divisão \- Brain

>Aplica-se a: Windows Server 2016

Você pode usar este tópico para aprender a configurar a política DNS no Windows Server &reg; 2016 para implantações de DNS de divisão-Brain, em que há duas versões de uma única zona-uma para os usuários internos na intranet da sua organização e outra para os usuários externos, que normalmente são usuários na Internet.

>[!NOTE]
>Para obter informações sobre como usar a política DNS para a implantação de DNS de divisão \- Brain com Active Directory zonas DNS integrado, consulte [usar a política DNS para DNS de divisão-brain no Active Directory](dns-sb-with-ad.md).

Anteriormente, esse cenário exigia que os administradores de DNS mantenham dois servidores DNS diferentes, cada um fornecendo serviços a cada conjunto de usuários, interno e externo. Se apenas alguns registros dentro da zona tiverem sido reduzidos \- ou se ambas as instâncias da zona (internas e externas) tiverem sido delegadas para o mesmo domínio pai, isso se tornaria um enigma de gerenciamento.

Outro cenário de configuração para implantação de divisão-Brain é o controle de recursão seletiva para a resolução de nomes DNS. Em algumas circunstâncias, espera-se que os servidores DNS corporativos executem a resolução recursiva pela Internet para os usuários internos, enquanto eles também devem atuar como servidores de nomes autoritativos para usuários externos e bloquear a recursão para eles.

Este tópico inclui as seções a seguir.

- [Exemplo de implantação de divisão-Brain do DNS](#bkmk_sbexample)
- [Exemplo de controle de recursão seletiva de DNS](#bkmk_recursion)

## <a name="example-of-dns-split-brain-deployment"></a><a name="bkmk_sbexample"></a>Exemplo de implantação de divisão-Brain do DNS
Veja a seguir um exemplo de como você pode usar a política de DNS para realizar o cenário descrito anteriormente de DNS de divisão-cérebro.

Esta seção contém os seguintes tópicos.

- [Como funciona a implantação de divisão-Brain do DNS](#bkmk_sbhow)
- [Como configurar a implantação de divisão-Brain do DNS](#bkmk_sbconfigure)

Este exemplo usa uma empresa fictícia, contoso, que mantém um site de carreira em www.career.contoso.com.

O site tem duas versões, uma para os usuários internos em que as postagens de trabalho internas estão disponíveis. Este site interno está disponível no endereço IP local 10.0.0.39.

A segunda versão é a versão pública do mesmo site, que está disponível no endereço IP público 65.55.39.10.

Na ausência da política DNS, o administrador precisa hospedar essas duas zonas em servidores DNS separados do Windows Server e gerenciá-las separadamente.

Usando políticas DNS essas zonas agora podem ser hospedadas no mesmo servidor DNS.

A ilustração a seguir descreve esse cenário.

![Implantação de DNS de divisão-Brain](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)

## <a name="how-dns-split-brain-deployment-works"></a><a name="bkmk_sbhow"></a>Como funciona a implantação de divisão-Brain do DNS

Quando o servidor DNS é configurado com as políticas de DNS necessárias, cada solicitação de resolução de nome é avaliada em relação às políticas no servidor DNS.

A interface do servidor é usada neste exemplo como os critérios para diferenciar entre os clientes internos e externos.

Se a interface do servidor na qual a consulta é recebida corresponde a qualquer uma das políticas, o escopo de zona associado é usado para responder à consulta.

Portanto, em nosso exemplo, as consultas DNS para www.career.contoso.com recebidas no IP privado (10.0.0.56) recebem uma resposta DNS que contém um endereço IP interno; e as consultas DNS que são recebidas na interface de rede pública recebem uma resposta DNS que contém o endereço IP público no escopo de zona padrão (isso é o mesmo que a resolução de consulta normal).

## <a name="how-to-configure-dns-split-brain-deployment"></a><a name="bkmk_sbconfigure"></a>Como configurar a implantação de divisão-Brain do DNS
Para configurar a implantação de divisão-Brain do DNS usando a política DNS, você deve usar as etapas a seguir.

- [Criar os escopos de zona](#bkmk_zscopes)
- [Adicionar registros aos escopos de zona](#bkmk_records)
- [Criar as políticas de DNS](#bkmk_policies)

As seções a seguir fornecem instruções de configuração detalhadas.

>[!IMPORTANT]
>As seções a seguir incluem exemplos de comandos do Windows PowerShell que contêm valores de exemplo para muitos parâmetros. Certifique-se de substituir os valores de exemplo nesses comandos por valores apropriados para sua implantação antes de executar esses comandos.

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes"></a>Criar os escopos de zona

Um escopo de zona é uma instância exclusiva da zona. Uma zona DNS pode ter vários escopos de zona, com cada escopo de zona contendo seu próprio conjunto de registros DNS. O mesmo registro pode estar presente em vários escopos, com endereços IP diferentes ou os mesmos endereços IP.

> [!NOTE]
> Por padrão, um escopo de zona existe nas zonas DNS. Esse escopo de zona tem o mesmo nome que a zona e as operações de DNS herdadas funcionam nesse escopo. Esse escopo de zona padrão hospedará a versão externa do www.career.contoso.com.

Você pode usar o seguinte comando de exemplo para particionar o escopo de zona contoso.com para criar um escopo de zona interno. O escopo da zona interna será usado para manter a versão interna do www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obter mais informações, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Adicionar registros aos escopos de zona

A próxima etapa é adicionar os registros que representam o host do servidor Web nos escopos de duas zonas-interno e padrão (para clientes externos).

No escopo da zona interna, o registro <strong>www.Career.contoso.com</strong> é adicionado com o endereço IP 10.0.0.39, que é um IP privado; e no escopo de zona padrão, o mesmo registro, <strong>www.Career.contoso.com</strong>, é adicionado com o endereço IP 65.55.39.10.

Não **, o parâmetro – ZoneScope** é fornecido nos comandos de exemplo a seguir quando o registro está sendo adicionado ao escopo de zona padrão. Isso é semelhante à adição de registros a uma zona de baunilha.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obter mais informações, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>Criar as políticas de DNS

Depois de identificar as interfaces de servidor para a rede externa e a rede interna e criar os escopos de zona, você deverá criar políticas de DNS que conectem os escopos de zona interna e externa.

>[!NOTE]
>Este exemplo usa a interface de servidor como os critérios para diferenciar entre os clientes internos e externos. Outro método para diferenciar entre clientes externos e internos é usar sub-redes de cliente como um critério. Se você puder identificar as sub-redes às quais os clientes internos pertencem, você pode configurar a política DNS para diferenciar com base na sub-rede do cliente. Para obter informações sobre como configurar o gerenciamento de tráfego usando critérios de sub-rede do cliente, consulte [usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com servidores primários](./primary-geo-location.md).

Quando o servidor DNS recebe uma consulta na interface privada, a resposta de consulta DNS é retornada do escopo da zona interna.

>[!NOTE]
>Nenhuma política é necessária para mapear o escopo de zona padrão.

No comando de exemplo a seguir, 10.0.0.56 é o endereço IP na interface de rede privada, conforme mostrado na ilustração anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

## <a name="example-of-dns-selective-recursion-control"></a><a name="bkmk_recursion"></a>Exemplo de controle de recursão seletiva de DNS

Veja a seguir um exemplo de como você pode usar a política DNS para realizar o cenário descrito anteriormente do controle de recursão seletiva de DNS.

Esta seção contém os seguintes tópicos.

- [Como funciona o controle de recursão seletiva de DNS](#bkmk_recursionhow)
- [Como configurar o controle de recursão seletiva de DNS](#bkmk_recursionconfigure)

Este exemplo usa a mesma empresa fictícia que no exemplo anterior, contoso, que mantém um site de carreira em www.career.contoso.com.

No exemplo de implantação de DNS Split-Brain, o mesmo servidor DNS responde aos clientes internos e externos e fornece a eles respostas diferentes.

Algumas implantações de DNS podem exigir que o mesmo servidor DNS execute a resolução de nomes recursiva para clientes internos, além de atuar como o servidor de nome autoritativo para clientes externos. Essa circunstância é chamada de controle de recursão seletiva de DNS.

Nas versões anteriores do Windows Server, a habilitação da recursão significava que ela foi habilitada no servidor DNS inteiro para todas as zonas. Como o servidor DNS também está ouvindo consultas externas, a recursão é habilitada para clientes internos e externos, tornando o servidor DNS um resolvedor aberto.

Um servidor DNS configurado como um resolvedor aberto pode estar vulnerável ao esgotamento de recursos e pode ser abuso por clientes mal-intencionados para criar ataques de reflexo.

Por isso, os administradores de DNS da Contoso não querem que o servidor DNS para contoso.com execute a resolução de nomes recursiva para clientes externos. Há apenas uma necessidade de controle de recursão para clientes internos, enquanto o controle de recursão pode ser bloqueado para clientes externos.

A ilustração a seguir descreve esse cenário.

![Controle de recursão seletiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg)


### <a name="how-dns-selective-recursion-control-works"></a><a name="bkmk_recursionhow"></a>Como funciona o controle de recursão seletiva de DNS

Se uma consulta para a qual o servidor DNS contoso é não autoritativo for recebida, como para https://www.microsoft.com , a solicitação de resolução de nome será avaliada em relação às políticas no servidor DNS.

Como essas consultas não se enquadram em nenhuma zona, as políticas no nível da zona, \( conforme definido no exemplo de divisão-Brain, \) não são avaliadas.

O servidor DNS avalia as políticas de recursão e as consultas que são recebidas na interface privada correspondem ao **SplitBrainRecursionPolicy**. Essa política aponta para um escopo de recursão em que a recursão está habilitada.

Em seguida, o servidor DNS executa a recursão para obter a resposta https://www.microsoft.com da Internet e armazena a resposta em cache localmente.

Se a consulta for recebida na interface externa, nenhuma política de DNS será correspondente e a configuração de recursão padrão, que nesse caso está **desabilitada** , será aplicada.

Isso impede que o servidor atue como um resolvedor aberto para clientes externos, enquanto está atuando como um resolvedor de cache para clientes internos.

### <a name="how-to-configure-dns-selective-recursion-control"></a><a name="bkmk_recursionconfigure"></a>Como configurar o controle de recursão seletiva de DNS

Para configurar o controle de recursão seletiva de DNS usando a política DNS, você deve usar as etapas a seguir.

- [Criar escopos de recursão DNS](#bkmk_recscopes)
- [Criar políticas de recursão de DNS](#bkmk_recpolicy)

#### <a name="create-dns-recursion-scopes"></a><a name="bkmk_recscopes"></a>Criar escopos de recursão DNS

Escopos de recursão são instâncias exclusivas de um grupo de configurações que controlam a recursão em um servidor DNS. Um escopo de recursão contém uma lista de encaminhadores e especifica se a recursão está habilitada. Um servidor DNS pode ter muitos escopos de recursão.

A configuração de recursão herdada e a lista de encaminhadores são referenciadas como o escopo de recursão padrão. Você não pode adicionar ou remover o escopo de recursão padrão, identificado pelo nome ponto \( "." \) .

Neste exemplo, a configuração de recursão padrão é desabilitada, enquanto um novo escopo de recursão para clientes internos é criado onde a recursão está habilitada.

```powershell
Set-DnsServerRecursionScope -Name . -EnableRecursion $False
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True
```

Para obter mais informações, consulte [Add-DnsServerRecursionScope](/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="create-dns-recursion-policies"></a><a name="bkmk_recpolicy"></a>Criar políticas de recursão de DNS

Você pode criar políticas de recursão do servidor DNS para escolher um escopo de recursão para um conjunto de consultas que correspondem a critérios específicos.

Se o servidor DNS não for autoritativo para algumas consultas, as políticas de recursão do servidor DNS permitirão que você controle como resolver as consultas.

Neste exemplo, o escopo de recursão interno com recursão habilitada está associado à interface de rede privada.

Você pode usar o comando de exemplo a seguir para configurar políticas de recursão de DNS.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
```

Para obter mais informações, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Agora o servidor DNS está configurado com as políticas de DNS necessárias para um servidor de nome de divisão ou um servidor DNS com controle de recursão seletiva habilitado para clientes internos.

Você pode criar milhares de políticas de DNS de acordo com seus requisitos de gerenciamento de tráfego e todas as novas políticas são aplicadas dinamicamente, sem reiniciar o servidor DNS-em consultas de entrada.

Para obter mais informações, consulte [Guia de cenários de política de DNS](DNS-Policy-Scenario-Guide.md).