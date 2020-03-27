---
title: Políticas de solicitação de conexão
description: Este tópico fornece uma visão geral das políticas de solicitação de conexão do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 021cef4a220b183f6580bca75bc6db68aba893cf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316232"
---
# <a name="connection-request-policies"></a>Políticas de solicitação de conexão

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar as políticas de solicitação de conexão do NPS para configurar o NPS como um servidor RADIUS, um proxy RADIUS ou ambos.

>[!NOTE]
>Além deste tópico, a documentação da política de solicitação de conexão a seguir está disponível.
> - [Configurar políticas de solicitação de conexão](nps-crp-configure.md)
> - [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)

As políticas de solicitação de conexão são conjuntos de condições e configurações que permitem que os administradores de rede designem quais servidores de serviço RADIUS (RADIUS) executam a autenticação e a autorização de solicitações de conexão que o o servidor que executa o servidor de diretivas de rede (NPS) recebe de clientes RADIUS. As políticas de solicitação de conexão podem ser configuradas para designar quais servidores RADIUS são usados para contabilização RADIUS.

Você pode criar políticas de solicitação de conexão para que algumas mensagens de solicitação RADIUS enviadas de clientes RADIUS sejam processadas localmente (o NPS é usado como um servidor RADIUS) e outros tipos de mensagens são encaminhados para outro servidor RADIUS (o NPS é usado como um proxy RADIUS).

Com as políticas de solicitação de conexão, você pode usar o NPS como um servidor RADIUS ou um proxy RADIUS, com base em fatores como o seguinte: 

- A hora do dia e o dia da semana
- O nome do Realm na solicitação de conexão
- O tipo de conexão que está sendo solicitada
- O endereço IP do cliente RADIUS

As mensagens de solicitação de acesso RADIUS serão processadas ou encaminhadas pelo NPS somente se as configurações da mensagem de entrada corresponderem a pelo menos uma das políticas de solicitação de conexão configuradas no NPS. 

Se as configurações de política forem correspondentes e a política exigir que o NPS processe a mensagem, o NPS atuará como um servidor RADIUS, Autenticando e autorizando a solicitação de conexão. Se as configurações de política forem correspondentes e a política exigir que o NPS encaminhe a mensagem, o NPS atuará como um proxy RADIUS e encaminhará a solicitação de conexão a um servidor RADIUS remoto para processamento.

Se as configurações de uma mensagem de solicitação de acesso RADIUS de entrada não corresponderem a pelo menos uma das políticas de solicitação de conexão, uma mensagem de rejeição de acesso será enviada ao cliente RADIUS e o usuário ou computador que está tentando se conectar à rede terá o acesso negado.

## <a name="configuration-examples"></a>Exemplos de configuração

Os exemplos de configuração a seguir demonstram como você pode usar as políticas de solicitação de conexão.

### <a name="nps-as-a-radius-server"></a>NPS como um servidor RADIUS

A política de solicitação de conexão padrão é a única política configurada. Neste exemplo, o NPS é configurado como um servidor RADIUS e todas as solicitações de conexão são processadas pelo NPS local. O NPS pode autenticar e autorizar usuários cujas contas estejam no domínio do domínio NPS e em domínios confiáveis.


### <a name="nps-as-a-radius-proxy"></a>NPS como um proxy RADIUS

A política de solicitação de conexão padrão é excluída e duas novas políticas de solicitação de conexão são criadas para encaminhar solicitações para dois domínios diferentes. Neste exemplo, o NPS está configurado como um proxy RADIUS. O NPS não processa nenhuma solicitação de conexão no servidor local. Em vez disso, ele encaminha as solicitações de conexão para o NPS ou outros servidores RADIUS configurados como membros de grupos de servidores RADIUS remotos.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS como servidor RADIUS e proxy RADIUS

Além da política de solicitação de conexão padrão, é criada uma nova política de solicitação de conexão que encaminha as solicitações de conexão para um NPS ou outro servidor RADIUS em um domínio não confiável. Neste exemplo, a política de proxy aparece primeiro na lista ordenada de políticas. Se a solicitação de conexão corresponder à política de proxy, a solicitação de conexão será encaminhada para o servidor RADIUS no grupo de servidores remotos RADIUS. Se a solicitação de conexão não corresponder à política de proxy, mas corresponder à política de solicitação de conexão padrão, o NPS processará a solicitação de conexão no servidor local. Se a solicitação de conexão não corresponder a nenhuma política, ela será descartada.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS como servidor RADIUS com servidores de contabilidade remoto

