---
title: Acesso sem fio planejamento de implantação
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseada em senha 802.1 X autenticado sem fio acesso"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1aa7d9fa66c480988ec7e3a97447157bd3eab9c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-planning"></a>Acesso sem fio planejamento de implantação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Antes de implantar acesso sem fio, você deve planejar os seguintes itens:

- Instalação de acesso sem fio pontos \(APs\) em sua rede

- Acesso e a configuração de cliente sem fio

As seções a seguir fornecem detalhes sobre essas etapas de planejamento.

## <a name="planning-wireless-ap-installations"></a>Planejando instalações de ponto de acesso sem fio
Ao projetar sua solução de acesso de rede sem fio, você deve fazer o seguinte:

1. Determinar quais padrões devem ter suporte para os pontos de acesso sem fio
2. Determinar as áreas de cobertura onde você deseja fornecer serviços sem fio
3. Determinar onde você deseja localizar pontos de acesso sem fio

Além disso, você deve planejar uma esquema de endereço IP para o ponto de acesso sem fio e sem fio clientes. Consulte a seção **planejar a configuração das PA sem fio no NPS** informações para relacionadas abaixo.

### <a name="verify-wireless-ap-support-for-standards"></a>Verificar o suporte de ponto de acesso sem fio para padrões
Para fins de consistência e a facilidade de implantação e gerenciamento de ponto de acesso, é recomendável que você implantar pontos de acesso sem fio a mesma marca e modelo.

Os pontos de acesso sem fio que você implantar devem permitir o seguinte:

- **IEEE 802.1 X**

- **Autenticação RADIUS**

- **Autenticação sem fio e criptografia.** Listadas em ordem da maior parte para menos preferencial:

    1.  WPA2\-Enterprise com AES

    2.  WPA2\-Enterprise com TKIP

    3.  WPA\-Enterprise com AES

    4.  WPA\-Enterprise com TKIP

>[!NOTE]
>Para implantar WPA2, você deve usar adaptadores de rede sem fio e pontos de acesso sem fio que também dão suporte a WPA2. Caso contrário, use WPA\ empresarial.

Além disso, para fornecer segurança aprimorada para a rede, os pontos de acesso sem fio devem suportar as seguintes opções de segurança:

- **Filtragem de DHCP.** O ponto de acesso sem fio deve filtrar as portas IP para evitar a transmissão de mensagens de difusão DHCP nesses casos em que o cliente sem fio está configurado como um servidor DHCP. O ponto de acesso sem fio deve impedir que o cliente envie pacotes IP da porta UDP 68 à rede.

- **Filtragem de DNS.** O ponto de acesso sem fio deve filtrar portas IP para impedir que um cliente executando como um servidor DNS. O ponto de acesso sem fio deve impedir que o cliente de enviar pacotes IP de TCP ou UDP porta 53 à rede.

- **Isolamento de cliente** se o seu ponto de acesso sem fio fornece recursos de isolamento de cliente, você deve habilitar o recurso evitar possíveis \(ARP\) Address Resolution Protocol falsificação explorações.

### <a name="identify-areas-of-coverage-for-wireless-users"></a>Identificar áreas de cobertura para usuários sem fio
Use desenhos arquitetônicos de cada andar para cada compilação para identificar as áreas em que você deseja fornecer cobertura sem fio. Por exemplo, identificar o escritórios apropriados, salas de conferências, lobbies, cafeterias ou courtyards.

Em desenhos, indique quaisquer dispositivos que interfiram sinais de sem fio, como equipamentos médicos, câmeras de vídeo sem fio, telefones que operam no 2.4 por meio de 2,5 GHz industriais, científica e médicos \(ISM\) a faixa e dispositivos habilitados para Bluetooth\.

O desenho, marcar aspectos do edifício que possam interferir sinais sem fio; objetos de metal usados na construção de um prédio podem afetar o sinal sem fio. Por exemplo, os seguintes objetos comuns podem interferir no propagação de sinal: elevadores, dutos aquecimento e condicionamento de air\ e girders suporte concreto.

Consulte o fabricante do seu ponto de acesso para obter informações sobre fontes que possa causar atenuação de radiofrequência de ponto de acesso sem fio. A maioria dos PAS oferecem software teste que você pode usar para verificar a intensidade do sinal, a taxa de erro e taxa de transferência de dados.

### <a name="determine-where-to-install-wireless-aps"></a>Determinar onde instalar pontos de acesso sem fio
Em desenhos arquitetônicos, localize os pontos de acesso sem fio fechar suficiente juntos para fornecer um amplo cobertura sem fio mas suficiente diferença que eles não interfiram uns aos outros.

