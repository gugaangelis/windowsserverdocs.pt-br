---
title: Planejamento da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseado em senha 802.1 X acesso autenticado sem fio"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2571f509fbbca8384e626ad3c8c13f1c0a50400
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855457"
---
# <a name="wireless-access-deployment-planning"></a>Planejamento da implantação de acesso sem fio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Antes de implantar o acesso sem fio, você deve planejar os seguintes itens:

- Instalação de pontos de acesso sem fio \(APs\) em sua rede

- Acesso e configuração de cliente sem fio

As seções a seguir fornecem detalhes sobre essas etapas de planejamento.

## <a name="planning-wireless-ap-installations"></a>Planejando instalações de AP sem fio
Ao projetar sua solução de acesso de rede sem fio, você deve fazer o seguinte:

1. Determinar para quais padrões devem oferecer suporte a seus PAS sem fio
2. Determinar as áreas de cobertura no qual você deseja fornecer serviços sem fio
3. Determine onde você deseja localizar APs sem fio

Além disso, você deve planejar um esquema de endereço IP de seu PA sem fio e sem fio de clientes. Consulte a seção **planeje a configuração de AP sem fio no NPS** abaixo para obter informações relacionadas.

### <a name="verify-wireless-ap-support-for-standards"></a>Verifique se o suporte de AP sem fio para padrões
Para fins de consistência e a facilidade de implantação e gerenciamento de ponto de acesso, é recomendável que você implante APs sem fio da mesma marca e modelo.

Os APs sem fio que você implanta devem oferecer suporte a seguir:

- **IEEE 802.1X**

- **Autenticação RADIUS**

- **Autenticação sem fio e codificação.** Listados na ordem do mais ao menos preferencial:

    1.  WPA2\-Enterprise com o AES

    2.  WPA2\-Enterprise com TKIP

    3.  WPA\-Enterprise com o AES

    4.  WPA\-Enterprise com TKIP

>[!NOTE]
>Para implantar o WPA2, você deve usar adaptadores de rede sem fio e APs sem fio que também oferecem suporte a WPA2. Caso contrário, use o WPA\-Enterprise.

Além disso, para fornecer segurança aprimorada para a rede, os APs sem fio devem oferecer suporte as opções de segurança a seguir:

- **Filtragem de DHCP.** O AP sem fio deve filtrar portas IP para impedir a transmissão de mensagens de difusão DHCP nos casos em que o cliente sem fio é configurado como um servidor DHCP. O AP sem fio deve bloquear o cliente envie pacotes IP da porta UDP 68 para a rede.

- **Filtragem de DNS.** O AP sem fio deve filtrar portas IP para impedir que um cliente executando como um servidor DNS. O AP sem fio deve bloquear o cliente enviar pacotes IP de TCP ou UDP 53 à rede da porta.

- **Isolamento de clientes** se o seu ponto de acesso sem fio fornece funcionalidades de isolamento de cliente, você deve habilitar o recurso evitar possíveis protocolo ARP \(ARP\) falsificação explorações.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar as áreas de cobertura para usuários sem fio
Use os desenhos de arquiteturas de cada andar para cada edifício para identificar as áreas onde você deseja fornecer cobertura sem fio. Por exemplo, identificar os escritórios apropriados, de salas de conferências, lobbies, cafeterias ou courtyards.

Sobre desenhos, indicar todos os dispositivos que interferem com os sinais sem fio, como equipamentos médicos, câmeras de vídeo sem fio, telefones que operam na 2.4 a 2,5 GHz Industrial, científico e Medical \(ISM\) intervalo e Bluetooth\-dispositivos habilitados.

O desenho, marcar os aspectos da construção que podem interferir com sinais sem fio; objetos de metal usados na construção de um edifício podem afetar o sinal sem fio. Por exemplo, os seguintes objetos comuns podem interferir na propagação de sinal: Elevadores, aquecimento e ar\-condicionamento dutos e girders suporte concreto.

Consulte o fabricante do AP para obter informações sobre as fontes que podem causar a atenuação de radiofrequência AP sem fio. A maioria dos APs fornecem teste de software que você pode usar para verificar para intensidade do sinal, a taxa de erro e a taxa de transferência de dados.

### <a name="determine-where-to-install-wireless-aps"></a>Determinar onde instalar o PAS sem fio
Nos desenhos de arquiteturas, localize o APs sem fio feche suficiente juntos para fornecer distância ampla cobertura sem fio, mas é suficiente que eles não interfiram uns com os outros.