Neste exemplo, o NPS local não está configurado para executar a contabilidade e a política de solicitação de conexão padrão é revisada para que as mensagens de contabilização RADIUS sejam encaminhadas para um NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS. Embora as mensagens de contabilização sejam encaminhadas, as mensagens de autenticação e autorização não são encaminhadas, e o NPS local executa essas funções para o domínio local e todos os domínios confiáveis.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS com RADIUS remoto para mapeamento de usuário do Windows

Neste exemplo, o NPS atua como um servidor RADIUS e como um proxy RADIUS para cada solicitação de conexão individual encaminhando a solicitação de autenticação para um servidor RADIUS remoto ao usar uma conta de usuário do Windows local para autorização. Essa configuração é implementada com a configuração do atributo Remote RADIUS para o mapeamento de usuário do Windows como uma condição da diretiva de solicitação de conexão. (Além disso, uma conta de usuário deve ser criada localmente que tenha o mesmo nome que a conta de usuário remoto na qual a autenticação é executada pelo servidor RADIUS remoto.)

## <a name="connection-request-policy-conditions"></a>Condições da política de solicitação de conexão

As condições de política de solicitação de conexão são um ou mais atributos RADIUS que são comparados aos atributos da mensagem de solicitação de acesso RADIUS de entrada. Se houver várias condições, todas as condições na mensagem de solicitação de conexão e na política de solicitação de conexão deverão corresponder para que a política seja imposta pelo NPS.


A seguir estão os atributos de condição disponíveis que você pode configurar em políticas de solicitação de conexão.

### <a name="connection-properties-attribute-group"></a>Grupo de atributos de propriedades de conexão

O grupo de atributos propriedades de conexão contém os atributos a seguir.

- **Protocolo Framed**. Usado para designar o tipo de enquadramento de pacotes de entrada. Os exemplos são protocolo PPP (PPP), protocolo de Internet de linha serial (SLIP), retransmissão de quadros e X. 25.
- **Tipo de serviço**. Usado para designar o tipo de serviço que está sendo solicitado. Os exemplos incluem Framed (por exemplo, conexões PPP) e logon (por exemplo, conexões Telnet). Para obter mais informações sobre os tipos de serviço RADIUS, consulte RFC 2865, "Remote Authentication Dial-in User Service (RADIUS)".
- **Tipo de Encapsulamento**. Usado para designar o tipo de túnel que está sendo criado pelo cliente solicitante. Os tipos de túnel incluem o protocolo PPTP (ponto a ponto de encapsulamento) e o L2TP (protocolo de encapsulamento de camada dois).

### <a name="day-and-time-restrictions-attribute-group"></a>Grupo de atributos de restrições de dia e hora 

O grupo de atributos restrições de dia e hora contém o atributo de restrições de dia e hora. Com esse atributo, você pode designar o dia da semana e a hora do dia da tentativa de conexão. O dia e a hora são relativos ao dia e à hora do NPS.

### <a name="gateway-attribute-group"></a>Grupo de atributos de gateway

O grupo de atributos gateway contém os atributos a seguir.

- **ID da estação chamada**. Usado para designar o número de telefone do servidor de acesso à rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.
- **Identificador de nas**. Usado para designar o nome do servidor de acesso à rede. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar identificadores de NAS.
- **Endereço IPv4 do nas**. Usado para designar o endereço IPv4 (protocolo IP versão 4) do servidor de acesso à rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Endereço IPv6 do nas**. Usado para designar o endereço IPv6 (protocolo IP versão 6) do servidor de acesso à rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Tipo de porta nas**. Usado para designar o tipo de mídia usado pelo cliente de acesso. Os exemplos são linhas telefônicas analógicas (conhecidas como Async), ISDN (rede digital de serviços integrados), túneis ou VPNs (redes virtuais privadas), IEEE 802,11 sem fio e comutadores Ethernet.

### <a name="machine-identity-attribute-group"></a>Grupo de atributos de identidade do computador

O grupo de atributos de identidade do computador contém o atributo de identidade do computador. Usando esse atributo, você pode especificar o método com o qual os clientes são identificados na política.

### <a name="radius-client-properties-attribute-group"></a>Grupo de atributos de propriedades do cliente RADIUS

O grupo de atributos propriedades do cliente RADIUS contém os seguintes atributos.

