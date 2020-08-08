---
title: Planejamento da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 803fd90b2ad790375da0408a3bf1303b97e84cee
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995530"
---
# <a name="wireless-access-deployment-planning"></a>Planejamento da implantação de acesso sem fio

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Antes de implantar o acesso sem fio, você deve planejar os seguintes itens:

- Instalação de pontos de acesso sem fio \( APS \) em sua rede

- Configuração e acesso de cliente sem fio

As seções a seguir fornecem detalhes sobre essas etapas de planejamento.

## <a name="planning-wireless-ap-installations"></a>Planejando instalações de AP sem fio
Ao projetar sua solução de acesso à rede sem fio, você deve fazer o seguinte:

1. Determinar quais padrões seus APs sem fio devem dar suporte
2. Determinar as áreas de cobertura onde você deseja fornecer o serviço sem fio
3. Determinar onde você deseja localizar os APs sem fio

Além disso, você deve planejar um esquema de endereço IP para seus clientes sem fio e AP sem fio. Consulte a seção **planejar a configuração do AP sem fio no NPS** abaixo para obter informações relacionadas.

### <a name="verify-wireless-ap-support-for-standards"></a>Verificar o suporte a AP sem fio para padrões
Para fins de consistência e facilidade de implantação e gerenciamento de AP, é recomendável que você implante APs sem fio da mesma marca e modelo.

Os APs sem fio que você implantar devem oferecer suporte ao seguinte:

- **IEEE 802.1X**

- **Autenticação Radius**

- **Autenticação e codificação sem fio.** Listado na ordem mais para a menos preferível:

    1.  WPA2 \- Enterprise com AES

    2.  WPA2 \- Enterprise com TKIP

    3.  WPA \- Enterprise com AES

    4.  WPA \- Enterprise com TKIP

>[!NOTE]
>Para implantar o WPA2, você deve usar adaptadores de rede sem fio e APs sem fio que também dão suporte a WPA2. Caso contrário, use o WPA \- Enterprise.

Além disso, para fornecer segurança aprimorada para a rede, os APs sem fio devem dar suporte às seguintes opções de segurança:

- **Filtragem de DHCP.** O AP sem fio deve filtrar as portas IP para evitar a transmissão de mensagens de difusão DHCP nesses casos em que o cliente sem fio está configurado como um servidor DHCP. O AP sem fio deve impedir que o cliente envie pacotes IP da porta UDP 68 para a rede.

- **Filtragem de DNS.** O AP sem fio deve filtrar as portas IP para impedir que um cliente execute como um servidor DNS. O AP sem fio deve impedir que o cliente envie pacotes IP da porta TCP ou UDP 53 para a rede.

- **Isolamento de cliente** Se o ponto de acesso sem fio fornece recursos de isolamento de cliente, você deve habilitar o recurso para impedir possíveis \( explorações de falsificação ARP do protocolo de resolução de endereço \) .

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar áreas de cobertura para usuários sem fio
Use desenhos arquitetônicos de cada andar para identificar as áreas em que você deseja fornecer cobertura sem fio. Por exemplo, identifique os escritórios apropriados, as salas de conferências, lobbies, lanchonetes ou Courtyards.

Nos desenhos, indique todos os dispositivos que interfiram com os sinais sem fio, como equipamentos médicos, câmeras de vídeo sem fio, telefones sem fio que operam no 2,4 a 2,5 GHz, o intervalo do ISM industrial, científico e médico e os \( \) \- dispositivos habilitados para Bluetooth.

No desenho, marque aspectos da construção que podem interferir com sinais sem fio; os objetos metálicos usados na construção de um edifício podem afetar o sinal sem fio. Por exemplo, os objetos comuns a seguir podem interferir na propagação de sinal: Elevadors, dutos de calefação e ar- \- condicionado e suporte concreto girders.

Consulte o fabricante do seu AP para obter informações sobre fontes que podem causar atenuação de radiofrequência de AP sem fio. A maioria dos APs fornece software de teste que você pode usar para verificar a intensidade do sinal, a taxa de erros e a taxa de transferência de dados.

### <a name="determine-where-to-install-wireless-aps"></a>Determinar onde instalar APs sem fio
Nos desenhos arquitetônicos, localize seus APs sem fio perto o suficiente para fornecer uma ampla cobertura sem fio, mas muito distante que eles não interferem uns com os outros.