A distância entre pontos de acesso necessária depende do tipo de ponto de acesso e antena PA, aspectos do edifício que bloqueiam sem fio sinais e outras fontes de interferência. Você pode marcar posicionamentos de ponto de acesso sem fio para que cada ponto de acesso sem fio não é mais de 300 pés de qualquer ponto de acesso sem fio adjacente. Consulte a documentação do fabricante do ponto de acesso sem fio para especificações de ponto de acesso e diretrizes de posicionamento.

Instale temporariamente os pontos de acesso sem fio nos locais especificados em seus desenhos arquitetônicos. Em seguida, usando um laptop equipado com um adaptador sem fio 802.11 e o software de pesquisa do site que normalmente é fornecido com adaptadores sem fio, determine a intensidade do sinal dentro de cada área de cobertura.

Em áreas de cobertura onde a intensidade do sinal for baixa, posicione o ponto de acesso para melhorar a intensidade do sinal para a área de cobertura, instale outros pontos de acesso sem fio para fornecer a cobertura necessária, reposicione ou remover fontes de interferência de sinal.  

Atualize seus desenhos arquitetônicos para indicar a posição final de todos os pontos de acesso sem fio. Ter um mapa de posicionamento PA preciso ajudará posteriormente durante operações de solução de problemas ou quando você deseja atualizar ou substituir PAS.

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>Planejar a configuração de ponto de acesso e cliente RADIUS de NPS sem fio
Você pode usar o NPS para configurar pontos de acesso sem fio individualmente ou em grupos.

Se você estiver implantando uma rede sem fio grande que inclui muitos pontos de acesso, é muito mais fácil de configurar pontos de acesso em grupos. Para adicionar os pontos de acesso como grupos de clientes RADIUS de NPS, você deve configurar os pontos de acesso com essas propriedades.

- Os pontos de acesso sem fio são configurados com endereços IP do mesmo intervalo de endereços IP.

- Os pontos de acesso sem fio são configurados com o mesmo segredo compartilhado.

### <a name="plan-the-use-of-peap-fast-reconnect"></a>Planejar o uso de PEAP reconexão rápida
Em uma infraestrutura de 802.1 X, pontos de acesso sem fio são configurados como clientes RADIUS para servidores RADIUS. Quando reconexão rápida do PEAP é implantada, um cliente sem fio que ocorre entre dois ou mais pontos de acesso não é necessário para ser autenticado com cada nova associação.

Reconexão rápida do PEAP reduz o tempo de resposta de autenticação entre o cliente e o autenticador porque a solicitação de autenticação é encaminhada do novo ponto de acesso ao servidor NPS que originalmente executou a autenticação e autorização da solicitação de conexão do cliente.

Como o cliente PEAP e o servidor NPS ambos usam previamente armazenadas em cache propriedades de conexão Transport Layer Security \(TLS\) \ (a coleção de que é chamada a handle\ TLS), o servidor NPS pode determinar rapidamente se o cliente está autorizado para uma reconexão.

>[!IMPORTANT]
>De modo rápido reconectá-lo para funcionar corretamente, os pontos de acesso devem ser configurados como clientes RADIUS no mesmo servidor NPS.

Se o servidor NPS original não estiver disponível ou se o cliente se mover para um ponto de acesso que esteja configurado como um cliente RADIUS para um servidor RADIUS diferente, deve ocorrer a autenticação completa entre o cliente e o autenticador de novo.

### <a name="wireless-ap-configuration"></a>Configuração de ponto de acesso sem fio
A lista a seguir resume itens comumente configurados em 802.1X\-compatível com pontos de acesso sem fio:

> [!NOTE]
> Os nomes de item podem variar de acordo com a marca e modelo e podem ser diferentes na lista a seguir. Consulte a documentação do ponto de acesso sem fio para obter detalhes de configuração específicos.