- **ID da estação de chamada**. Usado para designar o número de telefone usado pelo chamador (o cliente de acesso). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar códigos de área.  Em autenticações 802.1 x, o endereço MAC é normalmente preenchido e pode ser correspondido do cliente.  Esse campo é normalmente usado para cenários de bypass de endereço Mac quando a política de solicitação de conexão está configurada para "aceitar usuários sem validar credenciais".  
- **Nome amigável do cliente**. Usado para designar o nome do computador cliente RADIUS que está solicitando a autenticação. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar nomes de cliente.
- **Endereço IPv4 do cliente**. Usado para designar o endereço IPv4 do servidor de acesso à rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Endereço IPv6 do cliente**. Usado para designar o endereço IPv6 do servidor de acesso à rede (o cliente RADIUS). Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões para especificar redes IP.
- **Fornecedor do cliente**. Usado para designar o fornecedor do servidor de acesso à rede que está solicitando a autenticação. Um computador que executa o serviço de roteamento e acesso remoto é o fabricante do NAS da Microsoft. Você pode usar esse atributo para configurar políticas separadas para diferentes fabricantes de NAS. Esse atributo é uma cadeia de caracteres. Você pode usar a sintaxe de correspondência de padrões.

### <a name="user-name-attribute-group"></a>Grupo de atributos de nome de usuário

O grupo de atributos nome de usuário contém o atributo nome de usuário. Usando esse atributo, você pode designar o nome de usuário ou uma parte do nome de usuário que deve corresponder ao nome de usuário fornecido pelo cliente de acesso na mensagem RADIUS. Esse atributo é uma cadeia de caracteres que normalmente contém um nome de realm e um nome de conta de usuário. Você pode usar a sintaxe de correspondência de padrões para especificar nomes de usuário.

## <a name="connection-request-policy-settings"></a>Configurações de política de solicitação de conexão

As configurações de política de solicitação de conexão são um conjunto de propriedades que são aplicadas a uma mensagem RADIUS de entrada. As configurações consistem nos seguintes grupos de propriedades.

- Autenticação
- Contabilização
- Manipulação de atributos
- Encaminhando a solicitação
- Avançado

As seções a seguir fornecem detalhes adicionais sobre essas configurações.

### <a name="authentication"></a>Autenticação

Ao usar essa configuração, você pode substituir as configurações de autenticação definidas em todas as políticas de rede e pode designar os métodos e tipos de autenticação necessários para se conectar à sua rede.

>[!IMPORTANT]
>Se você configurar um método de autenticação na diretiva de solicitação de conexão que seja menos segura do que o método de autenticação configurado na diretiva de rede, o método de autenticação mais seguro que você configurar na política de rede será substituído. Por exemplo, se você tiver uma política de rede que exija o uso do protocolo de autenticação extensível protegida – protocolo de autenticação de handshake de desafio da Microsoft versão 2 \(PEAP-MS-CHAP v2\), que é um método de autenticação baseado em senha para sem fio seguro e você também configura uma política de solicitação de conexão para permitir o acesso não autenticado, o resultado é que nenhum cliente é solicitado a autenticar usando o PEAP-MS-CHAP v2. Neste exemplo, todos os clientes que se conectam à sua rede recebem acesso não autenticado.

### <a name="accounting"></a>Contabilização

Ao usar essa configuração, você pode configurar a política de solicitação de conexão para encaminhar informações de estatísticas para um NPS ou outro servidor RADIUS em um grupo de servidores remotos RADIUS para que o grupo de servidores remotos RADIUS execute a contabilidade.

>[!NOTE]
>Se você tiver vários servidores RADIUS e quiser informações de estatísticas para todos os servidores armazenados em um banco de dados de contabilidade RADIUS central, poderá usar a configuração contabilidade de política de solicitação de conexão em uma política em cada servidor RADIUS para encaminhar dados contábeis de todos os servidores para um NPS ou outro servidor RADIUS designado como um servidor de contabilidade.

As configurações de contabilidade de política de solicitação de conexão funcionam independentemente da configuração de contabilidade do NPS local. Em outras palavras, se você configurar o NPS local para registrar informações de contabilização RADIUS em um arquivo local ou em um Microsoft SQL Server banco de dados, isso fará isso, independentemente de você configurar uma política de solicitação de conexão para encaminhar mensagens contábeis para um RADIUS remoto grupo de servidores.