A distância necessária entre os APs depende do tipo de antena AP e PA, aspectos da construção que bloqueia sinais sem fio e outras fontes de interferência. Você pode marcar lugares de ponto de acesso sem fio para que cada AP sem fio não tenha mais de 300 pés de qualquer AP sem fio adjacente. Consulte a documentação do fabricante do AP sem fio para obter as especificações e diretrizes de AP para posicionamento.

Instale temporariamente os APs sem fio nos locais especificados em seus desenhos arquitetônicos. Em seguida, usando um laptop equipado com um adaptador sem fio 802,11 e o software de pesquisa de site que normalmente é fornecido com adaptadores sem fio, determine a intensidade do sinal dentro de cada área de cobertura.

Em áreas de cobertura em que a intensidade do sinal é baixa, posicione o AP para melhorar a intensidade do sinal para a área de cobertura, instale APs sem fio adicionais para fornecer a cobertura necessária, realocar ou remover fontes de interferência de sinal.

Atualize seus desenhos arquitetônicos para indicar o posicionamento final de todos os APs sem fio. Ter um mapa de colocação de ponto de acesso preciso ajudará mais tarde durante operações de solução de problemas ou quando você quiser atualizar ou substituir APs.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planejar a configuração de cliente do AP sem fio e do NPS RADIUS
Você pode usar o NPS para configurar APs sem fio individualmente ou em grupos.

Se você estiver implantando uma rede sem fio grande que inclui muitos APs, será muito mais fácil configurar o APs em grupos. Para adicionar os APs como grupos de clientes RADIUS no NPS, você deve configurar o APs com essas propriedades.

- Os APs sem fio são configurados com endereços IP do mesmo intervalo de endereços IP.

- Os APs sem fio são todos configurados com o mesmo segredo compartilhado.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planejar o uso da reconexão rápida de PEAP
Em uma infraestrutura 802.1 X, os pontos de acesso sem fio são configurados como clientes RADIUS para servidores RADIUS. Quando o PEAP reconnect Fast é implantado, um cliente sem fio que faz roaming entre dois ou mais pontos de acesso não precisa ser autenticado com cada nova associação.

A reconexão rápida do PEAP reduz o tempo de resposta para autenticação entre o cliente e o autenticador, pois a solicitação de autenticação é encaminhada do novo ponto de acesso para o NPS que originalmente realizou a autenticação e autorização para a solicitação de conexão do cliente.

Como o cliente PEAP e o NPS usam as propriedades de conexão TLS de segurança da camada de transporte em cache \( \) \( , a coleção da qual é chamada de identificador TLS \) , o NPS pode determinar rapidamente que o cliente está autorizado para uma reconexão.

>[!IMPORTANT]
>Para que a reconexão rápida funcione corretamente, os APs devem ser configurados como clientes RADIUS do mesmo NPS.

Se o NPS original ficar indisponível ou se o cliente mudar para um ponto de acesso configurado como um cliente RADIUS para um servidor RADIUS diferente, a autenticação completa deverá ocorrer entre o cliente e o novo autenticador.

### <a name="wireless-ap-configuration"></a>Configuração de AP sem fio
A lista a seguir resume os itens normalmente configurados em \- APS sem fio compatíveis com 802.1 x:

> [!NOTE]
> Os nomes de item podem variar de acordo com a marca e o modelo e podem ser diferentes daqueles na lista a seguir. Consulte a documentação do AP sem fio para obter \- detalhes específicos de configuração.

- Identificador do conjunto de ** \( serviços SSID \) **. Esse é o nome da rede sem fio, \( por exemplo, ExampleWlan \) , e o nome que é anunciado para clientes sem fio. Para reduzir a confusão, o SSID que você escolher anunciar não deve corresponder ao SSID que é transmitido por qualquer rede sem fio que esteja dentro do intervalo de recepção da sua rede sem fio.

    Em casos em que vários APs sem fio são implantados como parte da mesma rede sem fio, configure cada AP sem fio com o mesmo SSID. Em casos em que vários APs sem fio são implantados como parte da mesma rede sem fio, configure cada AP sem fio com o mesmo SSID.

    Nos casos em que você tem a necessidade de implantar diferentes redes sem fio para atender às necessidades de negócios específicas, o AP sem fio em uma rede deve transmitir um SSID diferente do SSID de outras redes \( \) . Por exemplo, se precisar de uma rede sem fio separada para seus funcionários e convidados, você poderá configurar seus APs sem fio para a rede de negócios com o SSID definido como Broadcast **ExampleWLAN**. Para sua rede de convidados, você pode definir cada SSID de AP sem fio para difundir **GuestWLAN**. Dessa forma, seus funcionários e convidados podem se conectar à rede pretendida sem confusão desnecessária.

    > [!TIP]
    > Alguns pontos de acesso sem fio têm a capacidade de difundir vários SSIDs para acomodar várias \- implantações de rede. Os pontos de acesso sem fio que podem difundir vários SSIDs podem reduzir os custos de implantação e manutenção operacional.