- **Identificador do conjunto de serviço \(SSID\)**. Este é o nome da rede sem fio \ (por exemplo, ExampleWlan\) e o nome que está anunciado para clientes sem fio. Para reduzir a confusão, SSID que você escolher para anunciar não deve corresponder o SSID transmitido por redes sem fio que estão dentro do alcance de recepção de sua rede sem fio.

    Em casos em que vários pontos de acesso sem fio são implantados como parte da mesma rede sem fio, configure cada ponto de acesso sem fio com o mesmo SSID. Em casos em que vários pontos de acesso sem fio são implantados como parte da mesma rede sem fio, configure cada ponto de acesso sem fio com o mesmo SSID.  

    Em casos em que você precisa implantar diferentes redes sem fio para atender às necessidades específicas de negócios, seu PA sem fio em uma rede deve transmitir um SSID diferente do que o SSID seus outros network\(s\). Por exemplo, se você precisar de uma rede sem fio separada dos seus funcionários e convidados, você pode configurar os pontos de acesso sem fio para rede da empresa com o SSID definido para transmitir **ExampleWLAN**. Sua rede de convidado, em seguida, você pode definir SSID do PA cada sem fio para transmitir **GuestWLAN**. Dessa forma, seus funcionários e convidados podem conectar à rede pretendida sem confusão desnecessário.  

    > [!TIP]  
    > Algum ponto de acesso sem fio têm a capacidade de transmissão vários SSIDS para acomodar as implantações de rede multi\. PA sem fio que pode transmitir vários SSID pode reduzir os custos operacionais manutenção e implantação.  

- **Autenticação e criptografia sem fio**.

    Autenticação sem fio é a autenticação de segurança que é usada quando o cliente sem fio associa um ponto de acesso sem fio.  

    Criptografia sem fio é a criptografia de criptografia de segurança que é usada com autenticação sem fio para proteger as comunicações que são enviadas entre o ponto de acesso sem fio e o cliente sem fio.  

- **Sem fio PA IP endereço \(static\)**. Em cada ponto de acesso sem fio, configure um endereço IP estático exclusivo. Se a sub-rede é atendida por um servidor DHCP, certifique-se de que todos os endereços IP PA se enquadram em um intervalo de exclusão de DHCP para que o servidor DHCP não tenta emitir o mesmo endereço IP para outro computador ou dispositivo. Intervalos de exclusão estão documentados no procedimento "para criar e ativar um novo escopo DHCP" no [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide). Se você pretende configurar pontos de acesso como clientes RADIUS por grupo no NPS, cada ponto de acesso no grupo deve ter um endereço IP do mesmo intervalo de endereços IP.

- **Nome DNS**. Alguns pontos de acesso sem fio podem ser configurados com um nome DNS. Configure cada ponto de acesso sem fio com um nome exclusivo. Por exemplo, se você tiver um pontos de acesso sem fio implantados em um prédio multi\ história, você pode nomear os pontos de acesso sem fio três primeiros que são implantados na terceiro piso AP3\-01, 02 AP3\ e AP3\ 03.

- **Máscara de sub-rede de ponto de acesso sem fio**. Configure a máscara para designar qual parte de IP é a identificação de rede e qual parte do endereço IP do host.

- **Serviço DHCP PA**. Se o seu ponto de acesso sem fio tiver um serviço DHCP built\, desabilitá-la.

- **Segredo compartilhado RADIUS**. Use um RAIO exclusivo segredo compartilhado para cada ponto de acesso sem fio, a menos que você pretende configurar clientes RADIUS de NPS em grupos - quais circunstância em, você deve configurar todos os pontos de acesso no grupo com o mesmo segredo compartilhado. Segredos compartilhados devem ser uma sequência aleatória de pelo menos 22 caracteres, com letras maiusculas e minúsculas, números e pontuação. Para garantir aleatoriedade, você pode usar um programa de geração de caractere aleatório para criar seu segredos compartilhados. É recomendável que você registrou o segredo compartilhado para cada ponto de acesso sem fio e armazená-la em um local seguro, como um escritório seguro. Quando você configurar clientes RADIUS no console do NPS criará uma versão virtual de cada ponto de acesso. O segredo compartilhado que você configurar em cada ponto de acesso virtual no NPS deve coincidir com o segredo compartilhado no ponto de acesso real, físico.

- **Endereço IP do servidor RADIUS**. Digite o endereço IP do servidor NPS que você deseja usar para autenticar e autorizar solicitações de conexão para esse ponto de acesso.

- **UDP port\(s\)**. Por padrão, o NPS usa as portas UDP 1812 e 1645 para mensagens de autenticação RADIUS e portas UDP 1813 e 1646 para mensagens de estatísticas RADIUS. É recomendável que você não alterar as configurações de portas UDP RADIUS padrão.

- **VSAs**. Alguns pontos de acesso sem fio exigem \(VSAs\) atributos vendor\ específicos para fornecer a funcionalidade de ponto de acesso sem fio completo.

- **Filtragem de DHCP**. Configure pontos de acesso sem fio para bloquear os clientes de enviar pacotes IP de porta UDP 68 à rede sem fio. Consulte a documentação do seu ponto de acesso sem fio configurar a filtragem de DHCP.

- **Filtragem de DNS**. Configure pontos de acesso sem fio para bloquear os clientes de enviar pacotes IP de porta TCP ou UDP 53 à rede sem fio. Consulte a documentação do seu ponto de acesso sem fio configurar a filtragem de DNS.