Se você quiser que as informações de contabilidade sejam registradas remotamente, mas não localmente, você deve configurar o NPS local para não executar a contabilidade, enquanto também configura a contabilidade em uma política de solicitação de conexão para encaminhar dados contábeis para um grupo de servidores remotos RADIUS.

### <a name="attribute-manipulation"></a>Manipulação de atributos

Você pode configurar um conjunto de regras de localização e substituição que manipulam as cadeias de caracteres de texto de um dos atributos a seguir.

- Nome do Usuário
- ID da estação chamada
- ID da estação de chamada

O processamento da regra localizar e substituir ocorre para um dos atributos anteriores antes que a mensagem RADIUS esteja sujeita às configurações de estatísticas e autenticação. As regras de manipulação de atributos se aplicam apenas a um único atributo. Você não pode configurar regras de manipulação de atributo para cada atributo. Além disso, a lista de atributos que você pode manipular é uma lista estática; Não é possível adicionar à lista de atributos disponíveis para manipulação.

>[!NOTE]
>Se você estiver usando o protocolo de autenticação MS-CHAP v2, não poderá manipular o atributo de nome de usuário se a política de solicitação de conexão for usada para encaminhar a mensagem RADIUS. A única exceção ocorre quando uma barra invertida (\) caractere é usada e a manipulação só afeta as informações à esquerda dela. Um caractere de barra invertida normalmente é usado para indicar um nome de domínio (as informações à esquerda do caractere de barra invertida) e um nome de conta de usuário dentro do domínio (as informações à direita do caractere de barra invertida). Nesse caso, somente as regras de manipulação de atributos que modificam ou substituem o nome de domínio são permitidas.