A distância necessária entre APs depende do tipo de ponto de acesso e antena de AP, aspectos do edifício que bloqueiam sem fio sinais e outras fontes de interferência. Você pode marcar os posicionamentos de AP sem fio para que cada AP sem fio não seja mais de 300 pés de qualquer adjacente AP sem fio. Consulte a documentação do fabricante de AP sem fio para as especificações de AP e diretrizes para posicionamento.

Instale temporariamente APs sem fio nos locais especificados em seus desenhos de arquiteturas. Em seguida, usando um laptop equipado com um adaptador sem fio 802.11 e o software de pesquisa de site que normalmente é fornecido com adaptadores sem fio, determine a intensidade do sinal dentro de cada área de cobertura.

Nas áreas de cobertura em que a intensidade do sinal é baixa, posicione o ponto de acesso para melhorar a intensidade do sinal para a área de cobertura, instale o PAS sem fio adicionais para fornecer a cobertura de necessário, realocar ou remover fontes de interferência de sinal.  

Atualize seus desenhos de arquiteturas para indicar o posicionamento de final de todos os APs sem fio. Ter um mapa de posicionamento de AP preciso auxiliará posteriormente, durante a solução de problemas de operações ou quando você deseja atualizar ou substituir APs.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planejar a configuração do ponto de acesso e o cliente do NPS RADIUS sem fio
Você pode usar o NPS para configurar APs sem fio, individualmente ou em grupos.

Se você estiver implantando uma grande rede sem fio que inclui muitos APs, é muito mais fácil de configurar os pontos de acesso em grupos. Para adicionar os pontos de acesso como grupos de clientes RADIUS no NPS, você deve configurar os pontos de acesso com essas propriedades.

- Os APs sem fio são configurados com endereços IP do mesmo intervalo de endereços IP.

- Os APs sem fio são configurados com o mesmo segredo compartilhado.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planejar o uso de reconexão rápida do PEAP
Em uma infraestrutura de 802.1 X, os pontos de acesso sem fio são configurados como clientes RADIUS para servidores RADIUS. Quando a reconexão rápida do PEAP é implantado, um cliente sem fio que passa entre duas ou mais pontos de acesso não é necessário para ser autenticado com cada nova associação.

Reconexão rápida do PEAP reduz o tempo de resposta para autenticação entre o cliente e o autenticador, porque a solicitação de autenticação é encaminhada do novo ponto de acesso para o NPS que originalmente executou a autenticação e autorização para o cliente solicitação de conexão.

Como o cliente PEAP e o NPS ambos usam anteriormente armazenados em cache Transport Layer Security \(TLS\) propriedades de conexão \(a coleção que é chamada de identificador TLS\), o NPS pode determinar que rapidamente o cliente está autorizado para uma reconexão.

>[!IMPORTANT]
>Para rápida reconexão para funcionar corretamente, os pontos de acesso devem ser configurados como clientes RADIUS do NPS mesmo.

Se o NPS original ficar indisponível ou se o cliente muda para um ponto de acesso que é configurado como um cliente RADIUS para outro servidor RADIUS, deve ocorrer a autenticação completa entre o cliente e o autenticador de novo.

### <a name="wireless-ap-configuration"></a>Configuração de ponto de acesso sem fio
A lista a seguir resume os itens que normalmente são configurados em 802.1 X\-APs sem fio compatíveis com:

> [!NOTE]
> Os nomes de item podem variar de acordo com a marca e modelo e podem ser diferentes na lista a seguir. Consulte a documentação do AP sem fio para a configuração\-detalhes específicos.

- **Identificador do conjunto \(SSID\)**. Esse é o nome da rede sem fio \(por exemplo, ExampleWlan\)e o nome que é anunciado para os clientes sem fio. Para reduzir a confusão, o que você escolher para anunciar SSID não deve corresponder o SSID que é transmitido por redes sem fio que estão dentro do limite de recepção da sua rede sem fio.

    Em casos em que vários APs sem fio são implantadas como parte da mesma rede sem fio, configure cada AP sem fio com o mesmo SSID. Em casos em que vários APs sem fio são implantadas como parte da mesma rede sem fio, configure cada AP sem fio com o mesmo SSID.  

    Em casos em que você tenha uma necessidade para implantar várias redes sem fio para atender às necessidades de negócios específicas, seu PA sem fio em uma rede deve transmitir um SSID diferente que o SSID outra rede\(s\). Por exemplo, se você precisar de uma rede sem fio separada para seus funcionários e convidados, você poderia configurar APs sem fio para a rede de negócios com o SSID definido como difusão **ExampleWLAN**. Para sua rede do convidado, depois você pode definir SSID do cada AP sem fio para difundir **GuestWLAN**. Dessa forma seus funcionários e convidados podem se conectar à rede pretendida sem confusão desnecessária.  

    > [!TIP]  
    > Alguns AP sem fio tem a capacidade de transmitir vários SSIDS para acomodar várias\-implantações de rede. Autoridades de AP sem fio que podem transmitir vários SSIDS podem reduzir custos de implantação e manutenção operacional.  