## <a name="planning-wireless-client-configuration-and-access"></a>Planejamento de acesso e a configuração de cliente sem fio

Ao planejar a implantação de 802.1X\-autenticados acesso sem fio, você deve considerar vários fatores de client\ específicas:

- **Suporte para vários padrões de planejamento**.

    Determine se todos os seus computadores sem fio estão usando a mesma versão do Windows ou se eles são uma mistura de computadores que executam sistemas operacionais diferentes. Se eles forem diferentes, certifique-se de que você compreende as diferenças nos padrões compatíveis com os sistemas operacionais.

    Determine se todos os adaptadores de rede sem fio em todos os computadores cliente sem fio oferecem suporte aos mesmos padrões sem fio, ou se é necessário dar suporte a diversos padrões. Por exemplo, determine se alguns drivers de hardware do adaptador de rede suportam WPA2\-Enterprise e AES, enquanto outros dão suporte apenas WPA\-Enterprise e TKIP.

- **Modo de autenticação de cliente planejamento**. Modos de autenticação definem como aos clientes do Windows processam as credenciais de domínio. Você pode selecionar os seguintes modos de autenticação de três rede na seção políticas de rede sem fio.  

    1. **Autenticação do usuário re\**. Esse modo Especifica que a autenticação sempre é realizada usando credenciais de segurança com base no estado atual do computador. Quando nenhum usuário estiver conectado ao computador, a autenticação é realizada usando as credenciais do computador. Quando um usuário fizer logon no computador, autenticação sempre é executada usando as credenciais do usuário.  

    2. **Somente computador**. Computador somente modo Especifica que a autenticação sempre é realizado usando somente as credenciais do computador.  

    3.  **Autenticação de usuário**. Modo de autenticação de usuário Especifica que a autenticação é executada somente quando o usuário fizer logon no computador. Quando não há nenhum usuário feito logon no computador, tentativas de autenticação não são executadas.  

- **Planejando restrições sem fio**. Determine se você deseja fornecer todos os seus usuários sem fio com o mesmo nível de acesso à sua rede sem fio, ou se você deseja restringir o acesso para alguns dos seus usuários sem fio. Você pode aplicar as restrições em NPS contra grupos específicos de usuários sem fio.  Por exemplo, você pode definir específico dias e horas que determinados grupos têm acesso à rede sem fio.  

- **Planejando os métodos para adicionar novos computadores sem fio**. Para computadores habilitados para wireless\ que fazem parte de seu domínio antes de implantar sua rede sem fio, se o computador estiver conectado a um segmento de rede com fio não estiver protegido pelo 802.1 X, as configurações sem fio são aplicadas automaticamente depois de configurar a rede sem fio \ (IEEE 802.11\) políticas no controlador de domínio e após a atualização de política de grupo no cliente sem fio.  

    Para computadores que já não fazem parte de seu domínio, no entanto, você deve planejar um método para aplicar as configurações que são necessárias para 802.1X\-acesso autenticado. Por exemplo, determine se você deseja ingressar o computador no domínio usando um dos métodos a seguir.

    1.  Conecte o computador para um segmento de rede com fio não estiver protegido pelo 802.1 X e, em seguida, ingressar o computador ao domínio.

    2.  Forneça aos usuários sem fio com as etapas e configurações que necessitam para adicionar seu próprio perfil de inicialização sem fio, que permite que eles ingressar o computador ao domínio.

    3.  Atribua a equipe de TI para participar de clientes sem fio ao domínio.

### <a name="planning-support-for-multiple-standards"></a>Suporte para vários padrões de planejamento

A rede com fio \ (IEEE 802.11\) extensão de políticas na política de grupo oferece uma ampla variedade de opções de configuração para dar suporte a uma variedade de opções de implantação.

Você pode implantar os pontos de acesso sem fio que estejam configurados com os padrões que você deseja dar suporte e, em seguida, configurar vários perfis sem fio na rede sem fio \ (IEEE 802.11\) políticas, com cada perfil especificando um conjunto de padrões que você precisa.

Por exemplo, se a rede tiver computadores sem fio que oferecem suporte WPA2\-Enterprise e AES, outros computadores que suportam WPA\-Enterprise e AES e outros computadores que suportam somente WPA\-Enterprise e TKIP, você deve determinar se deseja:

- Configurar um único perfil para dar suporte a todos os computadores sem fio usando o método de criptografia mais fraco que todos os computadores dão suporte - neste caso, WPA\-Enterprise e TKIP.  
- Configure dois perfis para fornecer a melhor segurança possível que é compatível com cada computador sem fio. Neste exemplo, você poderia configurar um perfil que especifica a criptografia mais forte \ (Enterprise WPA2\ e AES\) e um perfil que usa a criptografia mais fraca WPA\-Enterprise e TKIP. Neste exemplo, é essencial que você coloque o perfil que usa WPA2\-Enterprise e AES mais alto na ordem de preferência. Computadores que não são capazes de usar WPA2\-Enterprise e AES irá pular para o próximo perfil na ordem de preferência e processar o perfil que especifica WPA\-Enterprise e TKIP automaticamente.

> [!IMPORTANT]
> Você deve colocar o perfil com os padrões mais seguros superiores na lista ordenada de perfis, como computadores que se conectam usam o primeiro perfil que eles são capazes de usar.

### <a name="planning-restricted-access-to-the-wireless-network"></a>Planejando acesso restrito à rede sem fio

Em muitos casos, convém fornecer os usuários sem fio com diferentes níveis de acesso à rede sem fio. Por exemplo, convém permitir acesso irrestrito alguns usuários, a qualquer hora do dia, todos os dias da semana. Para outros usuários, somente convém permitir acesso durante o horário core, segunda a sexta-feira e negar acesso no sábado e domingo.

Este guia fornece instruções para criar um ambiente de acesso que coloca todos os seus usuários sem fio em um grupo comuns acesso aos recursos sem fio. Você cria um grupo de segurança de usuários sem fio no e computadores do Active Directory Users snap\-in e, em seguida, verifique todos os usuários para o qual você deseja conceder acesso sem fio um membro desse grupo.

Quando você configurar as políticas de rede NPS, especifique o grupo de segurança de usuários sem fio como o objeto NPS processa ao determinar a autorização.

No entanto, se sua implantação requer suporte para diferentes níveis de acesso necessário somente faça o seguinte:  

1. Crie mais de um grupo de segurança de usuários sem fio para criar grupos de segurança adicionais sem fio no Active Directory Users and Computers. Por exemplo, você pode criar um grupo que contém os usuários que têm acesso completo, um grupo para aqueles que só têm acesso durante o horário normal e outros grupos que se enquadram outros critérios que corresponda às suas necessidades.

2. Adicione usuários aos grupos de segurança adequadas que você criou.

3. Configurar as políticas de rede NPS adicionais para cada grupo de segurança sem fio adicionais e configurar as políticas para aplicar as condições e restrições que exigem para cada grupo.

### <a name="planning-methods-for-adding-new-wireless-computers"></a>Planejando os métodos para adicionar novos computadores sem fio

É o método preferencial para participar de novos computadores sem fio para o domínio e, em seguida, fazer logon no domínio usando uma conexão com fio para um segmento da LAN que tem acesso aos controladores de domínio e não está protegida por uma autenticação 802.1 X comutador Ethernet.

Em alguns casos, no entanto, não seria prático usar uma conexão com fio para ingressar computadores ao domínio, ou, para que um usuário usar uma conexão com fio para sua primeira tentativa de logon usando computadores que já ingressou no domínio.

Para ingressar um computador no domínio usando uma conexão sem fio ou para os usuários façam logon no domínio na primeira vez usando um computador associado a domain\ e uma conexão sem fio, os clientes sem fio devem primeiro estabelecer uma conexão à rede sem fio em um segmento que tenha acesso aos controladores de domínio de rede usando um dos métodos a seguir.

1. **Um membro da equipe de TI ingressa em um computador sem fio no domínio e configura um logon único bootstrap perfil sem fio.** Com esse método, um administrador de TI conecta o computador sem fio à rede Ethernet com fio e, em seguida, ingressa o computador ao domínio. Em seguida, o administrador distribui o computador para o usuário. Quando o usuário inicia o computador, as credenciais de domínio que especificar manualmente para o processo de logon do usuário são usadas para estabelecer uma conexão à rede sem fio e fazer logon no domínio.  

2. **O usuário configura manualmente o computador sem fio com bootstrap perfil sem fio e, em seguida, ingressa no domínio.** Com esse método, os usuários configurar manualmente seus computadores sem fio com um perfil sem fio bootstrap com base nas instruções de um administrador de TI. O perfil sem fio bootstrap permite que os usuários estabelecer uma conexão sem fio e, em seguida, ingressar o computador ao domínio. Depois de ingressar o computador ao domínio e reiniciar o computador, o usuário pode fazer logon no domínio usando uma conexão sem fio e suas credenciais de conta de domínio.

Para implantar o acesso sem fio, consulte [implantação de acesso sem fio](e-wireless-access-deployment.md).