Para obter exemplos de como manipular o nome de realm no atributo de nome de usuário, consulte a seção "exemplos de manipulação do nome de realm no atributo de nome de usuário" no tópico [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Encaminhando a solicitação

Você pode definir as seguintes opções de solicitação de encaminhamento que são usadas para mensagens de solicitação de acesso RADIUS:

- **Autenticar solicitações neste servidor**. Usando essa configuração, o NPS usa um domínio do Windows NT 4,0, Active Directory ou o banco de dados de contas de usuário do SAM (Gerenciador de contas de segurança) local para autenticar a solicitação de conexão. Essa configuração também especifica que a diretiva de rede correspondente configurada no NPS, juntamente com as propriedades de discagem da conta de usuário, são usadas pelo NPS para autorizar a solicitação de conexão. Nesse caso, o NPS é configurado para executar como um servidor RADIUS.

- **Encaminhe solicitações para o seguinte grupo de servidores remotos RADIUS**. Ao usar essa configuração, o NPS encaminha as solicitações de conexão para o grupo de servidores remotos RADIUS que você especificar. Se o NPS receber uma mensagem de aceitação de acesso válida que corresponda à mensagem de solicitação de acesso, a tentativa de conexão será considerada autenticada e autorizada. Nesse caso, o NPS atua como um proxy RADIUS.

- **Aceite usuários sem validar credenciais**. Ao usar essa configuração, o NPS não verifica a identidade do usuário que está tentando se conectar à rede e o NPS não tenta verificar se o usuário ou o computador tem o direito de se conectar à rede. Quando o NPS é configurado para permitir acesso não autenticado e recebe uma solicitação de conexão, o NPS imediatamente envia uma mensagem de aceitação de acesso ao cliente RADIUS e o usuário ou computador recebe acesso à rede. Essa configuração é usada para alguns tipos de encapsulamento compulsório em que o cliente de acesso é encapsulado antes de as credenciais do usuário serem autenticadas.

>[!NOTE]
>Essa opção de autenticação não pode ser usada quando o protocolo de autenticação do cliente de acesso for MS-CHAP v2 ou EAP-TLS (protocolo de autenticação extensível), que fornece autenticação mútua. Na autenticação mútua, o cliente de acesso comprova que se trata de um cliente de acesso válido para o servidor de autenticação (o NPS), e o servidor de autenticação comprova que é um servidor de autenticação válido para o cliente de acesso. Quando essa opção de autenticação é usada, a mensagem de aceitação de acesso é retornada. No entanto, o servidor de autenticação não fornece validação para o cliente de acesso e a autenticação mútua falha.

Para obter exemplos de como usar expressões regulares para criar regras de roteamento que encaminham mensagens RADIUS com um nome de realm especificado para um grupo de servidores RADIUS remotos, consulte a seção "exemplo de encaminhamento de mensagens RADIUS por um servidor proxy" no tópico [usar expressões regulares no NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avançado

Você pode definir propriedades avançadas para especificar a série de atributos RADIUS que são:

- Adicionado à mensagem de resposta RADIUS quando o NPS está sendo usado como um servidor de autenticação ou contabilização RADIUS. Quando há atributos especificados em uma diretiva de rede e na política de solicitação de conexão, os atributos que são enviados na mensagem de resposta RADIUS são a combinação dos dois conjuntos de atributos.
- Adicionado à mensagem RADIUS quando o NPS está sendo usado como um proxy de autenticação ou estatísticas RADIUS. Se o atributo já existir na mensagem que é encaminhada, ele será substituído pelo valor do atributo especificado na política de solicitação de conexão. 

Além disso, alguns atributos que estão disponíveis para configuração na guia **configurações** de diretiva de solicitação de conexão na categoria **avançado** fornecem funcionalidade especializada. Por exemplo, você pode configurar o atributo **RADIUS remoto para mapeamento de usuário do Windows** quando desejar dividir a autenticação e a autorização de uma solicitação de conexão entre dois bancos de dados de contas de usuário.

O atributo **RADIUS remoto para o mapeamento de usuários do Windows** especifica que a autorização do Windows ocorre para usuários que são autenticados por um servidor RADIUS remoto. Em outras palavras, um servidor RADIUS remoto executa a autenticação em uma conta de usuário em um banco de dados de contas de usuário remoto, mas o NPS local autoriza a solicitação de conexão com uma conta de usuário em um banco de dados de contas de usuário local. Isso é útil quando você deseja permitir que os visitantes acessem sua rede.

Por exemplo, os visitantes de organizações parceiras podem ser autenticados por seu próprio servidor RADIUS da organização parceira e, em seguida, usam uma conta de usuário do Windows em sua organização para acessar uma rede local de convidado (LAN) em sua rede.

Outros atributos que fornecem funcionalidade especializada são:

- **MS-Quarantine-IPFilter e MS-Quarantine-Session-Timeout**. Esses atributos são usados quando você implanta o NAQC (controle de quarentena de acesso à rede) com a implantação de VPN de roteamento e acesso remoto.
- **Passport-User-Mapping-UPN-sufixo**. Esse atributo permite que você autentique solicitações de conexão com as credenciais da conta de usuário do Windows Live™ ID.
- **Tunnel-Tag**. Esse atributo designa o número de ID de VLAN para o qual a conexão deve ser atribuída pelo NAS quando você implanta redes locais virtuais (VLANs).

## <a name="default-connection-request-policy"></a>Política de solicitação de conexão padrão

Uma política de solicitação de conexão padrão é criada quando você instala o NPS. Esta política tem a configuração a seguir.

- A autenticação não está configurada.
- A contabilidade não está configurada para encaminhar informações de contabilidade para um grupo de servidores RADIUS remotos.
- O atributo não está configurado com regras de manipulação de atributos que encaminham solicitações de conexão para grupos de servidores remotos RADIUS.
- A solicitação de encaminhamento é configurada para que as solicitações de conexão sejam autenticadas e autorizadas no NPS local.
- Atributos avançados não estão configurados.

A política de solicitação de conexão padrão usa o NPS como um servidor RADIUS. Para configurar um servidor que executa o NPS para atuar como um proxy RADIUS, você também deve configurar um grupo de servidores remotos RADIUS. Você pode criar um novo grupo de servidores remotos RADIUS enquanto estiver criando uma nova política de solicitação de conexão usando o assistente de nova política de solicitação de conexão. Você pode excluir a política de solicitação de conexão padrão ou verificar se a política de solicitação de conexão padrão é a última política processada pelo NPS, colocando-a por último na lista ordenada de políticas.

>[!NOTE]
>Se o NPS e o serviço de acesso remoto estiverem instalados no mesmo computador e o serviço de acesso remoto estiver configurado para autenticação e contabilidade do Windows, é possível que as solicitações de autenticação e contabilização de acesso remoto sejam encaminhadas para um servidor RADIUS . Isso pode ocorrer quando as solicitações de autenticação e contabilização de acesso remoto correspondem a uma política de solicitação de conexão configurada para encaminhá-las a um grupo de servidores remotos RADIUS.