- **Autenticação e criptografia sem fio**.

    A autenticação sem fio é a autenticação de segurança que é usada quando o cliente sem fio se associa a um ponto de acesso sem fio.

    A criptografia sem fio é a codificação de criptografia de segurança usada com a autenticação sem fio para proteger as comunicações que são enviadas entre o AP sem fio e o cliente sem fio.

- **Endereço IP do AP sem fio \( estático \) **. Em cada AP sem fio, configure um endereço IP estático exclusivo. Se a sub-rede for atendida por um servidor DHCP, verifique se todos os endereços IP de AP se enquadram em um intervalo de exclusão DHCP para que o servidor DHCP não tente emitir o mesmo endereço IP para outro computador ou dispositivo. Os intervalos de exclusão são documentados no procedimento "para criar e ativar um novo escopo de DHCP" no [Guia de rede principal](../../core-network-guide.md). Se você estiver planejando configurar APs como clientes RADIUS por grupo no NPS, cada AP no grupo deverá ter um endereço IP do mesmo intervalo de endereços IP.

- **Nome DNS**. Alguns APs sem fio podem ser configurados com um nome DNS. Configure cada AP sem fio com um nome exclusivo. Por exemplo, se você tiver um APs sem fio implantado em uma compilação de várias \- histórias, poderá nomear os três primeiros APS sem fio implantados no terceiro andar AP3 \- 01, AP3 \- 02 e AP3 \- 03.

- **Máscara de sub-rede AP sem fio**. Configure a máscara para designar qual parte do endereço IP é a ID de rede e qual parte do endereço IP é o host.

- **Serviço DHCP do AP**. Se o AP sem fio tiver um \- serviço DHCP interno, desabilite-o.

- **Segredo compartilhado RADIUS**. Use um segredo compartilhado RADIUS exclusivo para cada AP sem fio, a menos que você esteja planejando configurar clientes RADIUS do NPS em grupos – em que circunstância você deve configurar todos os APs no grupo com o mesmo segredo compartilhado. Os segredos compartilhados devem ser uma sequência aleatória de pelo menos 22 caracteres de comprimento, com letras maiúsculas e minúsculas, números e pontuação. Para garantir a aleatoriedade, você pode usar um programa de geração de caracteres aleatório para criar seus segredos compartilhados. É recomendável que você registre o segredo compartilhado para cada AP sem fio e armazene-o em um local seguro, como um escritório seguro. Ao configurar clientes RADIUS no console do NPS, você criará uma versão virtual de cada AP. O segredo compartilhado que você configura em cada ponto de acesso virtual no NPS deve corresponder ao segredo compartilhado no AP físico real.

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do NPS que você deseja usar para autenticar e autorizar solicitações de conexão a este ponto de acesso.

- **Porta \( s \) UDP**. Por padrão, o NPS usa as portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e portas UDP 1813 e 1646 para mensagens de contabilização RADIUS. É recomendável que você não altere as configurações padrão de portas UDP RADIUS.

- **VSAs**. Alguns APs sem fio exigem \- atributos específicos do fornecedor \( VSAs \) para fornecer funcionalidade completa de AP sem fio.

- **Filtragem de DHCP**. Configure os APs sem fio para impedir que clientes sem fio enviem pacotes IP da porta UDP 68 para a rede. Consulte a documentação do AP sem fio para configurar a filtragem de DHCP.

- **Filtragem de DNS**. Configure os APs sem fio para impedir que clientes sem fio enviem pacotes IP da porta TCP ou UDP 53 para a rede. Consulte a documentação do seu AP sem fio para configurar a filtragem de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planejando a configuração e o acesso do cliente sem fio

Ao planejar a implantação do \- acesso sem fio autenticado 802.1 x, você deve considerar vários \- fatores específicos do cliente:

- **Planejando o suporte para vários padrões**.

    Determine se os computadores sem fio estão usando a mesma versão do Windows ou se eles são uma mistura de computadores que executam sistemas operacionais diferentes. Se eles forem diferentes, verifique se você entendeu quaisquer diferenças em padrões com suporte nos sistemas operacionais.

    Determine se todos os adaptadores de rede sem fio em todos os computadores cliente sem fio oferecem suporte aos mesmos padrões sem fio ou se você precisa dar suporte a padrões variados. Por exemplo, determine se alguns drivers de hardware de adaptador de rede dão suporte a WPA2 \- Enterprise e AES, enquanto outros dão suporte apenas a WPA \- Enterprise e TKIP.

- **Planejando o modo de autenticação do cliente**. Os modos de autenticação definem como os clientes Windows processam as credenciais de domínio. Você pode selecionar entre os três modos de autenticação de rede a seguir nas políticas de rede sem fio.

    1. **Re- \- autenticação do usuário**. Esse modo especifica que a autenticação é sempre executada usando credenciais de segurança com base no estado atual do computador. Quando nenhum usuário estiver conectado ao computador, a autenticação será executada usando as credenciais do computador. Quando um usuário faz logon no computador, a autenticação é sempre executada usando as credenciais do usuário.

    2. **Computador somente**. O modo somente computador Especifica que a autenticação é sempre executada usando apenas as credenciais do computador.

    3.  **Autenticação do usuário**. Modo de autenticação de usuário especifica que a autenticação é executada somente quando o usuário faz logon no computador. Quando não há usuários conectados ao computador, as tentativas de autenticação não são executadas.

- **Planejando restrições sem fio**. Determine se você deseja fornecer a todos os seus usuários sem fio o mesmo nível de acesso à sua rede sem fio ou se deseja restringir o acesso para alguns dos seus usuários sem fio. Você pode aplicar restrições no NPS em grupos específicos de usuários sem fio.  Por exemplo, você pode definir dias e horas específicos para os quais certos grupos têm permissão de acesso à rede sem fio.

- **Métodos de planejamento para adicionar novos computadores sem fio**. Para \- computadores com capacidade sem fio que ingressaram em seu domínio antes de implantar sua rede sem fio, se o computador estiver conectado a um segmento da rede com fio que não está protegido pelo 802.1 x, as definições de configuração sem fio serão aplicadas automaticamente depois que você configurar \( as políticas de rede sem fio IEEE 802,11 \) no controlador de domínio e depois que política de grupo for atualizada no cliente sem fio.

    No entanto, para computadores que ainda não ingressaram em seu domínio, você deve planejar um método para aplicar as configurações necessárias para o \- acesso autenticado 802.1 x. Por exemplo, determine se você deseja ingressar o computador no domínio usando um dos métodos a seguir.

    1.  Conecte o computador a um segmento da rede com fio que não está protegida pelo 802.1 X e, em seguida, ingresse o computador no domínio.

    2.  Forneça aos seus usuários sem fio as etapas e configurações necessárias para adicionar seu próprio perfil de Bootstrap sem fio, o que permite que eles ingressem o computador no domínio.

    3.  Atribua à equipe de ti para ingressar clientes sem fio no domínio.

### <a name="planning-support-for-multiple-standards"></a>Planejando o suporte para vários padrões

A extensão de políticas de rede sem fio \( IEEE 802,11 \) no política de grupo fornece uma ampla gama de opções de configuração para dar suporte a uma variedade de opções de implantação.

Você pode implantar APs sem fio configurados com os padrões que você deseja dar suporte e, em seguida, configurar vários perfis sem fio em políticas IEEE 802,11 de rede sem fio \( \) , com cada perfil especificando um conjunto de padrões que você precisa.

Por exemplo, se sua rede tiver computadores sem fio que dão suporte a WPA2 \- Enterprise e AES, outros computadores que dão suporte \- a WPA Enterprise e AES e outros computadores que dão suporte apenas a WPA \- Enterprise e TKIP, você deverá determinar se deseja:

- Configure um único perfil para dar suporte a todos os computadores sem fio usando o método de criptografia mais fraco ao qual todos os seus computadores dão suporte. nesse caso, o WPA \- Enterprise e o TKIP.
- Configure dois perfis para fornecer a melhor segurança possível que seja suportada por cada computador sem fio. Nessa instância, você configuraria um perfil que especifica a criptografia mais forte \( WPA2 \- Enterprise e AES \) , e um perfil que usa a criptografia WPA \- Enterprise e TKIP mais fraca. Neste exemplo, é essencial que você coloque o perfil que usa o WPA2 \- Enterprise e o AES de forma mais alta na ordem de preferência. Os computadores que não são capazes de usar o WPA2 \- Enterprise e AES irão pular automaticamente para o próximo perfil na ordem de preferência e processar o perfil que especifica WPA \- Enterprise e TKIP.

> [!IMPORTANT]
> Você deve posicionar o perfil com os padrões mais seguros mais altos na lista ordenada de perfis, pois a conexão de computadores usa o primeiro perfil que eles são capazes de usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planejando o acesso restrito à rede sem fio

Em muitos casos, talvez você queira fornecer aos usuários sem fio vários níveis de acesso à rede sem fio. Por exemplo, talvez você queira permitir que alguns usuários tenham acesso irrestrito, qualquer hora do dia, todos os dias da semana. Para outros usuários, talvez você queira apenas permitir o acesso durante as horas principais, de segunda-feira a sexta-feira, e negar acesso no sábado e no domingo.

Este guia fornece instruções para criar um ambiente de acesso que coloca todos os seus usuários sem fio em um grupo com acesso comum a recursos sem fio. Você cria um grupo de segurança de usuários sem fio no Active Directory usuários e computadores e \- faz com que todos os usuários para quem você deseja conceder acesso sem fio a um membro desse grupo.

Ao configurar as políticas de rede do NPS, você especifica o grupo de segurança usuários sem fio como o objeto que o NPS processa ao determinar a autorização.

No entanto, se sua implantação exigir suporte para vários níveis de acesso, você só precisará fazer o seguinte:

1. Crie mais de um grupo de segurança de usuários sem fio para criar grupos de segurança sem fio adicionais em Active Directory usuários e computadores. Por exemplo, você pode criar um grupo que contém usuários que têm acesso completo, um grupo para aqueles que têm acesso somente durante o horário de trabalho normal e outros grupos que se ajustam a outros critérios que correspondem às suas necessidades.

2. Adicione usuários aos grupos de segurança apropriados que você criou.

3. Configure políticas de rede NPS adicionais para cada grupo de segurança sem fio adicional e configure as políticas para aplicar as condições e restrições que você precisa para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Métodos de planejamento para adicionar novos computadores sem fio

O método preferencial para unir novos computadores sem fio ao domínio e, em seguida, fazer logon no domínio é usando uma conexão com fio a um segmento da LAN que tem acesso aos controladores de domínio e não é protegido por um comutador Ethernet de autenticação 802.1 X.

Em alguns casos, no entanto, talvez não seja prático usar uma conexão com fio para ingressar computadores no domínio, ou para que um usuário use uma conexão com fio para o primeiro logon na tentativa usando computadores que já ingressaram no domínio.

Para ingressar um computador no domínio usando uma conexão sem fio ou para que os usuários façam logon no domínio na primeira vez usando um \- computador ingressado no domínio e uma conexão sem fio, os clientes sem fio devem primeiro estabelecer uma conexão com a rede sem fio em um segmento que tenha acesso aos controladores de domínio de rede usando um dos métodos a seguir.

1. **Um membro da equipe de ti une um computador sem fio ao domínio e, em seguida, configura um perfil sem fio de Bootstrap de logon único.** Com esse método, um administrador de ti conecta o computador sem fio à rede Ethernet com fio e, em seguida, une o computador ao domínio. Em seguida, o administrador distribui o computador ao usuário. Quando o usuário inicia o computador, as credenciais de domínio que eles especificam manualmente para o processo de logon do usuário são usadas para estabelecer uma conexão com a rede sem fio e fazer logon no domínio.

2. **O usuário configura manualmente o computador sem fio com o perfil sem fio de Bootstrap e, em seguida, une o domínio.** Com esse método, os usuários configuram manualmente seus computadores sem fio com um perfil sem fio de Bootstrap com base nas instruções de um administrador de ti. O perfil sem fio de Bootstrap permite que os usuários estabeleçam uma conexão sem fio e, em seguida, ingressem o computador no domínio. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário poderá fazer logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para implantar o acesso sem fio, consulte [implantação de acesso sem fio](e-wireless-access-deployment.md).