- **Wireless autenticação e criptografia**.

    Autenticação sem fio é a autenticação de segurança que é usada quando o cliente sem fio se associa a um ponto de acesso sem fio.  

    A criptografia sem fio é a codificação de criptografia de segurança que é usada com a autenticação sem fio para proteger as comunicações que são enviadas entre o AP sem fio e o cliente sem fio.  

- **Sem fio de endereço IP do PA \(estáticos\)**. Em cada AP sem fio, configure um endereço IP estático exclusivo. Se a sub-rede é atendida por um servidor DHCP, certifique-se de que todos os endereços IP do PA se enquadram em um intervalo de exclusão do DHCP para que o servidor DHCP não tentará emitir o mesmo endereço IP para outro computador ou dispositivo. Intervalos de exclusão estão documentados no procedimento "para criar e ativar um novo escopo DHCP" no [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Se você estiver planejando configurar pontos de acesso como clientes RADIUS por grupo no NPS, cada ponto de acesso no grupo deve ter um endereço IP do mesmo intervalo de endereços IP.

- **Nome DNS**. Alguns APs sem fio podem ser configurados com um nome DNS. Configure cada AP sem fio com um nome exclusivo. Por exemplo, se você tiver um APs sem fio implantados em uma de várias\-história de construção, você pode nomear os três primeiros APs sem fio que são implantados no terceiro andar AP3\-01, AP3\-02 e AP3\-03.

- **Máscara de sub-rede de AP sem fio**. Configure a máscara para designar qual parte do IP endereço é a ID de rede e qual parte do endereço IP é o host.

- **Serviço DHCP AP**. Se seu PA sem fio tem um built\-no serviço DHCP, desabilitá-lo.

- **Segredo compartilhado RADIUS**. Use um RAIO exclusivo segredo compartilhado para cada PA sem fio, a menos que você estiver planejando configurar clientes RADIUS de NPS em grupos – em que circunstância, você deve configurar todos os pontos de acesso no grupo com o mesmo segredo compartilhado. Os segredos compartilhados devem ser uma sequência aleatória de pelo menos 22 caracteres, com letras maiusculas e minúsculas, números e pontuação. Para garantir a aleatoriedade, você pode usar um programa de geração aleatória de caractere para criar seus segredos compartilhados. É recomendável que você registre o segredo compartilhado para cada AP sem fio e armazená-lo em um local seguro, como um seguro do office. Quando você configurar clientes RADIUS no console do NPS, você criará uma versão virtual de cada ponto de acesso. O segredo compartilhado que você configura em cada AP virtual no NPS deve coincidir com o segredo compartilhado no AP real, físico.

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do NPS que você deseja usar para autenticar e autorizar solicitações de conexão para este ponto de acesso.

- **A porta UDP\(s\)**. Por padrão, o NPS usa as portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e as portas UDP 1813 e 1646 para mensagens de contabilização RADIUS. É recomendável que você não altere as configurações de portas UDP RADIUS padrão.

- **VSAs**. Alguns APs sem fio exigem fornecedor\-atributos específicos \(VSAs\) para fornecer a funcionalidade de AP sem fio completo.

- **Filtragem de DHCP**. Configure APs sem fio para bloquear clientes sem fio de enviar pacotes IP da porta UDP 68 para a rede. Consulte a documentação para o seu PA sem fio configurar a filtragem de DHCP.

- **Filtragem DNS**. Configure APs sem fio para bloquear clientes sem fio de enviar pacotes IP da porta TCP ou UDP 53 para a rede. Consulte a documentação para o seu PA sem fio configurar a filtragem de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planejamento de acesso e a configuração de cliente sem fio

Ao planejar a implantação do 802.1 X\-acesso autenticado sem fio, você deve considerar vários cliente\-fatores específicos:

- **Planejando o suporte para vários padrões**.

    Determine se todos os seus computadores sem fio estão usando a mesma versão do Windows ou se eles são uma mistura de computadores que executam sistemas operacionais diferentes. Se eles forem diferentes, certifique-se de que você compreenda as diferenças nos padrões suportados pelos sistemas operacionais.

    Determine se todos os adaptadores de rede sem fio em todos os computadores cliente sem fio dão suporte os mesmos padrões sem fio, ou se é necessário dar suporte a diversos padrões. Por exemplo, determinar se a alguns drivers de hardware do adaptador de rede oferece suporte a WPA2\-Enterprise e AES, enquanto outros dão suporte somente WPA\-Enterprise e TKIP.

- **Modo de autenticação de cliente planejamento**. Modos de autenticação definem como os clientes do Windows processam as credenciais de domínio. Você pode selecionar os seguintes modos de autenticação de rede de três nas diretivas de rede sem fio.  

    1. **Usuário re\-autenticação**. Esse modo Especifica que a autenticação é sempre executada usando as credenciais de segurança com base no estado atual do computador. Quando nenhum usuário estiver conectado ao computador, a autenticação é executada usando as credenciais do computador. Quando um usuário fizer logon no computador, a autenticação é sempre feita usando as credenciais do usuário.  

    2. **Computador somente**. Computador somente modo Especifica que a autenticação é sempre executada usando apenas as credenciais do computador.  

    3.  **Autenticação do usuário**. Modo de autenticação de usuário Especifica que a autenticação é realizada somente quando o usuário fizer logon no computador. Quando não há nenhum usuário conectado ao computador, as tentativas de autenticação não são executadas.  

- **Planejamento de restrições sem fio**. Determine se você deseja fornecer todos os seus usuários sem fio com o mesmo nível de acesso à rede sem fio ou se você deseja restringir o acesso para alguns dos seus usuários sem fio. Você pode aplicar restrições no NPS em relação a grupos específicos de usuários sem fio.  Por exemplo, você pode definir específico dias e horas que determinados grupos podem acessar a rede sem fio.  

- **Planejamento de métodos para adicionar novos computadores sem fio**. Para rede sem fio\-compatível com computadores que fazem parte de seu domínio antes de implantar sua rede sem fio, se o computador estiver conectado a um segmento de rede com fio que não seja protegido pelo 802.1 X, as definições de configuração sem fio são aplicado automaticamente depois de configurar a rede sem fio \(IEEE 802.11\) políticas no controlador de domínio e depois que a política de grupo é atualizada no cliente sem fio.  

    Para computadores que já não estão associados ao seu domínio, no entanto, você deve planejar um método para aplicar as configurações que são necessárias para 802.1 X\-acesso autenticado. Por exemplo, determine se você deseja ingressar o computador ao domínio usando um dos métodos a seguir.

    1.  Conectar o computador a um segmento de rede com fio que não seja protegido pelo 802.1 X e, em seguida, ingressar o computador no domínio.

    2.  Fornece aos usuários sem fio com as etapas e as configurações necessárias para adicionar seu próprio perfil sem fio de bootstrap, que lhes permite ingressar o computador no domínio.

    3.  Atribua a equipe de TI para clientes sem fio ingressar no domínio.

### <a name="planning-support-for-multiple-standards"></a>Planejando o suporte para vários padrões

A rede sem fio \(IEEE 802.11\) extensão de políticas na diretiva de grupo fornece uma ampla variedade de opções de configuração para dar suporte a uma variedade de opções de implantação.

Você pode implantar APs sem fio que são configurados com os padrões que você deseja dar suporte e, em seguida, configure vários perfis sem fio na rede sem fio \(IEEE 802.11\) políticas, com cada perfil especificando um conjunto de padrões Se você precisar.

Por exemplo, se sua rede tiver computadores sem fio que dão suporte a WPA2\-Enterprise e AES, outros computadores que dão suporte ao WPA\-Enterprise e AES e outros computadores que dão suporte ao WPA apenas\-Enterprise e TKIP, você deve: Determine se deseja:

- Configurar um único perfil para dar suporte a todos os computadores sem fio usando o método de criptografia mais fraco que todos os seus computadores com suportem - nesse caso, WPA\-Enterprise e TKIP.  
- Configure dois perfis para fornecer a melhor segurança possível com suporte de cada computador sem fio. Nesse caso, você poderia configurar um perfil que especifica a criptografia mais forte \(WPA2\-Enterprise e AES\)e um perfil que usa o WPA mais fraco\-Enterprise e TKIP criptografia. Neste exemplo, é essencial que você coloque o perfil que usa o WPA2\-Enterprise e AES mais alto na ordem de preferência. Computadores que não são capazes de usar WPA2\-Enterprise e AES será automaticamente pular para o próximo perfil na ordem de preferência e processar o perfil que especifica o WPA\-Enterprise e TKIP.

> [!IMPORTANT]
> Você deve colocar o perfil com os padrões mais seguros a mais alta na lista ordenada de perfis, como computadores que se conectam usam o primeiro perfil que são capazes de usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planejando o acesso restrito à rede sem fio

Em muitos casos, você talvez queira fornecer aos usuários sem fio níveis variados de acesso à rede sem fio. Por exemplo, você talvez queira permitir algum acesso irrestrito de usuários, todas as horas do dia, todos os dias da semana. Para outros usuários, você talvez só queira permitir o acesso durante o horário, de segunda a sexta-feira e negar o acesso no sábado e domingo.

Este guia fornece instruções para criar um ambiente de acesso que coloca todos os seus usuários sem fio em um grupo de acesso comum aos recursos sem fio. Criar um grupo de segurança de usuários sem fio em que os usuários do Active Directory e computadores encaixar\-no e, em seguida, verifique todos os usuários para quem deseja conceder acesso sem fio um membro desse grupo.

Quando você configura políticas de rede do NPS, você especifica o grupo de segurança de usuários sem fio como o objeto que o NPS processa ao determinar a autorização.

No entanto, se sua implantação exigir suporte para vários níveis de acesso precisa apenas faça o seguinte:  

1. Crie mais de um grupo de segurança de usuários sem fio para criar grupos de segurança sem fio adicionais em computadores e usuários do Active Directory. Por exemplo, você pode criar um grupo que contém os usuários que têm acesso completo, um grupo para aqueles que só têm acesso durante o horário de trabalho regulares e outros grupos que se ajustem a outros critérios que correspondam aos seus requisitos.

2. Adicione usuários a grupos de segurança apropriados que você criou.

3. Configurar políticas de rede do NPS adicionais para cada grupo de segurança sem fio adicionais e configurar as políticas para aplicar as condições e restrições que você requer para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Planejamento de métodos para adicionar novos computadores sem fio

É o método preferencial para ingressar em novos computadores sem fio no domínio e, em seguida, faça logon no domínio usando uma conexão com fio em um segmento de LAN que tenha acesso aos controladores de domínio e não é protegido por uma autenticação 802.1 X comutador Ethernet.

Em alguns casos, no entanto, ele pode não ser prático usar uma conexão com fio para ingressar computadores ao domínio, ou, para um usuário para usar uma conexão com fio para seu primeiro logon em tentativa usando computadores que já ingressaram no domínio.

Para ingressar um computador ao domínio usando uma conexão sem fio ou para os usuários fazer logon no domínio na primeira vez, usando um domínio\-computador ingressado no e uma conexão sem fio, os clientes sem fio devem primeiro estabelecer uma conexão com a rede sem fio em um segmento que tem acesso aos controladores de domínio de rede usando um dos métodos a seguir.

1. **Um membro da equipe de TI ingressa um computador sem fio no domínio e, em seguida, configura um Single Sign On bootstrap perfil sem fio.** Com esse método, um administrador de TI conecta o computador sem fio à rede Ethernet com fio e, em seguida, adiciona o computador ao domínio. Em seguida, o administrador distribui o computador para o usuário. Quando o usuário inicia o computador, as credenciais de domínio especificadas manualmente para o processo de logon do usuário são usadas para estabelecer uma conexão com a rede sem fio e fazer logon domínio.  

2. **O usuário configura manualmente o computador sem fio com o perfil sem fio de bootstrap e, em seguida, ingressa no domínio.** Com esse método, os usuários manualmente configurar seus computadores sem fio com um perfil sem fio de bootstrap com base nas instruções do administrador de TI. O perfil sem fio de bootstrap permite aos usuários estabelecer uma conexão sem fio e, em seguida, ingressar o computador no domínio. Depois de ingressar o computador no domínio e reiniciar o computador, o usuário pode faça logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para implantar o acesso sem fio, consulte [implantação de acesso sem fio](e-wireless-access-deployment.md).
